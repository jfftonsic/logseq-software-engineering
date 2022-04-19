tags:: WIP
sources:: https://en.wikipedia.org/wiki/PACELC_theorem, https://www.scylladb.com/glossary/pacelc-theorem/

- Short Description
	- Adds to [[CAP Theorem]] the fact that, in absence of partitions, one has to choose between latency and consistency.
- Acronym
	- P ([[Network Partition]]) A ([[Availability]]) C ([[Consistency]]) E (or else) L ([[latency]]) C ([[consistency]])
- In case of network partition, choose tradeoff between availability and consistency (just as CAP Theorem)
- In absence of partitions, choose tradeoff between latency and consistency
- A <span style="color: orange">high availability</span> requirement implies that the system <span style="color: orange">must replicate data</span>.
	- #+BEGIN_IMPORTANT
	  As soon as a distributed system replicates data, a trade-off between consistency and latency arises.
	  #+END_IMPORTANT
- What was the purpose for the creation of this theorem?
	- Address the oversight of ignoring the consistency/latency trade-off of replicated systems
- Is extension of
	- [[CAP Theorem]]
- ![image.png](../assets/image_1650391043621_0.png)
  Image from [wikipedia](https://en.wikipedia.org/wiki/PACELC_theorem), go there if you want the most current information.