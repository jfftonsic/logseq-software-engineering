sources:: ((62793833-bc6c-4b23-a679-0cc07d4ed7c2))
alias:: load balance, load balancing

- Short description
	- distribute incoming client requests to computing resources such as application servers and databases.
- Purposes
  heading:: true
	- Preventing requests from going to unhealthy servers
	- Preventing overloading resources
	- Helping to eliminate a single point of failure
	- Help with horizontal scaling
	- Extras
	  heading:: true
		- SSL termination
			- Decrypt incoming requests and encrypt server responses so backend servers do not have to perform these potentially expensive operations
		- Removes the need to install X.509 certificates on each server
		- Session persistence
			- Issue cookies and route a specific client's requests to same instance if the web apps do not keep track of sessions
- Implementation / Setting up
  heading:: true
	- can be implemented with hardware (expensive) or with software such as HAProxy.
	- for [[reliability]] set up multiple load balancers, either in active-passive or active-active mode.
	- can route traffic based on various metrics, including:
		- Random
		- Least loaded
		- Session/cookies
		- Round robin or weighted round robin
		- [[Layer 4]]
			- look at info at the <span class="hl-neutral-01">transport layer</span> to decide how to distribute requests.
			- Generally, this involves the source, destination IP addresses, and ports in the header, but not the contents of the packet.
			- They forward network packets to and from the upstream server, performing Network Address Translation (NAT).
			- less time and computing resources than Layer 7
			  background-color:: #497d46
			- less flexibility than Layer 7
			  background-color:: #793e3e
			- https://www.nginx.com/resources/glossary/layer-4-load-balancing/
		- [[Layer 7]]
			- look at the application layer to decide how to distribute requests.
			- This can involve contents of the header, message, and cookies.
			- Layer 7 load balancers
				- terminate network traffic
				- reads the message
				- makes a load-balancing decision
				- then opens a connection to the selected server.
			- more time and computing resources than Layer 4
			  background-color:: #793e3e
			- more flexibility than Layer 4
			  background-color:: #497d46
- Disadvantages
  heading:: true
	- can become a <span class="hl-neutral-01">performance bottleneck</span> if it does not have enough resources or if it is not configured properly.
	- . <span class="hl-neutral-01">A single load balancer is a single point of failure,</span> configuring multiple load balancers further <span class="hl-neutral-01">increases complexity</span>.
- {{embed ((627c1430-02e1-4b30-992b-2e2353ac47a5))}}
- Examples
  heading:: true
	- [[Netflix Ribbon]]
	- [[AWS ELB]]
-