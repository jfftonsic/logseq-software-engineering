- Keyboard
  heading:: true
	- Set ABNT2 keyboard layout
	  heading:: true
		- ```bash
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
	-
- Network
  heading:: true
	-
- Network
  heading:: true
	-