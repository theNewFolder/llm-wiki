---
type: entity
title: theNewFolder — goals, learning objectives, interests, gaps
tags: [profile, goals, learning, github]
confidence: medium
sources: [raw/2026-10-17-github-profile-analysis-thenewfolder.md, raw/2026-10-16-gunix-myosx-digest.md, raw/ai-system-instructions-sysinstr.md]
created: 2026-10-17
updated: 2026-10-17
---

# theNewFolder (Omran "devo" Altunaiji)

GitHub user id 180199920, tz Asia/Dubai, follows cachix; public non-keyboard repos: `gunix`, `llm-wiki` (this repo), `wallzscript` fork; private/local: `nix-darwin`, `them5`. Keyboard repos out of scope. Operator profile and format rules: [[sysinstr-response-format]].

## Goals (evidence-ranked)
1. Declarative, reproducible OS with a minimal trusted base (gunix: ~50-package closure, flakes).
2. Agent-integrated workstation — MCP registry, canonical AGENTS.md, shared memory ([[mcp-config-sync-three-clis]], [[agent-instruction-files]], [[shared-agent-memory-basic-memory]]) — strongest current momentum.
3. One flake spanning NixOS + macOS with real secrets ([[flake-layout-shared-nixos-darwin]], [[secrets-1password-op-run]]).
4. Keyboard-driven, Emacs-centred desktop; tiling WM ([[tiling-wm-macos]]).
5. AI-maintained PKM that compounds (this wiki; vault — [[memory-stores-on-the-mac]]).
6. Performance tuning (CachyOS/CFLAGS on Linux; on the Mac: Nix settings, defaults, [[local-inference-m5]]).
7. Local inference and fine-tuning (MLX LoRA; early).
8. Guix/Guile dual layer — receding (gunix's own migration notes point to NixOS-only).
9. Fleet DevOps (builders, caches, Colmena) and suckless/C++ — stated, no public artefacts.

## Learning objectives (status)
NixOS flakes intermediate → nix-darwin + home-manager-as-module applied (private); Nix perf/ops studying; Guix early; Elisp active but wrapped; MCP/agent engineering active; PKM with LLMs active; Bash/installer applied; Wayland active (Linux side); C++/suckless stated; local LLM early; LVM/Btrfs applied, ZFS stated; secrets moving agenix → sops/1Password. Mac-side toolchains declared in them5: Common Lisp (SBCL/Roswell), Clojure/Babashka, Rust (cargo-watch/nextest/evcxr), MLX, buckets (mc/rclone/R2), OrbStack.

## Contradictions worth resolving
FOSS purity vs NVIDIA/1Password/proprietary CLIs/macOS; Guix primary vs NixOS-only; suckless identity vs no public C; fleet DevOps vs no public IaC/CI; strict secrets posture vs gunix's committed `initialPassword = "o"`; "the grunt work is the learning" vs full delegation to agents.

## Gaps → recommended next steps
Publish the Mac flake with `checks`/`devShells`/CI; ADR on Guix (daemon-only or retire; archive gunix); Attic/Harmonia on the NixOS box; package `ai-mcp` and FastMCP servers as flake packages with tests; `skills/` with SKILL.md ([[agent-skills-locations-2026]]); Nix-packaged llama.cpp/Ollama + eval harness before fine-tuning; literate `config.org` Emacs; point the LLM-wiki pattern at the vault and serve it via Basic Memory.

gunix defects not to re-import: default password + passwordless sudo, six nonexistent npm MCP packages, Guix package names in `home.nix`, `cargoHash = null`, channel import inside a flake, contradictory WM claims.

Source pages: [[github-profile-analysis-2026-10]], [[ai-system-instructions]].
