# WSL 2

## Installation

See <https://docs.microsoft.com/en-us/windows/wsl/install-win10> for details

Open "Turn Windows features on of off" dialog and enable the following:

* Virtual Machine Platform
* Windows Subsystem for Linux

Reboot, then download and install the WSL2 Linux kernel update from <https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi>. Once that has finished, open an elevated command prompt and run `wsl --set-default-version 2`. Don't forget to download and install your distro of choice from the Microsoft Store.

## Post-installation

### Install `file` command

`sudo apt install file`

### Allow execution of 32-bit binaries

`sudo apt install gcc-multilib`
