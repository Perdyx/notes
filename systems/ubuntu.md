# Ubuntu

## Contents

- [Installing stock GNOME on Ubuntu](#installing-stock-gnome-on-ubuntu)
  - [Customizing the GNOME shell](#customizing-the-gnome-shell)
    - [Setting the default terminal emulator](#setting-the-default-terminal-emulator)
    - [Classic](#classic)
      - [Rearranging panel items](#rearranging-panel-items)
      - [Disabling the bottom panel](#disabling-the-bottom-panel)

## Installing stock GNOME on Ubuntu

Ubuntu's default desktop environment and login managers are heavily modified versions of GNOME and GDM. If this is not desirable, you can easily install stock GNOME and GDM over it.

To install stock GNOME, open a terminal and and run

`sudo apt install gnome-session`

To install stock GNOME classic session, run

`sudo apt install gnome-shell-extensions`

For GDM, run the following. When it asks for an option, choose whichever one doesn't mention Ubuntu in the path.

`sudo update-alternatives --config gdm3.css`

### Customizing the GNOME shell

##### Setting the default terminal emulator

`sudo update-alternatives --config x-terminal-emulator`

#### Classic

##### Rearranging panel items

In the Gnome Classic session, panel items are fixed by default. This can be changed by simply editing their respective entries in /usr/share/gnome-shell/modes/classic.json. If the extension is not represented here, locate the extension's directory under /usr/share/gnome-shell/extensions and edit its extension.js file. Change the following line, replacing POSITION and ALIGNMENT as desired.

`Main.panel.addToStatusAre( ... POSITION, 'ALIGNMENT');`

##### Disabling the bottom panel

To disable the bottom panel, simply delete or move the [Window List](https://extensions.gnome.org/extension/602/window-list/) extension out of the directory.

`mkdir /usr/share/gnome-shell/extensions/inactive`

`mv /usr/share/gnome-shell/extensions/window-list@gnome-shell-extensions.gcampax.github.com /usr/share/gnome-shell/extensions/inactive`
