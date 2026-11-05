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

- **Project site not read.** The fetch of the GenAI4Earth site was stopped by the user. The deck has no partner names, WP7 wording or T7.2 text. Read the site before adding them
- The user's claude.ai chat history is not visible to Claude Code. A search of past Claude Code sessions found nothing on GenAI4Earth
- Last slide links to `github.com/developmentseed/mcp-toolsets-runtime`. Replace it if a better link exists
- Some slides only name a topic, because the survey had no evidence: quantisation, pgvector, Chroma, GraphRAG (not used yet by the user), ESA EVE, GitHub Action cron build, Helm chart as stated by the user. Check them before adding facts

## The flow (21 slides, seven parts)

Each slide has a small tag with its part number.

1. **Scope:** the question; what we index (datasets, docs, literature; ESA EVE as example); flow of the talk
2. **Data foundation:** catalogue (STAC, cloud-native assets, virtual Zarr / Kerchunk); keep the index up to date (GitHub Action on a cron, CI is core); storage and deployment (Kubernetes, Helm)
3. **Search core:** chunking; embedding model; vector database; metadata next to vectors; reranking vs plain vector search; GraphRAG
4. **Agent layer:** from RAG to tools (four steps); tools that make valid queries and geocode; three ways in; MCP views
5. **Quality:** evals at two levels
6. **Production:** stay modular; no LLMs inside tools; generic tools; plug-and-play models
7. **Discussion:** users, first datasets, scope, GenAI4Earth integration, catalogues

## Ideas from the user (brain dump, all in the deck)

- RAG database options: Qdrant, pgvector, Chroma
- Reranking vs plain vector search
- Embedding model: size, quantisation, vector length
- Evals: first the top results of vector search; then, in the agent flow, is the answer right and did the agent call the search tool at the right time
- Access points: custom app needs a custom agent; chat app (web, mobile) needs MCP; coding harness (Claude Code, Codex, Antigravity) needs a CLI with a plugin (skills + code)
- Metadata as filter fields next to similarity: AOI geometry covered, update time, time span covered, collection
- GraphRAG: what it is, when it helps
- MCP views: easy to render output, interactive
- Underlying catalogue: STAC? cloud-native assets? virtual Zarr / Kerchunk?
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

- Wants simple bullets first, detail later
- Wants a sequential flow that makes sense
- Prefers plain, short sentences (ASD-STE100 style, see the global CLAUDE.md)
- The deck uses the shared DevSeed theme, the section tag style `<p class="text-primary font-mono">N · Part</p>`, and `<LogoHorPos>` on each content slide

## Next steps

1. Read the GenAI4Earth site. Add the project context and the T7.2 wording
2. Add a few numbers from the survey to the slides (embedding test, index sizes, eval counts)
3. Add one or two diagrams: the flow from catalogue to index to tools to access points; the three access points
4. Add speaker notes
5. Check the timing. The webinar slot length is not known yet
