sources:: https://dev.to/sabyasachi/buildpack-with-spring-boot-300o
tags:: Spring Boot, Spring, Build Tools

- Using [[Buildpacks]]
- Spring uses Packeto buildpack which is an implementation of CNCF buildpack spec, and has it integrated with spring boot maven/gradle plugin.
- paketo-buildpacks/bellsoft-liberica
	- buildpack provides JDK/JRE
- paketo-buildpacks/spring-boot
	- provides spring boot dependencies and slices an application to multiple layers.
- #+BEGIN_CAUTION
  when we override default buildpacks we need to provide all the buildpacks which are required. https://dev.to/sabyasachi/buildpack-with-spring-boot-300o
  Another quirk is if you do not provide the tag like latest it will not work.
  #+END_CAUTION
-
- Building native image
	- use builder `paketobuildpacks/builder:tiny`
		- help create a distro-less image suitable for Graal native image type artifacts.
	- set `BP_NATIVE_IMAGE` option to true
		- instruct the builder to generate a graal native image.
		-