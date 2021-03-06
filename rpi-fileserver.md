# Setting up a Raspberry Pi fileserver using Raspbian + Samba

## Initial setup

Download the latest Raspbian Lite image from [here](https://www.raspberrypi.org/downloads/raspbian/). Flash the image to your SD card, plug in a keyboard, HDMI, and ethernet cables into the Pi and boot.

Log in as the default user:<br>
Username: `pi`<br>
Password: `raspberry`

Run `sudo raspi-config` to access the setup menu.

Change the hostname to something memorable.

Change the default locale, timezone, and keyboard layout. Defaults are set to GB standards.

Finish the setup and reboot the Pi to apply the settings.

## Setting up an administrator account

Add a new administrative user with sudo privileges.<br>
`sudo useradd -m -d /home/testuser/ -s /bin/bash -G sudo <username>`

Set a password for the new user.<br>
`sudo passwd <username>`

Log out and back in with the new user and delete the pi user.<br>
`sudo userdel -r pi`

## Enabling SSH

Raspbian should already come with ssh installed, but if it isn't, just install it from the repository:<br>
`sudo apt install openssh`

Enable the ssh server to run at boot.<br>
`sudo systemctl enable ssh`

## Setting up Samba

Make sure the system is updated.<br>
`sudo apt update && apt dist-upgrade`

Install Samba.<br>
`sudo apt install samba samba-common-bin`

Create a new user group.<br>
`sudo groupadd <groupname>`

Create a new user to access the share and add it to the group.<br>
`sudo useradd -G smb <username>`<br>
`sudo passwd <username>`

Add the new user to Samba.<br>
`sudo smbpasswd -a <username>`

Plug in a drive via USB. It should be listed as something like `sda1` when you run `lsblk`.

Format the drive.<br>
`sudo mkfs.ext4 /dev/sda1`

Create and mount point and mount the drive to it.<br>
`sudo mkdir /mnt/smb1`<br>
`sudo mount /dev/sda1 /mnt/smb1`

Run `blkid` and copy the UUID for /dev/sda1, then append the following line to /etc/fstab:<br>
`UUID=<drive_uuid> <mount_point> ext4 defaults 0 2`

Change directory to the newly mounted drive and create the following directory structure for the share.
- share
    - shared
    - user1
    - user2
    - user3 ...

Configure permissions for the shared directory, replacing "<groupname>" with the name of the group you created earlier.<br>
`sudo chown -R :<groupname> shared`<br>
`sudo chmod 770 shared`

Configure permissions for each user directory, replacing "<groupname>" with the name of the group you created earlier.<br>
`sudo chown -R <username>:<groupname> <user_directory>`<br>
`sudo chmod 740 <user_directory>`

Add the following to the end of /etc/samba/smb.conf:<br>
```
[<share_address>]
path = <path_to_share_directory>
writeable = yes
create mask = 0777
directory mask = 0777
public = no
```

## Configuring drive power management

Having a USB hard drive running constantly can severely decrease the drive's lifespan. Setting the drive to sleep after a short period can help.

Install hdparm.<br>
`sudo apt install hdparm`

Reboot to allow hdparm to calibrate.

Set the drive to automatically sleep after five minutes of inactivity.<br>
`sudo hdparm -B 1 -S 60 /dev/sda`<br>
To make the changes permanent, add the previous line before the `exit 0` line in /etc/rc.local.

Reboot to ensure everything has been set up correctly.
