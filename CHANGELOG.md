# Changelog

## [2.0.2] - 2026-03-29

### Fixed
- **Window height overcalculation in left layout** — auto-size formula applied the top header height (115px) even in `layout: "left"` mode where no top header exists; header contribution is now 32px (outer margins only) when sidebar layout is active, preventing the excess empty space at the bottom of the window

---

## [2.0.1] - 2026-03-29

### Fixed
- **Duplicate disk entries** — devices mounted at multiple points (e.g. btrfs subvolumes) no longer appear as separate rows; deduplication is based on the block device name
- **Window height clipping** — replaced hardcoded height with dynamic calculation based on actual content (sections + rows); caps at 90% of monitor height using `Gdk.Display` instead of deprecated `Gdk.Screen`
- **First label auto-highlighted on open** — GTK's automatic focus on the first selectable widget caused the first value (e.g. Uptime) to appear highlighted on launch; fixed with `set_can_focus(False)` after `set_selectable(True)`

---

## [2.0] - 2026-03-18

### Added
- **`--config <path>` CLI flag** — load any config file directly; `extends` is ignored in this mode
- **`extends` key in config** — point to a theme file that wins over your personal config
  - Load order: `DEFAULTS` → `config.json` → `extends file` (extended file has highest priority)
  - One level only — `extends` inside an extended file is ignored
  - Designed for theme switching: keep personal settings in `config.json`, swap themes via `extends`
- **Layout system** — `layout: "top"` (classic) or `layout: "left"` (vertical sidebar)
  - Sidebar shows OS name and date rotated 90°, logo at top or bottom (`logo.sidebar_position`)
  - Sidebar hides locale string for a cleaner look
- **Animation system** — live background animations rendered via Cairo
  - `rain` — diagonal streaks falling at 15°, configurable color, speed, drop count
  - `snow` — circular flakes with sinusoidal horizontal drift
  - `matrix` — falling half-width katakana columns with bright head and fading trail
  - `aurora` — undulating sine bands using accent, hi, and ok palette colors
  - `warp` — stars radiating from center, accelerating outward (starfield effect)
  - `scope` param: `"body"` (below header only) or `"full"` (entire window)
- **Gradient bar** — animated 2-color scrolling bar replacing the header separator
  - Horizontal in top layout, vertical in left layout
  - Configurable colors, disabled by default
- **Background image overlay** — any image rendered at bottom-right corner
  - Scales source image to configured size
  - Adjustable opacity, rendered above content but behind nothing (Overlay stack)
  - `bg_image.enabled` flag — set to `false` to disable completely (no image, no distro logo fallback)
- **Theme files** — ready-made themes in `themes/` folder (21 included)
  - Each file is fully self-contained: all config keys explicitly defined, no ambiguity
  - Use with `--config` for direct testing or point `extends` to activate
- Added `python-cairo` as explicit dependency

### Changed
- Window now uses `Gtk.Overlay` stack: base content → animation → background image
- Body content uses its own `Gtk.Overlay` to allow scoped animations
- `DEFAULTS` in script is now the single source of truth for all config keys

### Multi-distro improvements
- **Session** — reads `XDG_CURRENT_DESKTOP`, `XDG_SESSION_DESKTOP`, `DESKTOP_SESSION` with session type; no longer Hyprland-only
- **Display** — cascade: `hyprctl` → `xrandr` → `wlr-randr` → `$DISPLAY`/`$WAYLAND_DISPLAY`
- **GPU** — removed hardcoded `[Discrete]` label; detects 3D controller as fallback
- **Shell** — reads `$SHELL` from environment instead of assuming fish
- **Terminal** — detects kitty, alacritty, wezterm, foot, xfce4-terminal, konsole, gnome-terminal, tilix; falls back to `$TERM_PROGRAM` / `$TERM`
- **Term Font** — reads kitty, alacritty (toml/yml), foot, wezterm configs
- **Packages** — detects pacman, dpkg, rpm, apk, flatpak, snap
- **OS Age** — reads pacman, dpkg, dnf, zypper, yum logs in order
- **Login** — uses `$USER`/`$LOGNAME` + `tty`; GUI sessions show `user@localhost`
- **bg_image** — falls back to distro logo from Papirus when no path is set

---

## [1.1] - 2026-03-17

### Added
- Full config file system via `~/.config/vsfetch/config.json`
  - `palette` — 14 color keys, full UI customization (defaults to Catppuccin Mocha)
  - `font` — font family override
  - `font_sizes` — independent sizes for `title`, `body`, and `small` text
  - `logo.size` — header logo pixel size
  - `logo.override` — custom logo: file path or icon theme name
  - `default_mode` — `"full"`, `"mini"`, or `"version"` (CLI flags still take priority)
  - `sections` — visible sections and display order for full mode
  - `dev_tools` — fully customizable development tools list (icon, name, command)
  - `chassis` — manual override for chassis type detection
  - `window` — width and height per mode
  - Partial configs supported — only specified keys override defaults
- Chassis auto-detection from `/sys/class/dmi/id/chassis_type`
- `config.json` example file in repo (installed to `/usr/share/doc/vsfetch-git/config.json.example`)

### Changed
- WM class changed from `arch-about` to `vsfetch`
- Header logo rendered directly on header background (no wrapper box or border radius)
- Header text vertically centered relative to logo
- Header text left padding increased (+15px)
- Logo padding added (5px all sides)

### Fixed
- Replaced deprecated `Gtk.Window.set_wmclass()` with `GLib.set_prgname()`
- Fixed invalid Python escape sequences causing `SyntaxWarning` on Python 3.12+

---

## [1.0] - 2026-03-16

### Initial release
- GTK3 "About This Computer" panel for Arch Linux / Hyprland
- Sections: Hardware, Desktop, Terminal, Development, Uptime
- `--mini` and `--version` modes
- Auto-detects OS and loads Papirus distributor logo
- Color-coded disk/RAM usage
