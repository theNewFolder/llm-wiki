---
type: source
title: Source — "Routing, Memory, and the Wiki" audit (2026-09-02→05)
tags: [source, audit, pi, omlx, hermes, yabai]
confidence: high
sources: [raw/2026-09-05-routing-memory-wiki-audit.md]
created: 2026-10-17
updated: 2026-10-17
---

# Source: Routing, Memory, and the Wiki (2026-09-05)

Live-machine audit (myox, M5 Max, macOS 27.0 beta) of three subsystems; every finding measured, nothing destructive executed.

Claims filed: [[local-inference-m5]] (oMLX pinned-model preload, Pi dead routes, supervision gap), [[memory-stores-on-the-mac]] (five stores, three proposers, Codex loss, Drive replica retirement), [[tiling-wm-macos]] (yabai double-free root cause and patch).

Still pending at time of ingest: unsigned commits (myox `e86c35f`, `35fbd6c`; vault `42b64d6`, `6196fc2`), yabai patch activation (moot after OmniWM switch), Drive replica retirement, oMLX launchd supervision, removal of the inert `memory` MCP block, hermes memory config caps.
