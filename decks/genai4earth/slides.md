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

---
layout: default
---

# The question

<p class="text-primary font-mono">1 · Scope</p>

- How do people **find** the right Earth data?
- How do they **query** it without being an expert?
- Catalogues are large: 300+ datasets in one archive, thousands of variables
- Documentation is long. Users do not read it
- Goal: ask in plain language, get a correct answer or a valid query

<LogoHorPos position="bottom-right" height="24px" />

---
layout: default
---

# What do we index?

<p class="text-primary font-mono">1 · Scope</p>

- **Datasets**: catalogue entries, variables, quality reports
- **Documentation**: user guides, forum, support answers
- **Scientific literature**: papers, methods. Much larger, much noisier
- Example: ESA EVE works with a corpus of documents about EO data
- Decision: start with datasets, add documents and papers in steps

<LogoHorPos position="bottom-right" height="24px" />

---
layout: default
---

# The flow of the talk

1. **Data foundation**: catalogue, formats, storage, updates
2. **Search core**: chunks, embeddings, database, metadata, reranking
3. **Agent layer**: from RAG to tools, MCP, access points
4. **Quality**: evals
5. **Production**: what to keep modular
6. **Discussion**: users, datasets, the GenAI4Earth ecosystem

<LogoHorPos position="bottom-right" height="24px" />

---
layout: default
---

# The catalogue underneath

<p class="text-primary font-mono">2 · Data foundation</p>

- What is the source of truth? **STAC** is our default
- Are the assets **cloud native**? COG, Zarr, GeoParquet
- Not cloud native? A virtual layer may be needed: **virtual Zarr, Kerchunk**
- Search finds the data. Access must work too
- Open: which catalogues does GenAI4Earth need?

<LogoHorPos position="bottom-right" height="24px" />

---
layout: default
---

# Keep the index up to date

<p class="text-primary font-mono">2 · Data foundation</p>

- Catalogues change. A static index goes stale
- Build the index **automatically**: GitHub Action on a cron schedule
- Fetch, chunk, embed, index. Version each build
- CI is a core part of the system, not an extra
- Re-run evals after each build

<LogoHorPos position="bottom-right" height="24px" />

---
layout: default
---

# Storage and deployment

<p class="text-primary font-mono">2 · Data foundation</p>

- Where does everything live?
  - Vector database
  - Versioned index builds (object storage)
  - Source catalogue and documents
- Deploy on **Kubernetes** with a **Helm chart**
- One chart per service: database, tools, agent, tracing
- Config in Git (GitOps)

<LogoHorPos position="bottom-right" height="24px" />

---
layout: default
---

# Chunking

<p class="text-primary font-mono">3 · Search core</p>

- Chunking is **crucial**. Bad chunks cap the quality of everything after
- Options:
  - Per document
  - Per section of a document
  - Fixed length. What is the right length?
- Images in papers: embed them too?
- Keep chunk text apart from vectors. Then you can swap the embedding model

<LogoHorPos position="bottom-right" height="24px" />

---
layout: default
---

# Embedding model

<p class="text-primary font-mono">3 · Search core</p>

- **Size**: from 0.6B to 8B parameters
- **Vector length**: 768 to 4096 dimensions. Longer costs more storage
- **Quantisation**: smaller vectors, small loss in quality. Test it
- Open models now match or beat proprietary ones
- Benchmark on our own queries (28 queries, 3 models):
  - nomic: 90% top-1
  - Qwen3-Embedding 0.6B: 100% top-1
- Use a standard API so the model is plug and play

<LogoHorPos position="bottom-right" height="24px" />

---
layout: default
---

# Vector database

<p class="text-primary font-mono">3 · Search core</p>

- Options: **Qdrant**, pgvector, Chroma, LanceDB, DuckDB
- We used Qdrant (hybrid, server-side BM25) and LanceDB (small, local)
- pgvector is attractive if Postgres is already there
- Choose by: hybrid search, filtering, scale, how easy to run
- Keep the database behind a thin layer. It will change

<LogoHorPos position="bottom-right" height="24px" />

---
layout: default
---

# Metadata next to vectors

<p class="text-primary font-mono">3 · Search core</p>

- Similarity alone is not enough. Add **filter fields**:
  - Area of interest: geometry covered
  - Time span covered
  - Last update time
  - Collection, provider, licence
