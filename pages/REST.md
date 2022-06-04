sources:: https://restfulapi.net/

- Short description
	- Acronym for REpresentational State Transfer,  an architectural style for distributed hypermedia systems that has its guiding principles and constraints.
- [[RESTful]]
	- ((627e9a70-ab24-4b32-a3ce-e4597e8daaf2))
- Terms
  heading:: true
	- _resource_
	  id:: 627e9b34-0dbc-441f-9319-ffde46d57b4f
		- The key abstraction of information in REST.
		- Any information that we can name.
	- _resource identifier_
	- _resource representation_
		- The state of a resource, at any particular time.
		- Consisted of
			- the data
			- the metadata describing the data
			- the hypermedia links that can help the clients in transition to the next desired state.
	- _resource model_
		- A REST API consists of an assembly of interlinked resources. This set of resources is known as the REST API’s resource model.
	- _resource methods_
		- GET/PUT/POST/DELETE/etc
			- but [this page](https://restfulapi.net/) mentions
				- #+BEGIN_QUOTE
				  A large number of people wrongly relate resource methods to HTTP methods (i.e., GET/PUT/POST/DELETE). Roy Fielding has never mentioned any recommendation around which method to be used in which condition. All he emphasizes is that it should be a uniform interface.
				  #+END_QUOTE
			- But in practice, for almost all Restful API's, resource methods really mean  the HTTP methods...
- Guiding Principles
  heading:: true
	- Uniform Interface
	  heading:: true
		- [[principle of generality]]
		- Identification of ((627e9b34-0dbc-441f-9319-ffde46d57b4f))s
		  heading:: true
			- The interface must uniquely identify each ((627e9b34-0dbc-441f-9319-ffde46d57b4f)) involved in the interaction between the client and the server.
		- Manipulation of ((627e9b34-0dbc-441f-9319-ffde46d57b4f))s through representations
		  heading:: true
			- The ((627e9b34-0dbc-441f-9319-ffde46d57b4f))s should have uniform representations in the server response.
			- API consumers should use these representations to modify the ((627e9b34-0dbc-441f-9319-ffde46d57b4f))s state in the server.
		- Self-descriptive messages
		  heading:: true
			- Each ((627e9b34-0dbc-441f-9319-ffde46d57b4f)) representation should:
				- carry enough information to describe how to process the message.
				- provide information of the additional actions that the client can perform on the ((627e9b34-0dbc-441f-9319-ffde46d57b4f)).
		- Hypermedia as the engine of application state
		  heading:: true
			- The client should have only the initial URI of the application.
			- The client application should dynamically drive all other ((627e9b34-0dbc-441f-9319-ffde46d57b4f))s and interactions with the use of hyperlinks.
	- Client-Server
	  heading:: true
		- separation of concerns
			- helps them evolve independently
		- improves
			- the portability of the user interface across multiple platforms
			- the scalability by simplifying the server components.
		- make sure that the interface/contract between the client and the server does not break
	- Stateless
	  heading:: true
		- each request from the client to the server must contain all of the information necessary to understand and complete the request.
		- The server cannot take advantage of any previously stored context information on the server.
		- The client application must entirely keep the session state.
	- Cacheable
	  heading:: true
		- a response should implicitly or explicitly label itself as cacheable or non-cacheable.
		- If the response is cacheable, the client application gets the right to reuse the response data later for equivalent requests and a specified period.
	- Layered System
	  heading:: true
		- allows an architecture to be composed of hierarchical layers by constraining component behavior.
		- each component cannot see beyond the immediate layer they are interacting with
	- Code on Demand (Optional)
	  heading:: true
		- allows client functionality to extend by downloading and executing code in the form of applets or scripts.
		- The downloaded code simplifies clients by reducing the number of features required to be pre-implemented.
		- Servers can provide part of features delivered to the client in the form of code, and the client only needs to execute the code.
	- Self-Descriptive
	  heading:: true
		- The client does not need to know if a resource is an employee or a device.
		- It should act based on the media type associated with the resource.
		- In practice, we will create lots of custom media types
			- usually one media type associated with one resource.
		- Every media type defines a default processing model.
			- For example
				- HTML defines a rendering process for hypertext and the browser behavior around each element.
- Resources
  heading:: true
  id:: 62793ca4-3063-43ea-a2fd-8e64cb2803a6
	- Martin Fowler article - Richardson Maturity Model
		- #+BEGIN_QUOTE
		  A model (developed by Leonard Richardson) that breaks down the principal elements of a REST approach into three steps. These introduce resources, http verbs, and hypermedia controls.
		  #+END_QUOTE
		- https://martinfowler.com/articles/richardsonMaturityModel.html