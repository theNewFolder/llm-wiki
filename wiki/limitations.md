---
type: concept
title: Limitations and Criticisms
confidence: high
sources: [raw/sources.md, raw/llm-wiki-deep-research-report-2026-09-20.md]
updated: 2026-09-20
---

# Limitations

1. **Learning-outsourcing objection** — bookkeeping is where understanding forms; keep humans in the ingest loop.
2. **Context degradation** — practical quality falls off around 200K–300K tokens despite larger nominal windows.
3. **Drift** — repeated rewriting can compound errors; mitigations: immutable raw layer, lint passes, git history.
4. **Complexity ceiling** — comfort zone ~50–200 sources; beyond that needs v2 extensions or RAG.
5. **Enterprise gaps** — no access control, no transactional writes, index overflow; consequences of personal-scale design assumptions, not bugs.

Related: [[comparisons]]
