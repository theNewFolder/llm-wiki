---
type: comparison
title: LLM Wiki vs RAG vs Agent Memory
confidence: high
sources: [raw/sources.md, raw/llm-wiki-deep-research-report-2026-09-20.md]
updated: 2026-09-20
---

# Comparisons

- **LLM Wiki** — compile once, query the artifact; stateful; sweet spot ~50–200 sources / 50K–100K tokens of compiled wiki; zero infrastructure; deterministic file-level retrieval.
- **RAG** — retrieve chunks per query; stateless; scales to millions of documents; probabilistic retrieval; infrastructure-heavy.
- **Agent memory** (MemGPT/Letta, Mem0, Zep) — orthogonal: remembers the user, not the domain.

Consensus: hybrid architectures win in practice — wiki as curated high-confidence layer, RAG for the long tail. No published benchmark yet compares wiki-compiled vs RAG answers on accuracy.

Related: [[architecture]], [[limitations]]
