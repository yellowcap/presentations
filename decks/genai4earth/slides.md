---
title: "T7.2 Tools for Intelligent Data Search / Querying: prototyping"
info: |
  GenAI4Earth WP7 webinar, state of the art. Task 7.2: tools for intelligent data search and querying, prototyping. Daniel Wiesmann, Development Seed.
class: text-center
highlighter: shiki
# Hash URLs (#/4), so a link to any slide works on GitHub Pages.
routerMode: hash
drawings:
  persist: false
  enable: false
transition: slide-left
mdc: true
favicon: /meta/favicon.png
addons:
    - slidev-addon-qrcode

theme: '../../theme'
themeConfig:
  laserColor: '#CF3F02'
  laserWidth: 8
  laserFade: 1500
layout: title
image: /images/theme/landsat9-bangladesh-coast.jpg
---

# Tools for Intelligent Data Search and Querying

::subtitle::
T7.2 · Prototyping

<DecorativeRectangle
  width="50%"
  height="40%"
  zIndex=20
  :position="{
    bottom: '2%',
    right: '2%',
  }"
  :customStyle="{ mixBlendMode: 'multiply' }"
>
  <div w-full h-full relative flex flex-col items-end justify-end p-4 text-white text-right>
    <h3 text-3xl>
      GenAI4Earth WP7 · Nov 9
    </h3>
    <h5 text-sm>
      Daniel Wiesmann · Development Seed
    </h5>
  </div>
</DecorativeRectangle>
<LogoHorPos position="top-left" height="24px" />

<img src="/images/logos/eosc-genai4earth.png" alt="EOSC | GenAI4Earth" style="position:absolute;top:1.6rem;right:2.5rem;height:34px;width:auto;z-index:30" />

---
layout: default
---

# The flow of the talk

<div class="g-flow" style="margin-top:3rem">
  <div class="g-step c-info" style="background:#2E86AB"><div class="g-n">1</div><div class="g-t">Embedding search</div><div class="g-s">in plain words</div></div>
  <div class="g-step" style="background:#CF3F02"><div class="g-n">2</div><div class="g-t">Why it matters</div><div class="g-s">GenAI4Earth, and where RAG fits</div></div>
  <div class="g-step" style="background:#4DA167"><div class="g-n">3</div><div class="g-t">Building blocks</div><div class="g-s">database, models, reranking, agents</div></div>
  <div class="g-step" style="background:#A71D31"><div class="g-n">4</div><div class="g-t">In practice</div><div class="g-s">evals, outlets, cost, speed</div></div>
  <div class="g-step" style="background:#565B65"><div class="g-n">5</div><div class="g-t">Summary</div><div class="g-s">and discussion</div></div>
</div>

<LogoHorPos position="bottom-right" height="24px" />

---
layout: cover
class: divider px-10
background: /images/theme/landsat9-apostle-islands-lake-superior.jpg
dim: true
---

<div class="d-n">01</div>

# Embedding search

<p class="d-sub">in plain words</p>

<LogoHorNegMono position="bottom-right" />

---
layout: two-cols
section: 1
gap: 8
---

# Search by meaning

<div mt-8 />

- **Keyword search** matches the words you type
- **Embedding search** matches the **meaning**
- Text becomes a **vector**: a list of numbers
- Close vectors, close meaning

::right::

<div mt-8 />

<EmbeddingMap />

<LogoHorPos position="bottom-right" height="24px" />

---
layout: default
section: 1
---

# How it works

<Flow :items="[
  {t:'Split', s:'documents into chunks', c:'info'},
  {t:'Embed', s:'each chunk becomes a vector', c:'primary'},
  {t:'Store', s:'vectors go in a vector database', c:'success'},
  {t:'Ask', s:'embed the question the same way', c:'danger'},
  {t:'Match', s:'return the closest chunks', c:'muted'},
]" />

<p class="text-muted" mt-8>Build once. Search many times.</p>

<LogoHorPos position="bottom-right" height="24px" />

