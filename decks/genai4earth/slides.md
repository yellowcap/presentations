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
T7.2 · State of the art webinar

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
  <div class="g-step" style="background:#CF3F02"><div class="g-n">1</div><div class="g-t">Why it matters</div><div class="g-s">GenAI4Earth and Task 7.2</div></div>
  <div class="g-step c-info" style="background:#2E86AB"><div class="g-n">2</div><div class="g-t">Embedding theory overview</div><div class="g-s">search by meaning, in plain words</div></div>
  <div class="g-step" style="background:#4DA167"><div class="g-n">3</div><div class="g-t">Building blocks</div><div class="g-s">from RAG to tools: data, models, retrieval</div></div>
  <div class="g-step" style="background:#A71D31"><div class="g-n">4</div><div class="g-t">In practice</div><div class="g-s">evals, cost, backend, three ways in</div></div>
  <div class="g-step" style="background:#565B65"><div class="g-n">5</div><div class="g-t">Demo time</div><div class="g-s">demos and discussion</div></div>
</div>

<LogoHorPos position="bottom-right" height="24px" />

<!--
The talk has five parts. First, why this matters: GenAI4Earth and Task 7.2. Then an overview of embedding theory in plain words. Then the building blocks, what we learned in practice, and questions for you.
-->

---
layout: cover
class: divider px-10
background: /images/theme/landsat9-taklimakan-desert-china.jpg
dim: true
---

<div class="d-n">01</div>

# Why it matters

<p class="d-sub">GenAI4Earth and Task 7.2</p>

<LogoHorNegMono position="bottom-right" />

---
layout: cover
class: px-16 
background: "#CF3F02"
---

# How do people find the right Earth data?

<p text-2xl mt-8>And query it without being an expert.</p>

<p text-2xl>Ask in plain language. Get a correct answer or a valid query.</p>

<LogoHorNegMono position="bottom-right" />

<!--
We start with the key question of the task. People must find the right Earth data. They must query it without expert knowledge. They ask in plain language and get a correct answer or a valid query.
-->

---
layout: default
section: 1
---

# GenAI4Earth in brief

<div style="display:grid;grid-template-columns:130px 1fr;gap:1.2rem;align-items:center;margin-top:0.8rem">
  <img src="/images/g4e/earth.jpg" alt="Earth from space" style="width:130px;height:130px;object-fit:cover;border-radius:50%" />
  <ul style="margin:0">
    <li><b>Goal:</b> trustworthy GenAI services for Earth System Science, inside <b>EOSC</b></li>
    <li>17 partners · 2026 to 2029 · <b>WP7:</b> AI-ready dataspaces</li>
  </ul>
</div>

<div class="g-cards c3" style="margin:0.9rem 0 0.6rem">
  <div class="g-card c-primary"><h4>Urban pilot</h4><p>Foundation models for change detection, climate stress, risk</p></div>
  <div class="g-card c-success"><h4>Agro-forestry pilot</h4><p>Trait-based phenotyping. An LLM with RAG is part of the pipeline</p></div>
  <div class="g-card c-info"><h4>Seismology pilot</h4><p>Seismic and fibre-optic (DAS) data for hazards</p></div>
</div>
<p class="text-muted" style="margin:0">Every pilot needs to find the right data first.</p>

<LogoHorPos position="bottom-right" height="24px" />

<!--
GenAI4Earth makes trustworthy GenAI services for Earth System Science in EOSC. It has 17 partners and runs from 2026 to 2029. Three pilots will use the services: urban, agro-forestry and seismology. Each pilot must find the right data first.
-->

---
layout: default
section: 1
---

# Task 7.2: search and querying

<DataToVectorSpace />

<p class="text-muted" style="margin:0.3rem 0 0">Builds on the French research data catalogue search and <code>stac-semantic-search</code></p>

<LogoHorPos position="bottom-right" height="24px" />

<!--
The data is very mixed: satellite images, field observations, seismic data and reports. The plan is to put the metadata from all sources into one vector space. Then people can find data across domains in a conversation. Development Seed leads the task with INRAE, CNRS, GFZ and NKUA.
-->

---
layout: default
section: 1
---

# Where T7.2 sits in WP7

<Wp7TaskMap />

<LogoHorPos position="bottom-right" height="24px" />

<!--
T7.1 makes better metadata, and search depends on it. The T7.5 discovery agent will call our search tools. T7.4 wants provenance and explanations. The three pilots use the result.
-->

---
layout: default
section: 1
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

