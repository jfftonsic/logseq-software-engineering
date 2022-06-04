sources:: https://github.com/paketo-buildpacks/bellsoft-liberica, https://dev.to/sabyasachi/buildpack-with-spring-boot-300o

- Short description
	- A modular tool to create OCI compliant images for your application, without using Dockerfile.
- Components
  heading:: true
	- Builder
		- An image that builds the image for your application.
	- Buildpack
		- Does the actual job of analysing your application code and contributes in image generation.
		- Each builder consists of multiple buildpacks.
		- There are also some special buildpacks which are called Metadata buildpack
			- these are aggregation of different buildpacks.
		- Any buildpack will have code for detect and build , detect script will enable the particular buildpack to participate in image building process and build script will actually have the code build images.
		- Some (or all?) buildpacks list https://github.com/paketo-buildpacks/base-builder/blob/main/builder.toml
	- Stack
		- Provide base image to builders.
		- Can define
			- a build image which will be used while building
			- a run image which will be used at running the image.
-
- Buildpack vs Dockerfile
  heading:: true
	- Buildpacks are opinionated and like any opinionated tech it gives us some defaults which helps to build image quickly without another piece of infrastructure code to maintain. We can get performant, secure images automatically without us need to write anything.
	- On the other side we lose flexibility and this is where Dockerfile shines.
		- We have more freedom to chose our base images, run any command etc.
		- But this flexibility means we are responsible for its correctness, performance, optimization etc.
	- Looking into the pros and cons we can say, if our application is trivial and not much customization is required buildpack can be a good option over using Dockerfiles.