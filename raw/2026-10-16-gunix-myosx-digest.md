# RAW — "gunix → myosx digest (macOS / Nix only)"

Source: claude.ai Project doc `claude/gunix-digest-and-myosx-skills.md`. Digest of github.com/theNewFolder/gunix @ 75e23e7 (2026-03-03). Everything Linux-only (CachyOS kernel, LVM, Wayland, Niri, EWM, dwl-guile, waybar, RADV, sysctl) dropped; Guix dropped (repo's own CLAUDE.md marks Guix→NixOS migration as current direction).

## Portable verbatim
`home.nix` zsh (history 50k/extended/share/ignoreAllDups, `bindkey -e`, aliases `nup nsr nsp nr e et`); git (zdiff3, `pull.rebase`, `push.default=current`, `diff.colorMoved=dimmed-zebra`); `.gitignore_global`; `home/emacs/init.el` + `early-init.el` (Vertico/Consult/Corfu/Embark, Magit, org-roam, Eglot, treesit-auto, envrc, GC tuning); firefox prefs (HTTPS-only, strict ETP, telemetry off, DDG + `@nix @gh`); `services.gpg-agent`; `nix.settings` substituters/keys, `auto-optimise-store`, weekly gc; `.claude/settings.json` allow-list (strip `guix:*`, `nixos-rebuild:*`, `systemctl:*`, `machinectl:*`; add `darwin-rebuild:*`, `launchctl:*`, `defaults:*`); `gemini-mcp/server.py` (Provider ABC: Gemini / Ollama fallback; tools chat/code/vision/embed/status) → `pkgs/ai-mcp/`.

## Portable with edits
Emacs: `emacs30-lucid` → `emacs30-macport`; remove `x-apply-session-resources` advice; fonts via `home.packages` + `fonts.packages`; `vterm` via `pkgs.emacsPackages.vterm`; gunix never wired `programs.emacs.extraPackages` (init.el `(when (require 'x nil t))` guards silently no-op) → use `emacsWithPackagesFromUsePackage` or explicit list.
MCP `.mcp.json`: `@anthropic/mcp-server-*` and `@anthropic/gemini-mcp-server` do not exist on npm; real names `@modelcontextprotocol/server-filesystem|-memory|-github|-fetch|-brave-search`, git server is Python `mcp-server-git` (uvx); paths `/gnu/store`, `/etc/nixos`, `/home/om` → `/Users/om`, `~/.config/nix-darwin`.
Ollama: `OLLAMA_MODEL=qwen2.5:3b` → M5 unified memory fits 7–14B Q4+; `services.ollama` is NixOS-only — darwin uses `launchd.user.agents` or brew.
Secrets: gunix has `SOPS_AGE_KEY_FILE`, agenix commented out, `initialPassword="o"`, `wheelNeedsPassword=false` → replace entirely; never port the password.

## Defects in gunix
1. `initialPassword = "o"` + passwordless sudo committed. 2. `.mcp.json` references 6 nonexistent npm packages. 3. `home.nix` lists packages not in nixpkgs / Guix names (`gcc-toolchain`, `nss-certs`, `linux-headers`, `libwl-clipboard`, `elogind`, `rust`+`rustup`+`cargo`). 4. `nix/ewm/default.nix` `cargoHash = null`, placeholder sha256. 5. `configuration.nix` imports `<home-manager/nixos>` inside a flake while also a flake input (impure). 6. `home.modules.*` options declared but never imported; `optimization.nix` sets NixOS options from an HM-looking module. 7. Two identical `nixosConfigurations`. 8. README EXWM vs repo Niri vs CLAUDE.md "EWM active".

## Optimizations → darwin equivalents
`documentation.enable = false` (nix-darwin has it); `lazy-trees` only under Determinate (`nix.custom.conf`); `follows` on every input; `programs.direnv.nix-direnv.enable`; `nix-index-database.darwinModules.nix-index` + comma; `programs.nh` (`nh darwin switch`); `auto-optimise-store` works but slow → weekly `nix store optimise` launchd; `nix.gc.automatic` + `interval`; remote builders: `nix.linux-builder` (vz) for Linux targets, NixOS laptop as `buildMachines`; Attic/Harmonia on the NixOS box, Mac adds substituter; CachyOS/BORE none (Apple scheduler); sysctl mostly locked on macOS; `mdutil -i off /nix`; reduce animations via `system.defaults.*`.
