# GenAI4Earth WP7 webinar deck: hand-off

This file is a hand-off for future sessions. It sums up the work so far on this deck.

## The talk

- **Event:** GenAI4Earth WP7 webinar series, "State of the art", **9 November 2026**
- **Title:** T7.2 – Tools for Intelligent Data Search / Querying: prototyping
- **Speaker:** Daniel Wiesmann, Development Seed
- **Project:** GenAI4Earth, https://genai4earth.earth-data.eu/ (task 7.2 of work package 7)
- **Status:** first draft with simple bullets. The user wants the flow right first and the detail later.

## Files

- `slides.md`: the deck (Slidev, DevSeed theme from `../../theme`)
- `public/`: static files of this deck only (font, favicon, logos, 2 landscape images)
- Site path: `/genai4earth/`. Build: `pnpm run build:genai4earth`. Preview: `pnpm dev:genai4earth`
- Other decks have their own folders in `decks/`. Do not share `public/` files between decks

## Open gaps

- Project site was read on 6 Nov 2026. See "Project context" below
- The user's claude.ai chat history is not visible to Claude Code. A search of past Claude Code sessions found nothing on GenAI4Earth
- Last slide links to `github.com/developmentseed/mcp-toolsets-runtime`. Replace it if a better link exists
- Some slides only name a topic, because the survey had no evidence: quantisation, pgvector, Chroma, GraphRAG (not used yet by the user), ESA EVE, GitHub Action cron build, Helm chart as stated by the user. Check them before adding facts

## Project context (site read 6 Nov 2026; WP7 text pasted by the user from the proposal)

