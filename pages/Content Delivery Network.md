alias:: CDN
sources:: https://github.com/donnemartin/system-design-primer#content-delivery-network

- Short description
	- A globally distributed network of proxy servers, serving content from locations closer to the user. Generally, static files, but sometimes also some dynamic content.
- Purpose
	- Performance improvement
		- Users receive content from data centers close to them
		- Your servers do not have to serve requests that the CDN fulfills
- Disadvantages
	- CDN costs could be significant depending on traffic
		- but you should take into account the costs you would incur not using a CDN.
	- Content might be <span class="hl-neutral-01">stale</span> if it is updated before the TTL expires it.
	- CDNs <span class="hl-neutral-01">require changing URLs for static content to point to the CDN</span>.
- Types
	- [[Push CDN]]
	- [[Pull CDN]]
	-