<!--
The proposal names RAG, LangChain agents, Mistral models and Nomic embeddings. This is our baseline. The next part explains these terms. In our recent work, open Qwen3 embeddings did better than nomic in our test, and we share tools as MCP servers. The goal stays the same. We keep the parts easy to replace.
-->

---
layout: default
section: 1
---

# What do we index?

<div class="g-cards c3">
  <div class="g-card c-info"><img src="/images/g4e/crops.jpg" style="height:80px;width:100%;object-fit:cover;border-radius:6px;margin-bottom:.5rem" /><h4>Datasets</h4><p>Catalogue entries, variables, quality reports</p></div>
  <div class="g-card c-success"><div class="g-big">?</div><h4>Documentation</h4><p>User guides, forum, support answers</p></div>
  <div class="g-card c-danger"><div class="g-big">?</div><h4>Literature</h4><p>Papers and methods: validation, known limits. Much larger, much noisier</p></div>
</div>

<p style="margin:0.3rem 0">If metadata and papers do not say it, search cannot find it.</p>
<div class="g-banner" style="margin-top:0.6rem">Should we include literature?</div>

<LogoHorPos position="bottom-right" height="24px" />

<!--
We start with datasets. We can add documentation and papers in steps. Fitness for purpose is often in papers, not in the metadata. If no source says it, search cannot find it. ESA EVE is one example of a document corpus about Earth observation data. Next: how does embedding search work?
-->

---
layout: cover
class: divider px-10
background: /images/theme/landsat9-apostle-islands-lake-superior.jpg
dim: true
---

<div class="d-n">02</div>

# Embedding theory overview

<p class="d-sub">how search by meaning works, in plain words</p>

<LogoHorNegMono position="bottom-right" />

---
layout: two-cols
section: 2
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

<!--
How does embedding search work? Keyword search finds the words that you type. Embedding search finds the meaning. A model changes each text into a vector. Texts with close meaning have close vectors.
-->

---
layout: default
section: 2
---

# How it works

<IndexQueryDiagram />

<LogoHorPos position="bottom-right" height="24px" />

<!--
We build the index one time. We split documents into chunks, change each chunk into a vector, and keep the vectors in a database. For each question, we change the question into a vector the same way. Then we return the closest chunks.
-->

---
layout: default
section: 2
---

# What is RAG?

<RagDiagram />

<p class="text-muted" style="margin-top:0.4rem">Retrieval Augmented Generation. The model reads. It does not need to know.</p>

<LogoHorPos position="bottom-right" height="24px" />

<!--
RAG means Retrieval Augmented Generation. A search tool finds the best chunks for the question. The language model reads only these chunks. It gives an answer with the sources. RAG is where the proposal starts. The next part shows the blocks that we build on top of it.
-->

---
layout: cover
class: divider px-10
background: /images/theme/landsat9-western-guinea-bissau.jpg
dim: true
---

<div class="d-n">03</div>

# Building blocks

<p class="d-sub">from RAG to tools: data, models, retrieval</p>

<LogoHorNegMono position="bottom-right" />

---
layout: default
section: 3
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

<!--
Our path went from classic RAG to better retrieval. Then search became one tool of many for an agent. Now we share tools as MCP servers on a shared runtime. Each step came from a prototype that hit a limit.
-->

---
layout: default
section: 3
---

# Catalogue scope: aggregator or host?

<CatalogueScope />

<p class="text-muted" style="margin-top:.2rem">Open questions: which catalogues do we index, and do we host any data ourselves?</p>

<LogoHorPos position="bottom-right" height="24px" />

<!--
This is about scope, and it is still open. At one end, we are an aggregator: a metadata repository that indexes other catalogues and links out. In the middle, we link to assets that others host, and we add virtual layers such as virtual Zarr or Kerchunk to make access easier. At the other end, we host data ourselves, for example model output. When we control the data, cloud-native formats make sense. STAC is a good option for spatial data in all three cases, because a lot of tooling is available.
-->

---
layout: default
section: 3
---

# Chunking

<ChunkingDiagram />

<p class="text-muted" style="margin:0.3rem 0 0">Bad chunks cap the quality of all later steps. Keep chunk text apart from vectors, then you can swap the model.</p>

<LogoHorPos position="bottom-right" height="24px" />

<!--
Chunking is crucial. We can use one chunk per document, one per section, or chunks of fixed length. Each choice has a cost. Images in papers are an open question.
-->

---
layout: default
section: 3
---

# Embedding model

<EmbeddingChoices />

<p class="text-muted" style="margin:0.3rem 0 0">No best model. Choose on your own data, and keep it swappable.</p>

