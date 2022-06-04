- [[How to move the vhdx of wsl2 to other disk]]
- [[Jetbrains toolbox on WSL on GUI]]
- [[Intellij on WSL on GUI]]
- [[WSL Ubuntu with GUI]]
- [[Fuzzy bash history search tool]]
- [[WSL Install]]
-
- All currently running distributions (`wsl -l`) are accessible via network connection. E.g. `\\wsl$\Ubuntu-18.04\home\<user name>\Project`
- Useful commands
  heading:: true
	- ```bash
	  wsl --set-default-version 2 : you'll probably use this once
	  wsl --list -v : lists installed linux distributions, status and with which wsl version it is running
	  wsl --shutdown
	  wsl --update : shutdown first if using this command
	  wsl --unregister <DistributionName> : it can be reinstalled or cleaned up. Caution: Once unregistered, all data, settings, and software associated with that distribution will be permanently lost. Reinstalling from the store will install a clean copy of the distribution.
	  
	  ```
- Global config file
  heading:: true
	- `C:\Users\<yourUserName>\.wslconfig`
	- ```
	  [wsl2]
	  kernel=C:\\temp\\myCustomKernel
	  memory=4GB # Limits VM memory in WSL 2 to 4 GB
	  processors=2 # Makes the WSL 2 VM use two virtual processors
	  
	  ```
	- If you change, you must `wsl –shutdown` then start to make the changes take effect.
- WSL RAM
  heading:: true
	- From what I understood, the amount consumed by the linux vm will be dynamically updated from the point of view of windows, so, it will not immediately allocate the maximum and will also reduce usage when the actual usage inside linux is reduced. 
	  The default maximum is 80% of the installed ram.
- WSL Network
  heading:: true
	- WSL 2 has a <span class="hl-neutral-01">virtualized ethernet adapter with its own unique IP address</span>.
	- WSL Accessing Linux networking apps from Windows (localhost)
	  heading:: true
		- Things that are bound to localhost inside the linux machine will also be accessible via 'localhost' in windows.
		  If  you want/need to see the actual address of that machine, run `ip addr`, it will be the eth0 interface.
	- Accessing Windows networking apps from Linux (host IP)
	  heading:: true
		- Obtain the IP address of your host machine by running this command from your Linux distribution:
		  `cat /etc/resolv.conf`
		  Copy the IP address following the term: nameserver.
	- Accessing a WSL 2 distribution from your local area network (LAN)
	  heading:: true
		- Example PowerShell command to add a port proxy that listens on port 4000 on the host and connects it to port 4000 to the WSL 2 VM with IP address 192.168.101.100:
		  ```powershell
		  netsh interface portproxy add v4tov4 listenport=4000 listenaddress=0.0.0.0 connectport=4000 connectaddress=192.168.101.100
		  ```
- WSL Disk
  heading:: true
	- WSL 2 uses a <span class="hl-neutral-01">Virtual Hard Disk (VHD)</span> to store your Linux files. In WSL 2, a VHD is represented on your Windows hard drive as a .vhdx file. 
	  The WSL 2 VHD uses the <span class="hl-neutral-01">ext4</span> file system. 
	  This VHD <span class="hl-neutral-01">automatically resizes</span> to meet your storage needs and has an initial maximum size of 256GB. 
	  If the storage space required by your Linux files exceeds this size you may need to expand it. 
	  If your distribution grows in size to be greater than 256GB, you will see errors stating that you've run out of disk space. Search google for that, if you get to a point to actually need it.
- WSL Docker
  heading:: true
	- Volumes
	  heading:: true
		- Navigate to `\\wsl$\docker-desktop-data\version-pack-data\community\docker\volumes`