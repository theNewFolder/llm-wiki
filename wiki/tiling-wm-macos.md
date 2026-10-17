---
type: timeline
title: Tiling WMs on this Mac — yabai → AeroSpace → OmniWM
tags: [macos, tiling, yabai, aerospace, omniwm, skhd]
confidence: high
sources: [raw/2026-09-19-myosx-nix-darwin-migration.md, raw/2026-09-05-routing-memory-wiki-audit.md, raw/2026-10-16-m5-hybrid-contract.md, raw/2026-10-17-live-machine-audit.md]
created: 2026-10-17
updated: 2026-10-17
---

# Tiling WM timeline

| When | State | Evidence |
|---|---|---|
| ≤2026-09 | yabai + skhd + JankyBorders (them5 era, `core/yabai`, `yabairc` with `sudo yabai --load-sa`) | them5 configs; yabai double-free crash analysed 2026-09-05 (`window_destroy → CFRelease`, SLS destroyed-handler identity swap never claims; patch `e86c35f`, not activated) |
| 2026-09-19 | research recommends AeroSpace (i3-like, TOML, no SIP, survives macOS updates) with yabai as fallback | migration doc |
| 2026-10-01 | nix-darwin manages AeroSpace config v2, Raycast scripts, `Alt+R` | `~/nix-setup-review/README.md` |
| 2026-10-10 | AeroSpace removed; OmniWM implemented and riced (Tokyo Night, 4 pt border, 28 pt workspace bar) | `~/aerospace-removal-2026-10-10`, `~/omniwm-*`; `modules/home/omniwm.nix`, `jankyborders.enable = false` |
| 2026-10-16 | hybrid contract: OmniWM is the only WM; them5 yabai/skhd/aerospace/sketchybar dirs are CONFLICT-skip | hybrid audit |

What yabai needs that the others don't: scripting addition (partial SIP: `csrutil enable --without fs --without debug --without nvram` + Reduced Security on Apple Silicon), sudoers line pinned to the binary's sha256, re-`--load-sa` after every point release; without SA no space focus/create/destroy, no cross-space moves, no opacity/shadow/animation. AeroSpace: public Accessibility API only, emulated workspaces, no native Spaces. OmniWM: ships its own border and palette; Ctrl+Opt+Space palette needs symbolic hotkey 61 disabled (`system.nix` does it with PlistBuddy).

Hotkey layer decision: firmware layers on QMK/ZMK boards; macOS side minimal — skhd only with yabai, Karabiner only if a kernel-level remap is unavoidable (`karabiner-elements` cask is installed), kanata for the built-in keyboard if needed.

Defaults that every tiling WM wants: [[macos-defaults-for-tiling-wm]]. Host: [[machine-oms-macbook-pro]].
