---
type: entity
title: oms-MacBook-Pro (M5 Max workstation)
tags: [macos, nix-darwin, machine, live-state]
confidence: high
sources: [raw/2026-10-17-live-machine-audit.md, raw/2026-10-16-m5-hybrid-contract.md, raw/2026-09-05-routing-memory-wiki-audit.md]
created: 2026-10-17
updated: 2026-10-17
---

# oms-MacBook-Pro — live state (2026-10-17)

Apple M5 Max, 128 GB unified memory, macOS 27.2 (26B5112a), user `om`, tz Asia/Dubai. Owned by the flake at `~/.config/nix-darwin` (flake-parts + nix-darwin + home-manager on Determinate Nix); `~/them5` is a script overlay — see [[hybrid-them5-overlay-contract]]. Supersedes the `gux`/`myosx` naming in [[nix-darwin-migration]] (never materialised; the real host is `oms-MacBook-Pro`).

| Layer | Live value |
|---|---|
| Nix | Determinate (`determinateNix.customSettings`, no `nix.*`), VM Linux builder on, GC automatic |
| WM | OmniWM (AeroSpace removed 2026-10-10; yabai never installed here) — [[tiling-wm-macos]] |
| Shell/terminal | zsh via home-manager, Ghostty, starship, Tokyo Night palette (`vars.palette`) |
| Emacs | `emacs-plus-app` cask + nix-managed `~/.config/emacs` |
| Secrets | 1Password `op.env.tpl` + opnix — [[secrets-1password-op-run]] |
| AI CLIs | `claude` 2.1.300 (`~/.local/bin`), `codex` 0.163.0 (`/opt/homebrew/bin`, **not on PATH**), `agy` 1.3.5 (Antigravity, replaces Gemini CLI) — [[mcp-config-sync-three-clis]] |
| MCP registry | `~/.config/mcp/servers.json` (11 servers) synced to Claude + Codex; **agy has 0 servers** (nix writes the wrong file) |
| Skills | `~/.claude/skills/myosx-*` + 80 `offensive-*`; `~/.agents/skills` only has the nix `nix-darwin` link — [[agent-skills-locations-2026]] |
| Knowledge | `~/Documents/ObsidianVault` (434 pages, git, 230 dirty) — [[memory-stores-on-the-mac]]; `~/code/llm-wiki` clean clone of this repo |
| Local inference | Ollama (brew, launchd env from `ai.nix`), oMLX, llama-cpp — [[local-inference-m5]] |

## Open defects (measured)
- `~/.gemini/settings.json` is dead: agy reads `~/.gemini/config/mcp_config.json` and `AGENTS.md`/`GEMINI.md`; fix belongs in `mcp-sync`.
- `codex` not on PATH (brew formula at `/opt/homebrew/bin`; shell PATH lacks it).
- `~/Documents/second-brain` does not exist; them5 `capture`, raycast capture, hermes scripts and `ai/wiki/*` point at it.
- Vault `AGENTS.md` cites stale `~/myox/modules/ai.nix` + `~/.config/mcp/mcp.json`.
- nix-darwin working tree has 29 uncommitted files on branch `hybrid-them5-overlay-2026-10-16`.
- them5 `ai/configs/CLAUDE.md`/`GEMINI.md` still describe yabai + `/Users/o/them5` + `just bootstrap`.

## Operating rules (from the live config)
`nh darwin switch` from Ghostty (App Management permission); `git add -N` new files before `nix eval`; `nix eval --raw .#darwinConfigurations.oms-MacBook-Pro.system.drvPath` before building; rollback `sudo darwin-rebuild switch --rollback`; after macOS update `sudo xcodebuild -license accept`.

Source pages: [[live-machine-audit-2026-10-17]], [[m5-hybrid-contract-2026-10-16]], [[routing-memory-wiki-audit-2026-09-05]].