---
layout: default
section: 1
---

# What is RAG?

<Flow :items="[
  {t:'Question', s:'from the user', c:'info'},
  {t:'Retrieve', s:'embedding search finds the best chunks', c:'primary'},
  {t:'Read', s:'the language model reads only those chunks', c:'success'},
  {t:'Answer', s:'with sources', c:'danger'},
]" />

<div class="g-banner" style="background:#565B65">Retrieval Augmented Generation. The model reads. It does not need to know.</div>

<LogoHorPos position="bottom-right" height="24px" />

---
layout: cover
class: divider px-10
background: /images/theme/landsat9-taklimakan-desert-china.jpg
dim: true
---

<div class="d-n">02</div>

# Why it matters

<p class="d-sub">in GenAI4Earth, and where RAG fits</p>

<LogoHorNegMono position="bottom-right" />

---
layout: image-right
image: /images/g4e/earth.jpg
class: image-narrow
section: 2
---

# GenAI4Earth in brief

<div mt-6 />

- **Goal:** trustworthy GenAI services for Earth System Science, inside **EOSC**
- 17 partners · 2026 to 2029
- **WP7:** AI-ready dataspaces
- Mixed data: satellite, field, seismic, reports

---
layout: default
section: 2
---

# Three pilots will use it

<div class="g-cards c3">
  <div class="g-card c-primary"><h4>Urban</h4><p>Foundation models for change detection, climate stress, risk. Munich, Leipzig, Grenoble, Strasbourg</p></div>
  <div class="g-card c-success"><h4>Agro-forestry</h4><p>Trait-based phenotyping of crops and trees. An LLM with RAG is part of the pipeline</p></div>
  <div class="g-card c-info"><h4>Seismology</h4><p>Seismic and fibre-optic (DAS) data to detect and forecast hazards</p></div>
</div>
<p class="text-muted">Every pilot needs to find the right data first.</p>

<LogoHorPos position="bottom-right" height="24px" />

---
layout: default
section: 2
---

# Task 7.2: search and querying

<Flow :items="[
  {t:'Metadata', s:'from all sources: images, field, seismic, reports', c:'info'},
  {t:'One vector space', s:'a shared semantic index', c:'primary'},
  {t:'Conversational discovery', s:'across domains', c:'success'},
]" />

<p>Builds on two prototypes: <b>French research data catalogue</b> search and <code>stac-semantic-search</code></p>
<p class="text-muted">Development Seed (lead) · INRAE · CNRS · GFZ · NKUA</p>

<LogoHorPos position="bottom-right" height="24px" />

---
layout: default
section: 2
---

# Where we start

<div class="g-cards c2">
  <div class="g-card c-info"><h4>The proposal</h4>
    <p>RAG</p><p>LangChain agents</p><p>Mistral models</p><p>Nomic embeddings</p></div>
  <div class="g-card solid c-primary"><h4>What we learned since</h4>
    <p>Open <b>Qwen3</b> embeddings beat nomic in our test</p>
    <p>Small, generic tools shared as <b>MCP</b> servers</p>
    <p>Hybrid search and honest "no evidence" matter more than the model</p></div>
</div>

<p class="text-muted">Same goal. The parts keep changing, so keep them swappable.</p>

<LogoHorPos position="bottom-right" height="24px" />

---
layout: cover
class: px-16 
background: "#CF3F02"
---

# How do people find the right Earth data?

<p text-2xl mt-8>And query it without being an expert.</p>

<p text-2xl>Ask in plain language. Get a correct answer or a valid query.</p>

<LogoHorNegMono position="bottom-right" />

---
layout: default
section: 2
---

# What do we index?

<div class="g-cards c3">
  <div class="g-card c-info"><img src="/images/g4e/crops.jpg" style="height:90px;width:100%;object-fit:cover;border-radius:6px;margin-bottom:.5rem" /><h4>Datasets</h4><p>Catalogue entries, variables, quality reports</p></div>
  <div class="g-card c-success"><div class="g-big">?</div><h4>Documentation</h4><p>User guides, forum, support answers</p></div>
  <div class="g-card c-danger"><div class="g-big">?</div><h4>Literature</h4><p>Papers and methods. Much larger, much noisier</p></div>
