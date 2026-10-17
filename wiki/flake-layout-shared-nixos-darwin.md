---
type: concept
title: Flake layout for a shared NixOS + nix-darwin repo
tags: [nix, flakes, nix-darwin, home-manager, layout]
confidence: high
sources: [raw/2026-09-19-myosx-nix-darwin-migration.md, raw/2026-10-17-github-profile-analysis-thenewfolder.md]
created: 2026-10-17
updated: 2026-10-17
---

# Flake layout: NixOS + nix-darwin in one repo

```
flake.nix  flake.lock  lib/default.nix
hosts/<nixos-host>/{default,disko,hardware-configuration}.nix
hosts/<mac>/default.nix
modules/{nixos,darwin,home}/      # home/ shared, gated on pkgs.stdenv.isDarwin
secrets/.sops.yaml
```

- `darwinConfigurations.<mac> = nix-darwin.lib.darwinSystem { system = "aarch64-darwin"; specialArgs = { inherit inputs; }; modules = [ ./hosts/<mac> sops-nix.darwinModules.sops home-manager.darwinModules.home-manager { home-manager.useGlobalPkgs = true; useUserPackages = true; users.<u> = import ./modules/home; } ]; }`.
- `useGlobalPkgs` makes home reuse system `pkgs` (overlays apply); `useUserPackages` installs into the system profile instead of `~/.nix-profile`.
- Branch inside `modules/home` only where platforms genuinely differ: WM (dwm vs none), Emacs toolkit (`emacs30-lucid` vs `emacs30-macport`), launchd vs systemd.
- Host file must set `system.primaryUser`, `users.users.<u>.home`, `system.stateVersion`, `nixpkgs.hostPlatform`.
- Make `systems` externally overridable with [[nix-systems-externally-extensible]]; the live repo already restricts `systems = [ vars.system ]`.
- Add `checks`, `formatter` (nixfmt), `devShells`, CI running `nix flake check` — recommended by [[thenewfolder-profile]]'s gap analysis; the live repo has `formatter` only.

Status: the live Mac repo ([[machine-oms-macbook-pro]]) is darwin-only; the shared layout is the target if the NixOS laptop is folded in. See [[nix-darwin-migration]].
