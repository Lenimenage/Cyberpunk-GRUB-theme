# Cyberpunk GRUB Theme

A neon yellow / cyan GRUB theme: your wallpaper in the background, a semi-transparent boxed menu in the centre, the [Orbitron](https://fonts.google.com/specimen/Orbitron) font and a highlighted selection bar.

![Preview](docs/preview.jpg)

## Features

- Boot menu inside a semi-transparent cyberpunk frame (75 % opacity, the wallpaper stays visible behind it)
- Orbitron Bold font, neon yellow title and selected entry, cyan entries
- Works with any wallpaper: the installer resizes and crops it for GRUB
- One-command install and uninstall, with an automatic backup of `/etc/default/grub`

## Requirements

- GRUB 2 (Fedora, Debian, Ubuntu and derivatives)
- ImageMagick (`sudo dnf install ImageMagick` / `sudo apt install imagemagick`)
- The **static** `Orbitron-Bold.ttf` from [Google Fonts](https://fonts.google.com/specimen/Orbitron) (inside the `static/` folder of the downloaded zip)

Tested on Fedora (UEFI, dual boot with Windows) at 1920×1080.

## Install

```bash
git clone https://github.com/<your-username>/cyberpunk-grub-theme.git
cd cyberpunk-grub-theme
sudo ./install.sh --font ~/Downloads/Orbitron/static/Orbitron-Bold.ttf --background ~/Pictures/wallpaper.png
```

| Option | Description |
|---|---|
| `--font PATH` | Path to `Orbitron-Bold.ttf` (optional if fontconfig already finds it) |
| `--background PATH` | Your wallpaper, any size (default: plain dark gradient) |
| `--resolution WxH` | Screen resolution, default `1920x1080` |

Then reboot.

## Uninstall

```bash
sudo ./uninstall.sh
```

This restores `/etc/default/grub` from the backup (`/etc/default/grub.bak-cyberpunk`) and removes the theme.

## GRUB configuration

The installer sets these lines in `/etc/default/grub` (see [`grub-config.example`](grub-config.example)):

```ini
GRUB_TERMINAL_OUTPUT="gfxterm"
GRUB_GFXMODE=1920x1080
GRUB_THEME="/boot/grub2/themes/cyberpunk/theme.txt"
```

On Debian/Ubuntu the path is `/boot/grub/themes/cyberpunk/theme.txt`. Optional lines to keep the menu visible longer:

```ini
GRUB_TIMEOUT=10
GRUB_TIMEOUT_STYLE=menu
```

Then regenerate the configuration:

```bash
sudo grub2-mkconfig -o /boot/grub2/grub.cfg    # Fedora
sudo update-grub                               # Debian / Ubuntu
```

## Customisation

Everything lives in [`theme/theme.txt`](theme/theme.txt).

| What | Where |
|---|---|
| Title text | `text = "SELECT SYSTEM"` in the `label` block |
| Entry colour | `item_color` (default `#00f0ff`) |
| Selected entry colour | `selected_item_color` (default `#ffff00`) |
| Menu position and size | `left`, `top`, `width`, `height` of `boot_menu` |
| Frame opacity | Change `rgba(0,0,0,0.75)` in `install.sh` and reinstall |

> **Note:** in a GRUB theme the first component is drawn on top of the next ones. The frame is therefore declared **last** in `theme.txt`, otherwise it hides the text.

## Troubleshooting

- **Plain black and white menu**: `GRUB_TERMINAL_OUTPUT` is still `"console"` (Fedora's default), or `GRUB_THEME` is missing. Check with `grep -E "^GRUB_(TERMINAL|THEME|GFXMODE)" /etc/default/grub`, then regenerate the config.
- **Text in the default font**: the font name in `theme.txt` must match the converted font (`Orbitron Bold 26`). Use the static Bold file, not the variable one.
- **Black screen instead of the wallpaper**: the image is too heavy or not a plain 8-bit PNG. Use `--background` so the installer converts it.
- **Frame hides the menu**: make sure the `image` block is the last one in `theme.txt`.
- **Wrong size on other resolutions**: the frame and fonts are designed for 1920×1080.

## Credits and licences

- Theme files and scripts: MIT, see [LICENSE](LICENSE).
- Orbitron font by Matt McInerney, SIL Open Font License. It is **not** bundled: the installer converts the copy you download yourself.
- No wallpaper is included. Use an image you have the right to use.
Un GRUB personnalisé avec un thème Cyberpunk Edgerunner. dispo sur fedora, debian, ubuntu. 
