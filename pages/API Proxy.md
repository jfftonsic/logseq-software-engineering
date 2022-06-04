sources:: https://heynode.com/tutorial/what-api-proxy/, https://www.techtarget.com/searchapparchitecture/tip/Getting-to-know-the-API-proxy
tags:: API

- Short description
	- A thin API server that exposes a stable interface for existing services.
	  id:: 62829938-0e63-4541-992f-8af9bd6da629
- can be considered a subset of the [[API Gateway]] pattern[*](https://www.techtarget.com/searchapparchitecture/tip/Getting-to-know-the-API-proxy)
- - <span style="color: yellow; background-color: black">In summary, it is a simpler </span> [[API Gateway]]
- The specific role of the proxy is
	- to connect an exposed API, sometimes referred to as the proxy endpoint, to a target API.
	- Then, it applies policies that regulate how requests travel from the exposed API to the target API, as well as how the target will respond.
- Gives your [[API]] access to pre-existing solutions / policies
	- security
	- transformation of data
	- load balancing
	- monitoring
	- auditability
- You can create a custom API interface for an application (often a frontend) that interacts with different parts of your backend.
	- This allows you to define an API which adapts to the needs of your application, without having to change the underlying services in your backend.
- API proxy versus API gateway
  heading:: true
	- proxies as simple gateways, used when only minimal processing of an inbound request is necessary[*](https://www.techtarget.com/searchapparchitecture/tip/Getting-to-know-the-API-proxy)
- When to use one or another?
  heading:: true
	- Generally, you'll want to consider an API proxy if
		- its policy requirements are limited to simple procedures like access security and compliance, data model reformats or monitoring.
	- If the API's needs go beyond this
		- consider a larger API gateway.
- Tips
  heading:: true
	- Avoid exposing the same target API through multiple proxies.
		- there is a chance that changes made to the target API might not reflect across every proxy it's paired with, which can cause major headaches surrounding mismatched management policies.
- Tools
  heading:: true
	- [[Nginx]]
	- [[Traefik]]