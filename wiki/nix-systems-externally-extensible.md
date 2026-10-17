---
type: concept
title: nix-systems — externally extensible flake systems
tags: [nix, flakes, nix-systems, evaluation-speed]
confidence: high
sources: [raw/zimbatm-externally-extensible-flake-systems.md]
created: 2026-10-17
updated: 2026-10-17
---

# nix-systems (zimbatm)

Flake `systems` lists are internal: `flake-utils.lib.eachSystem [...]`, flake-parts `systems = [...]`, or `nixpkgs.lib.genAttrs [...]` all hard-code the set, so consumers evaluate every system a dependency enumerates.

Fix: a non-flake input holding the list.
```nix
inputs.systems.url = "github:nix-systems/default";   # or path:./flake.systems.nix, flake = false
inputs.treefmt.inputs.systems.follows = "systems";    # propagate to deps
outputs = { nixpkgs, systems, ... }: let eachSystem = nixpkgs.lib.genAttrs (import systems); in …
# flake-parts: systems = import systems;
```
Override from the CLI: `nix run github:numtide/treefmt --override-input systems github:nix-systems/x86_64-linux`.

Measured (`NIX_SHOW_STATS=1 nix flake show`): cpuTime 5.23 s → 1.29 s, GC totalBytes 672 MB → 62.6 MB.

The "real" fix (a first-class `systems` flake attribute with `inputs.<dep>.systems.follows = "self"`) does not exist. Recap: flake authors use nix-systems; Nix authors add the feature.

Applied in [[flake-layout-shared-nixos-darwin]]; the live repo pins `systems = [ vars.system ]` which gets the speed without the override hook.

Source pages: [[zimbatm-nix-systems-talk]].
