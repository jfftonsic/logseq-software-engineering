- #+BEGIN_CAUTION
  `NavigableSet.add(...)` **does NOT replace** an object if it is considered equal to an object that is already in the list. From javadoc:
  > Adds the specified element to this set if it is not already present (optional operation). 
  #+END_CAUTION