- Short description
	- Master serves reads and writes, replicating writes to one or more slaves, which serve only reads.
- Slaves can also replicate to additional slaves in a tree-like fashion.
- If the master goes offline
	- the system can continue to operate in read-only mode until
		- a slave is promoted to a master
		- or a new master is provisioned.
- Disadvantages
	- Additional logic is needed to promote a slave to a master.
	  background-color:: #793e3e
	- And the general disadvantages of [[replication]]
	  background-color:: #793e3e