---
type: concept
title: Local inference on the M5 Max — Ollama, oMLX, MLX
tags: [ollama, mlx, omlx, local-llm, apple-silicon]
confidence: medium
sources: [raw/2026-09-05-routing-memory-wiki-audit.md, raw/2026-10-17-live-machine-audit.md, raw/2026-10-16-m5-hybrid-contract.md, raw/2026-10-16-gunix-myosx-digest.md]
created: 2026-10-17
updated: 2026-10-17
---

# Local inference on 128 GB unified memory

- Ollama runs as a brew launchd agent; it does not read the login shell, so `ai.nix` pushes env with `launchctl setenv`: `OLLAMA_KEEP_ALIVE=24h`, `OLLAMA_NUM_PARALLEL=4`, `OLLAMA_MAX_LOADED_MODELS=3`, `OLLAMA_FLASH_ATTENTION=1`, `OLLAMA_KV_CACHE_TYPE=q8_0`. gunix's `qwen2.5:3b` default is wrong for this box — 14B–32B Q4 coders fit, 70B Q4 is possible.
- oMLX lesson (2026-09-05): `~/.omlx/model_settings.json` `is_pinned: true` preloads the model at every server start and exempts it from idle timeout — 82 GB (Hy3-OptiQ-2bit) sat resident. Unpin, unload via the authenticated route, restart from the current generation; the menubar app does not autostart the server, so a launchd agent (once `ai.nix` is committed) is the supervision fix.
- Pi routing: a local route must name a provider that exists in `models.json` and the enabled packages, else every role silently dead-ends (4 dead routes → 0 after re-deriving from the catalog).
- MLX stays in `uv` venvs (nixpkgs `mlx` is CPU-only); `llama-cpp` is in `packages-them5.nix`; PyTorch MPS fallback + no high-watermark cap in `ai.nix`; HF cache under `~/.cache/huggingface` with XET high-performance.
- Sizing before download: `uvx hf-mem --model-id <id> [--gguf-file <quant>] [--experimental --max-model-len N]`.
- Candidates noted, not pulled: Qwen3.8-27B-8bit (~29 GB) until 4-bit Qwen3.6 is shown to be the bottleneck.

Related: [[machine-oms-macbook-pro]], [[thenewfolder-profile]] (fine-tuning is a stated learning goal; keep MLX LoRA in venvs, add an eval harness before fine-tuning).
