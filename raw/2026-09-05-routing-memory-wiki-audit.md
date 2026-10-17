# RAW — "Routing, Memory, and the Wiki" (live-machine audit, myox · M5 Max · macOS 27.0 26A5425a, 2026-09-02 → 2026-09-05)

Source: user-supplied artifact (claude.ai artifact JpvMwvhRX3qfC5Vu5x9QRH). Three subsystems audited against the live machine, not docs. Board: Pi model routes 10/10 live; skills Pi can load 89/89; memory proposers 3/3 (Pi, Hermes, Claude); oMLX resident 0 GB (was 82 GB pinned).

## Routing
- Pi's live settings routed everything to `ollama/qwen3-coder-next:q4_K_M`, unreachable three ways (no ollama provider in `models.json`, not in Pi's catalog, `pi-local-models` filtered out of `.packages`). Fixed `3f4925b`; dead-route warnings 4 → 0; not yet exercised by a Pi session.
- oMLX pinned model: `~/.omlx/model_settings.json` had `is_pinned: true` on Hy3-OptiQ-2bit → preloaded at every server start (82.22 GB, 14 s after start) and exempt from idle timeout. Unpinned, unloaded via authenticated unload route, server restarted from current generation (a stale-generation menubar app had been supervising it). Gap: menubar app does not autostart the server; CLI-started server is unsupervised until Start pressed or a launchd job exists.
- Judgment calls: T3 Code default model is user-owned in `~/.t3/userdata/settings.json` (`ai.nix` manages only driver enablement); Hy3 kept in Pi enabled models as vision escape hatch; Qwen3.8-27B-8bit (~29 GB) is an upgrade candidate, not pulled; nix-darwin/stylix zero-diff on update (FlakeHub 26.05 range), Determinate 3.22.3 docs-only.

## Memory (five stores, two alive, three proposers)
- `hermes-memory-sync` hardcoded `~/.hermes/memories/` (empty) while 19 entries lived in `~/.pi/agent/pi-hermes-memory/`; fixed `e9c5419`; hourly job ran 7× exit 0; mirror 22 entries. `.recovery-*` files are the extension's write-ahead policy (7-day grace, 32 max) — do not prune.
- Claude's memory had no proposer; `claude-memory-digest` added (`35fbd6c`), read-only, same fingerprinting and secret filter, weekly Sunday 18:30; first dry run 42 candidates, 29 promotable, zero duplicates. Promotions stay human-approved.
- Drive checkout of the vault: 15 behind, 0 ahead, ancestor of Documents HEAD, 22 dirty entries, one unique page (`synthesis/codex-history-patterns-2026-07.md`); recommendation: copy that page, `git stash push -u`, stop pulling, drop fallback default in `pi-memory-digest`.
- Codex memory is gone: no snapshots, no Time Machine destination, `~/.codex` recreated 2026-08-24, no Drive/iCloud copy; current Codex keeps memory in `memories_1.sqlite` and would not read the old file. Two vault pages hold what survives.

## Wiki (163 canonical pages; lint 92 → 64)
- Frontmatter lint over five canonical trees: 4 pages had no frontmatter, 20 missing required keys (dates recovered via `git log --follow`, never invented); MCP roster count disagreed across six pages → now one roster page; `bin/vault-health-check.sh` rewritten, `bin/vault-lint.py` new; 64 remaining are ambiguous `type: note` and blank lines after opening fence; stale tool pages (kanata, AeroSpace, colima, Tokyo Night) already marked removed. Commit `42b64d6`.

## yabai double-free
- Both 09-05 crashes fault in `window_destroy → CFRelease` at `0x4d55545800000008` ("MUTX"+8): `ref` read from a freed struct recycled as a pthread mutex. Two producers race: Accessibility observer claims with a real CAS; SLS destroyed-handler uses identity swap (`&id → &id`) that never claims. Present in upstream master. Patch `e86c35f` makes the SLS guard a real claim and lets `window_destroy` refuse a second run; package builds; activation needs preflight gate, switch, and Accessibility re-grant at the keyboard.

## Needs a person
Unlock 1Password and sign commits (myox `e86c35f`, `35fbd6c`; vault `42b64d6`, `6196fc2`); activate yabai patch; retire Drive replica; supervise oMLX server (Start in menubar or launchd agent once `ai.nix` clean); after `ai.nix` committed: remove inert `memory` MCP block, add `~/.pi/agent/hermes-memory-config.json` (limit 8000, timeout 300 s) — MEMORY.md at 93% of cap, failures.md at 98%.
