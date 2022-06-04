- Short description
	- Benchmarking, profiling and making changes in queries and in the database to minimize bottlenecks, maximize performance and resource consumption.
-
- It's important to benchmark and profile to simulate and uncover bottlenecks.
	- Benchmark - Simulate high-load situations with tools such as ab.
	- Profile - Enable tools such as the slow query log to help track performance issues.
- Benchmarking and profiling might point you to the following optimizations.
- Tighten up the schema
  heading:: true
	- Set the NOT NULL constraint where applicable to improve search performance.
	- Use DECIMAL for currency to avoid floating point representation errors.
	- Use CHAR instead of VARCHAR for fixed-length fields.
	- CHAR effectively allows for fast, random access, whereas with VARCHAR, you must find the end of a string before moving onto the next one.
	- Avoid storing large BLOBS
		- store the location of where to get the object instead.
	- Use TEXT for large blocks of text such as blog posts.
	- TEXT also allows for boolean searches.
	- Using a TEXT field results in storing a pointer on disk that is used to locate the text block.
	- Use INT for larger numbers up to 2^32 or 4 billion.
	- VARCHAR(255) is the largest number of characters that can be counted in an 8 bit number, often maximizing the use of a byte in some RDBMS.
	- MySQL dumps to disk in contiguous blocks for fast access.
- Use good indices
  heading:: true
	- When loading large amounts of data, it might be faster to disable indices, load the data, then rebuild the indices.
	- Columns that you are querying (SELECT, GROUP BY, ORDER BY, JOIN) could be faster with indices.
	- Indices are usually represented as self-balancing B-tree
		- keeps data sorted and allows searches, sequential access, insertions, and deletions in logarithmic time.
	- Placing an index can
		- keep the data in memory, requiring more space.
		- make writes slower, since the index also needs to be updated.
- Avoid expensive joins
  heading:: true
	- [Denormalize]([[denormalization]]) where performance demands it.
- Partition tables
  heading:: true
	- Break up a table by putting hot spots in a separate table to help keep it in memory.
- Tune the query cache
  heading:: true
	- In some cases, the query cache could lead to performance issues.
-
- Links
  heading:: true
	- [Tips for optimizing MySQL queries](http://aiddroid.com/10-tips-optimizing-mysql-queries-dont-suck/)
	- [Is there a good reason i see VARCHAR(255) used so often?](http://stackoverflow.com/questions/1217466/is-there-a-good-reason-i-see-varchar255-used-so-often-as-opposed-to-another-l)
	- [How do null values affect performance?](http://stackoverflow.com/questions/1017239/how-do-null-values-affect-performance-in-a-database-search)
	- [Slow query log](http://dev.mysql.com/doc/refman/5.7/en/slow-query-log.html)