# RAW — "M5 setup — hybrid contract (2026-10-16)" + them5 hybrid audit

Source: claude.ai Project doc `claude/m5-hybrid-nix-darwin-them5.md` and the swarm outputs in `~/nix-setup-review/2026-10-16-swarm/out/` (`hybrid-audit.md`, `them5-README-hybrid.md`).

## Decision
`~/.config/nix-darwin` (theNewFolder/nix-darwin) owns the Mac. `~/them5` (theNewFolder/them5) is an overlay of scripts only, linked by home-manager `mkOutOfStoreSymlink` (edits live, no rebuild).

## Live (generation vcbds4y7…, switched 2026-10-16 15:26 +04)
- `modules/home/them5.nix`: links `bin/{ai-context-packer,ai-scratchpad,emacs-client-workspace,emacs-profile-switch,mic-toggle,repo-health-check,screen-ocr-ai,smart-resume}`, `ai/memory/hermes-{bridge,memory-digest,memory-sync}.py`, `emacs/agent-emacs/agent-emacs.sh` → `~/.local/bin`; `core/raycast/scripts` → `~/.config/raycast-scripts`; aliases `t5`, `t5j`; `capture` CLI (appends to `~/Documents/second-brain/inbox.org`).
- `modules/home/packages-them5.nix`: sbcl roswell rlwrap clojure babashka; cargo-watch/nextest/edit/binstall evcxr; llama-cpp; awscli2 minio-client rclone wrangler; k9s; croc. MLX stays in `uv` venvs (nixpkgs build CPU-only).
- `palettes/{tokyo-night,retro-phosphor}.nix`; `theme.nix` reads `vars.palette`; switching palette = one line in vars.nix + `nh darwin switch` + hand-edit 6 OmniWM TOML color tables.
- Casks added: karabiner-elements, visual-studio-code. atuin built with `doCheck = false`.
- them5 Justfile: `bootstrap/link-configs/apply-defaults/yabai-*/mcp-sync/colima-up/ollama-up` guarded (print nix equivalent, exit 1); new `hybrid-status`, `nix-switch`, `nix-update`.
- Branches: nix-darwin `hybrid-them5-overlay-2026-10-16`; them5 `hybrid-nix-darwin-2026-10-16`. Rollback: `sudo darwin-rebuild switch --rollback`; previous gen + `flake.lock.before` in `~/nix-setup-review/2026-10-16-swarm/`.

## Contract table (nix owns / them5 may own)
nix-darwin: packages+casks (`homebrew.nix`, `packages.nix`), macOS defaults (`system.nix`), WM = OmniWM (`omniwm.nix` + `omniwm-settings.toml`), Emacs (`emacs-plus-app` cask + real `~/.config/emacs`), shell (`shell.nix`), Ghostty (`terminals.nix` + `theme.nix`), MCP registry + `mcp-sync` (`mcp.nix`), secrets (`secrets/op.env.tpl`, `with-secrets`/`op-run`/`load-op-env`, opnix files), wallpaper.
them5: `bin/*` userland scripts (no yabai/brew services/defaults write/ln into ~/.config), `core/raycast/scripts/*`, `emacs/agent-emacs/`, `ai/memory/*.py`, `ai/wiki`, `ai/configs`, `.agents/`, Justfile userland recipes.

## Audit verdicts (selected)
DANGEROUS: `just bootstrap/link-configs/apply-defaults` (ln -sfn over nix-owned dirs, `brew bundle` yabai/skhd/aerospace/emacs-plus@30), `core/yabai/*`, `core/macos/defaults.sh` (writes `/etc/pam.d/sudo_local`, `killall Dock`), `core/raycast/scripts/wm-switch.sh`, `ai/mcp/mcp-sync.py` (replaces whole files, bakes `${GITHUB_TOKEN}` into disk via `expandvars`, writes bogus `~/.claude/mcp.json`), `secrets/bin/inject-secrets.sh` (plaintext `.env`).
DUPLICATE-prefer-nix: Brewfile, ghostty config (them5 Eldritch neon vs nix Tokyo Night; `font-thicken`, `display-p3` deliberately off in nix), `defaults.json`, `devops/shell/*`, `starship.toml`, `ai/mcp/mcp-config.json.tpl` (hard-codes `/Users/o/them5`, wrong user).
Unported but harmless to add: `MLX_GPU_MEMORY_LIMIT`, `DOCKER_HOST` OrbStack fallback, aliases `mlx-*`, `ol-run/ol-ps`, `cr/cb/ct/cw`, `tf`, functions `j`, `fe`.
nix `mcp-sync.py` (92 lines): registry `~/.config/mcp/servers.json` → merges per-server into `~/.claude.json`, `~/.codex/config.toml`, `~/.grok/config.toml`; atomic `os.replace` 0600; parse-before-write; `--check` drift; runs on every `nh darwin switch`.

## Gotchas
Run `darwin-rebuild switch` from Ghostty (App Management permission; Terminal.app fails `checkAppManagementPermission`). After a macOS update `sudo xcodebuild -license accept` or `brew bundle` aborts activation. New files need `git add -N` before `nix eval`. Validate with `nix eval --raw .#darwinConfigurations.oms-MacBook-Pro.system.drvPath` before building.

## Open follow-ups
Emacs.app not running (launchd agent loaded); rewrite `them5/ai/configs/CLAUDE.md` (still says yabai / `just bootstrap`); optional shell.nix ports; OmniWM settings attrset diff prepared, not applied.
