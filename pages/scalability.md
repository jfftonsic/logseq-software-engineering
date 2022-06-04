alias:: scalable
sources:: ((627ab639-9cec-4c04-87d9-012a65393028))

- Short description
	- a system’s ability to cope with increased load.
- Objective
	- Reasonably <span class="hl-neutral-01">dealing with the growth of a system</span> (in data volume, traffic volume, or complexity).
- Types
	- [[Horizontal Scaling]]
	- [[Vertical Scaling]]
- Describing Load
	- How to describe the current load on the system?
		- Can be described with a few numbers which we call <span class="hl-neutral-01">load parameters</span>
		- The best choice of parameters <span class="hl-neutral-01">depends</span> on the architecture of your system
			- requests per second to a web server
			- ratio of reads to writes in a database
			- number of simultaneously active users in a chat room
			- hit rate on a cache
		- Define also <span class="hl-neutral-01">what measurement matters</span>
			- E.g. perhaps the average case is what matters for you, or perhaps your bottleneck is dominated by a small number of extreme cases.
- Describing Performance
	- [[throughput]]
	- [[response time]]
	- [[latency]]
	- Investigate what happens when the load increases
		- When you increase a load parameter and keep the system resources (CPU, memory, network bandwidth, etc.) unchanged, how is the performance of your system affected?
		- When you increase a load parameter, how much do you need to increase the resources if you want to keep performance unchanged?
-