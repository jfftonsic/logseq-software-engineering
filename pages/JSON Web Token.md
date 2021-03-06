sources:: toptal.com/web/cookie-free-authentication-with-json-web-tokens-an-example-in-laravel-and-angularjs, https://jwt.io/
alias:: JWT

- Short description
	- Used to send information that can be verified and trusted by means of a digital signature. It comprises a compact and URL-safe JSON object, which is cryptographically signed to verify its authenticity, and which can also be encrypted if the payload contains sensitive information. Usually used in HTTP Authorization headers or URL query parameters.
	  id:: 6298fba5-d08f-49b6-9cae-7632c93b95fb
- Representation
	- a sequence of base64url encoded values that are separated by period characters.
	- e.g. `eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ0b3B0YWwuY29tIiwiZXhwIjoxNDI2NDIwODAwLCJodHRwOi8vdG9wdGFsLmNvbS9qd3RfY2xhaW1zL2lzX2FkbWluIjp0cnVlLCJjb21wYW55IjoiVG9wdGFsIiwiYXdlc29tZSI6dHJ1ZX0.yRQYnWzskCZUxPwaQupWkiUzKELZ49eM7oWxAQK_ZXw`
- Diagram
  collapsed:: true
	- ![image.png](../assets/image_1654193201121_0.png)
- Structure
  collapsed:: true
	- Header
		- ```json
		  {
		    "alg": "HS256", // encryption algorithm, in this case, HMAC SHA-256
		    "typ": "JWT" // type of signature
		  }
		  ```
	- Payload ([Claims]([[claim]]))
		- ```json
		  {
		    "iss": "toptal.com",
		    "exp": 1426420800,
		    "https://www.toptal.com/jwt_claims/is_admin": true,
		    "company": "Toptal",
		    "awesome": true
		  }
		  ```
	- Signature
		- generated by combining the encoded JWT Header and the encoded JWT Payload, and signing it using a strong encryption algorithm, such as HMAC SHA-256.
		- The signature’s secret key is held by the server so it will be able to verify existing tokens and sign new ones.
		- generation e.g.
			- ```
			  $encodedContent = base64UrlEncode(header) + "." + base64UrlEncode(payload);
			  $signature = hashHmacSHA256($encodedContent);
			  ```
- It is critical to use TLS/SSL in conjunction with JWT
	- prevent man-in-the-middle attacks.
- 2 options for dealing with data from JWT payload
	- if we want to add an additional layer of protection
		- encrypt the JWT payload itself using the [[JSON Web Encryption]] ([[JWE]]) specification.
	- or avoid additional overhead of using JWE by keeping sensitive information in the database, and use our token for additional API calls to the server whenever we need to access sensitive data.
- Token based/[[JWT]] authentication is stateless
	- no need to store user information in the session.
	- This gives us the ability to scale our application without worrying where the user has logged in.
	- We can easily use the same token for fetching a secure resource from a domain other than the one we are logged in to.
- How JSON Web Tokens Work
	- A browser or mobile client makes a request to the authentication server containing user login information.
	- The authentication server generates a new JWT access token and returns it to the client.
	- On every request to a restricted resource, the client sends the access token in the query string or Authorization header.
	- The server then validates the token and, if it’s valid, returns the secure resource to the client.
	- The authentication server can sign the token using any secure signature method.
		- For example, a symmetric key algorithm such as HMAC SHA-256 can be used if there is a secure channel to share the secret key among all parties. Alternatively, an asymmetric, public-key system, such as RSA, can be used as well, eliminating the need for further key-sharing.
- Advantages
	- Stateless, easier to scale
		- The token contains all the information to identify the user, eliminating the need for the session state.
		- If we use a load balancer, we can pass the user to any server, instead of being bound to the same server we logged in on.
	- Reusability
		- We can have many separate servers, running on multiple platforms and domains, reusing the same token for authenticating the user.
		- It is easy to build an application that shares permissions with another application.
	- JWT Security
		- Since we are not using cookies, we don’t have to protect against cross-site request forgery (CSRF) attacks.
		- We should still encrypt our tokens using JWE if we have to put any sensitive information in them, and transmit our tokens over HTTPS to prevent man-in-the-middle attacks.
	- Performance
		- There is no server side lookup to find and deserialize the session on each request.
			- The only thing we have to do is calculate the HMAC SHA-256 to validate the token and parse its content.