tags:: infrastructureTech
content-name:: Docker

- Short description
	- This is Docker's short description
	  id:: 625d9a16-3e56-4cf7-a257-fd715f1a7b2d
-
- notes from "TUDO O QUE VOCÊ PRECISA SABER SOBRE DOCKER EM 2022" youtube video
	- video
	  collapsed:: true
		- {{youtube https://www.youtube.com/watch?v=MeFyp4VnNx0}}
	- Dockerfile
		- entrypoint "similar to CMD, the difference is that it is the main process of your container, if entrypoint dies there is no reason for anything else to exist"
	- Sharing on docker hub
		- your image should have the nomenclature `your-docker-hub-username/image-name:version`
		- you need first to login. `docker login -u your-docker-hub-username`, it will ask Password, but that is not your user password, it is an access token that you should generate on dockerhub account settings -> security.
		- then push. `docker push your-docker-hub-username/image-name:version`
-