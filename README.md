<div align="center">

# GamerX OS

**The agentic Arch.**

An Arch-based, Hyprland-first, agent-ready operating system.

[![License](https://img.shields.io/badge/license-GPL--3.0-00D9FF.svg)](LICENSE)
[![Status](https://img.shields.io/badge/status-pre--alpha-FF2EC4.svg)](#status)
[![Base](https://img.shields.io/badge/base-Arch%20Linux-1793D1.svg)](https://archlinux.org)
[![Desktop](https://img.shields.io/badge/desktop-Hyprland-7C3AED.svg)](https://hyprland.org)

</div>

---

## What is GamerX OS

GamerX OS is a custom Arch-based Linux distribution with a Hyprland desktop, deep theming and customization, and reserved integration points for the **Aria** agent assistant.

This is the **meta repo**. It carries the spec, decision log, roadmap, and links to every sub-project. The actual code, packages, and assets live in their own repos.

## Sub-projects

| Repo | Purpose |
|---|---|
| [gamerx-iso](https://github.com/GamerXECO-sys55/gamerx-iso) | archiso profile that builds the bootable ISO |
| [gamerx-shell](https://github.com/GamerXECO-sys55/gamerx-shell) | Hyprland desktop shell (Quickshell + Waybar + dotfiles + theme engine) |
| [gamerx-packages](https://github.com/GamerXECO-sys55/gamerx-packages) | Source PKGBUILDs for the `[gamerx-core]` and `[gamerx-testing]` repos |
| [gamerx-installer](https://github.com/GamerXECO-sys55/gamerx-installer) | Calamares branding and custom modules |
| [gamerx-branding](https://github.com/GamerXECO-sys55/gamerx-branding) | Logos, wallpapers, GRUB / Plymouth / SDDM / hyprlock themes |
| [gamerx-welcome](https://github.com/GamerXECO-sys55/gamerx-welcome) | First-boot welcome app + lightweight tweaker |
| [gamerx-repo](https://github.com/GamerXECO-sys55/gamerx-repo) | Built `.pkg.tar.zst` artifacts and signed pacman db |
| [gamerx-docs](https://github.com/GamerXECO-sys55/gamerx-docs) | User and developer documentation (mkdocs-material) |

## Editions

- **GamerX OS** — vanilla Hyprland distribution.
- **GamerX OS Aria** — vanilla + the [Aria](https://github.com/GamerX3560/Aria-V11) agent pre-wired and auto-started. Same ISO build path; Aria edition adds one extra repo and a few systemd units.

## Status

| Phase | Description | State |
|---|---|---|
| P0 | Housekeeping, repo skeleton | ✅ done |
| P1 | Branding & assets v0 | 🛠 in progress |
| P2 | Core packages (keyring, mirrorlist, branding, meta) | ⏳ |
| P3 | gamerx-shell (Quickshell + Waybar + theme engine) | ⏳ |
| P4 | GRUB / Plymouth / SDDM / hyprlock themes | ⏳ |
| P5 | Repo signing and publishing | ⏳ |
| P6 | archiso profile | ⏳ |
| P7 | Calamares custom modules | ⏳ |
| P8 | Welcome app + tweaker | ⏳ |
| P9 | Docs site + release CI | ⏳ |
| P10 | Aria edition + full GamerX Settings (v1.2) | ⏳ |

## Quick links

- 📋 [Specification](docs/SPEC.md) — locked decisions
- 🛣 [Roadmap](docs/ROADMAP.md) — phase plan
- 📐 [Architecture](docs/ARCHITECTURE.md) — system layout
- 📝 [Decision log](docs/DECISIONS.md) — why we picked what we picked
- 📄 [Planning doc (DOCX)](docs/GamerX-OS_Planning.docx) — original Q&A

## License

- Code: [GPL-3.0](LICENSE)
- Branding assets: CC-BY-NC-SA 4.0 (kept commercial rights to the GamerX brand)

## Credits

Built on [Arch Linux](https://archlinux.org), [Hyprland](https://hyprland.org), [Quickshell](https://quickshell.outfoxxed.me), [Calamares](https://calamares.io), and the work of countless upstream maintainers we respect.
