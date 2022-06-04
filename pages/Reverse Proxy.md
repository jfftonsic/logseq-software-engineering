- Short description
	- A web server that centralizes internal services and provides unified interfaces to the public. Requests from clients are forwarded to a server that can fulfill it before the reverse proxy returns the server's response to the client.
- Benefits
	- Increased security
	  background-color:: #497d46
		- Hide information about backend servers, blacklist IPs, limit number of connections per client
	- Increased scalability and flexibility
	  background-color:: #497d46
		- Clients only see the reverse proxy's IP, allowing you to scale servers or change their configuration
	- SSL termination
	  background-color:: #497d46
		- Decrypt incoming requests and encrypt server responses so backend servers do not have to perform these potentially expensive operations
			- #+BEGIN_TIP
			  Removes the need to install X.509 certificates on each server
			  #+END_TIP
	- Compression
	  background-color:: #497d46
		- Compress server responses
	- Caching
	  background-color:: #497d46
		- Return the response for cached requests
	- Static content
	  background-color:: #497d46
		- Serve static content directly
		- HTML/CSS/JS
		- Photos
		- Videos
		- Etc
- Disadvantages
	- . <span class="hl-neutral-01">Increased complexity</span>.
	- A <span class="hl-neutral-01">single reverse proxy is a single point of failure</span>, configuring multiple reverse proxies (ie a failover) further increases complexity.
- {{embed ((627c1430-02e1-4b30-992b-2e2353ac47a5))}}