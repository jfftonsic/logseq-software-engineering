- Creating arrays for generic types
  background-color:: #533e7d
  collapsed:: true
	- for example
	- ```java
	  public class A<TYPE> {
	    TYPE[] typeArray;
	    
	    {
	      // the following line is not allowed
	      typeArray = new TYPE[10];
	      
	      // but the next is
	      typeArray = (TYPE[]) new Object[10];
	    }
	    
	  }
	  ```
- How to use for each loop in a collection that you implemented?
  background-color:: #533e7d
  collapsed:: true
	- Make your collection `Iterable` and implement your iterator.
- Comparator
  heading:: true
  collapsed:: true
	- If you need a simple comparator for two instances of an arbitrary class
		- `Comparator.comparing(x -> x.get(0)) // in this ex, x is a list, but you get the drift`
- Functional
  heading:: true
  collapsed:: true
	- Function chaining
		- ```java
		  Function<String, Integer> func = x -> x.length();
		  Function<Integer, Integer> func2 = x -> x * 2;
		  Integer result = func.andThen(func2).apply("abc");   // 6
		  ```
	- Reduce
	  collapsed:: true
		- **identity**: initial value or default if no elements in the stream
		  **accumulator**: takes two parameters: a partial result of the reduction (e.g. the sum of all processed integers so far) and the next element of the stream. It returns a new partial result.
		  **combiner**: aggregates the results of the accumulator. Seems like this is only called for parallel streams.
		  ```java
		  int total = numbers.stream().reduce(0, (a,b) -> a+b);
		  ```
	- Collect
		- Counting elements on stream
			- ```java
			  Stream.of("","").count(); // mapToLong(e -> 1L).sum()
			  Stream.of("","").collect(Collectors.counting()); // reducing(0L, e -> 1L, Long::sum)
			  ```
		- Joining to a String
			- ```java
			  String listToString = productList.stream().map(Product::getName)
			    .collect(Collectors.joining(", ", "[", "]"));
			  ```
		- Group by
			- Simplest
				- ```java
				  Map<Integer, List<Product>> collectorMapOfLists = productList.stream()
				    			.collect(Collectors.groupingBy(Product::getPrice));
				  ```
			- Aggregating over a value
				- ```java
				  ...collect(
				    Collectors.groupingBy(
				            Employee::getCity, 
				            Collectors.reducing(0, Employee::getSalary, Integer::sum)
				  )
				  ); // returns Map<String, Integer>
				  
				  ...collect(
				           Collectors.groupingBy(
				  			  Employee::getCity,
				  			  Collectors.averagingInt(Employee::getAge) // summingInt, etc.
				  		 )
				  ); // returns Map<String, Double>
				  ```
			- Mapping objects by something to lists of something and thus allowing for repeated keys in a way
				- ```java
				  ...collect(
				           Collectors.groupingBy(
				  		   Employee::getCity,
				  		   Collectors.mapping(Employee::getAge, Collectors.toList())
				           )
				  ); // returns Map<String, List<Integer>>
				  ```
				  Output example: `{Elgin=[35, 30], Schaumburg=[35, 31]}`
		- Partition
			- ```java
			  Map<Boolean, List<Product>> mapPartioned = productList.stream()
			    .collect(Collectors.partitioningBy(element -> element.getPrice() > 15));
			  ```
		-
		-
- Collections
  heading:: true
  collapsed:: true
	- Set intersection (or lots of other collections also)
		- `retailAll` method
- Streams
  id:: 627033ae-c7a9-403b-9741-60b0f87d1a6b
  heading:: true
  collapsed:: true
	- Concatenation of streams
		- `Stream.concat(...)`
	- Stream creation
		- ```java
		  Stream<String> streamEmpty = Stream.empty();
		  Stream<String> streamOfArray = Stream.of("a", "b", "c");
		  
		  String[] arr = new String[]{"a", "b", "c"};
		  Stream<String> streamOfArrayFull = Arrays.stream(arr);
		  Stream<String> streamOfArrayPart = Arrays.stream(arr, 1, 3);
		  
		  Stream<String> streamBuilder = Stream.<String>builder().add("a").add("b").add("c").build();
		  
		  Stream<String> streamGenerated = Stream.generate(() -> "element").limit(10)
		  
		  Stream<Integer> streamIterated = Stream.iterate(40, n -> n + 2).limit(20);
		  
		  IntStream intStream = IntStream.range(1, 3);
		  LongStream longStream = LongStream.rangeClosed(1, 3);
		  
		  Random random = new Random();
		  DoubleStream doubleStream = random.doubles(3);
		  
		  IntStream streamOfChars = "abc".chars();
		  
		  Stream<String> streamOfString =
		    Pattern.compile(", ").splitAsStream("a, b, c");
		    
		  Path path = Paths.get("C:\\file.txt");
		  Stream<String> streamOfStrings = Files.lines(path);
		  Stream<String> streamWithCharset = 
		  Files.lines(path, Charset.forName("UTF-8"));
		  ```
	- Matching, returns boolean
		- [all | any | none]match(predicate) -> boolean
	- Finding, returns element
		- find[First | Any]()
	- Sorting
		- sorted([comparator])
	- toArray
	- count / distinct / max / min
	- dropWhile(predicate) / takeWhile(predicate) / limit / skip
	- forEach[Ordered] / peek
	- Mapping, flat mapping, multi mapping
	  collapsed:: true
		- [flat]map[Multi][To[Int | Long | Double]]
		- flatMap
			- One-to-many transformation to the elements of the stream, and then flattening the resulting elements into a new stream.
		- mapMulti
			- Returns a stream consisting of the results of replacing each element of this stream with zero or more elements.
			- Replacement is performed by applying the provided mapping function to each element in conjunction with a consumer argument that accepts replacement elements.
			- The mapping function calls the consumer zero or more times to provide the replacement elements.
			- Complex example:
				- If we have an Iterable<Object> and need to recursively expand its elements that are themselves of type Iterable, we can use mapMulti as follows:
				  ```java
				   class C {
				     static void expandIterable(Object e, Consumer<Object> c) {
				         if (e instanceof Iterable) {
				             for (Object ie: (Iterable<?>) e) {
				                 expandIterable(ie, c);
				             }
				         } else if (e != null) {
				             c.accept(e);
				         }
				     }
				  public static void main(String[] args) {
				         Stream<Object> stream = ...;
				         Stream<Object> expandedStream = stream.mapMulti(C::expandIterable);
				     }
				   }
				  ```
	- onClose(Runnable)
		- Returns an equivalent stream with an additional close handler.
		  Close handlers are run when the close() method is called on the stream, and are executed in the order they were added.
		  All close handlers are run, even if earlier close handlers throw exceptions.
		  If any close handler throws an exception, the first exception thrown will be relayed to the caller of close(), with any remaining exceptions added to that exception as suppressed exceptions (unless one of the remaining exceptions is the same exception as the first exception, since an exception cannot suppress itself.) May return itself.
		  This is an intermediate operation.
	- teeing, since java 12
		- returns a Collector which <span class="hl-neutral-01">aggregates the results</span> of two supplied collectors.