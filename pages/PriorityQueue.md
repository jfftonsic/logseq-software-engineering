tags:: java, [[Java Collections]]

- #+BEGIN_CAUTION
  When you are debugging, the inner structure of the queue may not be in your expected order.
  But if you do `poll` operations, the objects returned will be in your desired order!
  **Spliterating or Iterating over it also does not retrieve element in order!!**
  It is written in `PriorityQueue`'s JavaDoc.
  #+END_CAUTION