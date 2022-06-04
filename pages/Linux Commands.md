- Keyboard
  heading:: true
	- Set ABNT2 keyboard layout
	  heading:: true
		- id:: 62926ead-aea5-49e5-be23-efb2315b162e
		  ```bash
		  setxkbmap -model abnt2 -layout br
		  ```
- Network
  heading:: true
	- lsof / netstat / ss
	  heading:: true
	  collapsed:: true
		- Check in use ports / listening ports
		  heading:: true
			- ```bash
			  sudo lsof -i -P -n | grep LISTEN 
			  sudo lsof -i:22 ## see a specific port such as 22 ## 
			  -i : to get network related things 
			  -n : show IPs rather than DNS names 
			  -P : show ports as numbers and not as descriptive texts if it is a known port. 
			  
			  netstat -tulpn | grep LISTEN 
			  
			  sudo ss -tulwn | grep LISTEN 
			  -tu = tcp and udp 
			  -l = listening sockets 
			  -p = show process name 
			  -n = don't resolve names (without this, some ports, like 80 will appear as http for example. And it will show the dns name instead of the IP if that is available)
			  ```
		- Get connections from IP X
		  heading:: true
			- ```bash
			  watch "netstat -anp | grep -i [IP]" 
			  What appears on ESTAB is what stablished the connection. 
			  What appears on SYN_SENT is ACL blocking.
			  ```
		- Searching processes that are opening a file
		  heading:: true
			- ```bash
			  lsof -n | grep filename
			  ```
		- Show file descriptors hold by a process
		  heading:: true
			- ```bash
			  lsof -n -p 113
			  ```
	- SSH
	  heading:: true
	  collapsed:: true
		- Reverse tunnel/proxy so that a remote machine can communicate to the local machine
		  heading:: true
			- ```bash
			  Connect via ssh to the remote machine
			  ssh <user>@<remote-machine-ip-or-dns>
			  At the remote machine, you must verify that the file /etc/ssh/sshd_config has the following fields and are as here:
			      PermitTunnel yes 
			      AllowTcpForwarding yes
			      GatewayPorts yes
			  If you had to change the values or add lines, you must restart the ssh service:
			  sudo service sshd restart
			  In your local machine, create a reverse tunnel:
			  ssh -R <port-at-remote-machine>:localhost:<port-at-local-machine> <user>@<remote-machine-ip-or-dns> -N -f
			  After this, at the remote machine, if you send things to its <port-at-remote-machine>, it will forward them to your local machine at port <port-at-local-machine>
			  
			  Example:
			  ssh -R 3128:localhost:3128 user@remote.machine -N -f
			  assuming you have a socket listening at port 3128 on your local machine, then if in a terminal at the remote machine you telnet localhost port 3128, you will actually connect to your local machine at the port you specified.
			  
			  ```
		- Normal tunnel
		  heading:: true
			- ``` BASH
			  ssh -f jtonsic@a1-hispania-s-pla1.host.intranet -L 10003:201.55.255.199:10003 -N
			  ```
		- Mount remote directory on local machine (sshfs must be installed)
		  heading:: true
			- ```BASH
			  sshfs -o idmap=user@any.machine.ip.or.dns.or:/ /home/localuser/whatever/
			  sshfs -o idmap=user [USER]@[HOSTNAME]:[REMOTEDIR] [LOCALDIR]
			  ```
	- NC
	  heading:: true
	  collapsed:: true
		- send something to an ip/port
		  heading:: true
			- ```bash
			  echo -n "any text" | nc localhost 51069
			  ```
		- connect to a ip / port similarly to telnet
		  heading:: true
			- ```bash
			  nc localhost 51069
			  relevant optional args:
			  -s <ip> : to send packets from the interface with this ip
			  -p <port> : to fix a source port to send packets from. (random available port if not set)
			  ```
		- listen (bind) to a ip/port
		  heading:: true
			- ```bash
			  nc -l -k localhost 9999
			  -l: listen
			  -k: When a connection is completed, listen for another one.
			  - nc -lU /var/tmp/dsocket
			  -U: Use UNIX-domain sockets.
			  ```
		- (As a nifty trick) opening a port and let anyone connected execute arbitrary command on your site (DANGEROUS).
		  heading:: true
			- ```bash
			  On ‘server’ side:
			  rm -f /tmp/f; mkfifo /tmp/f
			  cat /tmp/f | /bin/sh -i 2>&1 | nc -l 127.0.0.1 1234 > /tmp/f
			  - On ‘client’ side:
			  nc host.example.com 1234
			  (shell prompt from host.example.com)
			  ```
		- Data transfer
		  heading:: true
			- ```bash
			  nc -l 1234 > filename.out
			  nc -N host.example.com 1234 < filename.in
			  ```
		- Talk to server by hand
		  heading:: true
			- ```bash
			  printf "GET / HTTP/1.0\r\n\r\n" | nc host.example.com 80
			  or even
			  nc [-C] localhost 25 << EOF
			           HELO host.example.com
			           MAIL FROM:<user@host.example.com>
			           RCPT TO:<user2@host.example.com>
			           DATA
			           Body of email.
			           .
			           QUIT
			           EOF
			  or even
			  echo -n "any text" | nc myserver.host.intranet 9999
			  ```
		- Port scanning
		  heading:: true
			- ```bash
			  nc -zv host.example.com 20-30
			  Connection to host.example.com 22 port [tcp/ssh] succeeded!
			  Connection to host.example.com 25 port [tcp/smtp] succeeded!
			  - echo "QUIT" | nc host.example.com 20-30
			  SSH-1.99-OpenSSH_3.6.1p2
			  Protocol mismatch.
			  220 host.example.com IMS SMTP Receiver Version 0.84 Ready
			  ```
	- iptables
	  heading:: true
	  collapsed:: true
		- lacking description, I think this means if your local machine receives a packet for port 3128 coming from IP 172.22.223.1 should be redirected to port 3128, but of where? If it is of the local machine, is it not the same port as the initially asked for? #WIP
		  background-color:: #793e3e
			- ```bash
			  sudo iptables -t nat -A PREROUTING -p tcp --dport 3128 -s 172.22.223.1 -j DNAT --to-destination :3128
			  ```
	- tcpdump
	  heading:: true
	  collapsed:: true
		- Dump to a file packets of any interface, that is related to port 8750
		  heading:: true
			- ```bash
			  sudo tcpdump -i any -w outputfile.pcap -vvv port 8750
			  ```
	- ip
	  heading:: true
	  collapsed:: true
		- Get IPv4 address from eth0
		  heading:: true
			- ```bash
			  ip addr show dev eth0 | grep "inet " | awk '{print $2}' | sed 's/\/.*//'
			  ```
	- traceroute / tracepath
	  heading:: true
	  collapsed:: true
		- ```bash
		  tracepath -p 80 -b google.com # DNS name or IP 
		  -p <port> 
		  -b print both name and ip 
		  -n no dns name resolution 
		  -4 use IPv4
		  -6 use IPv6 
		  -l <length> use packet <length>  
		  -m <hops> use maximum <hops> 
		  disadvantage: cannot specify device, and other things. 
		  traceroute command seem to differ by package that you installed it from, from inetutils-traceroute package it is very similar to tracepath so no point in using it. 
		  
		  Now from package 'traceroute', it is a much more complete command than tracepath, see man to details of other options: 
		  
		  sudo traceroute -T -A -n -w 30 -p 10003 -i virbr0 201.55.255.199 
		  -p <port> 
		  -i <device-name>: only if you want to force the usage of a device. 
		  -n: Do not try to map IP addresses to host names when displaying them. 
		  -w <waittime>: Set the time (in seconds) to wait for a response to a probe (default 5.0 sec). 
		  -T: Use TCP SYN for probes 
		  -A: Perform AS path lookups in routing registries and print results directly after the corresponding addresses. (i don't know the reason to specify this) 
		  ```
	- curl
	  heading:: true
	  collapsed:: true
		- POST request with header and query string
		  heading:: true
			- ```bash
			  curl --request POST 'http://whatever.host.intranet:34070/bla' --header 'Content-Type:application/x-www-form-urlencoded' --data "start=2016-06-02T00:00:00&end=2016-06-02T04:00:00&seq=220"
			  ```
		- Sending binary-style data (doesn't matter if file is actually a text or not)
		  heading:: true
			- ```bash
			  curl -X POST --verbose --header "Content-Type:application/xml" --data-binary @/home/user/file.any http://localhost:50090/whatever
			  ```
	- squid
	  heading:: true
	  collapsed:: true
		- Local HTTP Proxy, install and configure squid
		  heading:: true
			- ```bash
			  Local HTTP Proxy, install and configure squid
			  Squid  : /usr/sbin/squid3 -N -YC -f /etc/squid3/squid.conf
			  Say you need to access internet in a remote machine that has it blocked but you are able to create a reverse tunnel to it.
			  
			  To configure it
			  sudo vim /etc/squid/squid.conf
			  
			  include a  line acl xxx src:
			  acl lanhome src <ip-remote-machine>/<submask-remote-machine>
			  acl lanhome src 10.0.0.0/255.255.255.0 (this is an example from the squid site)
			  acl lanstg5 src 10.138.80.191/20 (this is another example from myself)
			  
			  include a line http_access allow xxx:
			  http_access allow lanstg5  (example to allow http access by the group lanstg5 created above)
			  
			  if squid is already up, call
			  sudo squid3 -k reconfigure
			  ```
	- Sending raw binary things (be it text or not) to any listening server (probably works not only for network things)
		- `dd if=any_file_bin_or_not > /dev/tcp/10.138.20.235/4080`
	- Getting ip only, attributed to a device
		- `ip addr show dev eth0 | grep "inet " | awk '{print $2}' | sed 's/\/.*//'`
- Security
  heading:: true
  collapsed:: true
	- Download certificate through command line instead of from browser
	  heading:: true
		- ```bash
		  openssl s_client -servername ws.mobile.pagseguro.intranet -connect ws.mobile.pagseguro.intranet:443 </dev/null | sed -ne '/-BEGIN CERTIFICATE-/,/-END CERTIFICATE-/p' > ~/ws.mobile.pagseguro.intranet.pem 
		  another one 
		  openssl x509 -in <(openssl s_client -connect ws.mobile.pagseguro.intranet:443 -prexit 2>/dev/null) -out ~/ws.mobile.pagseguro.intranet.crt
		  ```
	- Importing certificate in JVM 
	  heading:: true
		- ```bash
		  sudo keytool -importcert -file ~/ws.mobile.pagseguro.intranet.crt -alias ws.mobile.pagseguro.intranet -keystore $JAVA_HOME/jre/lib/security/cacerts -storepass changeit
		  ```
	-
- File
  heading:: true
	- tail
		- ```bash
		  tail ---disable-inotify --retry -f /tmp/binance-integration.log
		  
		  ---disable-inotify = when using WSL and tailing a file that is out of the linux file system
		  --retry = if the file gets truncated (and a new one with same name is beginning to get written), without this tail will stop following.
		  
		  ```
	- Find in files
		- `grep -r --include="*.kts" "if (project.hasProperty(\"jvmArgs\")" ~/workspace`
	- Rename multiple files
		- `find . -name '*Elo*' -type f -exec bash -c 'mv "$1" "${1/Elo/Master}"' -- {} \;`
	- Replace in files
		- `find . -type f -exec sed -i 's/ELO/MASTER/g' {} +`
	- Appending to files
		- ```bash
		  echo "text here" >> filename 
		  command >> filename 
		  date >> filename 
		  printf "\nTEXT HERE\n$(ls)" >> lists.txt 
		  echo '104.20.186.5 www.cyberciti.biz' | sudo tee -a /etc/hosts         # when using sudo 
		  sudo sh -c 'echo my_text >> file1'                                     # when using sudo 
		  sudo -- bash -c 'echo "some data" >> /my/path/to/filename.txt'         # when using sudo 
		  echo 'text' &>>filename                                                # appending also the stderr
		  
		  ```
	- Find files and remove
		- ```bash
		  find /dir/to/search/ -type f -name "FILE-TO-FIND-Regex" -exec rm -f {} \;
		  -type f : Only match files and do not include directory names.
		  -type d : Only match dirs and do not include files names.
		  Modern version of find command has -delete option too
		  -maxdepth option to control descend
		  
		  ```
	- Multiline grep-like example (must install pcregrep tool)
		- ```bash
		  pcregrep -M "08407A00000000310109B02E800\",nsuTerminal=2485]( )*\R( )*ps.swt.tlv.exception.SwitchServiceException: null" /export/logs/pagseguro-switch-server/pagseguro-switch.log
		  ```
	- List folder and subfolders sizes ordered
		- `sudo du -h --max-depth=2 . | sort -h 2> /dev/null     # sudo only if folders with those permissions are wanted; choose your max-depth; The "2> /dev/null" is if you want it to not show the errors it gets trying to access things that it can't.`
	- To unzip protected zip files
		- ```bash
		  # (normal unzip may result in "need PK compat. v5.1 (can do v4.5)"
		  7z x protected.zip
		  ```
	- Untar
		- `tar.gz, untar, comando: tar -C [pasta] -xzf [arquivo.tar.gz]`
	- Removing n characters from a line's beginning and then sort the file's contents
		- `cut -c 12- ./v2/UPSRA83020160614183101.TXT | sort > ./v2/sortedUPSRA83020160614183101.TXT`
	- Copying everything from one directory to another also showing the progress
		- `sudo rsync -arh --progress --ignore-existing /opt /media/user/`
	- Shows all file systems, the type, usage, free space, where are they mounted to
		- `df -Tak`
	- Files have special attributes/flags
		- ```
		  Files have special attributes/flags, to show them: 
		  lsattr 
		  to change them: 
		  chattr 
		  
		  Some of the flags: 
		  a : append only 
		  c : compressed 
		  d : no dump 
		  e : extent format 
		  i : immutable -> blocks file modification 
		  j : data journalling 
		  s : secure deletion 
		  t : no tail-merging 
		  u : undeletable 
		  A : no atime updates -> don't update last read marking when reading the file 
		  C : no copy on write 
		  D : synchronous directory updates -> only frees the thread that makes the write after the bytes were really applied to hard disk.
		  S : synchronous updates 
		  T : top of directory hierarchy
		  
		  ```
	- Print binary file contents in binary or hex
		- ```
		  For bin :xxd -b file
		  For hex: xxd file
		  Also: hexdump -C yourfile.bin
		  ```
- Process
  heading:: true
	- Start process in background
		- follow a command with &
	- Process search
		- ```bash
		  pgrep <regex_to_match_process_name> # shows PID, -a: also shows full command line, -f: regex will match full command line
		  ```
	- keep processes running even after exiting the shell
		- ```bash
		  prevents the processes or jobs from receiving the SIGHUP (Signal Hang UP) signal. 
		  nohup <command> <arguments>
		  # will ignore input and append output to nohup.out, or you redirect output with "> output.txt" or "> myoutput.txt >2&1" if u want also the stderr.
		  ```
	- Killing first process of a grepped ps aux
		- ```bash
		  kill -9 $(ps aux | grep 'applicationwhatever' | awk '{print $2}') 
		  or 
		  ps aux | egrep "java -jar.*applicationwhatever" | grep -v grep | awk '{print $2}' | xargs sudo kill 
		  ```
	- Prevent ps aux grep from showing itself on the results
		- `ps aux | grep "[f]nord"`
	- Searching processes that are opening a file
		- `lsof -n | grep filename`
	- Show file descriptors hold by a process
		- `lsof -n -p 113`
	- /proc folder
		- ```
		  /proc has informations of system and processes 
		  /proc/<PID>/limits 
		  /proc/<PID>/environ 
		  /proc/<PID>/cpuset 
		  /proc/<PID>/fd 
		  /proc/<PID>/status 
		  
		  ```
	-
- Terminal
  heading:: true
	- Searching history for regex matches
		- `history | grep -E "regex"`
	- Shortcuts
		- ```
		  Alt+d: delete word from cursor forwards, stops at whitespaces and non-word chars e.g.: ‘-’
		  Alt+backspace: delete word from cursor backwards, stops at whitespaces and non-word chars e.g.: ‘-’
		  Ctrl+w: delete word from cursor backwards, but only stops at whitespaces.
		  
		  Stop process 
		  ctrl+z 
		  then commands: 
		  fg -> returns execution of the stopped process to the foreground 
		  bg -> returns execution of the stopped process to the background
		  
		  The obvious ones: 
		  ctrl+c: kill foreground process
		  etc
		  ```
-
- System
  heading:: true
	- Show linux distro and release:
		- ```bash
		  lsb_release -a 
		  Example of what it shows on ubuntu: 
		  No LSB modules are available. 
		  Distributor ID: Ubuntu 
		  Description:    Ubuntu 20.04.2 LTS 
		  Release:        20.04 
		  Codename:       focal
		  ```
	- Changing system properties
		- `sysctl`
		- Some important system properties
			- ```
			  # 0 - 100 (0 = only ram, 100 = only swap) 
			  vm.swappiness = 60 
			  
			  # controls the machine firewall's maximum of sessions 
			  net.netfilter.nf_conntrack_max = 65536 
			  
			  # timeout for the kernel to free closed connections 
			  net.ipv4.tcp_fin_timeout = 30 
			  
			  # system's max file descriptors that can be opened 
			  fs.file-max = 1000000 
			  
			  ```
	- System calls
		- ```bash
		  strace 
		  strace -f -p
		  ```
	- For distros that use apt
		- ```bash
		  sudo apt update # download package information from all configured sources. Do it before 'upgrade' 
		  sudo apt upgrade # install available upgrades of all packages currently installed 
		  sudo apt <install/reinstall/remove/purge> XXX # the XXX may be a regex, glob or exact name. Each name may be followed with a '=<version>'. 
		  apt search <regex> # looks for packages with name or description that matches the input 
		  apt show <package> # show info for the pkg (dependencies, installation and download size, ...)
		  apt-file list <pkg> # list contents of a pkg, may also be 'show' 
		  apt-file search <pattern> # search which pkgs contain a file (doesn't match folders), may also be 'find'
		  ```