<LogoHorPos position="bottom-right" height="24px" />

<!--
The embedding model has a few knobs. A larger model can give more detail, but it is slower. A longer vector holds more detail but needs more storage. Quantisation keeps the same vector with fewer bits, so it needs about 4 or 32 times less memory. Use the same model for documents and questions, put it behind a standard API, and test it on your own queries, because a new model means you must embed everything again.
-->

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

<!--
There are many options. We use Qdrant for hybrid search with BM25. pgvector is good if Postgres is already there. Keep the database behind a thin layer, because it will change.
-->

---
layout: default
section: 3
---

# Filter by attributes

<FilterByAttributes />

<p class="text-muted" style="margin:0.3rem 0 0">Metadata next to the vectors makes the filters possible. The tools pick only valid values.</p>

<LogoHorPos position="bottom-right" height="24px" />

<!--
Example from the urban pilot: a user asks for Sentinel-2 images of Munich. The area, the time and the collection become filters, because we keep these metadata fields next to the vectors; then we rank the rest by similarity. The tools pick only valid values, so the STAC query is valid by construction. A place name can match more than one area, so the place tool returns candidates with ids first.
-->

---
layout: default
section: 3
---

# Search is only as good as the metadata

<div class="g-cards c2">
  <div class="g-card c-danger"><h4>Thin</h4><p><code>SST_v2</code></p><p>"Sea surface temp"</p><p class="g-small">Nothing to match.</p></div>
  <div class="g-card c-success"><h4>Rich and verbose</h4><p>Daily sea surface temperature, 0.05° grid, global, 1982 to now. Built from satellite and buoy data. Known bias near sea ice. Good for marine heatwaves.</p></div>
</div>

<p class="text-muted" style="margin:0.4rem 0 0">What it measures, how, where, when, limits, typical uses.</p>
<div class="g-banner" style="background:#2E86AB">Better metadata in: see <b>T7.1 – LLM tools for metadata management: prototyping</b></div>

<LogoHorPos position="bottom-right" height="24px" />

<!--
Thin metadata gives search nothing to match. Rich metadata tells what the data measures, how, where, when, its limits and its typical uses. T7.1 helps here, because it improves the metadata. See the T7.1 talk, LLM tools for metadata management.
-->

---
layout: default
section: 3
---

# Retrieval: grep, vectors, hybrid, reranker

<RetrievalFunnel />

<p style="margin:0.2rem 0 0"><b style="color:#CF3F02">Benchmark on your own data.</b> <span class="g-small">Grep often beats vectors: <i>Is Grep All You Need?</i> · May 2026</span></p>

<LogoHorPos position="bottom-right" height="24px" />

<!--
Keyword search and vector search run in parallel. We fuse the results, and a reranker puts the top hits in a better order. A May 2026 paper says grep often beats vectors, but the result depends on the corpus, the harness and the token budget. When nothing fits, the tool must say "no evidence".
-->

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

<!--
GraphRAG keeps a graph of entities and links. It can answer questions that need linked facts. We have not used it yet. The open question is if the build cost is worth it.
-->

---
layout: cover
class: divider px-10
background: /images/theme/landsat8-ord-river-australia.jpg
dim: true
---

<div class="d-n">04</div>

# In practice

<p class="d-sub">evals, cost, backend, three ways in</p>

<LogoHorNegMono position="bottom-right" />

---
layout: default
section: 4
---

# Evaluation

<EvalLevels />

<LogoHorPos position="bottom-right" height="24px" />

<!--
We test at two levels. First, is the right dataset in the top results? Second, is the agent answer right, and did the agent call the search tool at the right time? We use versioned regression sets from real questions.
-->

---
layout: default
section: 4
---

# Cost and speed

<CostSpeed />

<p class="text-muted" style="margin:0.3rem 0 0">Measure first. Most of the time and cost is in the LLM calls.</p>

<LogoHorPos position="bottom-right" height="24px" />

<!--
For each question, the LLM calls take most of the time and cost. The search tools and the database are usually fast, and an index and a cache keep them fast. A reranker adds time, so rerank only the top hits. Embedding the corpus is a one-off cost, but a new model means you embed everything again, and storage grows with the vector length.
-->

---
layout: default
section: 4
---

# Infrastructure backend

<SystemArchitecture />

<LogoHorPos position="bottom-right" height="24px" />

<!--
The build runs offline as a nightly cron job in CI. It chunks and embeds the sources, keeps a versioned build in object storage, and runs the evals before the index goes live. The serve side runs on Kubernetes with a Helm chart for each service. This backend is the same for all clients. Next: the three ways in.
-->

