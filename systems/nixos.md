# NixOS

## Installation

Manual: https://nixos.org/nixos/manual/index.html#ch-installation

### Installation in VirtualBox (minimal)

- Use UEFI options during installation
- Enabling ssh can be done via `sudo systemctl start sshd` (as shown in the installer motd) but will also need `passwd` to be set
- VirtualBox will sometimes cause the ssh connection to be refused, if this is the case, set the network adapter to "Bridged Adapter" in the VM's settings (this does not require a restart, just be patient)
