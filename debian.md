# Debian

## Configuring

### Setting up sudo

Log into the root user.

`su -c "apt install sudo"`

Give a user sudo privileges, replacing USERNAME with your username.

`su -c "/sbin/adduser USERNAME sudo"`

Log out and back in again for the changes to take effect.

### Configuring Sources

Replace the contents of /etc/apt/sources.list with the following:

```
deb http://deb.debian.org/debian buster main contrib non-free
deb-src http://deb.debian.org/debian buster main contrib non-free

deb http://deb.debian.org/debian-security/ buster/updates main contrib non-free
deb-src http://deb.debian.org/debian-security/ buster/updates main contrib non-free

deb http://deb.debian.org/debian buster-updates main contrib non-free
deb-src http://deb.debian.org/debian buster-updates main contrib non-free
```

Update and upgrade.

`sudo apt update && sudo apt dist-upgrade`

### Installing wireless drivers

`sudo apt install firmware-iwlwifi`

### Installing net-tools

`sudo apt install net-tools`

### Installing Gnome

`sudo apt install gdm3 x-window-system-core`

### Setting CPU power management

View current setting:

`cat /sys/devices/system/cpu/cpufreq/policy*/energy_performance_preference`

Set it to performance:

`echo performance | sudo tee /sys/devices/system/cpu/cpufreq/policy*/energy_performance_preference`
