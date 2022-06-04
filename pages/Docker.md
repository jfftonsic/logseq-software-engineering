sources:: [Docker docs - best practices](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/), [Docker compose file reference](https://docs.docker.com/compose/compose-file/), [.dockerignore file docs](https://docs.docker.com/engine/reference/builder/#dockerignore-file)
tags:: infrastructureTech
content-name:: Docker

- Short description
	- Is one of multiple [[container runtime]]s available.
	  id:: 625d9a16-3e56-4cf7-a257-fd715f1a7b2d
-
-
- Tips and tricks
  heading:: true
	- Docker on docker
	  collapsed:: true
		- docker run -v /var/run/docker.sock:/var/run/docker.sock ...
	- Network troubleshooting image
	  collapsed:: true
		- {{embed ((6265657b-3e7a-497e-b391-523629a99ecf))}}
	- Docker build without using using cached stages
	  collapsed:: true
		- `docker build --no-cache`
	- Prune build intermediary stages
	  collapsed:: true
		- `docker build prune`
	- Include docker compose files into other docker compose files
	  collapsed:: true
		- `docker-compose.yml`
			- ```yml
			  version: "3.9"
			  services:
			  
			    postgres:
			      extends:
			        file: a-postgres-docker-compose.yml
			        service: postgres1
			  
			    webapp:
			      depends_on:
			        - postgres
			      image: employees-api
			      build:
			        context: ../..
			        dockerfile: docker/app-image/Dockerfile
			        labels:
			          com.company.module: "EmployeesAPI"
			      environment:
			        - server.address=0.0.0.0
			        - spring.datasource.url=jdbc:postgresql://db1:5432/
			      ports:
			        - ${API_EXPOSE_PORT:-8081}:8081
			      links:
			        - postgres:db1
			  
			  volumes:
			    postgresavol: { }
			  
			  ```
		-
		- `a-postgres-docker-compose.yml`
			- ```yml
			  version: "3.9"  # optional since v1.27.0
			  services:
			    postgres1:
			      # Use postgres/a user/password credentials
			      image: postgres
			      ports:
			        - ${POSTGRES_EXPOSE_PORT:-5432}:5432
			      environment:
			        POSTGRES_PASSWORD: a
			      volumes:
			        - postgresavol:/var/lib/postgresql
			    adminer:
			      image: adminer
			      ports:
			        - ${ADMINER_EXPOSE_PORT:-8085}:8080
			      links:
			        - postgres1:db1
			  
			  volumes:
			    postgresavol: { }
			  ```
	- Good docker up and down commands
	  collapsed:: true
		- ```bash
		  docker compose -p an-arbitrary-environment-name up
		  docker compose -p an-arbitrary-environment-name down --remove-orphans
		  ```
		- why?
		  collapsed:: true
			- `-p` gives a good identifiable name to the specific environment you are upping
			- `--remove-orphans` help you prune extraneous containers, uncluttering your disk.
	- Using environment variables on your docker compose files
	  collapsed:: true
		- The `:-` let's you choose a default value.  [*](https://docs.docker.com/compose/compose-file/#interpolation)
		  ```yml
		      ports:
		        - ${API_EXPOSE_PORT:-8081}:8081
		  ```
	- Gradle build + Spring Boot app with no environment requisites except docker
	  collapsed:: true
		- PS: <span style="color: red; background-color: black; font-weight: bold">the following example is not a layered Spring Boot image, I have yet to be able to use the `bootBuildImage` gradle task inside the gradle container.</span> Layered images are recomended
		- Also, be careful with build caches. Make sure that docker will rebuild your app instead using a build cache after you changed something in your app.
		- The build potentially takes a lot of time, because it is as if you just installed gradle, it will have to download every single dependency it needs.
		- A simple multi-stage dockerfile
			- ```Dockerfile
			  # This is a multi-stage docker build
			  FROM gradle:7.4.2-jdk17-alpine AS TEMP_BUILD_IMAGE
			  USER gradle
			  COPY --chown=gradle:gradle . /home/gradle/
			  RUN gradle build -x test --no-daemon --info --stacktrace
			  
			  # actual container
			  FROM openjdk:17-jdk-alpine3.14
			  ENV ARTIFACT_NAME=your-jar-file.jar
			  ENV APP_HOME=/usr/app/
			  
			  WORKDIR $APP_HOME
			  COPY --from=TEMP_BUILD_IMAGE /home/gradle/build/libs/$ARTIFACT_NAME .
			  
			  EXPOSE 8080
			  ENTRYPOINT exec java -jar ${ARTIFACT_NAME}
			  ```
		- `docker build -t your-app-image-name .`
	- Prune things to save your disk space
	  collapsed:: true
		- https://docs.docker.com/config/pruning/
	- If you need to write a starter script for a single executable [*](https://docs.docker.com/engine/reference/builder/#exec-form-entrypoint-example)
	  collapsed:: true
		- you can ensure that the final executable receives the Unix signals by using exec and gosu commands
		- example
			- ```bash
			  #!/usr/bin/env bash
			  set -e
			  
			  if [ "$1" = 'postgres' ]; then
			      chown -R postgres "$PGDATA"
			  
			      if [ -z "$(ls -A "$PGDATA")" ]; then
			          gosu postgres initdb
			      fi
			  
			      exec gosu postgres "$@"
			  fi
			  
			  exec "$@"
			  ```
	- If you need to do some extra cleanup (or communicate with other containers) on shutdown, or are co-ordinating more than one executable [*](https://docs.docker.com/engine/reference/builder/#exec-form-entrypoint-example)
	  collapsed:: true
		- you may need to ensure that the ENTRYPOINT script receives the Unix signals, passes them on, and then does some more work
		- example
			- ```bash
			  #!/bin/sh
			  # Note: I've written this using sh so it works in the busybox container too
			  
			  # USE the trap if you need to also do manual cleanup after the service is stopped,
			  #     or need to start multiple services in the one container
			  trap "echo TRAPed signal" HUP INT QUIT TERM
			  
			  # start service in background here
			  /usr/sbin/apachectl start
			  
			  echo "[hit enter key to exit] or run 'docker stop <container>'"
			  read
			  
			  # stop service and clean up here
			  echo "stopping apache"
			  /usr/sbin/apachectl stop
			  
			  echo "exited $0"
			  ```
	-
	-
- Best practices
  heading:: true
	- Remove everything you don't need from docker build context. [*](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/#understand-build-context)
	  collapsed:: true
		- Use a `.dockerignore` file. Complete reference [here](https://docs.docker.com/engine/reference/builder/#dockerignore-file).
		- example of ignoring everything except some things:
			- ```
			  *
			  !src/
			  !build.gradle.kts
			  !settings.gradle.kts
			  !gradle.properties
			  ```
	- Create containers that can be stopped and destroyed, then rebuilt and replaced with an absolute minimum set up and configuration.
	- Use labels
	  collapsed:: true
		- help organize images by project, record licensing information, to aid in automation
		- https://docs.docker.com/config/labels-custom-metadata/
		-
	- In dockerfiles
		- if possible
			- use non-root user [*](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/#user)
				- Start by creating the user and group in the Dockerfile with something like `RUN groupadd -r postgres && useradd --no-log-init -r -g postgres postgres`
				- use USER to change to a non-root user.
			- use absolute paths for your `WORKDIR`. [*](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/#workdir)
			- use `WORKDIR` instead of proliferating instructions like `RUN cd … && do-something`
			- If you would like your container to run the same executable every time, then you should consider using `ENTRYPOINT` in combination with `CMD`
		- Avoid
			- installing or using sudo
			- switching USER back and forth frequently.
-
- Some summarized useful sections of documentation
  heading:: true
	- Dockerfile
	  heading:: true
		- Environment replacement [*](https://docs.docker.com/engine/reference/builder/#environment-replacement)
			- ```dockerfile
			  FROM busybox
			  ENV FOO=/bar
			  WORKDIR ${FOO}   # WORKDIR /bar
			  ADD . $FOO       # ADD . /bar
			  COPY \$FOO /quux # COPY $FOO /quux
			  ```
		- `RUN`
			- shell form
				- ```dockerfile
				  RUN /bin/bash -c 'source $HOME/.bashrc; \
				  echo $HOME'
				  
				  # equivalent to RUN /bin/bash -c 'source $HOME/.bashrc; echo $HOME'
				  ```
			- exec form
				- ```dockerfile
				  RUN ["/bin/bash", "-c", "echo hello"]
				  # The exec form is parsed as a JSON array
				  # it is necessary to escape backslashes
				  # you must use double-quotes (“) around words not single-quotes (‘)
				  # RUN [ "echo", "$HOME" ]  will not do variable substitution on $HOME , if you want it, do RUN [ "sh", "-c", "echo $HOME" ]
				  
				  ```
			- The cache for RUN instructions isn’t invalidated automatically during the next build
		- `CMD`
		  collapsed:: true
			- Main purpose
				- provide defaults for an executing container
			- If CMD is used to provide default arguments for the ENTRYPOINT instruction
				- both the CMD and ENTRYPOINT instructions should be specified with the JSON array format.
		- `EXPOSE`
			- Does not actually publish the port
				- (use `-p` flag on docker run for that,  or the `-P` flag to publish all exposed ports and map them to high-order ports.)
			- It functions as a type of documentation
		- `ENV`
			- they persist when a container is run from the resulting image
		- `ENTRYPOINT`
			- allows you to configure a container that will run as an executable.
			- also has exec form and shell form, like `RUN`
			- to override
				- `docker run --entrypoint` flag.
		- More information on how `CMD` and `ENTRYPOINT` interact [here](https://docs.docker.com/engine/reference/builder/#understand-how-cmd-and-entrypoint-interact).
		- <span style="color: red; background-color: black; font-weight: bold">CONTINUE FROM</span> https://docs.docker.com/engine/reference/builder/#volume
		  #WIP
	- Compose file
		- <span style="color: red; background-color: black; font-weight: bold">CONTINUE FROM</span> https://docs.docker.com/compose/compose-file/
		  #WIP
-
-
- Concepts
  heading:: true
	- [[container]]
-
- notes from "TUDO O QUE VOCÊ PRECISA SABER SOBRE DOCKER EM 2022" youtube video
	- video
	  collapsed:: true
		- {{youtube https://www.youtube.com/watch?v=MeFyp4VnNx0}}
	- Dockerfile
		- entrypoint "similar to CMD, the difference is that it is the main process of your container, if entrypoint dies there is no reason for anything else to exist"
	- Network
		- If you start a container in a specific docker network it will not be available to be found by dnsname yet, you must start the container with `--network-alias xxxx` to give a dnsname to a container inside a specific docker network.
	- Sharing on docker hub
		- your image should have the nomenclature `your-docker-hub-username/image-name:version`
		- you need first to login. `docker login -u your-docker-hub-username`, it will ask Password, but that is not your user password, it is an access token that you should generate on dockerhub account settings -> security.
		- then push. `docker push your-docker-hub-username/image-name:version`