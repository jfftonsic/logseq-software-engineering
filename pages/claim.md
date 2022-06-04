- Context of [[JWT]]
	- Short description
		- a statement about an entity (typically, the user), as well as additional metadata about the token itself. The claim contains the information we want to transmit, and that the server can use to properly handle [[JWT]] authentication.
	- There are multiple claims we can provide
		- Registered JWT Claims
			- registered in the [IANA JSON Web Token Claims registry](https://tools.ietf.org/html/draft-ietf-oauth-json-web-token-32#section-10.1).
			- are not intended to be mandatory but rather to provide a starting point for a set of useful, interoperable claims.
			- e.g.
				- iss: The issuer of the token
				  sub: The subject of the token
				  aud: The audience of the token
				  exp: JWT expiration time defined in Unix time
				  nbf: “Not before” time that identifies the time before which the JWT must not be accepted for processing
				  iat: “Issued at” time, in Unix time, at which the token was issued
				  jti: JWT ID claim provides a unique identifier for the JWT
		- Public Claims
			- need to have collision-resistant names.
				- By making the name a URI or URN, naming collisions are avoided for JWTs where the sender and receiver are not part of a closed network.
			- e.g.: https://www.toptal.com/jwt_claims/is_admin, and the best practice is to place a file at that location describing the claim so that it can be dereferenced for documentation.
		- Private Claims
			- may be used in places where JWTs are only exchanged in a closed environment between known systems, such as inside an enterprise.
			- We can define ourselves, like user IDs, user roles, or any other information.
			- Using claim-names that might have conflicting semantic meanings outside of a closed or private system are subject to collision, so use them with caution.
		- It is important to note that we want to <span style="color: orange">keep a web token as small as possible</span>
			- use only necessary data inside public and private claims.