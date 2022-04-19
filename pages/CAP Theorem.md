- Acronym
	- [[Consistency]] [[Availability]] [[Partitioning]]
- Short description
	- a theorem about the properties of shared-data systems (distributed data store).
- Does not refer to scalability
- about [[Consistency]] in this context
  collapsed:: true
	- represented by [[ACID]]
		- What does 'represented' mean here? #WIP
		  background-color:: #793e3e
	- Consistency in this context can be <span style="color: orange">measured in a spectrum</span>, it is not a yes/no answer.
		- e.g. by choosing the number of nodes you require the answer from in order to consider the operation went successfully
	- a total order ( <span style="color: red; background-color: black; font-weight: bold">global order?</span> #WIP ) must exist on all operations such that each operation looks as if it were completed at a single instance. For distributed shared memory this means (among other things) that  _all read operations that occur after a write operation completes must return the value of this (or a later) write operation_ .
- about [[Availability]] in this context
  collapsed:: true
	- represented by [[BASE]]
	- Availability can be <span style="color: orange">measured in a spectrum</span>, it is not a yes/no answer.
	- every request received by a non-failing node must result in a response.
		- This means, any algorithm used by the service must eventually terminate.
- about Tolerance towards Network Partition in this context
  collapsed:: true
	- this <span style="color: orange">is a binary property</span>
	- the network is allowed to lose arbitrarily many messages sent from one node to another
	- #+BEGIN_IMPORTANT
	  by definition any distributed system needs to tolerate partitions.
	  #+END_IMPORTANT
	-
- Extension:
	- [[PACELC Theorem]]
- Mixing different levels of availability and consistency yields a better result.
- #+BEGIN_IMPORTANT
  Only for scenarios of network partitions you need to consider the tradeoff between Consistency and Availability and that consideration can be made independently at different points of your system
  #+END_IMPORTANT
	- e.g. with Cassandra you can specify per-query what is the trade-off you want
- Options you have to decide
	- In case of network partition
		- Forfeit Consistency
		  collapsed:: true
			- traits
				- It implies [[Optimistic Locking]]
				- inconsistency resolving protocols.
		- Forfeit Availability
		  collapsed:: true
			- traits
				- Pessimistic Locking
					- need to lock any updated object until the update has been propagated to all nodes
	- Forfeiting partition tolerance
		- undefined behavior in case of network partition
		- this option is not feasible in realistic environments, since we will always have network partitions
		- partition Tolerance can only be forfeited in a hypothetical environment where no partition can happen.  But any real system that would forfeit partition tolerance would not be working correctly and thus the option Availability and Consistency should not be considered.
		- a trait of this choice is the [[2-Phase Commit]] , although it supports temporarily partitions (node crashes, lost messages) by waiting until all messages are received.