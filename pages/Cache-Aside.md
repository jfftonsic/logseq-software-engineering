sources:: [Microsoft docs](https://docs.microsoft.com/en-us/azure/architecture/patterns/cache-aside)
tags:: cloud design pattern, caching

- Short description
	- Load data on demand into a cache from a data store.
- Advantages
	- can improve performance
	- - <span style="color: yellow; background-color: black">helps to maintain</span> ( <span style="color: red; background-color: black; font-weight: bold">but doesn't guarantee</span> ) <span style="color: yellow; background-color: black">consistency</span> between data held in the cache and data in the underlying data store.
- Use when
	- A cache doesn't provide native read-through and write-through operations.
	- Resource <span style="color: orange">demand is unpredictable</span>.
		- This pattern enables applications to load data on demand.
		- It makes no assumptions about which data an application will require in advance.
- Not use when
	- If the data is static and will fit into the available cache space
	  collapsed:: true
		- prime the cache with the data on startup and apply a policy that prevents the data from expiring.
	- Caching <span style="color: orange">session state information</span> in a web application hosted in a web farm.
	  collapsed:: true
		- In this environment, you should <span style="color: orange">avoid introducing dependencies based on client-server affinity</span>.
- Issues and Considerations
	- Lifetime of cached data
		- ensure that the expiration policy matches the pattern of access for applications that use the data.
	- Evicting data
		- Most caches adopt a least-recently-used policy for selecting items to evict, but this might be customizable.