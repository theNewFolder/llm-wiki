---
type: concept
title: macOS defaults for a tiling WM (nix-darwin system.defaults)
tags: [macos, defaults, nix-darwin, tiling, performance]
confidence: high
sources: [raw/2026-09-19-myosx-nix-darwin-migration.md, raw/2026-10-17-live-machine-audit.md]
created: 2026-10-17
updated: 2026-10-17
---

# macOS defaults a tiling WM needs

Required: `system.defaults.spaces.spans-displays = false` (= "Displays have separate Spaces" ON — the plist boolean is inverted vs the UI label; verify with `defaults read com.apple.spaces spans-displays`); `dock.mru-spaces = false` (stable Space order); `WindowManager.{EnableTilingByEdgeDrag,EnableTopTilingByEdgeDrag,EnableTilingOptionAccelerator,EnableTiledWindowMargins} = false` (Sequoia+ native tiling off); `WindowManager.GloballyEnabled = false` (Stage Manager off).

Speed: `NSGlobalDomain.NSAutomaticWindowAnimationsEnabled = false`, `NSWindowResizeTime = 0.001`, `NSUseAnimatedFocusRing = false`, `dock.autohide-delay = 0.0`, `autohide-time-modifier = 0.0`, `expose-animation-duration` small, `launchanim = false`; Finder `DisableAllAnimations`. Tahoe: `dock.slow-motion-allowed = true` is required for `mineffect`/animation keys to apply.

Focus: `AppleSpacesSwitchOnActivate` (`false` live — activating an app does not yank focus across Spaces), `AppleWindowTabbingMode = "manual"`, `NSWindowShouldDragOnGesture = true`.

TCC-protected (`com.apple.universalaccess` reduceMotion/reduceTransparency): writing needs Full Disk Access for the terminal running the switch — the live config deliberately leaves these to System Settings.

No `defaults(1)` key → `system.activationScripts.postActivation`: `pmset -c sleep 0`, `nvram SystemAudioVolume=" "`, `mdutil -i off /nix` (Spotlight; see nix-darwin #1576 for why `/nix` indexing is hard), `activateSettings -u` to apply without logout; `launchd.daemons.limit-maxfiles` (`launchctl limit maxfiles 524288 524288`) or `serviceConfig.SoftResourceLimits.NumberOfFiles` on daemons.

Dead or changed keys: `com.apple.finder CreateDesktop` → `WindowManager.StandardHideDesktopIcons`; legacy NSGlobalDomain animation keys still parse but are weakened under the Sequoia/Tahoe WindowManager; `com.apple.dock`/`com.apple.spaces` CustomUserPreferences may need `killall Dock` or logout. macOS Tahoe also gates script-wrapped LaunchDaemons through Background Task Management — if `/nix` disappears after an upgrade, re-enable the two "sh" items under Login Items & Extensions → Allow in the Background (`sfltool dumpbtm` shows `disallowed`).

Live status: all of the above except `universalaccess` are set in `modules/darwin/system.nix`; `limit-maxfiles` and `mdutil` are not yet. WM context: [[tiling-wm-macos]].
