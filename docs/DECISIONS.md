# GamerX OS — Decision Log

A running log of every locked decision and why we picked it.
Append-only. New decisions go at the bottom; revisions reference and supersede previous entries.

---

## D-001 · Project name & identity

- **Name**: GamerX OS
- **Tagline**: *The agentic Arch.*
- **Why**: Brand-led, owned by the user; the tagline owns the AI angle without sounding gimmicky.
- **Date**: 2026-05-28

## D-002 · Two editions, vanilla first

- **Decision**: Build GamerX OS (vanilla) first; GamerX OS Aria is the same ISO with one extra repo and a few systemd units flipped on.
- **Why**: Aria is a complete project of its own; the OS layer should not block on it.

## D-003 · Versioning

- **Format**: CalVer + codename (e.g. `26.06 "Nebula"`).
- **Why**: Date in the filename for engineers; codename for marketing and theme story.

## D-004 · Architecture targets

- **v1.0**: x86_64
- **v1.1**: x86_64-v3 (CachyOS-style optimized repo)
- **v2.0**: aarch64 (deferred)

## D-005 · Bootloader

- **Pick**: GRUB (themed)
- **Why**: User explicitly asked for themable GRUB. GRUB also pairs cleanly with `grub-btrfs` for snapshot rollback.

## D-006 · Default kernel

- **Pick**: linux-zen (default in installer, highlighted)
- **Alternates installed by default**: linux-lts (always, safety net)
- **Optional**: linux, linux-cachyos (selectable in installer)

## D-007 · Filesystem

- **Default**: Btrfs + zstd + snapper + grub-btrfs
- **Alternative offered**: ext4
- **snap-pac**: yes (snapshots before every `pacman -Syu`)
- **Encryption**: LUKS2 toggle, default off

## D-008 · Display stack

- Hyprland + Quickshell (overlays, control center, launcher, OSD) + Waybar (status bar)
- SDDM (themed greeter) + hyprlock + swaync + swww + walker

## D-009 · Terminal & shell

- Ghostty as the terminal
- fish as the default user shell (autosuggestions, syntax highlighting OOTB)
- zsh installed and one-click switchable
- starship configured for both

## D-010 · Hyprdots strategy

- **Pick**: Build our own Quickshell-based shell from clean room.
- **Why**: User asked for professionalism, not a fork or rebrand. We may study top dotfiles for technique but we ship original code.

## D-011 · Theme single source of truth

- **Pick**: A single `gamerx-theme` CLI; all UIs (Welcome, Tweaker, GamerX Settings) call it.
- **Why**: Consistency, scriptability, makes Aria's "switch to Tokyo Night" trivial.

## D-012 · Repo strategy

- **Pick**: Split repos under `GamerXECO-sys55` user account.
- **Repos**: `gamerx-os`, `gamerx-iso`, `gamerx-shell`, `gamerx-packages`, `gamerx-installer`, `gamerx-branding`, `gamerx-welcome`, `gamerx-repo`, `gamerx-docs`.
- **Why**: User explicitly preferred split repos; easier to manage ownership and CI per concern.

## D-013 · Repo channels

- `[gamerx-core]` (stable) and `[gamerx-testing]` (dev)
- Default channel: `[gamerx-core]`

## D-014 · Repo hosting (v1)

- GitHub Releases on `gamerx-repo`. Migrate to Cloudflare R2 + custom domain at v1.1 if needed.

## D-015 · Defaults

- Hostname: `gamerx-os`
- Username: `gamerx`
- Default user shell: fish

## D-016 · Telemetry

- **Strictly zero**, not configurable.

## D-017 · License

- Code: GPL-3.0
- Branding/assets: CC-BY-NC-SA 4.0

## D-018 · Asset generation strategy

- v0 assets generated programmatically (SVG/code) by the build agent.
- High-quality assets later, generated via Midjourney / Stable Diffusion / Flux by the user or Aria.
- Placeholders explicitly marked in the repo.

## D-019 · Performance defaults

- zram + ananicy-cpp (cachyos rules) + earlyoom + GameMode + CachyOS sysctls
- MangoHud installed but off by default.

## D-020 · Welcome app & Settings scope

- v1.0: Welcome app + lightweight GTK4 tweaker
- v1.2: Native GamerX Settings (Tauri or GTK4) with full customization

## D-021 · Aria reservations approved

- Filesystem, user config, systemd, keybind, Waybar slot, D-Bus name, port 7173, meta package, welcome tile — all approved as drafted in the planning doc.

## D-022 · Recovery

- snap-pac auto-snapshots
- grub-btrfs rollback entries
- live ISO doubles as rescue with chroot tools

## D-023 · Localization

- Use Arch's full locale list; English default; user picks at install in Calamares.

