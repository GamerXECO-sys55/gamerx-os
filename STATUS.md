# Project Status

Live status of the project. Updated whenever a phase advances.

| Field | Value |
|---|---|
| Current phase | **P4 — Boot-chain themes** (GRUB + Plymouth + SDDM + hyprlock theme polish) |
| Last updated | 2026-05-28 |
| Next milestone | First themed live ISO that boots in QEMU |
| Blocking | None |

## Recent activity

- 2026-05-28 · **P3 complete.** End-to-end theme system shipping. Pushed to `gamerx-shell` and `gamerx-packages`:
  - **Hyprland**: 9 modular configs (monitors, input, env, decoration, keybinds, rules, autostart, Aria reservations, density+animation include points). 3 density presets, 3 animation presets with a custom GamerX cubic-bezier(0.16, 1, 0.3, 1).
  - **Status bar**: Waybar default + minimal styles (config.jsonc + style.css per style).
  - **Launcher**: walker list + grid styles.
  - **Notifications**: swaync default style with rich control center.
  - **Lockscreen**: hyprlock template (palette tokens replaced by `60-hyprlock.sh` renderer).
  - **Quickshell**: `shell.qml` + 4 modules (ControlCenter, PowerMenu, OSD, ThemeLoader). IPC target `gamerx` exposes toggle/show/reload functions.
  - **Theme renderers**: `10-palette` → `80-wallpaper`. State.toml → theme.json → all components, hot-reloaded.
  - **End-to-end test**: 9/9 checks pass. `gamerx-theme set palette tokyo-night ; set density compact ; set animation snappy ; set bar minimal ; set launcher grid` correctly propagates to every component.
  - **PKGBUILD**: dual-mode (GitHub clone or `GAMERX_LOCAL_SRC` env override). 37 KB any-arch package, namcap clean.
- 2026-05-28 · **P2 v0 complete.** 7 core packages built and pushed.
- 2026-05-28 · **P1 v0 complete.** Branding pushed.
- 2026-05-28 · **P0 complete.** Repos cleaned & created.

## Next up — P4 boot-chain themes

1. GRUB theme (background, fonts, menu styling, brand mark)
2. Plymouth animated theme (pulsing GamerX orb)
3. SDDM theme (image / video / GIF backdrop, GamerX mark, palette-aware)
4. hyprlock theme polish (already templated; iterate on visual)
5. Each as its own PKGBUILD in `gamerx-packages/`
