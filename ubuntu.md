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