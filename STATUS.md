# Project Status

Live status of the project. Updated whenever a phase advances.

| Field | Value |
|---|---|
| Current phase | **P6 in progress — ISO builds + boots, polishing live session** |
| Last updated | 2026-05-28 |
| Next milestone | First themed live ISO with Calamares auto-launching cleanly |
| Blocking | None — manual rebuild + QEMU test loop |

## Recent activity

- 2026-05-28 · **P6 boot-chain rewrite (round 4).** Switched from per-file Hyprland exec-once tweaks to a single unified orchestrator script `/usr/bin/gamerx-shell-start` that:
  - imports session env into systemd + dbus
  - starts polkit, swww, Quickshell (with Waybar fallback if Quickshell crashes from Qt ABI mismatches), swaync, hypridle, cliphist, nm-applet
  - detects `/etc/gamerx-live` marker → schedules welcome notification + auto-launches Calamares (via `sudo -E` since the live user has wheel NOPASSWD)
  - logs everything to `~/.cache/gamerx-shell-start.log` for diagnosis
- 2026-05-28 · **P6 root-cause fixes shipped:**
  - mkinitcpio uses `/etc/mkinitcpio.conf.d/archiso.conf` (from `mkinitcpio-archiso`) — `customize_airootfs.sh` now patches it to insert `plymouth` after `udev` and adds KMS modules, then runs `mkinitcpio -P`.
  - `scripts/build.sh` copies `unicode.pf2` from host into `grub/fonts/` before mkarchiso runs (mkarchiso doesn't auto-copy it; without it GRUB falls back to text mode).
  - Hyprland `exec-once` lines using `&`, `&&`, `>` etc. were broken — Hyprland doesn't go through a shell. Replaced with single orchestrator call.
  - Polkit rule for `gamerx` user lets Calamares run without password prompt on live ISO.
- 2026-05-28 · **P5 publishing pipeline live.** 11 packages on `gamerx-core-x86_64`, 6 AUR packages on `gamerx-aur-x86_64` GitHub Releases. Verified end-to-end with real pacman client.
- 2026-05-28 · **P0–P5 complete.** Repos cleaned & created, branding v0, 7 core packages, full theme system, boot-chain themes (GRUB/Plymouth/SDDM), repo signing & publishing.

## Next up — close out P6 → P7 Calamares custom

1. Rebuild ISO with the orchestrator changes.
2. Boot in QEMU. Expected: branded GRUB, Plymouth pulsing orb for the full kernel boot, SDDM autologin, Hyprland with Waybar (or Quickshell if it survives), wallpaper, Calamares auto-opens.
3. Iterate any remaining visual issues.
4. **P7 Calamares custom**: re-skin Calamares (slideshow QML, branding QSS), add the kernel/browser/app-bundles/driver-detector custom modules.