- Filter first, then rank by similarity
- Quality of the metadata sets the quality of the filters

<LogoHorPos position="bottom-right" height="24px" />

---
layout: default
---

# Reranking or plain vector search?

<p class="text-primary font-mono">3 · Search core</p>

- **Plain vector search**: fast, cheap, good first pass
- **Hybrid** (vector + BM25 keywords, rank fusion): catches exact names and codes
- **Reranker**: slower, better order of the top results
- Real problem: relevant and irrelevant hits are close in score
- Say "no evidence" when nothing fits. Do not fake a match

<LogoHorPos position="bottom-right" height="24px" />

---
layout: default
---

# GraphRAG

<p class="text-primary font-mono">3 · Search core</p>

- Build a **graph** of entities and links from the documents
- Answer questions that need several linked facts
- Possible use: datasets, variables, sensors, papers and their links
- We have not used it yet
- Open: when is the extra build cost worth it?

<LogoHorPos position="bottom-right" height="24px" />

---
layout: default
---

# From RAG to tools

<p class="text-primary font-mono">4 · Agent layer</p>

1. **Classic RAG**: retrieve, then answer
2. **Better retrieval**: hybrid, honest
3. **Agents with tools**: search is one tool of many
4. **MCP toolsets**: tools as standalone servers

<p mt-6>Each step came from a prototype that hit a limit.</p>

<LogoHorPos position="bottom-right" height="24px" />

---
layout: default
---

# Tools that make valid queries

<p class="text-primary font-mono">4 · Agent layer</p>

- Natural language becomes a **STAC query**, **SQL** or an **API request**
- The model picks from lists of valid options. It does not invent values
- Places are a tool too: return **candidates with ids**, then geometry
- "Amazon" can mean the river, the forest or the company

<LogoHorPos position="bottom-right" height="24px" />

---
layout: default
---

# Three ways in

<p class="text-primary font-mono">4 · Agent layer</p>

| Access point | Needs |
| --- | --- |
| Custom app | A custom **agent** |
| Chat app (web, mobile) | An **MCP** server |
| Coding harness (Claude Code, Codex, Antigravity) | A **CLI** with a plugin: skills + code |

<p mt-6>One set of tools. Three thin wrappers.</p>

<LogoHorPos position="bottom-right" height="24px" />

---
layout: default
---

# MCP views

<p class="text-primary font-mono">4 · Agent layer</p>

- Text answers are not enough for maps and tables
- Tools can return a **view**: a rendered, interactive output
- Easy to show a map, a chart, a table of datasets
- The user can act on it: select, filter, ask the next question
- Big data stays in state. The view reads from state

<LogoHorPos position="bottom-right" height="24px" />

---
layout: default
---

# Evaluation

<p class="text-primary font-mono">5 · Quality</p>

- **Level 1, search**: are the right datasets in the top results?
- **Level 2, agent**: is the final answer right?
- **Level 2, agent**: did the agent call the search tool at the right time?
- Deterministic checks first: dataset, place, dates
- LLM judge for the answer text
- Versioned regression sets. Real user questions as a source

<LogoHorPos position="bottom-right" height="24px" />

---
layout: default
---

# For production

<p class="text-primary font-mono">6 · Production</p>

- **Stay modular.** The landscape moves fast
- **Keep LLMs out of tools** as much as possible
- **Keep tools generic.** Workflow logic lives in the agent or in a skill
- **Plug-and-play models** through a standard protocol (OpenAI-style API)
- Keep big data out of the model's context
- Test early. CI runs the build and the evals

<LogoHorPos position="bottom-right" height="24px" />

---
layout: default
---

# For discussion

<p class="text-primary font-mono">7 · Discussion</p>

- **Who are the users?** How do we get input from the use cases?
- **Which datasets first?**
- **Scope:** datasets only, or literature too?
- **Integration:** how does this fit the GenAI4Earth ecosystem?
- **Catalogues:** which ones, and are the assets cloud native?

<LogoHorPos position="bottom-right" height="24px" />

---
layout: cover
background: /images/theme/sentinel2a-southern-tibetan-plateau.jpg
dim: true
class: px-10
---

# Thank you

<p text-2xl mt-6>Daniel Wiesmann · Development Seed</p>

<p text-xl mt-10><code text-primary>github.com/developmentseed/mcp-toolsets-runtime</code></p>

<LogoHorNegMono position="bottom-right" />
