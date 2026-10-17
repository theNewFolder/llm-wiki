---
type: concept
title: NixOS → nix-darwin migration (myosx plan vs what shipped)
tags: [nix-darwin, macos, migration, determinate]
confidence: high
sources: [raw/2026-09-19-myosx-nix-darwin-migration.md, raw/2026-10-16-gunix-myosx-digest.md, raw/2026-10-17-live-machine-audit.md]
created: 2026-10-17
updated: 2026-10-17
---

# NixOS → nix-darwin migration

The September plan ("myosx", user `gux`) and the October reality ([[machine-oms-macbook-pro]], user `om`) agree on the mechanics and differ on the shell around them.

| Topic | Plan (2026-09-19) | Shipped (2026-10) |
|---|---|---|
| Flake shape | one flake, `hosts/` + `modules/{nixos,darwin,home}`, NixOS host beside darwin — [[flake-layout-shared-nixos-darwin]] | darwin-only flake-parts repo; NixOS laptop stays separate (no `hosts/`) |
| Nix | Determinate ⇒ `nix.enable = false`, `/etc/nix/nix.custom.conf` | Determinate module: `determinateNix.customSettings`, GC automatic, VM Linux builder |
| `system.primaryUser` | required (nix-darwin #1457, 2025-05-19), temporary shim | set to `om`; `stateVersion = 7` |
| Secrets | sops-nix + ssh-to-age — [[sops-nix-on-darwin]] | 1Password `op run` + opnix — [[secrets-1password-op-run]]; sops only for cross-host files |
| WM | AeroSpace recommended, yabai fallback | OmniWM — [[tiling-wm-macos]] |
| MCP | one attrset → three configs — [[mcp-config-sync-three-clis]] | same idea, `mcp.nix` + `mcp-sync.py`; Gemini target is wrong (agy) |
| Memory | Basic Memory on a synced dir — [[shared-agent-memory-basic-memory]] | reference JSONL `server-memory` still in registry (inert) |
| Defaults | [[macos-defaults-for-tiling-wm]] | `system.nix` covers it; `universalaccess` skipped (TCC) |

## Bootstrap (unchanged)
`xcode-select --install` → install Nix → clone to `~/.config/nix-darwin` → `sudo nix run nix-darwin/master#darwin-rebuild -- switch --flake .#<host>` → later `nh darwin switch` (from Ghostty). Bump `system.stateVersion` only after `darwin-rebuild changelog`.

## What gunix contributed
zsh/git/firefox prefs, Emacs init (needs `emacs30-macport` + explicit `extraPackages`), the Gemini/Ollama MCP server (→ `ai-mcp`), nix settings. Dropped: CachyOS, LVM/Btrfs, Wayland, Guix, Stow. Never re-import: `initialPassword = "o"`, nonexistent `@anthropic/mcp-server-*` npm names, `<home-manager/nixos>` inside a flake. Full list in [[thenewfolder-profile]].

## Darwin-specific traps
No `boot.*`/`hardware.*`/`systemd`/`services.ollama` — use `launchd.*` or brew; `~/.codex` and `~/.gemini` are not XDG (`home.file`); `com.apple.spaces spans-displays` polarity is inverted vs the UI; `CustomUserPreferences` for `com.apple.dock`/`spaces` may need `killall Dock`; App Management permission is per-terminal.

Source pages: [[myosx-migration-research-2026-09-19]], [[gunix-myosx-digest-2026-10-16]].