</div>

<p class="text-muted">Start with datasets. Add documents and papers in steps. Example corpus: ESA EVE.</p>

<LogoHorPos position="bottom-right" height="24px" />

---
layout: default
section: 2
---

# Fitness for purpose

<Flow :items="[
  {t:'Dataset metadata', s:'does it say how good, and for what?', c:'info'},
  {t:'Papers and studies', s:'validation, known limits, case studies', c:'success'},
  {t:'Can I use it here?', s:'my place, my question', c:'primary'},
]" />

<p>If neither says it, search cannot find it.</p>
<div class="g-banner">Should we include a body of literature as well?</div>

<LogoHorPos position="bottom-right" height="24px" />

---
layout: default
section: 2
---

# From RAG to tools

<Flow :items="[
  {t:'Classic RAG', s:'retrieve, then answer', c:'info'},
  {t:'Better retrieval', s:'hybrid, honest', c:'success'},
  {t:'Agents with tools', s:'search is one tool of many', c:'primary'},
  {t:'MCP toolsets', s:'tools as standalone servers', c:'danger'},
]" />

<p class="text-muted">Each step came from a prototype that hit a limit.</p>

<LogoHorPos position="bottom-right" height="24px" />

---
layout: cover
class: divider px-10
background: /images/theme/landsat9-western-guinea-bissau.jpg
dim: true
---

<div class="d-n">03</div>

# Building blocks

<p class="d-sub">database, models, reranking, agent layer</p>

<LogoHorNegMono position="bottom-right" />

---
layout: default
section: 3
---

# The catalogue underneath

<div class="g-cards c3">
  <div class="g-card c-primary"><h4>STAC</h4><p>Our default source of truth</p></div>
  <div class="g-card c-success"><h4>Cloud native</h4><p>COG, Zarr, GeoParquet. Search finds it, access must work too</p></div>
  <div class="g-card c-info"><h4>Not cloud native?</h4><p>Add a virtual layer: virtual Zarr, Kerchunk</p></div>
</div>
<p class="text-muted">Open: which catalogues does GenAI4Earth need?</p>

<LogoHorPos position="bottom-right" height="24px" />

---
layout: default
section: 3
---

# Chunking

<div class="g-cards c3">
  <div class="g-card c-info"><h4>Per document</h4><p>Simple. Loses detail</p></div>
  <div class="g-card c-success"><h4>Per section</h4><p>Follows the structure of the text</p></div>
  <div class="g-card c-warning"><h4>Fixed length</h4><p>Predictable. Which length?</p></div>
</div>
<p>Bad chunks cap the quality of everything after. Images in papers: embed them too?</p>
<p class="text-muted">Keep chunk text apart from vectors. Then you can swap the model.</p>

<LogoHorPos position="bottom-right" height="24px" />

---
layout: default
section: 3
---

# Embedding model

<div class="g-cards c3">
  <div class="g-card c-info"><div class="g-big">0.6–8B</div><h4>Size</h4><p>Open models now match proprietary ones</p></div>
  <div class="g-card c-primary"><div class="g-big">768–4096</div><h4>Vector length</h4><p>Longer costs more storage. Quantise and test</p></div>
  <div class="g-card c-success"><div class="g-big">100% / 90%</div><h4>Top-1 on our 28 queries</h4><p>Qwen3 0.6B vs nomic</p></div>
</div>
<p class="text-muted">Use a standard API so the model is plug and play.</p>

<LogoHorPos position="bottom-right" height="24px" />

---
layout: default
section: 3
---

# Vector database

