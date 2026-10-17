---
type: concept
title: Hybrid contract — nix-darwin owns, them5 overlays
tags: [nix-darwin, them5, home-manager, contract]
confidence: high
sources: [raw/2026-10-16-m5-hybrid-contract.md, raw/2026-10-17-live-machine-audit.md]
created: 2026-10-17
updated: 2026-10-17
---

# Hybrid contract (decision 2026-10-16)

`~/.config/nix-darwin` owns the system; `~/them5` is an overlay of userland scripts linked with `config.lib.file.mkOutOfStoreSymlink` from `modules/home/them5.nix`, so edits are live without a rebuild. Applies to [[machine-oms-macbook-pro]].

| nix-darwin owns (never link/run from them5) | them5 may own |
|---|---|
| packages + casks (`homebrew.nix`, `packages.nix`; not `Brewfile`) | `bin/*` scripts — no `yabai`, `brew services`, `defaults write`, no writes under `~/.config` or `~/.local/bin` |
| macOS defaults (`system.nix`; not `core/macos/defaults.sh`) | `core/raycast/scripts/*` (dir-linked to `~/.config/raycast-scripts`) |
| WM: OmniWM (`omniwm.nix` + TOML; not yabai/skhd/borders/aerospace/sketchybar) | `emacs/agent-emacs/` (own socket `/tmp/emacs0/server`) |
| Emacs: `emacs-plus-app` cask + real `~/.config/emacs` | `ai/memory/*.py` (hermes-bridge / -digest / -sync) |
| shell, starship, fzf/eza/bat/atuin (`shell.nix`) | `ai/wiki`, `ai/configs`, `.agents/` (docs, prompts, skills) |
| Ghostty (`terminals.nix` + `theme.nix`) | Justfile userland recipes (`emacs`, `memory-*`, `mlx-*`, `r2-*`, `capture`, `hybrid-status`) |
| MCP registry + `mcp-sync` (`mcp.nix`; not `ai/mcp/*`) | |
| secrets (`op.env.tpl`, `with-secrets`/`op-run`, opnix; not `secrets/templates/.env.tpl`) | |

Guarded Justfile recipes print the nix equivalent and exit 1: `bootstrap`, `link-configs`, `apply-defaults`, `yabai-*`, `mcp-sync`, `colima-up`, `ollama-up`.

## Adding a linked script
1. `them5/bin/<name>` (bash, `set -euo pipefail`, executable). 2. Add to `binScripts` in `them5.nix` (name must not shadow `~/.local/bin`). 3. `git add -A && nh darwin switch` (flakes see tracked files only). 4. Raycast scripts need no nix change — the whole dir is linked; call targets by absolute `$HOME/them5/bin/<name>`.

## Why the audit mattered
them5's `mcp-sync.py` replaced whole config files and baked `${GITHUB_TOKEN}` into disk; `defaults.sh` rewrote `/etc/pam.d/sudo_local`; `link-configs` would clobber the nix `~/.config/emacs`. The nix `mcp-sync.py` merges per server, atomic `os.replace` 0600, `--check` for drift — see [[mcp-config-sync-three-clis]], [[secrets-1password-op-run]].

Stale after the contract: `them5/ai/configs/CLAUDE.md|GEMINI.md`, the `~/Documents/second-brain` path, Brewfile WM entries.
