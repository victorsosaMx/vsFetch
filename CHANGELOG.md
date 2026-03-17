# Changelog

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
