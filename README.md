# dwl (my configuration)

<!-- TODO: add rice screenshot -->  

Personal build of **dwl** (a dwm Wayland compositor) with patches and a configuration focused on:

- gaps + smartgaps
- bar
- autostart
- follow (follow windows when sending them to another tag)
- directional swap/focus (vim keys / arrow keys)

> Note: dwl is configured by compiling (like dwm). Changes in `config.h`/`config.def.h` require rebuilding and restarting your Wayland session.

## Included patches

- [follow](https://codeberg.org/dwl/dwl-patches/src/branch/main/patches/follow)
- [swapandfocusdir](https://codeberg.org/dwl/dwl-patches/src/branch/main/patches/swapandfocusdir)
- [autostart](https://codeberg.org/dwl/dwl-patches/src/branch/main/patches/autostart)
- [gaps](https://codeberg.org/dwl/dwl-patches/src/branch/main/patches/gaps)
- [bar](https://codeberg.org/dwl/dwl-patches/src/branch/main/patches/bar)

## Dependencies

### Build dependencies

Dependencies (they vary by distro):

- wayland
- wlroots (in this repo: `wlroots-0.19`)
- libinput
- xkbcommon
- pixman
- fcft
- wayland-protocols (build-time only)
- pkg-config (build-time only)
- wayland-scanner

#### XWayland (optional)

In this repo it is **enabled** in `config.mk` (`XWAYLAND = -DXWAYLAND`). If you want it:

- Xwayland (runtime)
- xcb and xcb-icccm (development/headers)

If you do not want XWayland, comment out `XWAYLAND`/`XLIBS` in `config.mk` and rebuild.

### Runtime dependencies for my config

These programs are launched by keybinds or autostart. You can change them in `config.def.h` if you use alternatives.

- Terminal: `foot`
- File manager: `thunar`
- Launcher: `wmenu-run`
- Browser: `brave`
- Locker: `hyprlock`
- Wallpaper picker: `waypaper`
- Notifications: `mako`
- Clipboard history: `cliphist`
- Clipboard (Wayland): `wl-clipboard` (`wl-copy`, `wl-paste`)
- Menu for choosing clipboard entries: `rofi` (used by the cliphist command)
- Logout menu: `wlogout`

## Install

### 1) Build

```sh
make
```

If you changed `config.def.h` and `config.h` already exists, it is usually best to force regeneration and rebuild:

```sh
rm -f config.h
make clean
make
```

### 2) Install

By default it installs into `/usr/local` (see `config.mk`):

```sh
sudo make install
```

To install into `/usr`:

```sh
sudo make PREFIX=/usr install
```

This installs:

- binary: `PREFIX/bin/dwl`
- manpage: `PREFIX/share/man/man1/dwl.1`
- session file: `PREFIX/share/wayland-sessions/dwl.desktop`

### 3) Run

- From a display manager (GDM/SDDM/ly/etc): choose the **dwl** session.
- From a TTY: run `dwl` (make sure logind/seatd is set up for your distro).

## Configuration overview

### Appearance

- `sloppyfocus = 1` (focus follows mouse)
- gaps enabled (`gaps = 1`) with `gappx = 4`
- `smartgaps = 1` (no outer gap when there is only one window)
- `borderpx = 1`
- bar enabled (`showbar = 1`) on top (`topbar = 1`)
- font: `monospace:size=10`

### Autostart

Launched at startup (autostart patch):

- `mako`
- `swww-daemon &`
- `wl-paste --watch cliphist store`

Note: autostart executes programs directly (no shell). If something relies on `~` expansion or environment variables, use an absolute path or wrap it with `sh -c`.

## Keybinds

Mod = Windows/Super key (`WLR_MODIFIER_LOGO`).

### Apps

- Mod+Enter: terminal (`foot`)
- Mod+e: file manager (`thunar`)
- Mod+d: launcher (`wmenu-run`)
- Mod+b: browser (`brave`)
- Mod+Ctrl+Shift+l: lock (`hyprlock`)
- Mod+Shift+w: wallpapers (`waypaper`)
- Mod+v: clipboard menu (cliphist + rofi + wl-copy)
- Mod+Shift+p: logout (`wlogout`)

### Window management

- Mod+q: close focused window (kill)
- Mod+m: quit dwl
- Mod+Space: toggle floating
- Mod+f: toggle fullscreen
- Mod+x: toggle gaps
- Mod+Shift+x: toggle bar

### Layouts

- Mod+t: tiled (`[]=`)
- Mod+g: floating (`><>`)
- Mod+p: monocle (`[M]`)

### Directional focus

- Mod+h/j/k/l: focus left/down/up/right
- Mod+←/↓/↑/→: focus using arrow keys

### Directional swap

- Mod+Shift+h/j/k/l: swap left/down/up/right
- Mod+Shift+←/↓/↑/→: swap using arrow keys

### Master size

- Mod+Ctrl+h: master -5%
- Mod+Ctrl+l: master +5%
- Mod+Ctrl+←: master -5%
- Mod+Ctrl+→: master +5%

### Promote to master

- Mod+Shift+Enter: zoom (promote to master / swap with top)

### Tags

- Mod+[1-9]: view tag
- Mod+Ctrl+[1-9]: toggle view tag
- Mod+Shift+[1-9]: move window to tag
- Mod+Ctrl+Shift+[1-9]: toggle tag on window
- Mod+Tab: view previous selection

### Monitors

- Mod+,: focus left monitor
- Mod+.: focus right monitor
- Mod+Shift+<: move window to left monitor
- Mod+Shift+>: move window to right monitor

### TTY

- Ctrl+Alt+XF86Switch_VT_[1-12]: switch VT
- Ctrl+Alt+Terminate_Server: quit

## Mouse bindings

- Click layout symbol (bar):
  - Left: tiled
  - Right: monocle
- Middle click on title (bar): zoom
- Middle click on status (bar): terminal
- Mod + drag Left on client: move (sets floating)
- Mod + Middle on client: toggle floating
- Mod + drag Right on client: resize (sets floating)
- Click tagbar:
  - Left: view
  - Right: toggle view
  - Mod+Left: tag
  - Mod+Right: toggle tag
