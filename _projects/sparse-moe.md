---
layout: page
title: "Sparse MoE: The Stability-Semantics Trade-off"
description: Investigating the limits of input locality in sparse neural networks through diagnostic analysis of the COMET architecture.
img: assets/img/sparse-moe/fig_3_2.png
importance: 1
category: research
github: https://github.com/zhangbaihan/COMET-Sparse-NN-Tuning
---

<div class="project-narrative">

<h2>The Question</h2>

<p>
Sparse neural networks use only a fraction of parameters at inference time, slashing compute without losing capacity.
The <strong>COMET</strong> architecture (Conditionally Overlapping Mixture of ExperTs) takes this further by routing inputs to experts using
<em>fixed random projections</em> — no learned gating, no collapse, no dead neurons. 
</p>

<p>COMET paper: https://arxiv.org/abs/2410.08003</p>

<p>
But this design rests on a strong assumption: that <strong>inputs close in pixel space should share experts</strong>.
A plane on a blue sky and a car on a blue ocean are, in Euclidean distance, nearly the same image.
Should they really activate the same neurons?
</p>

<p>This project stress-tests that assumption.</p>

<hr />

<h2>Diagnosing the Baseline</h2>

<p>
I replicated the COMET architecture on CIFAR-10 — a 4-layer MLP with 3,000 neurons per layer and 50% sparsity — achieving
41.7% test accuracy, consistent with the original paper.
</p>

<p>
The confusion matrix immediately revealed a smoking gun: <strong>569 planes misclassified as ships</strong>.
Both classes share uniform blue backgrounds (sky vs. water), suggesting the router was grouping by <em>texture</em>, not <em>meaning</em>.
</p>

<div class="row justify-content-center">
  <div class="col-md-10">
    {% include figure.liquid loading="lazy" path="assets/img/sparse-moe/fig_3_1.png" class="img-fluid rounded z-depth-1" caption="Baseline confusion matrix. Note the heavy plane → ship misclassification driven by shared blue-background statistics." %}
  </div>
</div>

<hr />

<h2>What Does the Router Actually See?</h2>

<p>
To find out exactly what drives routing decisions, I built a sensitivity analysis framework with four targeted perturbations:
background noise, object occlusion, grayscale conversion, and high-pass filtering.
</p>

<div class="row justify-content-center">
  <div class="col-md-10">
    {% include figure.liquid loading="lazy" path="assets/img/sparse-moe/fig_3_2.png" class="img-fluid rounded z-depth-1" caption="Router sensitivity analysis. High bars = the router ignored the perturbation. The router is almost blind to color removal (0.90) but highly sensitive to background changes (0.54)." %}
  </div>
</div>

<p>
The router <strong>ignores color</strong> (grayscale similarity: 0.90) but is <strong>highly sensitive to background texture</strong> (0.54).
It doesn't care what the object <em>is</em> — it cares about the statistical fingerprint of the pixels surrounding it.
To the router, a plane on blue sky and a ship on blue water are functionally identical.
</p>

<hr />

<h2>The Intervention: Guided-Center</h2>

<p>Could we fix this? I designed <strong>Guided-Center</strong>, a dual-stream architecture that separates the routing signal from the classification signal:</p>

<ul>
  <li><strong>Stream A (Backbone):</strong> Gets the raw input for classification.</li>
  <li><strong>Stream B (Router):</strong> Gets a centered, filtered version of the input, processed through a hybrid router combining a fixed random anchor with a learnable semantic guide.</li>
</ul>

<p>
The learnable component warms in gradually (α: 0→1 over 20 epochs) to avoid destabilizing early training.
</p>

<hr />

<h2>The Surprise: It Got Worse</h2>

<p>
The Guided-Center model converged to <strong>37.9%</strong> — a 4-point drop. Worse, the plane-ship confusion <em>escalated</em> from 569 to
622 misclassifications.
</p>

<div class="row">
  <div class="col-md-6">
    {% include figure.liquid loading="lazy" path="assets/img/sparse-moe/fig_3_1.png" class="img-fluid rounded z-depth-1" caption="Baseline: 569 planes → ships" %}
  </div>
  <div class="col-md-6">
    {% include figure.liquid loading="lazy" path="assets/img/sparse-moe/fig_4_1.png" class="img-fluid rounded z-depth-1" caption="Guided-Center: 622 planes → ships — worse." %}
  </div>
</div>

<p>
The semantic coherence metrics told the same story. Intra-class Jaccard similarity <em>decreased</em> from 0.371 to 0.363 — barely above the random baseline of 0.33.
The learnable router didn't learn to group by meaning; it injected noise.
</p>

<div class="row">
  <div class="col-md-6">
    {% include figure.liquid loading="lazy" path="assets/img/sparse-moe/fig_4_2.png" class="img-fluid rounded z-depth-1" caption="Baseline coherence (J = 0.371)" %}
  </div>
  <div class="col-md-6">
    {% include figure.liquid loading="lazy" path="assets/img/sparse-moe/fig_4_3.png" class="img-fluid rounded z-depth-1" caption="Guided-Center coherence (J = 0.363) — lower." %}
  </div>
</div>

<hr />

<h2>Forensic Analysis: Why?</h2>

<p>
A post-hoc analysis of the router's internal state revealed the mechanism. The semantic branch contributed
<strong>~38% of the total routing magnitude</strong> — it was not dead, not collapsed. It was <em>actively interfering</em>.
</p>

<div class="row justify-content-center">
  <div class="col-md-10">
    {% include figure.liquid loading="lazy" path="assets/img/sparse-moe/fig_5_1.png" class="img-fluid rounded z-depth-1" caption="Branch magnitude histograms. The semantic branch (red) contributes significant signal — but that signal is non-stationary, acting as high-magnitude noise." %}
  </div>
</div>

<p>
The backbone was trying to specialize for a routing pattern that kept shifting under its feet. This is the <strong>Stability-Plasticity Dilemma</strong> in action:
the fixed router is semantically wrong, but it's <em>stable</em>. The learned router could be semantically right, but it's a moving target that prevents the backbone from converging.
</p>

<hr />

<h2>The Insight</h2>

<p>
The t-SNE visualization of learned representations confirms the plane-ship entanglement — their clusters overlap almost entirely in the embedding space.
</p>

<div class="row justify-content-center">
  <div class="col-md-10">
    {% include figure.liquid loading="lazy" path="assets/img/sparse-moe/fig_7_3.png" class="img-fluid rounded z-depth-1" caption="t-SNE of the penultimate layer. Plane (blue) and Ship (red) clusters are thoroughly entangled." %}
  </div>
</div>

<p>
<strong>The core finding:</strong> In sparse networks, the <em>stationarity</em> of the routing target matters more than its <em>semantic quality</em>.
A fixed, semantically-flawed router outperforms a co-trained, semantically-aware one because it gives the backbone a stable optimization landscape.
</p>

<p>
This suggests future work should explore <strong>pre-trained, frozen routers</strong> to get both semantic alignment <em>and</em> stability.
</p>

<div class="row justify-content-center mt-4">
  <div class="col-auto">
    <a href="https://github.com/zhangbaihan/COMET-Sparse-NN-Tuning" class="btn btn-outline-dark" target="_blank">
      <i class="fa-brands fa-github"></i> View Code
    </a>
  </div>
</div>

</div>
