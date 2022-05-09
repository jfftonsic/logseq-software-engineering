sources:: https://www.geeksforgeeks.org/difference-between-clustered-and-non-clustered-index, https://www.guru99.com/clustered-vs-non-clustered-index.html

- Cluster index is a type of index that sorts the data rows in the table on their key values whereas the Non-clustered index stores the data at one location and indices at another location.
- Clustered index stores data pages in the leaf nodes of the index while Non-clustered index method never stores data pages in the leaf nodes of the index.
- Cluster index doesn’t require additional disk space whereas the Non-clustered index requires additional disk space.
- Cluster index offers faster data accessing, on the other hand, Non-clustered index is slower.
- | Parameters            | Clustered                                                                                 | Non-clustered                                                                                                       |
  |-----------------------|-------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------|
  | Use for               | You can sort the records and store clustered index physically in memory as per the order. | A non-clustered index helps you to creates a logical order for data rows and uses pointers for physical data files. |
  | Storing method        | Allows you to stores data pages in the leaf nodes of the index.                           | This indexing method never stores data pages in the leaf nodes of the index.                                        |
  | Size                  | The size of the clustered index is quite large.                                           | The size of the non-clustered index is small compared to the clustered index.                                       |
  | Data accessing        | Faster                                                                                    | Slower compared to the clustered index                                                                              |
  | Additional disk space | Not Required                                                                              | Required to store the index separately                                                                              |
  | Type of key           | By Default Primary Keys Of The Table is a Clustered Index.                                | It can be used with unique constraint on the table which acts as a composite key.                                   |
  | Main feature          | A clustered index can improve the performance of data retrieval.                          | It should be created on columns which are used in joins.                                                            |
- |Clustered index|Non-clustered index|
  |--|--|
  |sorts the data rows in the table on their key values|stores the data at one location and indices at another location.|
  |stores data pages in the leaf nodes of the index|never stores data pages in the leaf nodes of the index.|
  |doesn’t require additional disk space|requires additional disk space.|
  |faster data accessing|slower data accessing|
  |Default and sorted data storage|Store key values only. Pointers to Heap/Clustered Index rows|
  |Helps you to store Data and index together|Bridge to the data|
- Advantages of Clustered Index
	- *   Clustered indexes are an ideal option for range or group by with max, min, count type queries
	  *   In this type of index, a search can go straight to a specific point in data so that you can keep reading sequentially from there.
	  *   Clustered index method uses location mechanism to locate index entry at the start of a range.
	  *   It is an effective method for range searches when a range of search key values is requested.
	  *   Helps you to minimize page transfers and maximize the cache hits.
- Advantages of Non-clustered index
	- *   A non-clustering index helps you to retrieves data quickly from the database table.
	  *   Helps you to avoid the overhead cost associated with the clustered index
	  *   A table may have multiple non-clustered indexes in RDBMS. So, it can be used to create more than one index.
- Disadvantages of Clustered Index
	- *   Lots of inserts in non-sequential order
	  *   A clustered index creates lots of constant page splits, which includes data page as well as index pages.
	  *   Extra work for SQL for inserts, updates, and deletes.
	  *   A clustered index takes longer time to update records when the fields in the clustered index are changed.
	  *   The leaf nodes mostly contain data pages in the clustered index.
- Disadvantages of Non-clustered index
	- *   A non-clustered index helps you to stores data in a logical order but does not allow to sort data rows physically.
	  *   Lookup process on non-clustered index becomes costly.
	  *   Every time the clustering key is updated, a corresponding update is required on the non-clustered index as it stores the clustering key.