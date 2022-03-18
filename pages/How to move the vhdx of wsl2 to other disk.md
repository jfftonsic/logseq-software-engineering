sources:: https://github.com/MicrosoftDocs/WSL/issues/412

- > Here is exactly what I did on 19035.1 and it worked without a reboot or errors.
- ```text
  PS C:\WINDOWS\system32> wsl -l
  Windows Subsystem for Linux Distributions:
  Ubuntu (Default)
  
  # mkdir S:\ISOs\
  
  PS C:\WINDOWS\system32> wsl --export Ubuntu S:\ISOs\ubuntu-wsl.tar
  
  # mkdir w:\VMs
  
  PS C:\WINDOWS\system32> cd w:\VMs
  PS W:\VMs> mkdir ubuntu-wsl
  PS W:\VMs> wsl --unregister Ubuntu
  Unregistering...
  PS W:\VMs> wsl --import Ubuntu W:\VMs\ubuntu-wsl S:\ISOs\ubuntu-wsl.tar
  PS W:\VMs> wsl -l
  Windows Subsystem for Linux Distributions:
  Ubuntu (Default)
  ```
  
  after this, the new ubuntu was starting with user root, so I needed to 
  ```text
  Open registry edit and navigate to HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Lxss{DISTRO-UUID}
  Replace the DefaultUid value with the UID value of the user in your distro, my normal user is 1000 and root is 0.
  ```
  
  https://github.com/microsoft/WSL/issues/4276
-