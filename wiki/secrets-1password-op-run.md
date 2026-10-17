---
type: concept
title: Secrets via 1Password — op run wrappers and opnix
tags: [secrets, 1password, op, opnix, mcp]
confidence: high
sources: [raw/2026-10-17-live-machine-audit.md, raw/2026-10-16-m5-hybrid-contract.md, raw/2026-09-19-myosx-nix-darwin-migration.md]
created: 2026-10-17
updated: 2026-10-17
---

# 1Password as the secrets source of truth

Live model on [[machine-oms-macbook-pro]]: no plaintext key ever hits disk.

- `secrets/op.env.tpl` (committed, safe): `KEY=op://<vault>/<item-id>/<field>` lines for GITHUB_*, ANTHROPIC/GEMINI/OPENROUTER, TAVILY/EXA/PERPLEXITY/CONTEXT7, HOSTINGER, SUPABASE, PROXMOX_*, SOPS_AGE_KEY.
- Shell helpers (`shell.nix`): `with-secrets <cmd>` = `op run --env-file <tpl> --no-masking -- <cmd>`; `op-run`; `load-op-env` (exports into the current shell, Touch ID).
- MCP servers needing a key are wrapped per server (`withSecrets "github" { envKey = …; }`): `op run --env-file <(awk 'index($0,key"=")==1' tpl)` so only that one `op://` reference resolves — a missing unrelated item (Proxmox) no longer blocks the server.
- Files on disk that must exist (certs, service tokens): `opnix` home-manager module with a service account.
- CLI wrappers pattern (from gunix/kimi-cc): `pkgs.writeShellApplication { runtimeInputs = [ _1password-cli claude-code ]; text = "exec op run --no-masking -- claude \"$@\""; }`.

Rejected: them5 `inject-secrets.sh` (`op inject` → plaintext `.env`), them5 `mcp-sync.py` (`os.path.expandvars` wrote keys into JSON), macOS Keychain (imperative, drifts). Cross-host encrypted files still use [[sops-nix-on-darwin]].

Rules: never literal keys in JSON/TOML/rc; never `initialPassword`; Codex gets `startup_timeout_sec = 30` for op-wrapped servers because 1Password auth exceeds the 10 s default.