**Project (https://genai4earth.earth-data.eu/)**

- Full name: Generative Artificial Intelligence for Earth System. Horizon Europe, call HORIZON-INFRA-01-EOSC-05, GA 101293266. Runs Sep 2026 to Aug 2029
- Mission: reusable, trustworthy GenAI services for Earth System Sciences (ESS). Integrate FAIR data, models and workflows into the EOSC ecosystem
- Objectives: GenAI tools for FAIRness, machine-actionability, AI-readiness, provenance; AI interfaces for discovery, access, cross-domain integration; responsible AI (reproducible, explainable, transparent, frugal); capacity building
- 17 partners, coordinator CNRS. Includes Development Seed. Advisory board: "coming soon"
- Part of the AI4EOSC ecosystem. Services will be available there (federated AAI, training catalogues, traceability). GenAI4Earth adds GenAI, explainability dashboards, bias monitoring, FAIRification pipelines
- Four pillars: (1) governance, policy, EOSC alignment WP1-6; (2) GenAI services and scientific co-design WP7-10; (3) infrastructure WP11-12 (federated HPC/cloud; WP12 deploys WP7/WP8 services); (4) capacity building WP13-14
- Three pilots (WP9/10): **Urban** (foundation models, VLMs, FO-DAS; Munich, Leipzig, Grenoble, Strasbourg); **Agro-forestry** (trait-based phenotyping; the pipeline uses an LLM with RAG; AnaEE, eLTER); **Seismology** (seismic + fibre-optic DAS; EPOS, Data Terra, NFDI)
- Site pages not read: outputs, news, trainings, validation opportunities, helpdesk

**WP7: AI-ready Dataspaces, Service Co-design and Prototyping** (lead INRAE, M1-M18)

- Dual strategy: AI as a backend tool to automate dataspace creation, and a frontend service for discovery. Co-design loop with the pilots. Frugal and responsible AI
- Scales an end-to-end system from the INRAE Recherche Data Gouv catalogue. Uses Development Seed's DestinE Digital Twin Assistant as a flagship example
- T7.1 metadata management (INRAE): pre-fill deposit forms from DMPs, papers, READMEs; "Curation Advisor" with an LLM agent swarm
- **T7.2 Tools for Intelligent Data Search / Querying (lead Development Seed; INRAE, CNRS, GFZ, NKUA; M2-M18)** - this talk
- T7.3 actionable DMPs (UEDIN); T7.4 explainable GenAI, provenance with C2PA, SHAP/LIME, human-aligned embeddings (NKUA); T7.5 GenAI scientific workflows, LangGraph agent swarm (KIT); T7.6 model engineering, aligned embeddings GeoCLIP/satCLIP (INRIA)

**T7.2 text from the proposal**

- Challenge: discover and combine very heterogeneous data: geospatial imagery, phenotype observations, seismological data, scientific reports
- Builds on TRL7 work: the semantic search prototype for the French research data catalogue (catalogue.proto-sandbox.portail-rdg-dev.fr) and the `stac-semantic-search` extension
- Strategy: RAG. Convert rich metadata from all sources into one semantic vector space. Conversational discovery across domains
- Proposal tech: agents with **LangChain**; **Mistral** models for interpretation; **Nomic** models for embeddings
- **Tension to handle with care:** our own tests found Qwen3-Embedding beat nomic, and our recent work uses MCP toolsets, not LangChain. The deck says "what we learned so far" and does not contradict the proposal. Decided 6 Nov 2026: keep the "Where we start" slide as written (proposal baseline next to what we learned)
- Links to other tasks: T7.5 "Data Discovery" agent will call these search tools; T7.4 wants provenance and explanations on AI-made metadata; T7.1 improves the metadata that search depends on

## The flow (35 slides, five parts)

Reordered on 6 Nov 2026. Visual pass and restructure done the same day. Reordered again on 9 Nov 2026: "Why it matters" is now part 1, the embedding part is part 2 and is called "Embedding theory overview". Parts 1 to 4 start with a divider slide. Part 5 has no divider. Every content slide has speaker notes (an HTML comment at the end of the slide, ASD-STE100).

0. Title; the flow of the talk
1. **Why it matters:** the question (orange slide, first after the divider); GenAI4Earth in brief with the three pilots (one slide, small Earth photo); Task 7.2 (`DataToVectorSpace`); where T7.2 sits in WP7 (`Wp7TaskMap`); where we start; what we index, with the literature question. The last note points forward to "how does embedding search work?"
2. **Embedding theory overview, in plain words:** meaning vs keywords (`EmbeddingMap`); how it works (`IndexQueryDiagram`); what RAG is (`RagDiagram`)
3. **Building blocks:** from RAG to tools (`Flow`); catalogue scope (`CatalogueScope`: aggregator vs host spectrum; metadata repository, link + virtual layers, host data in cloud-native formats; STAC across all; open question); chunking (`ChunkingDiagram`); embedding model (`EmbeddingChoices`: size, vector length, quantisation, fit, rules; no benchmark numbers); vector database; filter by attributes (`FilterByAttributes`: urban-pilot example, Sentinel-2 over Munich; area, time and collection filters from metadata next to the vectors, then similarity rank, then a valid STAC query; place candidates with ids. Replaces the old "Metadata next to vectors" and "Tools that make valid queries" slides); metadata quality (thin vs rich, with a pointer line to T7.1 – LLM tools for metadata management: prototyping; no URL known); retrieval: grep, vectors, hybrid, reranker (`RetrievalFunnel`, May 2026 paper, text from the user, paper not read); GraphRAG (inline SVG graph)
4. **In practice:** evals (`EvalLevels`); cost and speed (`CostSpeed`: illustrative share per step, one-off costs, levers; no project numbers); infrastructure backend, build and serve (`SystemArchitecture`); three ways in (`AccessPoints`); custom app + agent (`CustomAppMock`); chat app on web and mobile, through MCP (`McpViewMock`); coding harness, through a plugin (`CodingHarnessMock`: `/plugin install genai4earth` installs skills and a CLI); design for production (blue slide). The backend comes before the three ways in, because the ways in are the client side
5. **Try it and discuss** (the green summary slide was removed 9 Nov 2026; titles of the three ways in have no "Way N" prefix): demo time (demo cards with QR codes: stac-zap, Global Nature Watch; DestinE assistant card with no link or QR, because it is not launched yet: demo live, do not share the URL; section 5); for discussion (section 5); thank you

## Visual system (how the deck is built)

- `slides.md` is generated by hand-written Slidev markdown. Slides use `section: N` in frontmatter. `global-bottom.vue` reads it and draws the footer: EOSC | GenAI4Earth logo and a breadcrumb (1 to 5). Its `parts` array is ['Why it matters', 'Embedding theory overview', 'Building blocks', 'In practice', 'Try it and discuss'] (changed 9 Nov 2026). Keep it in step with the section order. Slides without `section` (title, flow of the talk, dividers, colored slides, thank-you) have no footer
- Footer logo is the combined EOSC | GenAI4Earth logo from the project site (`public/images/logos/eosc-genai4earth.png`, cropped). It is also on the title slide and, in a white pill, on the last slide
- `components/Flow.vue`: row of numbered colored steps (now only on "From RAG to tools"). `components/EmbeddingMap.vue`: SVG map of meaning space
- Diagram components: `IndexQueryDiagram`, `RagDiagram`, `DataToVectorSpace`, `Wp7TaskMap`, `ChunkingDiagram`, `EmbeddingChoices`, `FilterByAttributes`, `RetrievalFunnel`, `EvalLevels`, `AccessPoints`, `CustomAppMock`, `McpViewMock` (chat app on web and mobile), `CodingHarnessMock`, `CostSpeed`, `SystemArchitecture`. `FilterThenRank` and `ValidQuery` are no longer used in the deck. Each is one self-contained inline SVG with no props. The content is hard-coded in the component. Each fits about 880x360 px. A slide with a diagram has the title, the tag, and at most one short line or banner
- `style.css`: classes `g-cards` (`c2`/`c3`/`c4`), `g-card` (`c-primary|info|success|danger|warning`, `solid`), `g-big`, `g-banner`, `g-funnel` (not used now), `g-card.demo` (text left, QR code right), `divider`
- Do not quote numbers from projects the audience does not know (for example the EIE embedding test or project timings). Decided 6 Nov 2026: the EIE chart and the cost and speed numbers were removed
- QR codes: `<QRCode data="..." :width="84" :height="84" />` from `slidev-addon-qrcode`
- Speaker notes: an HTML comment `<!-- ... -->` as the last thing on a slide. Use 2 to 4 short sentences. Use only facts from this file
- Dividers: theme `cover` layout with a dimmed satellite image and `class: divider`. Colored slides: `cover` layout with `background: "#hex"`
- In SVG, set font size with `style="font-size:Npx"`, not the attribute. The deck CSS overrides the attribute
- New files (components, style.css, global-bottom.vue) need a dev server restart to be picked up
- Photos from the internet: `public/images/g4e/earth.jpg` (NASA Blue Marble 2005, public domain, via Wikimedia Commons) and `crops.jpg` (NASA Landsat crops in Kansas, public domain, via Wikimedia Commons). Satellite theme images were copied from the other decks
- The Browser pane must be shown (`show_pane preview`) for screenshots. Wait about 3 s after a hash change, or the shot shows the previous slide

## Ideas from the user (brain dump, all in the deck)

- RAG database options: Qdrant, pgvector, Chroma
- Reranking vs plain vector search
- Embedding model: size, quantisation, vector length
- Evals: first the top results of vector search; then, in the agent flow, is the answer right and did the agent call the search tool at the right time
- Access points: custom app needs a custom agent; chat app (web, mobile) needs MCP; coding harness (Claude Code, Codex, Antigravity) needs a CLI with a plugin (skills + code)
- Metadata as filter fields next to similarity: AOI geometry covered, update time, time span covered, collection
- GraphRAG: what it is, when it helps
- MCP views: easy to render output, interactive
- Catalogue scope: aggregator (metadata only) or host data too (e.g. model output)? STAC for spatial data; CNG if we host; virtual layers if we only link
- Users and first datasets; integration into the GenAI4Earth ecosystem (these are questions for the audience at the end)
- Deployment on Kubernetes with a Helm chart
- Models behind a standard protocol (OpenAI-style API)
- Keep the index current with a GitHub Action cron job; CI is important
- Storage: where do the parts live?
- Chunking is crucial: per document, per section, fixed length (which length?), images in papers
- Datasets only, or scientific literature too?
- ESA EVE as an example of a corpus of documents about EO data
- Production: keep modular, keep LLMs out of tools, keep tools generic, put workflow logic in the agent or in a skill

## Evidence from the local repos (survey, 5 Nov 2026)

About 45 repos in `/home/tam/Documents/repos/` were checked. Use these for the detail pass.

**Best repos for this talk**

| Repo | What it shows |
| --- | --- |
| `dss-agentic-ai-services` | ECMWF CDS/ADS/EWDS assistant. MCP toolsets on `mcp-toolsets-runtime`. Hybrid search in Qdrant (Qwen3-Embedding-4B, 2560-d, plus BM25) over five sections, searched separately and merged. Points: cds_docs 29,370; eqc_qa 2,297; cards 171. States "no EQC content" from a catalogue index, because retrieval cannot prove absence. Evals ported from gnw-evals: 132-question gold CSV; one run 19/29 passed |
| `copernicus-rag-core` | Three-tier RAG dataset over CMEMS, CDS, ADS, EWDS. About 450k chunks in Qdrant. Tiers: discover (1,418 dataset cards, 1,054 variables), analyze (~58k doc chunks), method (392k chunks from 11.5k papers). Dense + BM25, rank fusion, reranker. Gemini 768-d vs Qwen3-Embedding-8B 4096-d (re-embed: 81 min on 4×A100). MCP server with 13 tools. 50/50 test queries pass. Author is Dmitrii Pantiukhin (probably AWI): credit him |
| `eie-agent` (NASA EIE) | STAC collection search. Embedding test on 28 queries: nomic top-1 0.90 (71% exact set), qwen3-embedding:0.6b top-1 1.00 (86%), embeddinggemma 1.00 (82%). Relevant and irrelevant collections are only about 0.06 apart in nomic, so no single threshold works. Deterministic evals, 11 cases. "Amazon" geocodes to the river. See `docs/evals-first-pass.md` |
| `project-zeno` (WRI GNW) | Dataset RAG (Gemini embeddings, top-3 to a small LLM). AOI search with Postgres full-text: 470 ms to 4.7 s with pg_trgm, about 8–13 ms after. "Replace RAG with the filesystem": ripgrep plus a static-embedding semantic grep. Subagents for AOI, dataset, data pull, insights |
| `gnw-gold-evals` | 124 versioned cases (104 active), 16 capability groups, content-hashed. Prod 94/109 pass (3 trials) |
| `stac-zap` | Natural language to a valid STAC query with decision models. 11 models compared. Best: 166/172 at about 300 ms, up to 250 options |
| `gazet`, `geodini` | Natural-language geocoding. Gazet: DuckDB over Overture and Natural Earth, LoRA fine-tuned Qwen3.5-0.8B. `AGENT_TOOL_HANDOFF.md`: return candidate ids and bounding boxes before full geometry |
| `mcp-toolsets-runtime` | Tools as MCP servers. Big results go to state; the model passes `@references`; receipts; tools can lock parameters. Source of the "data in state, not in context" idea |
| `digital-twin-assistant` | DestinE assistant for ECMWF. Docs RAG through self-hosted Onyx; GRIB parameter search with Mistral embeddings |
| `zoo/egu26` | EGU26 poster "Making Scientific Data Accessible with LLMs". Matches this talk |

**Patterns across the repos**

1. The path went from RAG, to agents with tools, to MCP toolsets on a shared runtime
2. Retrieval became hybrid (dense + BM25 + rank fusion + reranker), split per section or tier, and honest about gaps
3. Open embedding models (Qwen3) beat nomic in our test and run at ECMWF
4. Query tools pick from lists of valid options (STAC, SQL, CDS requests), so queries are valid by construction
5. Geocoding is its own tool that returns candidates with ids
6. Evals grew from LLM-judge sheets, to deterministic checks, to versioned regression sets. The harness design was reused at WRI, ECMWF and NASA

## How the user works

- Wants simple bullets first, detail later. Now prefers less text and more visuals (6 Nov 2026)
- Wants a sequential flow that makes sense
- Prefers plain, short sentences (ASD-STE100 style, see the global CLAUDE.md)
- The deck uses the shared DevSeed theme, `section: N` frontmatter for the footer breadcrumb, and `<LogoHorPos>` on content slides. The last slide has the EU funding note (flag at `public/images/eu-flag.svg`, grant agreement 101293266)

## Next steps

1. Read the GenAI4Earth site. Add the project context and the T7.2 wording
2. Add a few numbers from the survey to the slides (embedding test, index sizes, eval counts)
3. Add one or two diagrams: the flow from catalogue to index to tools to access points; the three access points
4. Add speaker notes
5. Check the timing. The webinar slot length is not known yet
