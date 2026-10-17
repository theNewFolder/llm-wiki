---
type: concept
title: Extensions and Standards
confidence: high
sources: [raw/sources.md, raw/llm-wiki-deep-research-report-2026-09-20.md]
updated: 2026-09-20
---

# Extensions and Standards

**LLM Wiki v2 (rohitg00):** memory lifecycle — confidence scoring, supersession chains, Ebbinghaus forgetting curves, four-tier consolidation (working → episodic → semantic → procedural). Practitioner verdict: governance features survive daily use; heavy machinery (vector DBs, event buses) gets ripped out. Adopt modularly.

**qmd:** local hybrid search for markdown (BM25 + vectors + rerank), single SQLite file, CLI + MCP server; recommended in the original gist for wikis beyond index scale.

**Google OKF v0.1 (June 2026):** vendor-neutral spec — markdown bundles + YAML frontmatter, one required field (`type`), reserved `index.md`/`log.md`, Apache 2.0. Turns the bespoke wiki practice into an interoperable format: any producer, any consumer.

Related: [[architecture]], [[ecosystem]]
