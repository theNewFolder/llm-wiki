---
type: source
title: Source — live machine audit 2026-10-17
tags: [source, audit, macos]
confidence: high
sources: [raw/2026-10-17-live-machine-audit.md]
created: 2026-10-17
updated: 2026-10-17
---

# Source: live machine audit (2026-10-17)

Read-only inspection of oms-MacBook-Pro via device shell and Desktop Commander, plus doc verification for Codex, Gemini CLI, Claude Code and Basic Memory.

Claims filed: [[machine-oms-macbook-pro]] (platform, config, CLIs, defects), [[mcp-config-sync-three-clis]] (agy reads `~/.gemini/config/mcp_config.json`, 0 servers), [[agent-skills-locations-2026]] (skill dirs per host), [[agent-instruction-files]] (AGENTS.md readers), [[shared-agent-memory-basic-memory]] (install/config facts), [[memory-stores-on-the-mac]] (vault at `~/Documents/ObsidianVault`, `second-brain` missing).

Contradicts: [[nix-darwin-migration]]'s assumption that Gemini CLI + `~/.gemini/settings.json` is the Gemini path (Antigravity `agy` replaced it); them5's `~/Documents/second-brain` paths; vault AGENTS.md's `~/myox` references.

Open questions: whether agy honours `GEMINI.md` at `~/.gemini/GEMINI.md` or only workspace-level; exact schema of `mcp_config.json` (derive by running `agy mcp add` once).