<div class="g-cards c4">
  <div class="g-card c-primary"><h4>Qdrant</h4><p>Hybrid search with server-side BM25</p></div>
  <div class="g-card c-info"><h4>pgvector</h4><p>Attractive if Postgres is already there</p></div>
  <div class="g-card c-success"><h4>LanceDB</h4><p>Small and local</p></div>
  <div class="g-card c-warning"><h4>Chroma · DuckDB</h4><p>Light options to start</p></div>
</div>
<p>Choose by hybrid search, filtering, scale, ease of running.</p><p class="text-muted">Keep it behind a thin layer. It will change.</p>

<LogoHorPos position="bottom-right" height="24px" />

---
layout: default
section: 3
---

# Metadata next to vectors

<div class="g-cards c4">
  <div class="g-card c-info"><h4>Area</h4><p>Geometry covered</p></div>
  <div class="g-card c-success"><h4>Time span</h4><p>Period covered</p></div>
  <div class="g-card c-warning"><h4>Last update</h4><p>How fresh</p></div>
  <div class="g-card c-danger"><h4>Collection</h4><p>Provider, licence</p></div>
</div>
<Flow :items="[{t:'Filter first', s:'on these fields', c:'muted'},{t:'Then rank', s:'by similarity', c:'primary'}]" />

<LogoHorPos position="bottom-right" height="24px" />

---
layout: default
section: 3
---

# Search is only as good as the metadata

<div class="g-cards c2">
  <div class="g-card c-danger"><h4>Thin</h4><p><code>SST_v2</code></p><p>"Sea surface temp"</p><p class="g-small">Nothing to match.</p></div>
  <div class="g-card c-success"><h4>Rich and verbose</h4><p>Daily sea surface temperature, 0.05° grid, global, 1982 to now. Built from satellite and buoy data. Known bias near sea ice. Good for marine heatwaves.</p></div>
</div>

<p class="text-muted">What it measures, how, where, when, limits, typical uses.</p>

<LogoHorPos position="bottom-right" height="24px" />

---
layout: default
section: 3
---

# Grep or vectors?

<p class="text-muted" style="margin-top:0"><i>Is Grep All You Need? How Agent Harnesses Reshape Agentic Search</i> · May 2026</p>

<div class="g-cards c3">
  <div class="g-card c-info"><h4>The corpus</h4><p>Code or prose</p></div>
  <div class="g-card c-success"><h4>The harness</h4><p>How the agent searches and loops</p></div>
  <div class="g-card c-warning"><h4>The token budget</h4><p>What you can afford to read</p></div>
</div>

<p>Grep often beats vector retrieval. But it depends on all three.</p>
<div class="g-banner">Benchmark on your own data.</div>

<LogoHorPos position="bottom-right" height="24px" />

---
layout: default
section: 3
---

# Reranking or plain vector search?

<div class="g-funnel">
  <div style="background:#2E86AB;width:92%"><b>Vector</b> · fast, cheap first pass over everything</div>
  <div style="background:#4DA167;width:74%"><b>Hybrid</b> · adds BM25 keywords, catches exact names and codes</div>
  <div style="background:#CF3F02;width:56%"><b>Reranker</b> · slower, better order of the top hits</div>
</div>

<p class="text-muted" mt-6>Relevant and irrelevant hits can score close. When nothing fits, say "no evidence".</p>

<LogoHorPos position="bottom-right" height="24px" />

---
layout: two-cols
section: 3
gap: 8
---

# GraphRAG

<div mt-8 />

- A **graph** of entities and links
- Answers questions that need linked facts
- We have not used it yet
- Open: is the build cost worth it?

::right::

<div mt-6 />

