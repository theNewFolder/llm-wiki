# RAW — GitHub profile analysis: theNewFolder (goals, learning objectives, interests)

Source: user-supplied research report compiled from public GitHub (user id 180199920, tz Asia/Dubai, follows cachix org) plus the user's context documents. Keyboard repos excluded by user instruction.

## Repo inventory (non-keyboard)
| repo | what | state |
|---|---|---|
| gunix | NixOS base (~50 pkgs, CachyOS kernel) + GNU Guix userspace; EXWM/Niri/dwl-guile; Gemini MCP server; CLAUDE.md, `.mcp.json` | README "Last Updated 2026-03-01"; 18 commits; placeholder clone URL; default password `o` + autologin documented; agenix commented out; `GUIX_TO_NIXOS_FINAL_SUMMARY.md`, `MIGRATION_STATUS.md`, `CLEANUP_STRATEGY.md` suggest Guix being cut back |
| llm-wiki | deep-research wiki about Karpathy's LLM Wiki pattern, structured as an LLM wiki | compiled 2026-09-20, 2 commits |
| wallzscript | fork of fr0st-xyz/wallz wallpapers | fork |

Not public despite documents: myosx flake, dwm/suckless build, C++ sources, local-LLM repo, org-roam config, Termux/Nix-on-Droid, FastMCP skills scaffold, JD/Obsidian vault.

## Goals (ranked by evidence)
1. Declarative, reproducible OS with minimal trusted base (stated: gunix README).
2. Dual-layer Nix + Guix for GNU/FOSS alignment (stated; inferred to be receding).
3. Agent-integrated workstation: MCP + Claude/Gemini, one attrset → three CLI configs, Basic Memory, canonical AGENTS.md.
4. Performance-tuned system (CachyOS BORE, x86_64-v3/v4 — CachyOS wiki: 5–20% uplift; `latest-x86_64-v3` kernel may not actually be BORE).
5. Keyboard-driven, Emacs-centred desktop (EXWM; System Crafters lineage; shifting to Wayland Niri/dwl).
6. AI-maintained PKM / knowledge that compounds (llm-wiki; Zettelkasten + PARA + Johnny Decimal).
7. One flake spanning NixOS and macOS with real secrets (myosx; sops-nix + ssh-to-age + `op run`).
8. Fleet-scale DevOps (remote builders, caches, Colmena/deploy-rs) — documents only.
9. Local/offline LLM inference and fine-tuning — exploratory (llama-2 LangChain script is dated; Llama 2 released 2023-07-18).
10. suckless/C++ minimalism — stated only.

## Learning objectives (assessment)
NixOS flakes/modules: active, intermediate (no hosts/modules split in gunix). nix-darwin/HM-as-module: active, private. Nix perf/ops (nix-fast-build, Attic/Harmonia, nh/nom/nvd, Colmena): studying. Guix/Guile: early-intermediate (config, not packages). Emacs Lisp: active, hidden behind Scheme wrapper. MCP/agent engineering: strongest momentum (gemini-mcp/server.py, .mcp.json, CLAUDE.md as schema). PKM with LLMs: active. Bash/installer engineering: applied (`--dry-run`, GPG-verified bootstrap). Wayland (Niri, dwl): active. C++/suckless: stated only. Local LLM: early. Storage LVM/Btrfs applied, ZFS stated. Secrets: moving agenix → sops-nix.

## Interests
Declarative systems (incl. flake design, nix-systems); FOSS/GNU ideology (in tension with NVIDIA, 1Password, proprietary AI CLIs, CachyOS, macOS); performance/hardware tuning; keyboard-centric ergonomics and tiling WMs; Lisp-family configuration (Guile, Elisp); AI agents as infrastructure; PKM; ricing (Catppuccin/Gruvbox/Nord); System Crafters lineage.

## Contradictions and gaps
FOSS purity vs pragmatic stack; Guix primary vs NixOS-only direction; suckless/dwm identity vs EXWM/Niri work and no C; fleet DevOps claims vs zero public IaC/CI; strict security posture vs committed default password; "learning by doing" vs full delegation to agents ("the grunt work IS the learning").

## Suggested next steps (report's)
Publish myosx as canonical flake (hosts/{nixos,darwin}, modules/{nixos,darwin,home}, `system.primaryUser`); flake-parts + nix-systems; `checks`, `formatter` (nixfmt), `devShells`, CI with `nix flake check` + nix-fast-build; sops-nix + ssh-to-age replacing default password; ADR on Guix (daemon-only or retire; archive gunix into `modules/nixos/guix.nix`); `documentation.enable = false`, `follows` on inputs, nh/nom/nvd, nix-index-database, nix-direnv, self-hosted Attic/Harmonia; Nix module rendering `.mcp.json` for Claude/Codex/Gemini/gptel from one attrset; package `gemini-mcp` + FastMCP servers as flake packages with pytest; `skills/` with SKILL.md; Nix-packaged llama.cpp/Ollama with eval harness, defer fine-tuning; literate `config.org` Emacs via emacs-overlay; Nix-built dwm/dwl or drop from self-description; point llm-wiki pattern at own JD vault, serve read-only via Basic Memory MCP.
