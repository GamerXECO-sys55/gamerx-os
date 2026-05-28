# GamerX OS — Roadmap

A phased plan from empty repo to a bootable ISO on a flash drive.

## Legend

- ✅ done · 🛠 in progress · ⏳ queued · 🚧 blocked

---

## Phase 0 · Housekeeping & tooling ✅

- [x] Secure GitHub token storage (`~/.config/gamerx-os/gh-token`, chmod 600)
- [x] GitHub credential helper installed
- [x] Git global config (user, email, default branch)
- [x] Audit existing GitHub repos
- [x] Delete legacy GamerX repos (per user instruction)
- [x] Create the 9 split repos with GPL-3.0
- [x] Clone all 9 repos locally
- [x] Scaffold meta `gamerx-os` repo (README, SPEC, ROADMAP, ARCHITECTURE, DECISIONS)

## Phase 1 · Branding v0 ✅

- [x] Generate v0 GamerX wordmark (SVG, geometric monogram)  ·  `logos/wordmark.svg` + light variant
- [x] Generate v0 orb mark (SVG, AI-leaning)  ·  `logos/orb.svg` + mono variant
- [x] Define accent palette (cyan + magenta + GamerX purple)  ·  `PALETTE.md` + 7 bundled palette TOMLs
- [x] First v0 wallpaper "Aurora" + sourcing plan for the other 7 placeholders
- [x] `os-release` template (vanilla + Aria) + `lsb-release` + `issue`
- [x] Pushed to `gamerx-branding`

## Phase 2 · Core packages ✅

- [x] `gamerx-keyring` PKGBUILD (master GPG key for the repo)
- [x] `gamerx-mirrorlist` PKGBUILD (initial mirror list)
- [x] `gamerx-branding` PKGBUILD (deploys logos, /etc/os-release, GRUB theme assets)
- [x] `gamerx-tweaks` PKGBUILD (sysctl, zram, ananicy rules)
- [x] `gamerx-meta` PKGBUILD (depends on the curated install set)
- [x] `gamerx-meta-aria` PKGBUILD (Aria edition)
- [x] `gamerx-theme` PKGBUILD (the CLI)
- [x] All 7 packages build cleanly with namcap clean modulo documented sandbox warnings
- [x] Theme CLI smoke-tested: show / get / set / list / validation all functional

## Phase 3 · gamerx-shell ✅

- [x] Hyprland config (animations, keybinds, rules, gaps presets) — modular, 9 fragments
- [x] Quickshell project skeleton — shell.qml + qmldir + IPC handler
- [x] Quickshell modules: ControlCenter, PowerMenu, OSD, ThemeLoader (Aria overlay slot reserved)
- [x] Waybar configs (default + minimal styles)
- [x] swaync configs (default style)
- [x] walker configs (list + grid styles)
- [x] hyprlock theme (template with palette tokens)
- [x] Palette files consumed (matugen + 7 curated already shipped via gamerx-branding)
- [x] Density presets (compact, comfortable, spacious)
- [x] Animation presets (snappy, smooth, cinematic) with custom GamerX bezier
- [x] Theme renderers (10-palette, 20-hyprland, 30-waybar, 40-walker, 50-swaync, 60-hyprlock, 70-quickshell, 80-wallpaper)
- [x] End-to-end test script: 9/9 OK — palette switch propagates to theme.json + Hyprland symlinks + Waybar/walker symlinks + hyprlock template render
- [x] gamerx-shell PKGBUILD (dual-mode: GitHub or GAMERX_LOCAL_SRC), built clean
- [x] Pushed to `gamerx-shell` and `gamerx-packages`

## Phase 4 · Boot-chain themes ✅

- [x] GRUB theme — `gamerx-branding/themes/gamerx-grub-theme/` + `gamerx-grub-theme` PKGBUILD (459K)
- [x] Plymouth theme — `gamerx-branding/themes/gamerx-plymouth-theme/` + `gamerx-plymouth-theme` PKGBUILD (517K, single-script pulsing orb)
- [x] SDDM theme — `gamerx-branding/themes/gamerx-sddm-theme/` (Main.qml with animated aurora + hyprlock-style card) + `gamerx-sddm-theme` PKGBUILD (19K)
- [x] hyprlock theme — already templated and rendered by P3
- [x] All built, namcap clean, pushed to `gamerx-branding` and `gamerx-packages`

## Phase 5 · Repo infra ✅

- [x] Generate master GPG keypair (offline kept; subkeys for signing)
- [x] All 11 packages signed locally
- [x] `repo-add --sign` builds gamerx-core.db.tar.zst with signed db
- [x] GitHub Release `gamerx-core-x86_64` created on `gamerx-repo` and populated
- [x] `gamerx-repo` README rewritten with live install instructions
- [x] **Verified end-to-end**: real `pacman -Sy` against the public URL fetches the db and lists all 11 packages
- [x] `scripts/sign_and_publish.sh` is idempotent — re-run any time to refresh the channel
- [ ] CI automation (lands in P9)

## Phase 6 · ISO profile ⏳

- [ ] Fork archiso `releng` profile into `gamerx-iso`
- [ ] `packages.x86_64` final list
- [ ] `airootfs/` overlay (live user, autologin, GamerX-themed live session)
- [ ] `pacman.conf` with `[gamerx-core]` enabled
- [ ] Boot loaders: GRUB (BIOS + UEFI), systemd-boot fallback
- [ ] Test: ISO boots in QEMU/UTM
- [ ] Test: ISO boots on real flash drive

## Phase 7 · Calamares ⏳

- [ ] Calamares branding (logos, slideshow, theme QSS)
- [ ] Custom module: kernel picker
- [ ] Custom module: browser picker
- [ ] Custom module: app bundles picker
- [ ] Custom module: GPU driver detector
- [ ] Filesystem flow (Btrfs subvols + ext4 + LUKS2)
- [ ] Test: full install on QEMU

## Phase 8 · Welcome app + tweaker ⏳

- [ ] GTK4 welcome app skeleton
- [ ] First-boot wizard pages
- [ ] Tweaker pages (palette, density, animation, etc.)
- [ ] Aria setup tile
- [ ] Package as `gamerx-welcome`

## Phase 9 · Docs site + release CI ⏳

- [ ] mkdocs-material site in `gamerx-docs`
- [ ] User docs (install, first boot, theming)
- [ ] Dev docs (build from source, contribute)
- [ ] GitHub Actions: ISO build on tag push, upload to release
- [ ] First public alpha release

## Phase 10 · Aria edition + GamerX Settings v1.2 ⏳

- [ ] Aria meta package
- [ ] Aria-specific Calamares tweak
- [ ] Build the Aria-edition ISO
- [ ] Native GamerX Settings (Tauri or GTK4) with full customization

---

## Cadence

- Weekly snapshots from this roadmap into `DECISIONS.md`.
- Tagged ISO releases approximately every quarter (`26.03`, `26.06`, `26.09`, ...).
- Continuous package updates via `[gamerx-core]`.
