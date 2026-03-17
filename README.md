# vsFetch

A graphical system info viewer for Linux — inspired by `fastfetch`, built with Python + GTK3.
Think of it as an "About This Computer" panel for your desktop.

[![AUR version](https://img.shields.io/aur/version/vsfetch-git?label=AUR&logo=archlinux)](https://aur.archlinux.org/packages/vsfetch-git)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

![vsFetch screenshot](screenshots/1.png)

---

## Features

- Displays system info in sections: **Hardware, Desktop, Terminal, Development, Uptime**
- Auto-detects OS and loads the matching distributor logo (via Papirus icon theme)
- Color-coded disk/RAM usage (green / yellow / red)
- `--mini` mode: shows only the header + Hardware section
- `--version` mode: About panel with author info and links

---

## Usage

```bash
vsfetch             # full view
vsfetch --mini      # hardware only
vsfetch --version   # about / credits
```

---

## Requirements

- Python 3
- GTK3 (`python-gobject`)
- `hyprctl` (for Hyprland monitor info)
- [Papirus Icon Theme](https://github.com/PapirusDevelopmentTeam/papirus-icon-theme) (optional, for OS logo)
- [JetBrainsMono Nerd Font](https://www.nerdfonts.com/) (optional, for icons in labels)

### Arch Linux

```bash
sudo pacman -S python-gobject papirus-icon-theme ttf-jetbrains-mono-nerd
```

### Ubuntu / Debian

```bash
sudo apt install python3-gi python3-gi-cairo gir1.2-gtk-3.0 papirus-icon-theme
```

> For Nerd Fonts on Ubuntu, download manually from [nerdfonts.com](https://www.nerdfonts.com/font-downloads) and place in `~/.local/share/fonts/`, then run `fc-cache -fv`.

### Fedora

```bash
sudo dnf install python3-gobject gtk3 papirus-icon-theme
```

> For Nerd Fonts on Fedora:
> ```bash
> sudo dnf install jetbrains-mono-fonts-all
> ```
> Then install the Nerd Font variant manually from [nerdfonts.com](https://www.nerdfonts.com/font-downloads).

---

## Install

### Arch Linux — AUR

```bash
yay -S vsfetch-git
```

Or manually with any AUR helper:
```bash
paru -S vsfetch-git
```

### Manual

```bash
git clone https://github.com/victorsosaMx/vsFetch.git
cd vsFetch
chmod +x vsfetch
cp vsfetch ~/.local/bin/vsfetch
```

### Hyprland — float window rule (optional)

Add to your `~/.config/hypr/modules/rules.conf`:

```
windowrule {
    name = vsfetch
    match:class = vsfetch
    float = yes
    size = 700 700
    center = yes
}
```

---

## Configuration

vsFetch reads `~/.config/vsfetch/config.json` on startup. If the file doesn't exist, built-in defaults (Catppuccin Mocha) are used. **Partial configs are supported** — only the keys you specify override the defaults.

```bash
mkdir -p ~/.config/vsfetch
cp /usr/share/doc/vsfetch-git/config.json.example ~/.config/vsfetch/config.json
```

### Reference

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| `font` | string | `"JetBrainsMono Nerd Font"` | UI font family |
| `default_mode` | string | `"full"` | Startup mode: `"full"`, `"mini"`, `"version"`. CLI flags override this. |
| `chassis` | string | `""` | Override chassis label. Empty = auto-detect from DMI. |

#### `palette`

| Key | Default | Used in |
|-----|---------|---------|
| `bg` | `#13131c` | Window background |
| `bg_header` | `#181825` | Header background |
| `border` | `#313244` | Separators |
| `text` | `#cdd6f4` | Primary text, values |
| `text_dim` | `#6c7086` | Secondary text, timestamps |
| `text_mid` | `#a6adc8` | Tertiary text |
| `accent` | `#89b4fa` | Section titles, icons, keys |
| `ok` | `#a6e3a1` | Usage < 50% |
| `mid` | `#f9e2af` | Usage 50–79% |
| `hi` | `#f38ba8` | Usage ≥ 80% |
| `btn_github` | `#238636` | GitHub button |
| `btn_github_hover` | `#2ea043` | GitHub button hover |
| `btn_web` | `#1d6fa5` | Web button |
| `btn_web_hover` | `#2484c1` | Web button hover |

#### `font_sizes`

| Key | Default | Used in |
|-----|---------|---------|
| `title` | `20` | OS name in header |
| `body` | `13` | Keys, values, icons |
| `small` | `11` | Section labels, timestamps |

#### `logo`

| Key | Default | Description |
|-----|---------|-------------|
| `size` | `56` | Logo pixel size in header |
| `override` | `""` | File path (`~/logo.png`) or icon theme name. Empty = auto-detect by OS. |

#### `window`

| Key | Default | Description |
|-----|---------|-------------|
| `width` | `700` | Window width |
| `height_mini` | `380` | Height in mini mode |
| `height_full` | `680` | Height in full mode |

#### `sections`

Controls which sections are visible and their order in full mode. Mini mode always shows Hardware only.

```json
"sections": ["hardware", "desktop", "terminal", "development", "uptime"]
```

Available values: `hardware`, `desktop`, `terminal`, `development`, `uptime`

#### `dev_tools`

List of tools shown in the Development section. Each entry runs a shell command — if it returns empty the tool is hidden.

```json
"dev_tools": [
  { "icon": "󱘗", "name": "Rust",   "cmd": "rustc --version 2>/dev/null | awk '{print $2}'" },
  { "icon": "󰊢", "name": "Git",    "cmd": "git --version 2>/dev/null | awk '{print $3}'" },
  { "icon": "󰌠", "name": "Python", "cmd": "python3 --version 2>/dev/null | awk '{print $2}'" }
]
```

---

## Credits

- **[fastfetch](https://github.com/fastfetch-cli/fastfetch)** — inspiration for the layout and system info approach
- **[Papirus Icon Theme](https://github.com/PapirusDevelopmentTeam/papirus-icon-theme)** by Alexey Varfolomeev — distributor logos used in the header
- **[Catppuccin Mocha](https://github.com/catppuccin/catppuccin)** — color palette used throughout the UI
- **[Nerd Fonts](https://www.nerdfonts.com/)** — icons used in section labels

---

## License

This project is licensed under the **MIT License** — see the [LICENSE](LICENSE) file for details.

**In short:** You're free to use, modify, and distribute this software for any purpose (commercial or personal), as long as you include the original license and copyright notice.

---

## Author

**Víctor Sosa**
🌐 [victorsosa.com](https://victorsosa.com/)
🐙 [github.com/victorsosaMx](https://github.com/victorsosaMx)