---
layout: default
section: 4
---

# Three ways in

<AccessPoints />

<LogoHorPos position="bottom-right" height="24px" />

<!--
One set of search tools is the core. A custom app needs a custom agent. A chat app on web or mobile needs an MCP server. A coding harness needs a CLI with a plugin. The next three slides show each way.
-->

---
layout: default
section: 4
---

# Custom app + agent

<CustomAppMock />

<p class="text-muted" style="margin:0.3rem 0 0">Our own frontend and our own agent. Full control of the flow and the look.</p>

<LogoHorPos position="bottom-right" height="24px" />

<!--
A custom app has its own frontend and its own agent. The agent calls the same search tools as the other clients. We control the full flow and the look.
-->

---
layout: default
section: 4
---

# Chat app, through MCP

<McpViewMock />

<p class="text-muted" style="margin:0.3rem 0 0">On web and mobile. A view shows the results as an interactive card.</p>

<LogoHorPos position="bottom-right" height="24px" />

<!--
A chat app on web or mobile connects to our MCP server. Text answers are not enough for maps and tables. Big tool results stay in state, not in the model context. A view shows the data as an interactive card, and the user can select an item and ask the next question.
-->

---
layout: default
section: 4
---

# Coding harness, through a plugin

<CodingHarnessMock />

<p class="text-muted" style="margin:0.3rem 0 0"><code>/plugin install genai4earth</code> adds skills and a CLI.</p>

<LogoHorPos position="bottom-right" height="24px" />

<!--
Coding harnesses such as Claude Code, Codex or Antigravity are a third way in. One command installs the plugin. The plugin adds skills, which tell the agent the workflow, and a CLI that calls the search tools.
-->

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

<!--
Stay modular, because the field changes fast. Keep LLMs out of the tools, and keep the tools generic. Put the workflow in the agent or in a skill. Test early in CI.
-->

---
layout: default
section: 5
---

# Demo time

<div class="g-cards c2" style="margin-top:0.8rem">
  <div class="g-card c-primary demo"><div><h4>stac-zap</h4><p>Natural language to a valid STAC query</p><p class="g-small"><code>developmentseed.org/stac-zap</code></p></div><QRCode data="https://developmentseed.org/stac-zap/" :width="84" :height="84" /></div>
  <div class="g-card c-success demo"><div><h4>Global Nature Watch · WRI</h4><p>An agent for nature and land data, with dataset search and place search</p><p class="g-small"><code>horizon.globalnaturewatch.org</code></p></div><QRCode data="https://horizon.globalnaturewatch.org/" :width="84" :height="84" /></div>
  <div class="g-card c-info" style="grid-column:span 2"><h4>DestinE Digital Twin Assistant · ECMWF</h4><p>The WP7 flagship example. Live demo now; not public yet</p></div>
</div>

<LogoHorPos position="bottom-right" height="24px" />

<!--
stac-zap changes a plain-language question into a valid STAC query. Global Nature Watch from WRI is an agent for nature and land data, with dataset search and place search. The DestinE Digital Twin Assistant for ECMWF is the WP7 flagship example. I show it live now. It is not launched yet, so there is no link to share.
-->

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
  <div class="g-card c-warning"><h4>Catalogue scope</h4><p>Aggregate only, or host data too?</p></div>
</div>

<LogoHorPos position="bottom-right" height="24px" />

<!--
We need your input. Who are the users, and which datasets come first? Do we include literature? Which catalogues do we use, and how does this fit the GenAI4Earth ecosystem?
-->

---
layout: cover
background: /images/theme/sentinel2a-southern-tibetan-plateau.jpg
dim: true
class: px-10
---

<div style="position:absolute;top:2rem;left:2.5rem;background:white;border-radius:10px;padding:.5rem 1rem"><img src="/images/logos/eosc-genai4earth.png" style="height:34px;width:auto" /></div>

# Thank you

<p text-2xl mt-6>Daniel Wiesmann · Development Seed</p>

<p text-lg mt-10><code text-primary>github.com/yellowcap</code></p>

<div absolute bottom-10 left-10 flex items-center gap-5 text-left>
  <img src="/images/eu-flag.svg" alt="European Union flag" style="height:64px;width:auto" />
  <div text-sm>
    <p font-bold text-base>Funded by the European Union</p>
    <p>Grant agreement number 101293266</p>
  </div>
</div>

<LogoHorNegMono position="bottom-right" />
