---
type: entity
title: Memory and knowledge stores on oms-MacBook-Pro
tags: [memory, obsidian, vault, hermes, pi, codex, audit]
confidence: high
sources: [raw/2026-09-05-routing-memory-wiki-audit.md, raw/2026-10-17-live-machine-audit.md, raw/2026-10-16-m5-hybrid-contract.md]
created: 2026-10-17
updated: 2026-10-17
---

# Memory stores on the Mac (five stores, three proposers)

| Store | Path | State |
|---|---|---|
| ObsidianVault (canonical wiki) | `~/Documents/ObsidianVault` — git, HEAD `9cb5ef8`, 230 dirty; root-level `sources/ concepts/ entities/ synthesis/ Projects/ skills/`; 434 pages active, 645 wider, `training/` archive 698 md | alive; its own AGENTS.md/CLAUDE.md/GEMINI.md, `bin/vault-health-check.sh`, lint 92 → 64 (2026-09-05) |
| Drive checkout of the vault | `$OBSIDIAN_VAULT` (Google Drive) | 15 behind, 0 ahead, one unique page → retire, stop pulling |
| Pi / Hermes memory | `~/.pi/agent/pi-hermes-memory/` (22 entries mirrored hourly by `hermes-memory-sync`; `~/.hermes/memories/` was the wrong, empty path) | alive; `.recovery-*` files are write-ahead policy, do not prune |
| Claude auto-memory | `~/.claude/projects/*/memory/` | alive; `claude-memory-digest` weekly (Sun 18:30), read-only, 29 promotable, 0 dupes at first run |
| Codex memory | `~/.codex/memories_1.sqlite` | old store lost (recreated 2026-08-24, no snapshots, no Time Machine); two vault pages hold survivors |
| MCP `memory` JSONL | registry entry `@modelcontextprotocol/server-memory` | inert; flagged for removal |

Proposers promote candidates into `AI-Knowledge/Memories.md` with human approval; MEMORY.md at 93% and failures.md at 98% of cap need `~/.pi/agent/hermes-memory-config.json` (limit 8000, timeout 300 s).

Dead paths: `~/Documents/second-brain` (them5 `capture`, raycast capture, hermes scripts, `ai/wiki/schema.md`) does not exist; `~/Documents/Obsidian Vault` (with space) is an empty vault from 2026-09-22; vault AGENTS.md still cites `~/myox/modules/ai.nix` and `~/.config/mcp/mcp.json`. `~/code/llm-wiki` is a clean clone of this repo.

Plan: expose the vault (and this wiki) through [[shared-agent-memory-basic-memory]]; keep digests as the promotion path; fix the stale paths in [[hybrid-them5-overlay-contract]]. Host context: [[machine-oms-macbook-pro]].
