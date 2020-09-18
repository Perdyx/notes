# Windows

## Disabling session locking (Win+L)

Newer Windows versions block the capture of various keyboard shortcuts (particularily those pertaining to security override features). This can be problematic when trying to use those shortcuts in a virtual machine. To disable the session lock feature in Windows 10, edit the following key in the registry (if it does not exist, create it): `HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Policies\System`
Create a new 32-bit DWORD called "DisableLockWorkstation" and give it a value of 1 (note: a value of 0 will re-enable the functionality).

## Configuring Windows Terminal

Windows Terminal can be configured by clicking the dropdown arrow and then "Settings." This will pull up the configuration file in your default text editor.

### Adding profiles

Profiles are executed via the dropdown arrow and can add ease of access to remote servers or custom shells. To add another profile, add the following in the list section:

```
{
    "guid":  "{GUID}",
    "hidden":  false,
    "name":  "NAME",
    "commandline":  "COMMAND"
},
```

The GUID is randomly generated. Either edit an existing one, or use an online generator.

### Changing the color scheme

Add new color schemes to the "schemes" block. The following example is the Dracula colour scheme from [here](https://draculatheme.com/windows-terminal):

```
{
    "name" : "Dracula",
    "background" : "#282A36",
    "black" : "#21222C",
    "blue" : "#BD93F9",
    "brightBlack" : "#6272A4",
    "brightBlue" : "#D6ACFF",
    "brightCyan" : "#A4FFFF",
    "brightGreen" : "#69FF94",
    "brightPurple" : "#FF92DF",
    "brightRed" : "#FF6E6E",
    "brightWhite" : "#FFFFFF",
    "brightYellow" : "#FFFFA5",
    "cyan" : "#8BE9FD",
    "foreground" : "#F8F8F2",
    "green" : "#50FA7B",
    "purple" : "#FF79C6",
    "red" : "#FF5555",
    "white" : "#F8F8F2",
    "yellow" : "#F1FA8C"
}
```

### Manually partitioning Windows installation

During the partitioning section of the Windows 10 installer, delete all partitions and press Shift+F10 to open a command prompt. Type `diskpart` to open the partitioning tool.

Type `list disk` and `select DISK_NUMBER` to select the drive. Be careful not to select the installer drive.

Create and format the ESP:

```
create partition efi size=SIZE_IN_MB
format quick fs=fat32 label=System
```

Exit and refresh the disks in the installer. Proceed to creating a new partition with the default size. Windows will automatically set up other required partitions.
