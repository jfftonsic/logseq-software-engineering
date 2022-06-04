- Short description
	- A database optimization technique in which redundant copies of the data are written in multiple tables to avoid expensive joins.
- attempts to improve read performance at the expense of some write performance.
- [[materialized view]]
- Once data becomes distributed with techniques such as [[federation]] and [[sharding]], managing joins across data centers further increases complexity. Denormalization might circumvent the need for such complex joins.
- In most systems, reads can heavily outnumber writes 100:1 or even 1000:1.
	- A read resulting in a complex database join can be very expensive, spending a significant amount of time on disk operations.
- Advantages
	- Circumvent the need for complex joins and increase the read operations for those complex queries that can retrieve all the data they need from fewer or even maybe 1 table or view.
	  background-color:: #497d46
- Disadvantages
	- Data is duplicated.
	  background-color:: #793e3e
	- Constraints can help redundant copies of information stay in sync, which increases complexity of the database design.
	  background-color:: #793e3e
	- Under heavy write load might perform worse than its normalized counterpart.
	  background-color:: #793e3e