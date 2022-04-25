sources:: https://docs.microsoft.com/en-us/azure/architecture/best-practices/api-design#uri-versioning, https://realpython.com/api-integration-in-python/

- Endpoints for simple resource
  heading:: true
  collapsed:: true
	- | HTTP method | API endpoint                          | Description                      |
	  |-------------|---------------------------------------|----------------------------------|
	  | GET         | /transactions                         | Get a list of transactions\.     |
	  | GET         | /transactions/<transaction\_id> | Get a single transaction\.       |
	  | POST        | /transactions                         | Create a new transaction\.       |
	  | PUT         | /transactions/<transaction\_id> | Update a transaction\.           |
	  | PATCH       | /transactions/<transaction\_id> | Partially update a transaction\. |
	  | DELETE      | /transactions/<transaction\_id> | Delete a transaction\.           |
- Endpoints for a nested resource
  heading:: true
  collapsed:: true
	- | HTTP method | API endpoint                           | Description                |
	  |-------------|----------------------------------------|----------------------------|
	  | GET         | /events/<event\_id>/guests             | Get a list of guests\.     |
	  | GET         | /events/<event\_id>/guests/<guest\_id> | Get a single guest\.       |
	  | POST        | /events/<event\_id>/guests             | Create a new guest\.       |
	  | PUT         | /events/<event\_id>/guests/<guest\_id> | Update a guest\.           |
	  | PATCH       | /events/<event\_id>/guests/<guest\_id> | Partially update a guest\. |
	  | DELETE      | /events/<event\_id>/guests/<guest\_id> | Delete a guest\.           |
- Versioning Strategies
  heading:: true
  collapsed:: true
	- URI versioning
	  heading:: true
		- `https://.../v2/...`
		- complicates implementation of HATEOAS because all links will need to include the version number in their URLs
		- become unwieldy as the web API matures through several iterations and the server has to support a number of different versions.
	- Query string versioning
	  heading:: true
		- `https://.../bla?version=2`
		- semantic advantage that the same resource is always retrieved from the same URI, but it depends on the code that handles the request to parse the query string and send back the appropriate HTTP response
		- suffers from the same complications for implementing HATEOAS as the URI versioning mechanism.
	- HTTP header versioning
	  heading:: true
		- ```
		  GET https://adventure-works.com/customers/3 HTTP/1.1
		  Custom-Header: api-version=1
		  ```
		- As with the previous two approaches, implementing HATEOAS requires including the appropriate custom header in any links.
		- issue: caching
			- typically require additional logic to examine the values in the custom header (which impacts the response content and thus what should be cached)
	- Media type versioning
	  heading:: true
		- ```
		  request
		  
		  GET https://adventure-works.com/customers/3 HTTP/1.1
		  Accept: application/vnd.adventure-works.v1+json
		  
		  response
		  
		  HTTP/1.1 200 OK
		  Content-Type: application/vnd.adventure-works.v1+json; charset=utf-8
		  
		  {"id":3,"name":"Contoso LLC","address":"1 Microsoft Way Redmond WA 98053"}
		  ```
		- issue: the client application may specify multiple formats in the Accept header
			- in which case the web server can choose the most appropriate format for the response body
		- issue: caching
			- typically require additional logic to examine the values in the Accept header (which impacts the response content and thus what should be cached)
		- arguably the purest of the versioning mechanisms and lends itself naturally to HATEOAS, which can include the MIME type of related data in resource links.