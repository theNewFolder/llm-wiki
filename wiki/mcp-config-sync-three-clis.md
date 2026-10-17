---
type: concept
title: One MCP registry → Claude Code, Codex, Antigravity/Gemini
tags: [mcp, claude-code, codex, gemini, antigravity, nix]
confidence: high
sources: [raw/2026-10-17-live-machine-audit.md, raw/2026-09-19-myosx-nix-darwin-migration.md, raw/2026-10-16-m5-hybrid-contract.md]
created: 2026-10-17
updated: 2026-10-17
---

# MCP config sync across CLIs

Single source: `servers` attrset in `modules/home/mcp.nix` → `~/.config/mcp/servers.json` → `mcp-sync` (Python, runs on every activation, `--check` reports drift) merges per server into each CLI's mutable file. Emacs `mcp.el` reads `servers.json` directly.

| CLI | Config file | Key | Transport quirks | Live status 2026-10-17 |
|---|---|---|---|---|
| Claude Code | `~/.claude.json` (user), `.mcp.json` (project) | `mcpServers` | stdio / SSE / streamable HTTP (`url`) | 12 servers incl. claude.ai connectors; merge, never replace |
| Codex CLI | `~/.codex/config.toml` | `mcp_servers` (underscore) | stdio or `url` + `bearer_token_env_var`; `startup_timeout_sec` | 11 synced; `codex` binary not on PATH |
| Gemini CLI | `~/.gemini/settings.json` | `mcpServers` | `command` / `url` (SSE) / `httpUrl` (streamable) | Gemini CLI not installed |
| Antigravity `agy` | `~/.gemini/config/mcp_config.json` (`agy mcp add <name> <cmd> [args]`, `--env`, `--header`, `-t http`) | — | stdio or http; `_meta antigravity/alwaysLoad` | **0 servers** — nix writes `settings.json`, which agy ignores |
| Grok | `~/.grok/config.toml` | `mcp_servers` | | synced if present |

Registry (live): filesystem, git, fetch (nixpkgs `mcp-server-fetch`), memory (reference JSONL — inert, slated for removal), sequential-thinking, nixos (`mcp-nixos`), github (op-wrapped `github-mcp-server`), context7, perplexity (op-wrapped), supasidebar, exa (http). Secrets via [[secrets-1password-op-run]].

Design rules: prefer nixpkgs binaries over `npx -y`/`uvx` (pinned by flake.lock, no cold start); `~/.codex`/`~/.gemini` are not XDG → `home.file`; Codex key names drop `type`; the `/import codex|gemini` command in Claude Code can pull the other way.

Fix required: add an `agy` target to `mcp-sync` (write `mcpServers` into `~/.gemini/config/mcp_config.json` or drive `agy mcp add`), delete the dead `~/.gemini/settings.json` generation. Instructions layer: [[agent-instruction-files]]; memory layer: [[shared-agent-memory-basic-memory]]; skills: [[agent-skills-locations-2026]].
