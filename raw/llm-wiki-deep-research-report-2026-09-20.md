# RAW — "The LLM Wiki Pattern: A Deep Research Report" (compiled 2026-09-20)

Source: user-supplied report. This is the same report that seeded this repository: its full text is `README.md` and its 28 web sources are items 1–28 in `raw/sources.md`. Recorded here only so the ingest trail is explicit; no new claims beyond README.md.

Pointers for this wiki's own practice (extracted, already reflected in [[architecture]] etc.):
- Three layers (raw / wiki / schema), three operations (ingest / query / lint), two files (index.md / log.md).
- Governance that earns its keep: confidence tags, supersession, reversible automation; skip vector DBs below ~100K tokens.
- Crossover to RAG ≈ 50K–100K tokens of compiled wiki; comfort zone 50–200 sources.
- qmd for >~500 pages; OKF (Google, 2026-06-12) as interoperable bundle format.
