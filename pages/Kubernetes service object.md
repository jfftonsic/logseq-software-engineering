- Short description
	- a stable endpoint that points to a group of pods based on label selectors. It proxies requests to the backend pods using labels and selectors. Serves the purpose of never changing the endpoint or IP address that will point to the list of running pods.
	  id:: 628bba4d-c16d-480b-a8c4-6d37c2b554f9
- The requests are also [load-balanced]([[load balancing]]) over a set of pods if multiple pods are running in the same application.