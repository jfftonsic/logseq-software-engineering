sources:: https://thorben-janssen.com/self-referencing-associations/
tags:: hibernate

-
- model through
	- a foreign key column that references the same table’s primary key.
- uni- or bidirectional many-to-many association
- A unidirectional mapping
	- only models the association in one direction
		- e.g., from a child to their parents
	- are easier to update
- A bidirectional mapping
	- from the uni-directional example, a bi-directional would also model the association from each parent to their children.
	- easier to use in your queries and business code
	- developers usually prefer bidirectional because, for most applications, the number and complexity of read operations are a lot higher than for write operations.
	- requires you to update both ends of your association
	  id:: 628e493a-ceec-4221-a276-f3da3f8bbcfa
		- code example
		  collapsed:: true
			- ```java
			  @Entity
			  public class Person {
			   
			      ...
			       
			      public void addParents(Person parent1, Person parent2) {
			          if (!this.parents.isEmpty()) {
			              throw new IllegalArgumentException();
			          }
			   
			          this.parents.add(parent1);
			          parent1.getChildren().add(this);
			          this.parents.add(parent2);
			          parent2.getChildren().add(this);
			      }
			    
			      public Person createChild(String firstName, String lastName, Person parent2) {
			          Person child = new Person();
			          child.setFirstName(firstName);
			          child.setLastName(lastName);
			          this.children.add(child);
			          child.getParents().add(this);
			   
			          if (parent2 != null) {
			              parent2.getChildren().add(child);
			              child.getParents().add(parent2);
			          }
			          return child;
			      }
			  }
			  
			  @Entity
			  public class Category {
			   
			      ...
			       
			      public Category addSubCategory(String categoryName) {
			          Category sub = new Category();
			          sub.setName(categoryName);
			          this.subCategories.add(sub);
			          sub.setParentCategory(this);
			          return sub;
			      }
			    
			      public void moveCategory(Category newParent) {
			          this.getParentCategory().getSubCategories().remove(this);
			          this.setParentCategory(newParent);
			          newParent.getSubCategories().add(this);
			      }
			  }
			  ```
- Example code for bi-directional many-to-many association
  collapsed:: true
	- ```java
	  @Entity
	  public class Person {
	       
	      @Id
	      @GeneratedValue(strategy = GenerationType.SEQUENCE)
	      private Long id;
	   
	      private String name;
	   
	      @ManyToMany
	      private Set<Person> parents = new HashSet<>();
	   
	      @ManyToMany(mappedBy = "parents")
	      private Set<Person> children = new HashSet<>();
	   
	      ...
	  }
	  ```
- Example code for bi-directional many-to-one and one-to-many association
  collapsed:: true
	- ```java
	  @Entity
	  public class Category {
	       
	      @Id
	      @GeneratedValue(strategy = GenerationType.SEQUENCE)
	      private Long id;
	   
	      private String name;
	   
	      @ManyToOne(fetch = FetchType.LAZY)
	      private Category parentCategory;
	   
	      @OneToMany(mappedBy = "parentCategory")
	      private Set<Category> subCategories = new HashSet<>();
	       
	      ...
	  }
	  ```
- Pitfalls
  heading:: true
- Forgetting to update all ends of the association
  collapsed:: true
	- {{embed ((628e493a-ceec-4221-a276-f3da3f8bbcfa))}}
- Fetching behavior
	- You should always use FetchType.LAZY for all of your associations.
		- is the default for all to-many associations
		- but you need to declare it all of your to-one associations.
	- If you have a hierarchical model and use EAGER, Hibernate will try to fetch all elements of your hierarchy.
- When performing queries on your entity hierarchy
	- make sure to provide an index for your foreign key column
	- If your application requires JOINs over a huge number of hierarchy levels, you might consider using a [[graph database]].
- Initializing Self-Referencing Associations
	- JOIN FETCH clauses and [[EntityGraph]]s enable you to avoid [[n+1 select issue]]s and to initialize your association efficiently.
	- If you use multiple JOIN FETCH clauses or complex EntityGraphs, your SQL query returns a huge product, which often slows down the app.
		- split your query into multiple ones