<svg viewBox="0 0 400 260" style="width:100%">
  <g stroke="#999" stroke-width="2.5">
    <line x1="200" y1="130" x2="80" y2="55"/><line x1="200" y1="130" x2="320" y2="55"/>
    <line x1="200" y1="130" x2="80" y2="205"/><line x1="200" y1="130" x2="320" y2="205"/>
    <line x1="80" y1="55" x2="320" y2="55" stroke-dasharray="5 5"/>
  </g>
  <g style="font-size:13px" text-anchor="middle" fill="white">
    <circle cx="200" cy="130" r="38" fill="#CF3F02"/><text x="200" y="135">Dataset</text>
    <circle cx="80" cy="55" r="32" fill="#2E86AB"/><text x="80" y="60">Variable</text>
    <circle cx="320" cy="55" r="32" fill="#4DA167"/><text x="320" y="60">Sensor</text>
    <circle cx="80" cy="205" r="32" fill="#A71D31"/><text x="80" y="210">Paper</text>
    <circle cx="320" cy="205" r="32" fill="#565B65"/><text x="320" y="210">Place</text>
  </g>
</svg>

<LogoHorPos position="bottom-right" height="24px" />

---
layout: default
section: 3
---

# Tools that make valid queries

<Flow :items="[
  {t:'Plain language', s:'“Sentinel-2 over the Amazon, 2024”', c:'info'},
  {t:'Pick from valid options', s:'collections, fields, places. No invented values', c:'primary'},
  {t:'Valid query', s:'STAC, SQL or API request', c:'success'},
]" />

<p>Places are a tool too: return <b>candidates with ids</b> first. “Amazon” can be a river, a forest or a company.</p>

<LogoHorPos position="bottom-right" height="24px" />

---
layout: cover
class: divider px-10
background: /images/theme/landsat8-ord-river-australia.jpg
dim: true
---

<div class="d-n">04</div>

# In practice

<p class="d-sub">evals, outlets, cost, speed, design</p>

<LogoHorNegMono position="bottom-right" />

---
layout: default
section: 4
---

# Evaluation

<div class="g-cards c2">
  <div class="g-card c-info"><h4>Level 1 · Search</h4><p>Are the right datasets in the top results?</p></div>
  <div class="g-card c-primary"><h4>Level 2 · Agent</h4><p>Is the final answer right?</p><p>Did it call the search tool at the right time?</p></div>
</div>

<Flow :items="[
  {t:'Deterministic checks', s:'dataset, place, dates', c:'success'},
  {t:'LLM judge', s:'for the answer text', c:'danger'},
  {t:'Regression sets', s:'versioned, from real questions', c:'muted'},
]" />

<LogoHorPos position="bottom-right" height="24px" />

---
layout: default
section: 4
---

# Three ways in

<div class="g-cards c3">
  <div class="g-card solid c-info"><h4>Custom app</h4><p>needs a custom <b>agent</b></p></div>
  <div class="g-card solid c-primary"><h4>Chat app</h4><p>web, mobile · needs an <b>MCP</b> server</p></div>
  <div class="g-card solid c-success"><h4>Coding harness</h4><p>Claude Code, Codex · needs a <b>CLI</b> with a plugin</p></div>
</div>

<div class="g-banner" style="background:#565B65">One set of tools. Three thin wrappers.</div>

<LogoHorPos position="bottom-right" height="24px" />

---
layout: default
section: 4
---

# MCP views

<Flow :items="[
  {t:'Tool result', s:'big data stays in state', c:'info'},
  {t:'View', s:'a rendered, interactive map, chart or table', c:'primary'},
  {t:'User acts', s:'select, filter, ask the next question', c:'success'},
]" />

<p class="text-muted">Text answers are not enough for maps and tables.</p>

<LogoHorPos position="bottom-right" height="24px" />

---
layout: default
section: 4
---

# Cost and speed

<div class="g-cards c4">
  <div class="g-card c-primary"><div class="g-big">470×</div><h4>Place search</h4><p>4.7 s to about 10 ms with a database index</p></div>
  <div class="g-card c-info"><div class="g-big">81 min</div><h4>Re-embedding</h4><p>450k chunks, large model, 4 GPUs</p></div>
  <div class="g-card c-success"><div class="g-big">768→4096</div><h4>Storage</h4><p>Longer vectors cost more. Quantisation helps</p></div>
  <div class="g-card c-danger"><div class="g-big">#1</div><h4>The LLM</h4><p>Main cost and delay. Keep it out of the tools</p></div>
