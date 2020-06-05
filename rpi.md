# Raspberry Pi

## Contents

- [Installation](#installation)
- [Connecting via wifi](#connecting-via-wifi)
- [Setting up virtual network interfaces](#setting-up-virtual-network-interfaces)
- [Setting up an access point (Raspberry Pi only)](#setting-up-an-access-point-raspberry-pi-only)
  - [Install hostapd and dnsmasq](#install-hostapd-and-dnsmasq)
  - [Configure the interfaces](#configure-the-interfaces)
  - [Configure hostapd](#configure-hostapd)
    - [Disabling SSID broadcast](#disabling-ssid-broadcast)
    - [Enabling MAC filtering](#enabling-mac-filtering)
  - [Configure dnsmasq](#configure-dnsmasq)
  - [Set up IPv4 forwarding](#set-up-ipv4-forwarding)
  - [Configure iptables](#configure-iptables)
  - [Enable hostapd and dnsmasq to run at boot](#configure-hostapd-and-dnsmasq-to-run-at-boot)

## Installation

If you are installing a variant of Raspbian on the Pi Zero/W, you can enable SSH before the first boot, aiding in a fully headless setup, by adding an empty file named "ssh" without an extension in the root of the boot partition on the newly-flashed SD card. This works fine on Windows, however if done from a Linux-based OS, you will need to ensure the file is created AS ROOT, or it will not enable SSH on boot.

## Connecting via wifi

Add the following to /etc/network/interfaces:

```
auto wlan0
iface wlan0 inet dhcp
wpa-ssid "SSID"
wpa-psk "PASSPHRASE"
```

If you want a static ip, use

```
auto wlan0
iface wlan0 inet static
address ADDRESS
netmask NETMASK
gateway GATEWAY
wpa-ssid "SSID"
wpa-psk "PASSPHRASE"
```

## Setting up virtual network interfaces

Virtual network interfaces allow you to split one network card into multiple interfaces. This allows a single card to run in multiple modes and can be extremely useful. Virtual interfaces work just like physical ones, so the usage is the same.

Add an interface.

`iw phy phy0 interface add NAME type MODE && ifconfig NAME up`

Delete an interface.

`ifconfig NAME down && iw dev NAME del`

## Setting up an access point

In order to use the Raspberry Pi on the go, it can be beneficial to set up a network access point. This will allow you to SSH into the Pi without having to be connected to a network, and will allow for an internet connection to be forwarded from wireless or ethernet adapters to clients connected to the access point.

If you do not want to go the slightly more reliable and much more configurable route using hostapd and dnsmasq, you can use `nmcli`, a cli tool for managing `network-manager`. To set up a hotspot at boot put the following in /etc/rc.local before the `exit 0` line.

```
sleep 60
nmcli device wifi hotspot ifname INTERFACE ssid SSID password PASSWORD
```

### Install hostapd and dnsmasq

Install [hostapd](https://en.wikipedia.org/wiki/Hostapd) and [dnsmasq](https://en.wikipedia.org/wiki/Dnsmasq) and update dhcpcd to the latest version.

`apt-get install hostapd dnsmasq dhcpcd5`

Reboot to apply any changes to the system.

`reboot`

### Configure the interfaces

Append `denyinterfaces wlan0` to the end of /etc/dhcpcd.conf and replace the contents of /etc/network/interfaces with

```
auto lo
iface lo inet loopback

allow-hotplug eth0
iface eth0 inet dhcp

auto wlan0
iface wlan0 inet static
address 192.168.230.1
netmask 255.255.255.0
network 192.168.230.0
broadcast 192.168.230.255

allow-hotplug wlan1
iface wlan1 inet dhcp
wpa-conf /etc/wpa_supplicant/wpa_supplicant.conf
```

*Note: The IP addresses can be changed, just be sure to change them in the following steps as well.*

On some Raspberry Pi models, plugging external networking adapters directly into the Pi can trigger a power surge, causing the device to reboot. It is advisable to plug in any adapters before booting, however the use of a usb hub, specifically one with capacitors, will bypass this issue. If, after booting with any adapters plugged in, any external wireless interface is acting as wlan0 rather than the internal card, try switching the 'wlan0' and 'wlan1' on the lines with 'allow-hotplug' in /etc/network/interfaces. You can check this by running `iw dev`. The wlan0 interface should have a vendor ID of "B8:27:EB."

Restart the dhcpcd service and apply the changes to wlan0.

`service dhcpcd restart`

`ifup wlan0`

### Configure hostapd

Create a new /etc/hostapd/hostapd.conf with the following contents

```
interface=wlan0
driver=nl80211

hw_mode=g
channel=6
ieee80211n=1
wmm_enabled=1
ht_capab=[HT40][SHORT-GI-20][DSSS_CCK-40]

auth_algs=1
wpa=2
wpa_key_mgmt=WPA-PSK
rsn_pairwise=CCMP
ssid=Pi-AP
wpa_passphrase=raspberry
```

*Note: Remember to set the SSID and wpa_passphrase to your own values.*

Edit /etc/default/hostapd and change the value of 'DAEMON_CONF' to '/etc/hostapd/hostapd.conf.' Do the same in /etc/init.d/hostapd.

#### Disabling SSID broadcast

To disable SSID broadcast add `ignore_broadcast_ssid=1` to /etc/hostapd/hostapd.conf.

#### Enabling MAC filtering

To enable MAC filtering, create a new file in /etc/hostapd called 'whitelist' with the contents being the MAC addresses of any devices you want to be able to connect to the access point and add the following to /etc/hostapd/hostapd.conf.

```
macaddr_acl=1
accept_mac_file=/etc/hostapd/whitelist
```

### Configure dnsmasq

Back up the original configuration.

`cp /etc/dnsmasq.conf /etc/dnsmasq.conf.orig`

Replace the contents of /etc/dnsmasq.conf with

```
interface=wlan0
listen-address=192.168.230.1
bind-interfaces
server=8.8.8.8
domain-needed
bogus-priv
dhcp-range=192.168.230.2,192.168.230.255,12h
```

### Set up IPv4 forwarding

Add the following to the end of /etc/sysctl.conf

`net.ipv4.ip_forward=1`

Apply the changes.

`sh -c "echo 1 > /proc/sys/net/ipv4/ip_forward"`

### Configure iptables

`update-alternatives --config iptables`

*Note: Be sure to select legacy mode.*

`iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE`

`iptables -A FORWARD -i eth0 -o wlan0 -m state --state RELATED,ESTABLISHED -j ACCEPT`

`iptables -A FORWARD -i wlan0 -o eth0 -j ACCEPT`

`iptables -t nat -A POSTROUTING -o wlan1 -j MASQUERADE`

`iptables -A FORWARD -i wlan1 -o wlan0 -m state --state RELATED,ESTABLISHED -j ACCEPT`

`iptables -A FORWARD -i wlan0 -o wlan1 -j ACCEPT`

Backup your iptables configuration.

`iptables-save > /etc/iptables.conf`

Make the changes persistent across reboots by adding the following to /etc/rc.local. It is not usually created by default, so just create a new one.

```
#!/bin/sh -e

iptables-restore < /etc/iptables.conf

exit 0
```

Make the file executable.

chmod +x /etc/rc.local

### Enable hostapd and dnsmasq to run at boot

`systemctl enable hostapd`

`systemctl enable dnsmasq`

*Note: If an error occurs regarding masked services, run `systemctl unmask SERVICE`.*

Reboot to ensure everything has been set up correctly.
