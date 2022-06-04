sources:: ((62793833-bc6c-4b23-a679-0cc07d4ed7c2))

- Short description
	- Tools that keep track of registered names, addresses, and ports for services, so that they can communicate with each other by asking what is the address/port for a given service name, so the clients don't need to have a fixed destination configured inside itself for the dependencies.
- . <span class="hl-neutral-01">Health checks</span> help verify service integrity and are often done using an HTTP endpoint.
- Example of tools
	- [[Consul]]
	- [[Etcd]]
	- [[ZooKeeper]]
	- [[Netflix Eureka]]
	- Both Consul and Etcd have a built in key-value store that can be useful for storing config values and other shared data.
	- [[Kubernetes service discovery]]