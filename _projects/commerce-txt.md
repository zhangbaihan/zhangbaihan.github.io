---
layout: page
title: "commerce.txt: AI-Readable Product Catalogs"
description: An idea to standardize webpages optimized for AI agents to read.
img: assets/img/commerce-txt/cover.png
importance: 2
category: engineering
github: https://github.com/zhangbaihan/YC-Hackathon-11-15
---

<div class="project-narrative">

<h2>The Problem</h2>

<p>
AI shopping agents are everywhere — but they can't actually <em>read</em> a product page.
E-commerce sites serve bloated HTML full of tracking scripts, popups, and layout markup.
An LLM parsing a J.Crew product listing page is like a human reading a novel printed on a billboard covered in Post-it notes.
</p>

<p>
<strong>commerce.txt</strong> is a Top-10-Product-winning protocol built at the <strong>YC Locus Hackathon (November 2025)</strong> that solves this:
take any product catalog and emit a condensed, link-rich Markdown file that AI agents can consume instantly.
Think <code>robots.txt</code> for product discovery — but instead of telling crawlers where <em>not</em> to go, it tells AI agents exactly what's for sale.
</p>

<hr />

<h2>How It Works</h2>

<p>The system is a three-stage pipeline:</p>

<ol>
  <li><strong>Parse:</strong> Ingest raw product data from HTML snapshots, JS bundles, or structured JSON. Custom parsers (J.Crew PLP pages, Netlify storefronts) extract normalized product objects.</li>
  <li><strong>Validate:</strong> Every product passes through strict Pydantic schemas — name, price, currency, URL, description, tags. No garbage in.</li>
  <li><strong>Compress:</strong> A deterministic Markdown compressor highlights price, positioning, and tags for each item, producing a clean numbered catalog with deep links.</li>
</ol>

<p>The output is a plain-text <code>.txt</code> file — no HTML, no JavaScript, no rendering required. An AI agent can fetch it, parse it in a single pass, and immediately reason about the products.</p>

<hr />

<h2>Architecture</h2>

<p>Built with <strong>FastAPI</strong> and designed for extensibility:</p>

<ul>
  <li><code>POST /process/from-file</code> — Accepts a JSON product file, validates it, and returns both the Markdown summary and normalized data.</li>
  <li><code>GET /jcrew/commerce.txt</code> — Streams the cached J.Crew Markdown artifact.</li>
  <li><code>GET /cozyknits/commerce.txt</code> — Streams the cached Cozy Knits catalog.</li>
  <li><code>GET /generate?url=…</code> — Provide a supported product URL and get the <code>commerce.txt</code> generated on the fly.</li>
</ul>

<p>
Each parser is a standalone module — <code>JCrewPlpParser</code> handles saved HTML category pages, <code>EffulgentParser</code> extracts products from compiled JS bundles.
The pipeline glue layer connects any parser to the Markdown compressor, making it trivial to add new storefronts.
</p>

<hr />

<h2>Example Output</h2>

<p>A snippet of what the AI agent actually sees:</p>

<div class="highlight">
<pre><code>## J.Crew men sweaters

1. [Cashmere V-neck sweater](https://www.jcrew.com/...) — $148 USD
   Premium cashmere, relaxed fit. Tags: cashmere, sweater, men

2. [Merino wool crewneck](https://www.jcrew.com/...) — $89.50 USD
   Lightweight merino, machine washable. Tags: merino, sweater, men
   ...
</code></pre>
</div>

<p>
Clean, scannable, and every product is a clickable link.
An agent can read 200 products in the time it would take to render a single product card in a browser.
</p>

<hr />

<h2>What's Next</h2>

<ul>
  <li>Scraping adapters to ingest live storefronts without saved snapshots.</li>
  <li>Provenance metadata per item for AI transparency.</li>
  <li>LLM summarization layers once the structured pipeline is stable.</li>
  <li>A registry where merchants can publish their <code>commerce.txt</code> endpoints — like a sitemap, but for AI commerce.</li>
</ul>

<div class="row justify-content-center mt-4">
  <div class="col-auto">
    <a href="https://github.com/zhangbaihan/YC-Hackathon-11-15" class="btn btn-outline-dark" target="_blank">
      <i class="fa-brands fa-github"></i> View Code
    </a>
  </div>
</div>

</div>
