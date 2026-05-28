# GamerX OS — Architecture

How the pieces fit together. This is the document Aria and any contributor should
read first to understand the system.

---

## High-level layering

```
                           ┌─────────────────────────────────────┐
                           │           User Applications         │
                           └─────────────────────────────────────┘
                                            │
   ┌────────────────────────────────────────┼────────────────────────────────────┐
   │                                        │                                    │
   │                          ┌──────────────────────┐                           │
   │                          │   GamerX Settings    │ (v1.2, GTK4/Tauri)        │
   │                          │   Welcome / Tweaker  │ (v1.0, GTK4)              │
   │                          └──────────┬───────────┘                           │
   │                                     │ calls                                 │
   │                          ┌──────────▼───────────┐                           │
   │                          │   gamerx-theme CLI   │ (single source of truth)  │
   │                          └─────┬───────┬───────┬┘                           │
   │                                │       │       │                            │
   │             ┌──────────────────┘       │       └──────────────────┐         │
   │             ▼                          ▼                          ▼         │
   │   ┌─────────────────┐      ┌────────────────────┐      ┌──────────────────┐ │
   │   │ Quickshell      │      │ Waybar / swaync /  │      │ Boot chain       │ │
   │   │ (launcher,      │      │ walker / swww      │      │ themes (GRUB,    │ │
   │   │ control center, │      │ configs            │      │ Plymouth, SDDM,  │ │
   │   │ OSD, Aria over) │      │                    │      │ hyprlock)        │ │
   │   └─────────────────┘      └────────────────────┘      └──────────────────┘ │
   │                                     │                                       │
   │                          ┌──────────▼──────────┐                            │
   │                          │      Hyprland       │                            │
   │                          └──────────┬──────────┘                            │
   │                                     │                                       │
   │                          ┌──────────▼──────────┐                            │
   │                          │   Wayland · pipewire · NetworkManager · systemd  │
   │                          └──────────┬──────────┘                            │
   │                                     │                                       │
   │                          ┌──────────▼──────────┐                            │
   │                          │   Linux kernel (linux-zen default)               │
   │                          └─────────────────────┘                            │
   │                                                                             │
   │   gamerx-aria (Aria edition only) overlay: D-Bus io.gamerx.Aria1,           │
   │   /opt/aria, ~/.config/aria, port 7173, SUPER+SPACE keybind, Waybar slot    │
   │                                                                             │
   └─────────────────────────────────────────────────────────────────────────────┘
```

## Package graph

```
                          ┌──────────────────┐
                          │   gamerx-meta    │  (vanilla install set)
                          └────────┬─────────┘
                                   │ depends
       ┌───────────────────────────┼─────────────────────────────┐
       ▼                           ▼                             ▼
 ┌──────────┐             ┌─────────────────┐           ┌─────────────────┐
 │ hyprland │             │  gamerx-shell   │           │ gamerx-branding │
 │ quickshell             │   (configs +    │           │ (logos, themes) │
 │ waybar   │             │   gamerx-theme) │           └─────────────────┘
 │ ghostty  │             └─────────────────┘
 │ ...      │
 └──────────┘                  ┌──────────────┐
                               │ gamerx-tweaks│ (zram, ananicy, sysctl)
                               └──────────────┘

       gamerx-meta-aria  =  gamerx-meta  +  gamerx-aria  (the agent overlay)
```

## Repo → Package map

| Source repo | Builds packages |
|---|---|
| `gamerx-shell` | `gamerx-shell` (configs), `gamerx-theme` (CLI), `gamerx-quickshell` (Quickshell modules) |
| `gamerx-branding` | `gamerx-branding`, `gamerx-grub-theme`, `gamerx-plymouth-theme`, `gamerx-sddm-theme`, `gamerx-hyprlock-theme`, `gamerx-wallpapers`, `gamerx-icon-theme` |
| `gamerx-packages` | `gamerx-keyring`, `gamerx-mirrorlist`, `gamerx-tweaks`, `gamerx-meta`, `gamerx-meta-aria` |
| `gamerx-installer` | `gamerx-calamares-config`, `gamerx-calamares-modules` |
| `gamerx-welcome` | `gamerx-welcome` |
| `gamerx-iso` | builds the ISO (no installable package) |
| `gamerx-repo` | publishes the signed `[gamerx-core]` and `[gamerx-testing]` dbs |

## Boot flow

```
Power on
   │
   ▼
GRUB (themed)  ──→ kernel cmdline picked
   │
   ▼
Linux kernel (linux-zen) + initramfs
   │
   ▼
Plymouth (animated splash)
   │
   ▼
systemd target (graphical.target)
   │
   ▼
SDDM (themed greeter, video/GIF capable)
   │
   ▼  user logs in
Hyprland
   │
   ▼  in parallel
Quickshell (overlay UIs)  ·  Waybar  ·  swaync  ·  swww  ·  hypridle
   │
   ▼  on Aria edition
aria.service starts → SUPER+SPACE overlays Aria
```

## Theme propagation

Every theme change goes through one CLI:

```
gamerx-theme set palette catppuccin-mocha
gamerx-theme set density compact
gamerx-theme set animation snappy
gamerx-theme set launcher mac
gamerx-theme set bar minimal
```

The CLI:
1. Writes to a single state file (`~/.config/gamerx/state.toml`).
2. Renders templates for: Hyprland (`hyprland.conf`), Waybar (`config.jsonc` + CSS), walker (CSS), swaync (CSS), Ghostty (config), Quickshell (theme JSON), GTK (`gtk-3.0/`, `gtk-4.0/`), Qt5/6 (`qt5ct`, `qt6ct`), kvantum, terminal cursors, icon theme, Hyprland window borders.
3. Hot-reloads everything that supports it (Hyprland, Waybar, walker, swaync, GTK).
4. For SDDM/GRUB/Plymouth: writes config and offers to `pacman -S` the matching theme package; takes effect on next boot.

This means **the welcome app and GamerX Settings never write configs directly**; they only call `gamerx-theme`. Users can also use the CLI directly.

## Repo distribution

```
GitHub Releases  ──HTTPS──→  pacman client
   │
   ├─ gamerx-core/x86_64/      ← stable channel (default)
   ├─ gamerx-core/x86_64/*.sig ← detached signatures
   ├─ gamerx-core/x86_64/gamerx-core.db
   ├─ gamerx-testing/x86_64/   ← bleeding edge
   └─ keyring published via `gamerx-keyring` package on first boot
```

Users get the repo by installing GamerX OS — `pacman.conf` ships pre-configured.

## Aria boundary (where it plugs in)

Aria edition adds:
- `/etc/pacman.d/aria.repo` enabling Aria's package repo.
- `aria-daemon.service` (system) + `aria.service` (user) auto-enabled.
- `aria-overlay.qml` Quickshell module loaded on `SUPER+SPACE`.
- A Waybar module on the right side bound to `aria-status` JSON socket.
- D-Bus name `io.gamerx.Aria1` reserved.
- `127.0.0.1:7173` reserved for Aria's local API.

Vanilla edition has all reservations but no daemons running, so installing the Aria
edition later is just `pacman -S gamerx-meta-aria`.
