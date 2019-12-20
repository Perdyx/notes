# Debian

## Contents

- [Configuring Debian](#configuring-debian)
  - [Setting up sudo](debian-linux-setup.md#setting-up-sudo)
  - [Configuring sources](debian-linux-setup.md#configuring-sources)
  - [Installing wireless drivers](debian-linux-setup.md#installing-wireless-drivers)
  - [Installing net-tools](debian-linux-setup.md#installing-net-tools)
  - [Installing Gnome](debian-linux-setup.md#installing-gnome)

## Configuring Debian

### Setting up sudo

Log into the root user.

`su -c "apt install sudo"`

Give a user sudo privileges, replacing USERNAME with your username.

`su -c "/usr/sbin/usermod -aG sudo USERNAME"`

Log out and back in again for the changes to take effect.

### Configuring Sources

Replace the contents of /etc/apt/sources.list with the following:

```
deb http://deb.debian.org/debian stretch main contrib non-free
deb-src http://deb.debian.org/debian stretch main contrib non-free

deb http://deb.debian.org/debian-security/ stretch/updates main contrib non-free
deb-src http://deb.debian.org/debian-security/ stretch/updates main contrib non-free

deb http://deb.debian.org/debian stretch-updates main contrib non-free
deb-src http://deb.debian.org/debian stretch-updates main contrib non-free
```

Update and upgrade.

`sudo apt update && sudo apt dist-upgrade`

### Installing wireless drivers

`sudo apt install firmware-iwlwifi`

### Installing net-tools

`sudo apt install net-tools`

### Installing Gnome

`sudo apt install gdm3 x-window-system-core`
