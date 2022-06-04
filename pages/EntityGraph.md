-
- independent of the query and defines which attributes to fetch from the database.
- can be used as a fetch or a load graph.
- fetch graph
	- only the attributes specified by the entity graph will be treated as FetchType.EAGER. All other attributes will be lazy.
- load graph
	- all attributes that are not specified by the entity graph will keep their default fetch type.
- big example
  collapsed:: true
	- order with a list of items and each item has a product. All relations are lazy.
	- Order class
	  collapsed:: true
		- ```java
		  @Entity
		  @Table(name = "purchaseOrder")
		  public class Order {
		   
		     @Id
		     @GeneratedValue
		     private Long id;
		   
		     @Version
		     private int version;
		   
		     private String orderNumber;
		   
		     @OneToMany(mappedBy = "order", fetch = FetchType.LAZY)
		     private Set<OrderItem> items = new HashSet<OrderItem>();
		   
		     ...
		  }
		  ```
	- OrderItem class
	  collapsed:: true
		- ```java
		  @Entity
		  public class OrderItem {
		   
		     @Id
		     @GeneratedValue
		     private Long id;
		   
		     @Version
		     private int version;
		   
		     private int quantity;
		   
		     @ManyToOne
		     private Order order;
		   
		     @ManyToOne(fetch = FetchType.LAZY)
		     private Product product;
		   
		     ...
		  }
		  ```
	- Product class
	  collapsed:: true
		- ```java
		  @Entity
		  public class Product implements Serializable
		  {
		   
		     @Id
		     @GeneratedValue
		     private Long id;
		   
		     @Version
		     private int version;
		   
		     private String name;
		   
		     ...
		  }
		  ```
	- entity graph graph.Order.items
	  collapsed:: true
		- load the list of OrderItem of an Order.
		- ```java
		  @Entity
		  @Table(name = "purchase_order")
		  @NamedEntityGraph(name = "graph.Order.items", 
		        attributeNodes = @NamedAttributeNode("items"))
		  public class Order { ... }
		  ```
	- after defining the entity graph, to use it in a query
	  collapsed:: true
		- ```java
		  EntityGraph graph = this.em.getEntityGraph("graph.Order.items");
		   
		  Map hints = new HashMap();
		  hints.put("javax.persistence.fetchgraph", graph);
		   
		  return this.em.find(Order.class, orderId, hints);
		  ```
	- sub-graph
		- to fetch an Order with all OrderItems and their Products
		- ```java
		  @Entity
		  @Table(name = "purchase_order")
		  @NamedEntityGraph(name = "graph.Order.items", 
		                 attributeNodes = @NamedAttributeNode(value = "items", subgraph = "items"), 
		                 subgraphs = @NamedSubgraph(name = "items", attributeNodes = @NamedAttributeNode("product")))
		  public class Order { ... }
		  ```
- Why using entity graphs instead of JOIN FETCH's on plain jpql? [*](https://stackoverflow.com/questions/56386584/jpql-join-fetch-vs-entitygraph)
	- #+BEGIN_QUOTE
	  EntityGraph are evaluated compile time and it is less error prone as it is tightly coupled with the Entity definition.
	  #+END_QUOTE
	- self-note: I don't know about 'compile time' since the attributes are defined through strings.. I'm not yet convinced that named graphs are a better solution, it increases verbosity