## D-024 · Update channel default

- New users default to **stable** (`[gamerx-core]`).


## D-025 · GPG signing identity (P2)

- Master key UID: `GamerX OS Repository Signing Key (https://github.com/GamerXECO-sys55/gamerx-os) <gamerx-os@gamerxeco-sys55.users.noreply.github.com>`
- Master fingerprint: `3C7C0F0E E2EC008D 417FA2D6 4354EA4A AA4736D3`
- Master key type: RSA-4096, no expiration, kept at `~/.config/gamerx-os/gpg/` (chmod 700)
- Signing subkey: RSA-4096, no expiration. Will be exported to CI for automated signing in P5.
- Date: 2026-05-28

## D-026 · Mirror hosting URL pattern (P2)

- Pacman mirrorlist points at:
  `https://github.com/GamerXECO-sys55/gamerx-repo/releases/download/$repo-$arch`
- This means each `[gamerx-core]` and `[gamerx-testing]` channel maps to a GitHub release tag named `core-x86_64` / `testing-x86_64`.
- Decision: keep tag names channel-arch pattern so we can add architectures cleanly later.

## D-027 · Theme CLI design (P2)

- Implemented in Python 3 (no compile step, easy for users to read and modify).
- State stored at `~/.config/gamerx/state.toml`.
- Renderers live under `/usr/share/gamerx-theme/renderers/`, shipped by `gamerx-shell` in P3 — keeps the CLI package tiny and decouples the theme engine from individual component configs.
- Why Python (not Rust/Go): the CLI is glue, not a hot loop. Python ships in `gamerx-meta` already. Trades a 50ms startup for orders of magnitude less code to maintain.

## D-028 · Meta package philosophy (P2)

- `gamerx-meta` lists ~110 packages explicitly. Yes, namcap warns "Dependency included, but may not be needed" for every one — that's the whole point of a meta package.
- Decision: do not split into sub-meta-packages (e.g. `gamerx-base`, `gamerx-desktop`, `gamerx-cli-tools`). The flat list is easier to audit and the warning is documented as expected.


## D-029 · Renderers source `_lib.sh` relatively (P3)

- Every renderer uses `. "$(dirname "$0")/_lib.sh"` instead of a hardcoded absolute path.
- Reason: makes renderers usable from any install prefix (system, test harness, future containerized builds) without modification. The hardcoded path was failing in the first e2e test run; relative sourcing is more robust and equally correct under the official `/usr/share/gamerx-theme/renderers/` install path.

## D-030 · gamerx-shell PKGBUILD dual-mode (P3)

- The PKGBUILD honors `GAMERX_LOCAL_SRC` env var. When set, it copies from that path; when not, it does the standard `git+https://github.com/...` clone.
- Reason: lets us iterate the shell repo and the PKGBUILD simultaneously without round-tripping through GitHub on every change. Identical contract to CachyOS's local-build helpers.

## D-031 · Quickshell + Waybar split (P3, formal)

- Quickshell hosts: ControlCenter, PowerMenu, OSD, ThemeLoader, future Aria overlay.
- Waybar hosts: status bar (and only the status bar).
- Communication path: `qs ipc call gamerx <handle>` invoked from Hyprland keybinds and Waybar buttons.
- Theme propagation: every component reads `~/.config/gamerx/theme.json` written by `10-palette.sh`. Quickshell hot-reloads via `ThemeLoader` (FileView with watchChanges). Waybar/walker/swaync get symlinks swapped + hot-reload signals.


## D-032 · P5 publish pipeline (manual for v1.0)

- `scripts/sign_and_publish.sh` is the source of truth. Steps: sign every .pkg.tar.zst in /home/gamerx/GamerX-OS/build/x86_64/, run `repo-add --sign --key <key>` to build gamerx-core.db.tar.zst, find-or-create the GitHub release `gamerx-core-x86_64` on `gamerx-repo`, delete-and-replace every asset (idempotent re-runs), upload `<plain>.db` and `<plain>.files` aliases for pacman compatibility.
- Verified end-to-end: `sudo pacman --config <test.conf> -Sy && pacman -Sl gamerx-core` returns all 11 packages over the public URL.
- CI automation deferred to P9. Manual is fine for v1.0 cadence.

## D-033 · GitHub Release as pacman repo

- Release tag pattern: `<repo>-<arch>` e.g. `gamerx-core-x86_64`, `gamerx-testing-x86_64`.
- Pacman fetches db via plain HTTPS using the URL pattern in `gamerx-mirrorlist`.
- We upload BOTH `<repo>.db.tar.zst` and the alias `<repo>.db` (same content, different filename) because pacman tries `.db` first by historical convention.
- Same applies to `.files` and `.sig` siblings.