sources:: https://ebrary.net/64710/computer_science/consistent_prefix_reads

- short description:
	- if a sequence of writes happens in a certain order, then anyone reading those writes will see them appear in the same order.
	  id:: 62571d0e-bbb7-4c34-9275-24acc8c004ca
- guarantees no replica reads data out of order even though the data read is stale at a given point in time
	- every replica receives these updates in the same order
	- At time t1, if a replica serves version A of a data, at time t2 > t1, it’ll either serve version A or higher if a newer version of the data gets replicated but never any lesser one.
- Possible anomalies
	- Dirty Read / Stale Read is possible.
- No bound on staleness.
- [[Data-Centric Perspective]]
- Real Life Examples:
	- Sports apps which track score of soccer, cricket etc.
	- Social media timelines (sorted by recency).