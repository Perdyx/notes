# GNOME Shell

## Disabling the file indexer

It's basically just a CPU warmer :).

`gsettings set org.freedesktop.Tracker.Miner.Files crawling-interval -2`

## Customizing

### Setting the default terminal emulator

`sudo update-alternatives --config x-terminal-emulator`

### Using the classic shell theme on regular GNOME to match Adwaita light

`mkdir -p ~/.themes/Classic/gnome-shell && cp /usr/share/themes/gnome-shell/theme/gnome-classic.css ~/.themes/Classic/gnome-shell/gnome-shell.css`

### Classic session tweaks

#### Rearranging panel items

In the Gnome Classic session, panel items are fixed by default. This can be changed by simply editing their respective entries in /usr/share/gnome-shell/modes/classic.json. If the extension is not represented here, locate the extension's directory under /usr/share/gnome-shell/extensions and edit its extension.js file. Change the following line, replacing POSITION and ALIGNMENT as desired.

`Main.panel.addToStatusAre( ... POSITION, 'ALIGNMENT');`

#### Disabling the bottom panel

To disable the bottom panel, simply delete or move the [Window List](https://extensions.gnome.org/extension/602/window-list/) extension out of the directory.

`mkdir /usr/share/gnome-shell/extensions/inactive`

`mv /usr/share/gnome-shell/extensions/window-list@gnome-shell-extensions.gcampax.github.com /usr/share/gnome-shell/extensions/inactive`
