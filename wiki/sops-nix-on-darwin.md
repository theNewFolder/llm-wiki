---
type: concept
title: sops-nix on nix-darwin (age key from SSH)
tags: [secrets, sops-nix, age, nix-darwin]
confidence: high
sources: [raw/2026-09-19-myosx-nix-darwin-migration.md, raw/2026-10-17-live-machine-audit.md]
created: 2026-10-17
updated: 2026-10-17
---

# sops-nix on darwin

Module: `sops-nix.darwinModules.sops` in `darwinConfigurations.<host>.modules`; user-level alternative `sops-nix.homeManagerModules.sops` (same `keys.txt`, `%r` = `$(getconf DARWIN_USER_TEMP_DIR)`).

Key: `$HOME/Library/Application Support/sops/age/keys.txt` (or `sops.age.keyFile`). Derive from the Mac's ed25519 key:
```bash
nix run nixpkgs#ssh-to-age -- -private-key -i ~/.ssh/id_ed25519 -o "$HOME/Library/Application Support/sops/age/keys.txt"
age-keygen -y "$HOME/Library/Application Support/sops/age/keys.txt"   # recipient for .sops.yaml
for f in $(grep -rl '^sops:' secrets/); do sops updatekeys -y "$f"; done   # re-key every file
```
`ssh-to-age` cannot read `id_ed25519_sk` (security-key) identities.

Declare: `sops.secrets."ai/openai_api_key" = { owner = "om"; path = "/Users/om/.secrets/openai_api_key"; mode = "0400"; };`. `neededForUsers` is NixOS-only. Composite env files for launchd: `sops.templates.<n>.content` with `config.sops.placeholder.<secret>`, then `set -a; . <path>; exec daemon`.

Migration checklist: keygen → ssh-to-age → add recipient → `updatekeys` → commit → add darwin module + secrets → switch → verify ownership; keep the old recipient so both hosts decrypt.

Where it stands on the live Mac: sops/age is reserved for cross-host files (`SOPS_AGE_KEY` lives in 1Password); runtime secrets use [[secrets-1password-op-run]]. See [[nix-darwin-migration]].
