# Windows

## Disabling session locking (Win+L)

Newer Windows versions block the capture of various keyboard shortcuts (particularily those pertaining to security override features). This can be problematic when trying to use those shortcuts in a virtual machine. To disable the session lock feature in Windows 10, edit the following key in the registry (if it does not exist, create it): `HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Policies\System`
Create a new 32-bit DWORD called "DisableLockWorkstation" and give it a value of 1 (note: a value of 0 will re-enable the functionality).
