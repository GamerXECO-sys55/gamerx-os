# Project Status

Live status of the project. Updated whenever a phase advances.

| Field | Value |
|---|---|
| Current phase | **P6 done (profile shipped); awaiting first `mkarchiso` build** |
| Last updated | 2026-05-28 |
| Next milestone | First themed live ISO that boots in QEMU |
| Blocking | None — `sudo bash gamerx-iso/scripts/build.sh` is a manual step |

## Recent activity

- 2026-05-28 · **P6 ISO profile shipped.** `gamerx-iso` repo now contains a complete archiso profile forked from `releng` and rebranded:
  - `profiledef.sh`: iso_name `gamerx-os`, label `GAMERX_<YYYYMM>`, install_dir `gamerx`.
  - `pacman.conf`: includes our live `[gamerx-core]` channel.
  - `packages.x86_64`: full GamerX desktop stack — Hyprland, Quickshell, Waybar, walker, swaync, swww, hyprlock, ghostty, fish, Nautilus + 9 GamerX packages + Calamares + Btrfs/snapper.
  - `airootfs/`: live user `gamerx` (uid 1000, fish shell), passwordless sudo, SDDM autologin into Hyprland with theme=gamerx, NetworkManager + sddm enabled via real systemd symlinks, /etc/skel populated to source modular shell configs, /usr/local/bin/gamerx-live-autostart bootstraps theme state on first login.
  - `scripts/build.sh` (sudo wrapper around mkarchiso) + `scripts/test-qemu.sh` (KVM/UEFI boot).
- 2026-05-28 · **P5 complete.** Repo signing & publishing pipeline live, 11 packages on the channel, verified end-to-end with real pacman client.
- 2026-05-28 · **P4 complete.** Boot-chain themes (GRUB, Plymouth, SDDM).
- 2026-05-28 · **P3 complete.** Theme system end-to-end.
- 2026-05-28 · **P2 v0 complete.** 7 core packages.
- 2026-05-28 · **P1 v0 complete.** Branding.
- 2026-05-28 · **P0 complete.** Repos cleaned & created.

## Next up — first ISO build (manual) → P7 Calamares custom

1. Run `sudo bash /home/gamerx/GamerX-OS/repos/gamerx-iso/scripts/build.sh`. Output: `out/gamerx-os-<calver>-x86_64.iso` (~3 GB, 10-20 min).
2. Boot it in QEMU: `bash /home/gamerx/GamerX-OS/repos/gamerx-iso/scripts/test-qemu.sh`. Should land at SDDM, autologin into Hyprland with the GamerX shell.
3. Iterate any visual issues spotted in the live boot.
4. **P7 Calamares custom**: re-skin Calamares (slideshow QML, branding QSS), add the kernel/browser/app-bundles/driver-detector custom modules.
