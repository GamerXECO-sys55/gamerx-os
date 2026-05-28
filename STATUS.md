# Project Status

Live status of the project. Updated whenever a phase advances.

| Field | Value |
|---|---|
| Current phase | **P5 — Repo signing & GitHub Releases publishing pipeline** |
| Last updated | 2026-05-28 |
| Next milestone | First themed live ISO that boots in QEMU |
| Blocking | None |

## Recent activity

- 2026-05-28 · **P4 complete.** Boot-chain themes shipped — pushed to `gamerx-branding` and `gamerx-packages`:
  - **GRUB theme**: 1920x1080 aurora background (programmatic SVG → PNG), Inter wordmark/title, accented progress bar, tinted item boxes.
  - **Plymouth theme**: pulsing duotone GamerX orb on aurora background. Single-script (no animation frame stack); 1.6 Hz sine pulse.
  - **SDDM theme**: 480-line QML greeter with **four** background modes — `aurora` (procedural animated default, no asset cost), `image`, `video`, `gif`. hyprlock-style password card, palette-aware, video-backdrop ready.
  - 3 new PKGBUILDs all dual-mode and namcap-clean.
- 2026-05-28 · **P3 complete.** Theme system end-to-end (9/9 e2e checks pass).
- 2026-05-28 · **P2 v0 complete.** 7 core packages.
- 2026-05-28 · **P1 v0 complete.** Branding.
- 2026-05-28 · **P0 complete.** Repos cleaned & created.

## Package inventory (built locally)

| Package | Version | Size | Purpose |
|---|---|---|---|
| gamerx-keyring | 20260528-1 | 20K | Master GPG public key + trust |
| gamerx-mirrorlist | 20260528-1 | 16K | Pacman mirrorlist |
| gamerx-branding | 20260528-1 | 624K | Logos, palettes, wallpaper, /etc/os-release |
| gamerx-tweaks | 20260528-1 | 17K | sysctl + zram + journald + makepkg + earlyoom |
| gamerx-meta | 20260528-1 | 16K | Vanilla edition meta (~110 deps) |
| gamerx-meta-aria | 20260528-1 | 15K | Aria edition meta |
| gamerx-theme | 0.1.0-1 | 21K | Theme CLI |
| gamerx-shell | 0.1.0-1 | 36K | Hyprland configs + Quickshell + renderers |
| gamerx-grub-theme | 0.1.0-1 | 459K | GRUB theme |
| gamerx-plymouth-theme | 0.1.0-1 | 517K | Plymouth boot animation |
| gamerx-sddm-theme | 0.1.0-1 | 19K | SDDM greeter |

**11 packages** total. Local build artifacts staged at `/home/gamerx/GamerX-OS/build/x86_64/`.

## Next up — P5 repo signing & publishing

1. Sign every `.pkg.tar.zst` with the master GPG key (or a dedicated subkey for CI).
2. Run `repo-add gamerx-core.db.tar.zst <packages...>` to assemble the pacman DB.
3. Create GitHub Release `core-x86_64` on `gamerx-repo`; upload all `.pkg.tar.zst`, `.sig`, and `.db.tar.zst` files.
4. Document the user-facing setup in `gamerx-repo/README.md` (already done).
5. Add `gamerx-repo`'s `[gamerx-core]` block to `gamerx-mirrorlist` source as the live URL.
6. Smoke-test by adding the repo on this machine and `pacman -Sy` reading the DB.
