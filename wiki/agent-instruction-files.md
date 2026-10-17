---
type: concept
title: AGENTS.md as the canonical instruction file (CLAUDE.md / GEMINI.md thin)
tags: [agents-md, claude-md, gemini-md, instructions, codex, antigravity]
confidence: high
sources: [raw/2026-10-17-live-machine-audit.md, raw/2026-09-19-myosx-nix-darwin-migration.md, raw/ai-system-instructions-sysinstr.md]
created: 2026-10-17
updated: 2026-10-17
---

# Instruction files: one canonical AGENTS.md

AGENTS.md (OpenAI, Aug 2025; donated to the Linux Foundation AAIF 2025-12-09 with MCP and goose; 60k+ projects) is the only file all three CLIs read natively:

| CLI | Reads | Global location | Notes |
|---|---|---|---|
| Codex | `AGENTS.md` (repo root, per-dir) | `~/.codex/AGENTS.md` | `project_doc_max_bytes` 32 KiB default; live file = "Working approach" doc |
| Claude Code | `CLAUDE.md`, or `AGENTS.md` in place of it | `~/.claude/CLAUDE.md` (absent live) | default `claude-md-or-agents-md`; `claude-md-and-agents-md` loads both, dedupes an imported AGENTS.md; `@AGENTS.md` import syntax; `/doctor prompt-audit` |
| Antigravity `agy` | `AGENTS.md`, `GEMINI.md` (strings in binary) | `~/.gemini/GEMINI.md` (absent live) | Gemini CLI equivalent: `context.fileName: ["AGENTS.md","GEMINI.md"]`; `/memory show\|reload` |

Pattern: canonical `AGENTS.md`; `CLAUDE.md` = `@AGENTS.md` + Claude-only lines (auto-memory is per-repo scratch; paste `nvd diff`); `GEMINI.md` = `@AGENTS.md` + Gemini-only lines. Never copy — import, so files cannot drift.

Content that belongs in AGENTS.md (facts held every session): operator profile and response format ([[sysinstr-response-format]]), repo layout and commands (`nix flake check`, `nh darwin switch`, `mcp-sync --check`, `with-secrets`), hard rules (declarative only; 1Password only; build before switch; Determinate ⇒ no `nix.*`; edit MCP only in `mcp.nix`; darwin has no systemd), memory policy ([[shared-agent-memory-basic-memory]]). Procedures go to skills, not here ([[agent-skills-locations-2026]]).

Existing drafts: `~/.claude/skills/myosx-ai-clis/reference/{AGENTS,CLAUDE,GEMINI}.md` (not installed); stale copies in `them5/ai/configs/` (yabai era) and the vault's `AGENTS.md` (references `~/myox`). The Documents workspace has its own `AGENTS.md`+`CLAUDE.md` pair, and the vault its own — three layers: global → workspace → repo.
