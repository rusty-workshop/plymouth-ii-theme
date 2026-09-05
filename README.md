# ii Plymouth theme

A Material You-styled Plymouth boot theme: flat surfaces, a rotating
arc spinner, a rounded icon badge. Colors are pulled from the same
generated Material palette Quickshell's `illogical-impulse` dots use
(`~/.local/state/quickshell/user/generated/colors.json`), so it can be
kept in sync with whatever wallpaper is active.

## Install

```bash
sudo cp -r . /usr/share/plymouth/themes/ii/
sudo plymouth-set-default-theme -R ii
```

Takes effect on the next reboot.

## Keeping it in sync with your wallpaper

`scripts/plymouth-theme-sync` regenerates the theme's assets from the
current palette and reinstalls it (skipping the work if nothing
actually changed). It's meant to be triggered automatically -- see the
`PlymouthThemeSync.qml` service in the `dots-hyprland` fork, which
watches `colors.json` and runs this on change.

Since it runs from a background process with nobody around to type a
password, `scripts/plymouth-theme-install` (the root half: copy +
initramfs rebuild) needs a scoped passwordless sudo rule:

```bash
sudo EDITOR=nano visudo -f /etc/sudoers.d/plymouth-theme-sync
```

```
<user> ALL=(root) NOPASSWD: /path/to/plymouth-theme-install
```

Both scripts hardcode absolute paths (`$HOME`-relative where possible,
but the sudoers rule itself requires a fixed path) -- adjust them if
you're using this on a different machine/user.

## Preview without rebooting

Plymouth needs the display to itself, which conflicts with a running
Wayland/X session. Switch to an unused VT first:

```bash
sudo plymouthd --tty=/dev/tty3 --debug
sudo plymouth --show-splash
# ... look around ...
sudo plymouth quit
sudo pkill plymouthd
```
