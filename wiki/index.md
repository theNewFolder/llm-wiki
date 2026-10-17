---
type: index
title: Wiki Index
updated: 2026-10-17
---

# Wiki Index

## The LLM Wiki pattern (research, 2026-09-20)
- [README.md](../README.md) — the compiled deep-research report: origins, architecture, comparisons, ecosystem, criticisms, outlook
- [[architecture]] — three layers, three operations, two special files
- [[comparisons]] — LLM Wiki vs RAG vs agent memory; hybrid architectures
- [[ecosystem]] — implementations, adoption metrics, tooling stack
- [[extensions-and-standards]] — v2 governance, qmd, OKF
- [[limitations]] — criticisms, scaling limits, enterprise gaps

## Workstation — macOS / nix-darwin (ingested 2026-10-17)
- [[machine-oms-macbook-pro]] — entity: live state of the M5 Max Mac, open defects, operating rules
- [[hybrid-them5-overlay-contract]] — nix-darwin owns the system, them5 is a script overlay; what each may touch
- [[nix-darwin-migration]] — September plan vs what shipped; bootstrap; darwin traps
- [[flake-layout-shared-nixos-darwin]] — hosts/modules layout for one flake spanning NixOS and macOS
- [[nix-systems-externally-extensible]] — zimbatm's nix-systems input; measured 4× eval speedup
- [[sops-nix-on-darwin]] — sops-nix darwin module, ssh-to-age key derivation, re-key loop
- [[secrets-1password-op-run]] — op.env.tpl, with-secrets, per-server op wrappers, opnix
- [[macos-defaults-for-tiling-wm]] — the system.defaults a tiling WM needs; speed keys; TCC/BTM traps
- [[tiling-wm-macos]] — timeline yabai → AeroSpace → OmniWM; SIP/SA costs; hotkey layer decision
- [[local-inference-m5]] — Ollama launchd env, oMLX pinned-model lesson, MLX in venvs, sizing

## Agents — instructions, skills, MCP, memory
- [[agent-instruction-files]] — AGENTS.md canonical, CLAUDE.md/GEMINI.md import it; who reads what
- [[agent-skills-locations-2026]] — comparison: skill dirs and invocation for Claude Code, Codex, Gemini, Antigravity
- [[mcp-config-sync-three-clis]] — one registry → per-CLI config files; agy target missing
- [[shared-agent-memory-basic-memory]] — native memories vs Basic Memory; install and wiring
- [[memory-stores-on-the-mac]] — entity: five stores, three proposers, dead paths

## People and profile
- [[thenewfolder-profile]] — goals, learning objectives, interests, contradictions, next steps
- [[sysinstr-response-format]] — operator persona and strict [Concept]/[Script]/[Commands] format

## Sources (one page per ingested source)
- [[llm-wiki-deep-research-report]] — the seed report (README.md)
- [[myosx-migration-research-2026-09-19]] — nix-darwin migration research; superseded claims listed
- [[github-profile-analysis-2026-10]] — profile analysis; assumptions and confidence
- [[zimbatm-nix-systems-talk]] — nix-systems slides
- [[ai-system-instructions]] — SYSINSTR block
- [[routing-memory-wiki-audit-2026-09-05]] — Pi/oMLX/memory/vault/yabai audit
- [[gunix-myosx-digest-2026-10-16]] — gunix port digest
- [[m5-hybrid-contract-2026-10-16]] — hybrid contract + them5 conflict audit
- [[live-machine-audit-2026-10-17]] — this session's read-only machine inspection

## Meta
- [../CLAUDE.md](../CLAUDE.md) — schema (the program)
- [../log.md](../log.md) — operation log
- [../raw/sources.md](../raw/sources.md) — immutable source list (web sources 1–28, local documents 29–37)
