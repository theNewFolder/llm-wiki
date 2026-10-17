---
type: source
title: Source — myosx NixOS→nix-darwin migration research (2026-09-19)
tags: [source, nix-darwin, research]
confidence: high
sources: [raw/2026-09-19-myosx-nix-darwin-migration.md]
created: 2026-10-17
updated: 2026-10-17
---

# Source: myosx migration research (2026-09-19)

Deep-research document written for a planned host `myosx` / user `gux` before the live Mac existed. Mechanics proved correct; naming, WM choice and secrets backend were superseded by what shipped ([[nix-darwin-migration]]).

Claims filed: [[flake-layout-shared-nixos-darwin]], [[sops-nix-on-darwin]], [[mcp-config-sync-three-clis]], [[agent-instruction-files]], [[shared-agent-memory-basic-memory]], [[tiling-wm-macos]], [[macos-defaults-for-tiling-wm]].

Superseded claims (confidence lowered where filed): AeroSpace as daily driver (→ OmniWM); Gemini CLI as the third CLI (→ Antigravity `agy`); `pkgs.gemini-cli` (removal note 2026-08-07 confirmed by the Antigravity switch); sops-nix as primary secrets (→ 1Password on the Mac). Still-open: Basic Memory adoption (recommended, not installed).
