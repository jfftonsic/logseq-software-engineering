- Scaling out using commodity machines is more cost efficient and results in higher availability than scaling up a single server on more expensive hardware, called [[Vertical Scaling]] .
- Load balancers can help with horizontal scaling
- Disadvantages
	- Scaling horizontally introduces <span class="hl-neutral-01">complexity and involves cloning servers</span>
		- Servers should be <span class="hl-neutral-01">stateless</span>
			- they should not contain any user-related data like sessions or profile pictures
		- #+BEGIN_TIP
		  Sessions can be stored in a centralized data store such as a database (SQL, NoSQL) or a persistent cache (Redis, Memcached)
		  #+END_TIP
	- Downstream servers such as caches and databases need to handle more simultaneous connections as upstream servers scale out