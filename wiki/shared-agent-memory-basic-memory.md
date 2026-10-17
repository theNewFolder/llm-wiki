---
type: concept
title: Shared agent memory — Basic Memory over native stores
tags: [memory, basic-memory, mcp, markdown, obsidian]
confidence: high
sources: [raw/2026-09-19-myosx-nix-darwin-migration.md, raw/2026-10-17-live-machine-audit.md, raw/2026-09-05-routing-memory-wiki-audit.md]
created: 2026-10-17
updated: 2026-10-17
---

# Shared memory across Claude Code, Codex, Antigravity

Native memories are machine-local, vendor-specific, and none accepts a shared directory cleanly:
- Claude Code auto-memory (v2.1.59+): `~/.claude/projects/<project>/memory/`, `MEMORY.md` capped at 200 lines / 25 KB loaded at start; `autoMemoryDirectory` setting relocates it; disable with `CLAUDE_CODE_DISABLE_AUTO_MEMORY=1`.
- Codex: `~/.codex/memories_1.sqlite` (current; the old markdown store was lost 2026-08-24 with no backup — see [[memory-stores-on-the-mac]]); generated state, not hand-edited.
- Gemini CLI `/memory add` → `~/.gemini/GEMINI.md`; agy equivalent absent.

Comparison of shared servers (from the migration research):

| Server | Format | One stdio server for all | git/Syncthing-syncable |
|---|---|---|---|
| `@modelcontextprotocol/server-memory` | one JSONL graph | yes | yes, but atomic rename resets perms; **currently in the registry, unused** |
| Mem0 / OpenMemory | vector DB + Docker + key | weak | no |
| Basic Memory (basicmachines) | Markdown + SQLite index, wikilinks, Obsidian-compatible | yes | yes |

Recommendation (unchanged since September, still not implemented): Basic Memory. Install `uv tool install basic-memory --prerelease=allow` (FastMCP 4 pre-release dep) or `brew install basicmachines-co/basic-memory/basic-memory`; MCP entry `command = "basic-memory"`, `args = ["mcp"]` (or `uvx --prerelease=allow basic-memory mcp`); default notes dir `~/basic-memory`, projects via `basic-memory project add <name> <path>`; tools `write_note read_note edit_note search_notes build_context recent_activity`; Codex may set `default_tools_approval_mode = "approve"` per server; Claude Code plugin `bm install claude-code` adds session briefings; shared `memory-*` skills via `npx skills add basicmachines-co/basic-memory/skills`.

Where the notes should live: a project pointed at the vault ([[memory-stores-on-the-mac]], `vars.obsidianVault`) keeps one knowledge graph; the Pi/Hermes/Claude digest proposers already promote into `AI-Knowledge/Memories.md` with human approval — Basic Memory becomes the read/write MCP front door, digests stay the promotion path. Static rules stay in [[agent-instruction-files]]. Never sync native stores through iCloud (placeholder downloads, open-file conflicts); git or Syncthing only.
