---
layout: page
title: "Doppelganger: a Simulation Machine"
description: Building agents to represent real people.
img: assets/img/doppelganger/cover.png
importance: 2
category: engineering
github: https://github.com/zhangbaihan/Doppelganger
---

<div class="project-narrative">

<h2>Doppelganger</h2>

<p>Try it out: https://doppelganger-three.vercel.app/</p>

<h3>1. Background &amp; Problem</h3>

<p>
Modern social surveying is built on an approximately 100-year-old paradigm: standardized questionnaires and professional infrastructure
(e.g., trained interviewers, online survey platforms, panels, and incentives).
This system depends on respondents making a largely passive contribution (e.g., filling out forms, answering prompts, and clicking options),
which leads to:
</p>

<ul>
  <li>low response rates,</li>
  <li>short or shallow answers,</li>
  <li>insufficient context and detail to explain <em>why</em> people think and behave as they do.</li>
</ul>

<hr />

<h3>2. Proposed Solution</h3>

<p>
We build a simulated world of AI agents, where each agent is instructed to represent a real person.
The long-term vision is a rich, expressive world that can support research and product use cases.
In the initial stage, we focus on a single use case:
</p>

<blockquote>
  <strong>Relationship finding</strong> (romantic or platonic).
</blockquote>

<p>
The system centers on user-configured AI “doppelgangers” that learn over time via voice chat or text with their owners.
The “doppelgangers” can act in a simulated environment on the user’s behalf with other doppelgangers.
</p>

<hr />

<h3>3. Initial Product Scope (v0)</h3>

<h4>3.1 Pixelworld simulated Stanford campus (web app)</h4>

<ul>
  <li>A simple, videogame-like, pixel-art-style simulated world of Stanford campus.</li>
  <li>Delivered as a web application.</li>
  <li>
    The world provides an environment where agents can “exist,” move, and potentially encounter other agents
    (details of interaction are intentionally minimal in v0).
  </li>
</ul>

<h4>3.2 Doppelganger configuration &amp; training</h4>

<ul>
  <li>
    A configuration page where a user creates an AI agent:
    <ul>
      <li>set the agent name,</li>
      <li>customize appearance (avatar/pixel character),</li>
      <li>manage basic settings.</li>
    </ul>
  </li>

  <li>Voice chat training: the user can speak with the agent to teach it about the user over time.</li>

  <li>
    Training content is casual and ongoing. The doppelganger could be thought of as a diary, or as a therapist, e.g.:
    <ul>
      <li><em>“Today I had great dinner at Arrillaga— they had shrimp!”</em></li>
      <li><em>“The world is just crazy right now; can you believe Trump just became president again?”</em></li>
    </ul>
  </li>

  <li>
    The doppelganger stores a private, secure collection of training inputs over time,
    and progressively becomes a better representative of the user.
  </li>
</ul>

<hr />

<h3>4. Data &amp; Privacy Requirements (high-level)</h3>

<ul>
  <li>User training data must be private and securely stored.</li>
  <li>Users should be able to review and delete past training inputs.</li>
  <li>The system should be explicit about what is stored and how it is used to train/condition the agent.</li>
</ul>

<hr />

<h3>5. User Incentives to Train a Doppelganger</h3>

<ol>
  <li>
    <strong>Relationship &amp; collaboration utility.</strong>
    Users train their doppelganger to improve their ability to find:
    <ul>
      <li>romantic matches (dating),</li>
      <li>platonic matches (friends),</li>
      <li>event companions (e.g., someone to sit with at a campus event),</li>
      <li>hackathon teammates and other project collaborators.</li>
    </ul>
  </li>

  <li>
    <strong>Future economic upside from one’s own data.</strong>
    Users may opt in to monetize their doppelganger by granting businesses limited, permissioned interaction privileges to:
    <ul>
      <li>test products and prototypes,</li>
      <li>run qualitative research,</li>
      <li>evaluate messaging and positioning.</li>
    </ul>
  </li>

  <li>
    <strong>Self-discovery through a therapeutic experience.</strong>
    Some users will be motivated by the experience itself: ongoing voice conversations with a non-judgmental, safe agent can function like journaling or reflection,
    helping users learn about their preferences, emotions, and patterns over time.
  </li>
</ol>

<hr />

<h3>6. Open Questions</h3>

<ul>
  <li>What does “trained on a real person” mean operationally in v0 (memory, preferences, persona constraints, embeddings, fine-tuning, or a hybrid)?</li>
  <li>What minimal agent-to-agent interaction is required to make relationship finding useful in the simulated world?</li>
  <li>What are the success metrics for the initial relationship-finding experience (matches, quality ratings, retention, time-to-first-meaningful-connection)?</li>
  <li>How do we prevent sensitive or harmful inferences from casual voice training?</li>
  <li>How do we make sure user feels at ease dumping their heart to this AI agent? What do we need to do to ensure privacy and security?</li>
</ul>

<hr />

<h3>7. What I'm building next</h3>

<p>
The dating app is just the seed functionality. Once we successfully train agents that predict users’ behaviors with high fidelity, we unlock a world of possibilities:
</p>

<ol>
  <li>Enabling monetization of personal data, helping businesses to A/B test;</li>
  <li>Allowing policymakers to pre-screen policy impact on simulated societies;</li>
  <li>Revolutionizing social science methods by enabling counterfactual probing studies.</li>
</ol>

<p>
The immediate next step after TreeHacks is to get the Stanford campus to fall in love with training their agents— the utility of matching improves as the user base grows.
Then, we must solve two technical problems:
</p>

<ol>
  <li>User data privacy concerns;</li>
  <li>Machine learning techniques to build powerful agents that represent their users well.</li>
</ol>

<div class="row justify-content-center mt-4">
  <div class="col-auto">
    <a href="https://github.com/zhangbaihan/Doppelganger" class="btn btn-outline-dark" target="_blank" rel="noopener">
      <i class="fa-brands fa-github"></i> View Code
    </a>
  </div>
</div>

</div>