</div>
<p class="text-muted">A reranker adds time to every query. Is the gain worth it?</p>

<LogoHorPos position="bottom-right" height="24px" />

---
layout: default
section: 4
---

# Keep the index up to date

<Flow :items="[
  {t:'Fetch', s:'catalogue changes', c:'info'},
  {t:'Chunk', c:'success'},
  {t:'Embed', c:'primary'},
  {t:'Index', s:'new versioned build', c:'danger'},
  {t:'Evals', s:'run after each build', c:'muted'},
]" />

<p>GitHub Action on a cron schedule. CI is a core part of the system.</p>

<LogoHorPos position="bottom-right" height="24px" />

---
layout: default
section: 4
---

# Storage and deployment

<div class="g-cards c3">
  <div class="g-card c-primary"><h4>Vector database</h4><p>The live index</p></div>
  <div class="g-card c-info"><h4>Object storage</h4><p>Versioned index builds</p></div>
  <div class="g-card c-success"><h4>Source</h4><p>Catalogue and documents</p></div>
</div>
<p><b>Kubernetes</b> with a <b>Helm chart</b> per service: database, tools, agent, tracing. Config in Git.</p>

<LogoHorPos position="bottom-right" height="24px" />

---
layout: cover
class: px-16 
background: "#2E86AB"
---

# Design for production

- **Stay modular.** The landscape moves fast
- **Keep LLMs out of tools**
- **Keep tools generic.** Workflow lives in the agent or a skill
- **Plug-and-play models** through a standard API
- **Test early.** CI runs the build and the evals

<LogoHorNegMono position="bottom-right" />

---
layout: cover
class: divider px-10
background: /images/theme/Tanezrouft_Basin.jpg
dim: true
---

<div class="d-n">05</div>

# Summary

<p class="d-sub">and discussion</p>

<LogoHorNegMono position="bottom-right" />

---
layout: cover
class: px-16 
background: "#4DA167"
---

# Summary

- Embedding search finds data by **meaning**
- RAG is a **start**. Tools and agents do the rest
- Search is only as good as the **metadata**
- Choose each block on **your data**: grep, vectors, hybrid, reranker
- **Evals** tell you what works
- Stay **modular**

<LogoHorNegMono position="bottom-right" />

---
layout: default
section: 5
---

# For discussion

<div class="g-cards c3">
  <div class="g-card c-primary"><h4>Users</h4><p>Who are they? How do we get input from the use cases?</p></div>
  <div class="g-card c-info"><h4>Datasets</h4><p>Which ones first?</p></div>
  <div class="g-card c-success"><h4>Scope</h4><p>Datasets only, or literature too? How do we judge fitness for purpose?</p></div>
  <div class="g-card c-danger"><h4>Integration</h4><p>How does this fit the GenAI4Earth ecosystem?</p></div>
  <div class="g-card c-warning"><h4>Catalogues</h4><p>Which ones? Are the assets cloud native?</p></div>
</div>

<LogoHorPos position="bottom-right" height="24px" />

---
layout: cover
background: /images/theme/sentinel2a-southern-tibetan-plateau.jpg
dim: true
class: px-10
---

<div style="position:absolute;top:2rem;left:2.5rem;background:white;border-radius:10px;padding:.5rem 1rem"><img src="/images/logos/eosc-genai4earth.png" style="height:34px;width:auto" /></div>

# Thank you

<p text-2xl mt-6>Daniel Wiesmann · Development Seed</p>

<p text-xl mt-10><code text-primary>github.com/developmentseed/mcp-toolsets-runtime</code></p>

<div absolute bottom-10 left-10 flex items-center gap-5 text-left>
  <img src="/images/eu-flag.svg" alt="European Union flag" style="height:64px;width:auto" />
  <div text-sm>
    <p font-bold text-base>Funded by the European Union</p>
    <p>Grant agreement number 101293266</p>
  </div>
</div>

<LogoHorNegMono position="bottom-right" />
