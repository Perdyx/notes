# Ubuntu

## Enabling predictable network names

Add the following parameter to the `GRUB_CMDLINE_LINUX` line in /etc/default/grub.

`net.ifnames=0`

Then generate a new grub file and reboot.

`sudo grub-mkconfig -o /boot/grub/grub.cfg`

## Installing stock GNOME on Ubuntu

Ubuntu's default desktop environment and login managers are heavily modified versions of GNOME and GDM. If this is not desirable, you can easily install stock GNOME and GDM over it.

To install stock GNOME, open a terminal and and run

`sudo apt install gnome-session`

To install stock GNOME classic session, run

`sudo apt install gnome-shell-extensions`

For GDM, run the following. When it asks for an option, choose whichever one doesn't mention Ubuntu in the path.

`sudo update-alternatives --config gdm3.css`

### Disabling GNOME's file indexer

It's basically just a CPU warmer :).

`gsettings set org.freedesktop.Tracker.Miner.Files crawling-interval -2`

### Customizing the GNOME shell

##### Setting the default terminal emulator

`sudo update-alternatives --config x-terminal-emulator`

##### Using the classic shell theme on regular GNOME to match Adwaita light

`mkdir -p ~/.themes/light/gnome-shell && cp /usr/share/themes/gnome-shell/theme/gnome-classic.css ~/.themes/light/gnome-shell/gnome-shell.css`

#### Classic

##### Rearranging panel items

In the Gnome Classic session, panel items are fixed by default. This can be changed by simply editing their respective entries in /usr/share/gnome-shell/modes/classic.json. If the extension is not represented here, locate the extension's directory under /usr/share/gnome-shell/extensions and edit its extension.js file. Change the following line, replacing POSITION and ALIGNMENT as desired.

`Main.panel.addToStatusAre( ... POSITION, 'ALIGNMENT');`

##### Disabling the bottom panel

To disable the bottom panel, simply delete or move the [Window List](https://extensions.gnome.org/extension/602/window-list/) extension out of the directory.

`mkdir /usr/share/gnome-shell/extensions/inactive`

`mv /usr/share/gnome-shell/extensions/window-list@gnome-shell-extensions.gcampax.github.com /usr/share/gnome-shell/extensions/inactive`
