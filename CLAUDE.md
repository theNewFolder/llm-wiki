# Schema: llm-wiki (a wiki about the LLM Wiki pattern)

This repo follows Karpathy's LLM Wiki pattern. Any agent working here should obey:

## Layers
- `raw/` — immutable sources. Never edit. `raw/sources.md` lists every source consulted.
- `wiki/` — LLM-owned compiled pages. Currently the compiled knowledge lives in `README.md`; split pages into `wiki/` as the corpus grows.
- `CLAUDE.md` — this schema. Co-evolve it with the user.

## Operations
1. **Ingest** — new source → `raw/sources.md` entry; update affected pages; update `wiki/index.md`; append to `log.md`. One source at a time; discuss takeaways before writing.
2. **Query** — read `wiki/index.md` first, then only relevant pages. Cite pages/sources. File good answers back as new pages.
3. **Lint** — on request or every ~10 ingests: find contradictions, stale claims, orphan pages, missing concept pages. Log the pass.

## Conventions
- One concept per page; YAML frontmatter with `type`, `title`, `tags`, `confidence`, `updated`.
- `[[wikilinks]]` for cross-references; flat structure, no deep hierarchies.
- Every claim carries an inline source link. Never fabricate sources.
- All automation must be reversible: git history + `log.md` diff entries.
