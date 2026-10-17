---
type: comparison
title: Where agent skills live — Claude Code vs Codex vs Gemini/Antigravity (2026)
tags: [skills, skill-md, claude-code, codex, gemini, antigravity]
confidence: high
sources: [raw/2026-10-17-live-machine-audit.md]
created: 2026-10-17
updated: 2026-10-17
---

# Agent Skills locations (verified against docs, 2026-10)

All three implement the Agent Skills standard: a folder with `SKILL.md` (YAML `name` + `description` first, then Markdown), optional `scripts/`, `references/`, `assets/`. Only name+description load at session start.

| Host | User scope | Project scope | Invocation | Extras |
|---|---|---|---|---|
| Claude Code | `~/.claude/skills/<n>/SKILL.md` | `.claude/skills/` (walks up to repo root; nested dirs load on first file read) | `/<n>`, auto by description | rich frontmatter (`when_to_use`, `disable-model-invocation`, `allowed-tools`, `model`, `context`, `agent`, `paths`, `hooks`); `/reload-skills`; `/skills`; `.claude/commands/*.md` legacy |
| Codex CLI | `~/.agents/skills` (legacy `~/.codex/skills`) | `.agents/skills` from CWD up to repo root; admin `/etc/codex/skills` | `$<n>`, `/skills`, implicit | only `name`+`description` in frontmatter; UI metadata in `agents/openai.yaml`; `skills.config` array-of-tables (`path`, `enabled`) in config.toml; follows symlinks; duplicate names are not merged |
| Gemini CLI | `~/.gemini/skills/` or `~/.agents/skills/` alias | `.gemini/skills/` or `.agents/skills/` (workspace must be trusted) | `activate_skill` with consent prompt | `.agents/skills` wins within a tier; `/skills list\|link\|enable\|disable\|reload`; `gemini skills install <git> --consent` |
| Antigravity `agy` | `~/.agents/skills/` (binary also scans `_agents/skills/`) | `.agents/skills/` | slash/`$` expansion | builtin skills bundled; plugins carry `skills.json` |

Consequence: `~/.agents/skills/<n>` serves Codex and Antigravity/Gemini; Claude Code needs `~/.claude/skills/<n>` → one source dir plus per-skill symlinks (Codex follows symlinks; home-manager `home.file` can create both).

Live on [[machine-oms-macbook-pro]]: `~/.claude/skills` holds `myosx-*` (5 hand-written host skills) + `nix-darwin` (from `olafkfreund/nix-skills`, also linked into `~/.agents/skills`) + ~80 `offensive-*`; `~/.agents/skills` has only that link; `~/.codex/skills` only `.system/`. Context cost note: Codex budgets the skills list to 2% of context and truncates descriptions when many are installed — 80 offensive skills are mostly dead weight for this host.

Decision rule: facts that apply every session → [[agent-instruction-files]]; procedures (deploy, rekey, ingest) → skills; capabilities/data → MCP ([[mcp-config-sync-three-clis]]).
