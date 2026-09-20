---
type: concept
title: Core Architecture
confidence: high
updated: 2026-09-20
---

# Core Architecture

Three layers: immutable `raw/` sources; an LLM-owned `wiki/` of interlinked markdown pages; and a schema file (CLAUDE.md/AGENTS.md) that governs agent behavior. "Obsidian is the IDE; the LLM is the programmer; the wiki is the codebase."

Three operations: **ingest** (one source touches 10–15 pages), **query** (index-first navigation, answers filed back as pages), **lint** (contradictions, orphans, stale claims).

Two special files: `index.md` (context-sized catalog, the agent's entry point) and `log.md` (append-only, grep-parseable ops record).

Scaling lever: qmd — local hybrid BM25/vector search with LLM reranking, single SQLite index, on-device.

See [[comparisons]] for when this beats RAG, and [[limitations]] for where it breaks.
