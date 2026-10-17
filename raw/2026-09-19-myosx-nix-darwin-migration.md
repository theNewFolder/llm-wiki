# RAW — myosx: NixOS→nix-darwin migration (research doc, 2026-09-19)

Source: user-supplied deep-research document "myosx — NixOS→nix-darwin migration: flake, secrets, MCP/memory sync, yabai, macOS defaults". Written for user `gux` (pre-dates the live Mac, where the user is `om`). Digest of claims, verbatim where load-bearing.

## TL;DR claims
- One flake: `hosts/`, `modules/{nixos,darwin,home}`, `lib/`; `darwinConfigurations.myosx` beside `nixosConfigurations`; home-manager via `home-manager.darwinModules.home-manager` with `useGlobalPkgs`/`useUserPackages`; shared home modules gated on `pkgs.stdenv.isDarwin`.
- `system.primaryUser` required since nix-darwin issue #1457 (2025-05-19, "system activation must now be run as root"); explicitly a temporary shim slated for removal.
- Determinate installer ⇒ `nix.enable = false`; nix.conf tweaks go to `/etc/nix/nix.custom.conf`; `nix.linux-builder` unavailable under Determinate (use Determinate's native Linux builder, preview since Determinate Nix 3.8.4).
- Bootstrap: `xcode-select --install` → install Nix → clone to `~/.config/nix-darwin` → `sudo nix run nix-darwin/master#darwin-rebuild -- switch --flake .#myosx` → thereafter `sudo darwin-rebuild switch --flake .#myosx`.
- `system.stateVersion = 6` (bump only after `darwin-rebuild changelog`).

## Secrets
- `sops-nix.darwinModules.sops`; age key at `$HOME/Library/Application Support/sops/age/keys.txt` (or set `sops.age.keyFile`); derive from ed25519 SSH key with `ssh-to-age -private-key -i ~/.ssh/id_ed25519 -o <keys.txt>`; `age-keygen -y` prints recipient; add to `.sops.yaml` key groups; re-key loop `for f in $(grep -rl '^sops:' secrets/); do sops updatekeys -y "$f"; done`.
- `neededForUsers` is NixOS-only; on darwin use plain `sops.secrets.<name>` with `owner`/`path`.
- home-manager module `sops-nix.homeManagerModules.sops`; `%r` expands to `$(getconf DARWIN_USER_TEMP_DIR)`.
- `sops.templates` + `config.sops.placeholder.<name>` for launchd env files.
- 1Password: `pkgs.writeShellApplication` wrappers running `op run --no-masking -- claude "$@"` or `op read 'op://Private/OpenAI/api key'`; `opnix` (brizzbuzz/opnix) for declarative `services.onepassword-secrets`.
- macOS Keychain (`security add-generic-password`) works but is imperative; prefer sops/op.
- ssh-to-age does not support `id_ed25519_sk` keys.

## MCP + memory (as of 2026)
- Claude Code: `~/.claude.json` user scope, project `.mcp.json`; key `mcpServers`; stdio/SSE/streamable HTTP; `claude mcp add`.
- Codex CLI: `~/.codex/config.toml`, `[mcp_servers.<name>]` (underscore); `startup_timeout_sec`, `tool_timeout_sec`, `bearer_token_env_var`; `codex mcp add`.
- Gemini CLI: `~/.gemini/settings.json`, key `mcpServers`; exactly one of `command` / `url` (SSE) / `httpUrl` (streamable HTTP); `context.fileName` to read AGENTS.md.
- AGENTS.md: released by OpenAI Aug 2025, donated to Linux Foundation AAIF 2025-12-09 with MCP and goose; adopted by 60k+ projects.
- Pattern: canonical `AGENTS.md`, thin `CLAUDE.md`/`GEMINI.md` with `@AGENTS.md` import; generate all three MCP configs from one Nix attrset (`pkgs.formats.toml` for Codex); `~/.gemini`, `~/.codex` are not XDG → `home.file`.
- Memory servers compared: reference `@modelcontextprotocol/server-memory` (single JSONL, `MEMORY_FILE_PATH`), Mem0/OpenMemory (vector DB + Docker, rejected), Basic Memory (Markdown + SQLite, `uvx basic-memory mcp`, git/Syncthing-syncable, Obsidian-compatible) — recommended.
- Native memories: Claude auto-memory (v2.1.59+, `~/.claude/projects/<project>/memory/`, MEMORY.md 200 lines/25 KB), Codex `~/.codex/memories/` (off by default, generated state), Gemini `/memory add` → `~/.gemini/GEMINI.md`. None accept a shared dir cleanly; do not sync native stores — use Basic Memory + git AGENTS.md.
- Nix packaging: `pkgs.claude-code`, `pkgs.codex`, `pkgs.gemini-cli` (removal note 2026-08-07: Google replacing Gemini CLI with Antigravity CLI); overlays `ryoppippi/nix-claude-code`, `sadjow/claude-code-nix`, `kissgyorgy/coding-agents`, numtide `llm-agents.nix`.

## yabai / AeroSpace
- `services.yabai` with `enableScriptingAddition`; `window_placement second_child`, `auto_balance off`, `focus_follows_mouse off`, `window_animation_duration 0.0`; rules/signals; SIP partial disable `csrutil enable --without fs --without debug --without nvram`; sudoers line pinned to sha256 of yabai binary.
- Recommendation: AeroSpace (i3-like, TOML, no SIP, "will never require you to disable SIP") as daily driver; yabai only for native Spaces depth. Known SA breakage on Sequoia 15.3/15.4.
- Hotkeys: skhd (minimal) vs Karabiner (kernel driver) vs kanata (declarative); keep layers in QMK/ZMK firmware.

## macOS defaults
- `system.defaults.spaces.spans-displays = false` ⇔ "Displays have separate Spaces" ON (inverted polarity); `dock.mru-spaces = false`; `WindowManager.EnableTilingByEdgeDrag/EnableTopTilingByEdgeDrag/EnableTilingOptionAccelerator/EnableTiledWindowMargins = false`; `NSAutomaticWindowAnimationsEnabled = false`; `NSWindowResizeTime = 0.001`; `dock.expose-animation-duration = 0.0`; `com.apple.universalaccess reduceMotion/reduceTransparency` (TCC-protected); `AppleSpacesSwitchOnActivate`; `DSDontWriteNetworkStores/USBStores`.
- Activation scripts: `pmset -c sleep 0`, `mdutil -i off /nix`; `launchd.daemons.limit-maxfiles` 524288; `nix.linux-builder` with `linux-builder-vz` for aarch64+x86_64 (only when `nix.enable = true`).
- Some `CustomUserPreferences` domains need `killall Dock` / logout.

## Wiki page draft included
JD page "10.03 myosx — nix-darwin host" with mermaid flow: AGENTS.md → CLAUDE.md/GEMINI.md imports; one attrset → three configs → Basic Memory MCP → git/Syncthing to other hosts; maintenance checklist (monthly `nix flake update`, reinstall yabai SA after macOS updates, verify sops, check `pkgs.gemini-cli` status, `basic-memory sync`).
