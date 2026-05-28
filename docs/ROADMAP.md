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

## Phase 1 · Branding v0 🛠

- [ ] Generate v0 GamerX wordmark (SVG, geometric monogram)
- [ ] Generate v0 orb mark (SVG, AI-leaning)
- [ ] Define accent palette (cyan + magenta + GamerX purple)
- [ ] Create 8 v0 wallpapers (3 cyberpunk, 2 abstract, 2 geometric, 1 photo) — placeholders sourced + later commissioned
- [ ] `os-release` template with GamerX branding
- [ ] `lsb-release` template
- [ ] Push to `gamerx-branding`

## Phase 2 · Core packages ⏳

- [ ] `gamerx-keyring` PKGBUILD (master GPG key for the repo)
- [ ] `gamerx-mirrorlist` PKGBUILD (initial mirror list)
- [ ] `gamerx-branding` PKGBUILD (deploys logos, /etc/os-release, GRUB theme assets)
- [ ] `gamerx-tweaks` PKGBUILD (sysctl, zram, ananicy rules)
- [ ] `gamerx-meta` PKGBUILD (depends on the curated install set)
- [ ] `gamerx-meta-aria` PKGBUILD (Aria edition)
- [ ] `gamerx-theme` PKGBUILD (the CLI)

## Phase 3 · gamerx-shell ⏳

- [ ] Hyprland config (animations, keybinds, rules, gaps presets)
- [ ] Quickshell project skeleton
- [ ] Quickshell modules: launcher, control center, OSD, notification panel, Aria overlay slot
- [ ] Waybar configs (5 styles)
- [ ] swaync configs (4 styles)
- [ ] walker configs (5 styles)
- [ ] hyprlock theme
- [ ] Palette files (matugen + 6 curated)
- [ ] Density presets
- [ ] Animation presets
- [ ] `gamerx-theme` CLI implementation
- [ ] Push to `gamerx-shell`

## Phase 4 · Boot-chain themes ⏳

- [ ] GRUB theme
- [ ] Plymouth theme (animated)
- [ ] SDDM theme (image/video/GIF backgrounds)
- [ ] hyprlock theme (matches SDDM)
- [ ] Push to `gamerx-branding`

## Phase 5 · Repo infra ⏳

- [ ] Generate master GPG keypair (offline kept; subkeys for signing)
- [ ] `gamerx-repo` GitHub Actions: build PKGBUILDs → sign → publish
- [ ] `gamerx-repo` README with `pacman.conf` snippet for users
- [ ] `gamerx-mirrorlist` final content

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
