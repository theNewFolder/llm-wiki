# RAW — "Externally extensible flake systems" (Jonas Chevalier / zimbatm, talk slides)

Source: user-supplied slide transcript.

## Problem
Flake `systems` lists are internal to each flake (`eachSystem ["x86_64-linux" "aarch64-darwin"]` in plain flakes, `flake-utils.lib.eachSystem`, `flake-parts` `systems = [...]`). Consumers cannot override which systems a dependency enumerates → `nix flake show` evaluates every system; slow.

## nix-systems
- https://github.com/nix-systems/nix-systems and https://github.com/nix-systems/default
- Pattern: `inputs.systems.url = "github:nix-systems/default"` (or `path:./flake.systems.nix` with `flake = false`), then `eachSystem = nixpkgs.lib.genAttrs (import systems)`; flake-parts: `systems = import systems;`; flake-utils: `eachDefaultSystem` already reads it.
- Dependencies follow it: `inputs.treefmt.inputs.systems.follows = "systems"`.
- CLI override: `nix run github:numtide/treefmt --override-input systems github:nix-systems/x86_64-linux`.

## Measured speedup (`NIX_SHOW_STATS=1 nix flake show`)
Before: cpuTime 5.23 s, heapSize 520 MB, totalBytes 672 MB. After (`--override-input systems github:nix-systems/x86_64-linux`): cpuTime 1.29 s, heapSize 403 MB, totalBytes 62.6 MB.

## "Real solution" (does not exist yet)
Proposed first-class `systems = [...]` flake attribute with `inputs.<dep>.systems.follows = "self"` and `nix run ... --systems x86_64-linux,aarch64-darwin`.

## Recap
Flake systems are not extensible; flake authors: use nix-systems; Nix authors: add this feature.
