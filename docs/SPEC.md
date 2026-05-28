# GamerX OS — v1.0 Specification

This is the authoritative locked spec, derived from the planning Q&A in
`GamerX-OS_Planning.docx`. Every decision here is final unless explicitly amended
in `DECISIONS.md`.

---

## 1. Identity

- **Name**: GamerX OS
- **Tagline**: *The agentic Arch.*
- **Brand**: GamerX (the user's personal brand; not gaming-exclusive)
- **Editions**:
  - **GamerX OS** — vanilla
  - **GamerX OS Aria** — vanilla + Aria pre-wired (built last, on top of vanilla)
- **Versioning**: CalVer + codename, e.g. `26.06 "Nebula"`. ISO filename pattern: `gamerx-os-<edition>-<calver>-<codename>-<arch>.iso`.
- **License**:
  - Code: GPL-3.0
  - Branding/assets: CC-BY-NC-SA 4.0

## 2. Architecture targets

- v1.0: `x86_64`
- v1.1: `x86_64-v3` (CachyOS-style optimized repo as a second mirror)
- v2.0: `aarch64` (deferred)

## 3. Hardware floor

| Edition | Minimum | Recommended |
|---|---|---|
| GamerX OS | 4 GB RAM, 2-core, 64 GB disk | 8 GB RAM, 4-core, 64 GB disk |
| GamerX OS Aria | 8 GB RAM, 4-core, 64 GB disk | 16 GB RAM, 4-core, 128 GB disk |

## 4. Boot stack

- **Bootloader**: GRUB (themed). Custom GamerX theme by default. Theme is swappable from GamerX Settings.
- **Default kernel**: `linux-zen` (highlighted in installer).
- **Selectable kernels in installer**: `linux-zen` (default), `linux-lts` (always installed as safety net), `linux`, `linux-cachyos` (opt-in).
- **Boot splash**: Plymouth with a custom GamerX animated logo.
- **Snapshots**: `snap-pac` installs Btrfs snapshots before every `pacman -Syu`. `grub-btrfs` adds rollback entries to GRUB.

## 5. Filesystem

- **Default**: Btrfs + zstd compression + snapper + grub-btrfs
- **Selectable in installer**: Btrfs (default), ext4
- **Encryption**: LUKS2 toggle in installer (default off)
- **Subvolume layout** (Btrfs): `@`, `@home`, `@var_log`, `@snapshots`, `@cache`

## 6. Display & desktop

- **Display server**: Wayland (Hyprland)
- **Greeter**: SDDM with our custom GamerX theme. Backgrounds: image / short video / GIF, importable by user.
- **Window manager**: Hyprland (latest stable from `[extra]`)
- **Shell**: Hybrid — Quickshell for launcher / control center / OSD / Aria overlay / notification panel; Waybar for the status bar.
- **Lockscreen**: hyprlock (themed to match GamerX brand and SDDM)
- **Notifications**: swaync (multiple style presets)
- **Wallpaper engine**: swww default. mpvpaper auto-loaded only when the user picks a video wallpaper.
- **Launcher**: rofi-wayland (multiple styles: list / grid; blur, transparency configurable)
- **File manager**: Nautilus with selectable icon packs (Papirus default; Tela, Reversal, etc. selectable; user import supported)
- **Terminal**: Ghostty
- **Shells**: fish (default) and zsh both installed; one-click switch via GamerX Settings; both pre-configured with starship

## 7. Theming

- **Theme engine**: matugen (Material You from wallpaper) + curated palettes
- **Bundled palettes**: GamerX Cyber, GamerX Purple (default), Catppuccin Mocha, Tokyo Night, Gruvbox Dark, Rose Pine, Nord
- **Single source of truth**: `gamerx-theme` CLI. Welcome app and GamerX Settings call it; users can call it directly.
- **Density profiles**: Compact, Comfortable (default), Spacious
- **Animation presets**: Snappy, Smooth (default), Cinematic
- **Customizable**: GRUB theme, Plymouth theme, SDDM theme, hyprlock theme, launcher style, bar style, notification style, wallpaper, palette, density, animations, icon pack — all switchable from GamerX Settings.

## 8. Pre-installed software

### Always-on base (in ISO, non-removable)
- Core: `base`, `base-devel`, `linux-firmware`, `networkmanager`, `pipewire`, `pipewire-pulse`, `wireplumber`, `bluez`, `bluez-utils`
- Shell tools: `nano`, `vim`, `fastfetch`, `btop`, `git`, `curl`, `wget`, `unzip`, `zip`, `p7zip`, `tar`, `rsync`, `openssh`, `ufw`
- Modern Unix toolbelt: `eza`, `bat`, `ripgrep`, `fd`, `fzf`, `zoxide`, `lazygit`, `tealdeer`
- Fonts: `noto-fonts`, `noto-fonts-emoji`, `ttf-jetbrains-mono-nerd`, `ttf-firacode-nerd`, `ttf-cascadia-code-nerd`
- Hyprland stack: `hyprland`, `quickshell`, `waybar`, `rofi-wayland`, `swaync`, `swww`, `hyprlock`, `hypridle`, `xdg-desktop-portal-hyprland`, `polkit-kde-agent`
- Theme tools: `matugen`, `papirus-icon-theme`
- Audio control: `pavucontrol`, `pamixer`, `playerctl`
- Display: `brightnessctl`, `wl-clipboard`, `cliphist`, `grim`, `slurp`, `swappy`, `wf-recorder`

### Selectable in installer (GamerX App Bundles)
- **Browsers**: Zen Browser (default), Firefox, Brave, LibreWolf
- **Editors**: VSCodium, Neovim (with our config), Zed
- **Media**: mpv, celluloid, imv, OBS Studio
- **Communications**: Discord, Telegram, Element, Signal
- **Office**: OnlyOffice, LibreOffice
- **Gaming**: Steam, MangoHud, GameMode, Lutris, Heroic, Bottles, Wine, ProtonUp-Qt
- **Creative**: GIMP, Inkscape, Krita, Blender, Audacity, Kdenlive
- **Dev**: Docker, Podman, Postman/Bruno, DBeaver, Insomnia
- **Aria** (Aria edition only): the Aria agent + its repo

## 9. Performance defaults

- `zram-generator` (50% RAM, zstd)
- `ananicy-cpp` with cachyos rules
- `earlyoom` (prevents OOM hangs)
- `gamemode` (auto-applied to launched games)
- `mangohud` (installed, off by default)
- CachyOS sysctl tunings (vm, net, sched) shipped via `gamerx-tweaks` package
- No telemetry

## 10. Custom repository

- **Repos**: `[gamerx-core]` (stable) and `[gamerx-testing]` (dev nightly)
- **Default channel**: `[gamerx-core]`
- **Hosting (v1)**: GitHub Releases on `gamerx-repo` (free, no infra)
- **Migration plan**: Cloudflare R2 + custom domain at v1.1 if scale demands it
- **Signing**: Master GPG key shipped via `gamerx-keyring`. Every package and the db file are signed.

## 11. Installer

- **Engine**: Calamares (heavily re-skinned for modern look)
- **Custom modules**:
  - Kernel picker (multi-select, `linux-zen` highlighted)
  - Browser picker (multi-select, Zen highlighted)
  - App bundles picker (categorized: Editors, Comms, Media, Office, Gaming, Creative, Dev)
  - GPU driver detector (auto-installs the right NVIDIA / AMD / Intel driver)
  - Filesystem picker (Btrfs+snapper default, ext4 selectable)
  - LUKS2 encryption toggle
  - Partition modes: Auto (whole-disk), Replace partition, Manual, Alongside (dual-boot)
  - Language picker (uses Arch's full locale list; English default)
- **Default hostname**: `gamerx-os`
- **Default username**: `gamerx`
- **Default shell for new user**: `fish`

## 12. Onboarding

- **Welcome app** (v1.0): first-boot wizard — palette, density, animation, optional packs, mirror selection, Aria setup tile (linked to Aria repo until shipped).
- **Lightweight tweaker** (v1.0): GTK4 app for switching palette / density / animation / launcher style / bar style / notification style / GRUB theme / SDDM theme / Plymouth theme / icon pack.
- **Full GamerX Settings** (v1.2): native Tauri or GTK4 control center with tons of options.

## 13. Aria reservations

| Domain | Reservation |
|---|---|
| Filesystem | `/opt/aria/`, `/var/lib/aria/`, `/var/log/aria/` |
| User config | `~/.config/aria/`, `~/.cache/aria/`, `~/.local/share/aria/` |
| Systemd | `aria.service` (user) + `aria-daemon.service` (system) + `aria.socket` |
| Hyprland keybind | `SUPER + SPACE` triggers Aria overlay |
| Waybar slot | Module on the right side reserved for Aria status icon |
| D-Bus | `io.gamerx.Aria1` on session bus |
| Local API | `127.0.0.1:7173` |
| Pacman group | `gamerx-aria` meta-package skeleton |
| Welcome app | "Set up Aria" tile |

## 14. Defaults summary table

| Item | Default |
|---|---|
| Hostname | `gamerx-os` |
| Username | `gamerx` |
| Shell | fish |
| Terminal | Ghostty |
| Editor | Neovim |
| Browser | Zen Browser |
| Bootloader | GRUB (themed) |
| Kernel | linux-zen |
| Filesystem | Btrfs + zstd + snapper + grub-btrfs |
| Encryption | Off (toggle in installer) |
| Bar | Waybar |
| Launcher | rofi-wayland |
| Notifications | swaync |
| Wallpaper engine | swww |
| Lockscreen | hyprlock |
| Greeter | SDDM (themed) |
| File manager | Nautilus |
| Icon pack | Papirus (selectable) |
| Update channel | stable (`[gamerx-core]`) |
| Telemetry | off (always; not configurable) |
| Default audience | Advanced + AI-curious users |

## 15. Recovery promises

- `pacman -Syu` always creates a Btrfs snapshot first via `snap-pac`.
- GRUB shows a "rollback" submenu populated by `grub-btrfs`.
- Live ISO doubles as a rescue environment with `chroot` tooling pre-loaded.
- Documentation includes a `chroot` recovery walkthrough at `docs.gamerxos.dev/recovery`.
