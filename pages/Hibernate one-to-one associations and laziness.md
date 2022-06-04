sources:: https://thorben-janssen.com/hibernate-tip-lazy-loading-one-to-one/

- depends on the mapping of the relationship and the Hibernate version
- when modeling the association using
	- independent primary key columns for both tables
	- additional foreign key column in one of the tables
- owning side of the association
	- The entity that represents the table containing the foreign key column
	- On this entity, Hibernate supports lazy loading as expected.
		- just set the fetch attribute of the @OneToOne association to FetchType.LAZY.
- if you model this as a bidirectional association
	- Hibernate always fetches the other end of the association eagerly
		- why?
			- Hibernate needs to know if it shall initialize the manuscript attribute with null or a proxy class.
	- With some Hibernate versions
	  collapsed:: true
		- you can set the optional attribute of the @OneToOne annotation to false to avoid the eager fetching.
			- what does Hibernate do then?
				- always initializes the referenced attribute with a proxy object.
		- #+BEGIN_CAUTION
		  Avoid using these types of obscure, version-dependent, workarounds.
		  #+END_CAUTION
	- Example code
		- ```java
		  @Entity
		  public class Book {
		   
		      @Id
		      @GeneratedValue
		      private Long id;
		   
		      @OneToOne(mappedBy = "book", fetch = FetchType.LAZY)
		      private Manuscript manuscript;
		   
		      ...
		  }
		  ```
- One possible solution
	- get rid of the foreign key column by using the same primary key value for both associated entities.
	- annotating the owning side of the association with @MapsId.
	- The shared primary key allows you to model the relationship as a unidirectional one.
	- You no longer need the referencing side of the association.
	- example code
		- ```java
		  @Entity
		  public class Manuscript {
		   
		      @Id
		      private Long id;
		   
		      @OneToOne
		      @MapsId
		      @JoinColumn(name = "id")
		      private Book book;
		   
		      ...
		  }
		  ```