# Docker

## Additional Resources
- https://saixiii.com/what-is-subnet/

## Terminologies
- OS-level virtualization
- container -> LXa LXc
- kubernates
- hypervisors
- docker hun or docker store
- images and container
- orchestration
	- Orchestration is the automated configuration, management, and coordination of computer systems, applications, and services. Orchestration helps IT to more easily manage complex tasks and workflows. IT teams must manage many servers and applications, but doing so manually isn't a scalable strategy.
- os kernel -> interact with hardware 
	- the software running on it make any os different.
- docker storage drivers
	- the bit of software that allows you to write data to the writable layer of your Docker container.
- copy on write
	- Copy on Write or simple COW is a resource management technique. One of its main use is in the implementation of the fork system call in which it shares the virtural memory(pages) of the OS.
	- In UNIX like OS, fork() system call creates a duplicate process of the parent process which is called as the child process.
	- The idea behind a copy-on-write is that when a parent process creates a child process then both of these processes initially will share the same pages in memory and these shared pages will be marked as copy-on-write which means that if any of these processes will try to modify the shared pages then only a copy of these pages will be created and the modifications will be done on the copy of pages by that process and thus not affecting other process.
![COW](https://media.geeksforgeeks.org/wp-content/uploads/20200512180436/11150.png)
- MacAddress, Gateway, subnet
	- links: 
		- https://whatismyipaddress.com/gateway?__cf_chl_jschl_tk__=d4e491163caa8e1804160fedfc410e559ed62ba8-1609301874-0-AR-wIVkkMT4CxgqtGYF08f6ziNsj7NuI6HHKToAqttaJHlsifEKZ9N8REMJw_yoaH35qHD6YJeDjbKr67b6spR1whIxveQJs5Xf7Q2ZIRJPt5PGBup9iupdPVd_ueHRbHHTc8XYq8MGx--_nJTwsF8gGpjupPGFnPJdPzlGVkAZwS_dtHIrWibIUL7V9C84Jgv2D0f8LqjqXdX6emiB-VZFy28xEP9XNBE501ZUSem6dv7i4MozWLJ9xvxaiLXW_E8jMOThcNOA3v7C46cu8CmCp3OIcf4gshtVdDiBBqH0yM9XOWwWlVcIFAOiH5gt_bojvNr82a8hEk1blxz8cPJPU1QsItk9VMxNpRvjuQDIeuVSYTGo27GRGdoD0Vrx3oQ
		- https://saixiii.com/what-is-subnet/
- network namespcae, virtual ethernet pairs
- linux file system -> https://opensource.com/life/16/10/introduction-linux-filesystems
- legacy container links -> https://docs.docker.com/network/links/

---

## Objectives
- What are Containers?
- What is Docker?
- Why do you need it?
- What can it do?
- Run Docker Containers
- Create a Docker Image
- Networks in Docker
- Docker Compose
- Docker Concepts in Depth
- Docker for Windows/Mac
- Docker Swarm
- Docker vs Kubernetes

## Why do you need docker?
- compatibility/Dependency
- Long setup time
- Different Dev/Test/Prod environments
- containers come before a docker but it quiet hard to set up low-level so docker help us to make it easily.

## What can it do?
- Containerize Application
- Run each service with its own dependencies in separate containers

---

## Docker commands
- links
	- https://www.scalyr.com/blog/create-docker-image/
	- https://codenotary.com/blog/extremely-useful-docker-commands/
- Commands
	- Help -> `sudo docker --help`
	- Run
		- tag -> tag is like a version of the image (default tag is latest which is the latest version of the software)
		- `-it` options -> i for interaction such as input, t for attach terminal with the container terminal
		- PORT mapping -> when we run a server container we are not able to see what's going on inside. There are 2 ways to access it:
			1. use internal ip of container -> `172.17.0.2` (only accessible with the docker host, user outside the docker host cannot access it)
			2. use docker host's ip but for that to work we must map the port inside the docker container to the free port on the docker host.
				- for example: to enable other access an application through port 80 on docker container that running on port 5000
					- we could map port 80 of localhost to port 5000 on the docker container
					`sudo docker run -p 80:5000 <container image>`
		- Volume mapping -> how data is persisted in the docker container
			- container has it own isolated file system.
			- any file created inside the container will be removed when the container is deleted.
			- to persist data, we need to map a directory outside the container that is on the docker host to a directory inside the container.
			- `docker run -v /opt/datadir:/var/lib/mysql mysql`
	- Inspetct Container -> to see additional to detail about a specific container
		- `docker inspect <container name or container id>` (no need to put all of the id just a bit of the start id'section)
	- Container Logs -> How do we see the logs of the container we ran on the background?
		- For example: running container with the detach mode (`-d` parameter)
		- `docker logs <container name or container id>`
	- For enter to container console, and kind in the console: `docker exec -it <container name> bash`
		- link: https://stackoverflow.com/questions/30172605/how-do-i-get-into-a-docker-containers-shell
---

## Lab
- build docker image form docker file
	- go to directory where the docker file is placed -> `docker build -t <image name> .` ('.' is mean current directory)
- see information about a docker file
	- `cat Dockerfile`
- `:lite` tag -> to make a smaller size image

---

## Lesson Learned
- Linux 
	- to view release version of currently os -> `cat /etc/*release*`
- Docker 
	- problems 
		- the error shown no such image `<image name>` but that image is exist -> This means that your docker state is corrupted and you need to clear the complete state. (https://stackoverflow.com/questions/46381888/docker-images-shows-image-docker-rmi-says-no-such-image-or-reference-doe)
		- How could I not create a new container of the docker/whalesay any more but restart existing stopped container of docker/whalesay and get the same result as `docker run docker/whalesay cowsay boo`
			- Answer: The container starts the second time but the difference is that you don't see the stdout as a default with start -> add `-a`, then you can see the stdout result.
				- more info: https://docs.docker.com/engine/reference/commandline/container_start/
- docker containers share the underlying kernel
- you wan't be able to run a windows based container on a docker host with Linux on it.
	- need virtual machine (vm) to run window and then run docker on vm
- images vs container
	- images -> a package or template just like a VM template that you might have worked with in the virtualization world. it is used to create one or more containers.
	- containers -> running instances of images that are isolated and have their own environments.
- Docker Editions
	- Community Edition vs Enterprise Edition
		- Community edition -> the set of free docker products.
		- Enterprise edition -> the certified and supported container platform that comes with enterprise add-ons like the image management, image security, universal control plan for managing and orchestrating container runtimes.
	- Community Edition
		- avaialable on Linux Mac Windows or on cloud platforms like AWS or Azure.
- What is docker's storage driver in layman's terms?
	- When you use the `FROM` command in a `Dockerfile` you are referring to a base image. Rather than copy everything in a new image, you will share the contents (a.k.a. fs layers); this is what is known as a copy-on-write filesystem. The docker storage driver is just which kind of COW implementation to use (`AUFS`, `BTRFS`...).
- Environment Variables
	- Environment variables are commonly used within the Bash shell. It is also a common means of configuring services and handling web application secrets.
	- To set an environment variable the export command is used. `export NAME=VALUE`
	- To unset an environment variable, which remove its existence all together, we use the unset command. `unset VARIABLE_NAME`
	- To diplay all exported environmet -> `env`
- EVN Variables in Docker
	- `docker run -e APP_COLOR=blue <docker image>`
	- Inspect Environment Variable -> `docker inspect <docker container>` and look for 'Env' props in 'Config' section.
- create our own image
	- because we cannot find a component or services that we want to use as part of your application on docker hub.
	- How to create our own image?  
	- Example: use 'Ubuntu' os
		- Overview
			1. OS - Ubuntu (define the base os)
			2. Update apt repo
			3. Install dependencies using apt
			4. Install Python dependencies using pip
			5. Copy source code to /opt folder
			6. Run the web server using 'flask' command
		- create a docker file and do all of the above tasks
		- `docker build <docker file> -t <tag name for the image>` (to build an image)
		- `docker push <image name>` (to push to docker registry)
- Docker file
	- Docker file is a text file written in s specific format and docker can understand.
	- It's in an instruction and argument format -> `INSTUCTION ARGUMENT` 
- Layered architecture
	- when docker build the images it builds these in the layered architecture.
	- each line of instructions creates a new layer in the docker image with just the changes from the previouse layer.
	- you can see the information by running a docker history command -> `docker history <docker image>`
- What can you containerize?
	- you can containerize almost all of the applications even simple one like browser.
	- Basically, you can containerize everything.
- Docker CMD vs ENTRYPOINT
	- Unlike a virtual machine, container are not meant to host an operating system. container are meant to run a specific tasks or processes such as to host an instance of web server, application server, a database or simply to carry out some kind of computation or analysis.
	- Once the task is completed, the container exits. A container only live as long as the process inside it is alive.
	- So, Who defines what process is run within the container?
		- If you look at the docker file for popular docker images like nginx, you will see an instruction called 'CMD' that stands for command that defines the program that will be run withing the container when it starts.
	- how do you specify a different command to start the container?
		1. append a command to the docker run command and that way it overrides the default command specified within the image. `docker run <docker image> [COMMAND]`
			- Example: `docker run ubuntu sleep 5`
		2. specify a command in a docker file.
			- there are different ways of specifying the command either the command simply as is in a shell form (`CMD command param1`) or is a JSON array format (`CMD ["command", "param1"]`).
			- But remember, when you specify in a JSON array format the first element in the array should be the executable.
	- Entry point instruction is like the command instruction as in you can specify the program that will be run when the container starts and whatever you specify on the command line in this case (`docker run ubuntu-sleeper 10`). '10' will get appended to the entry point so the command that will be run when the container starts is sleep 10.
	
	```docker
	FROM Ubuntu
	ENTRYPOINT ["sleep"]
	CMD ["5"]
	```

	- In the above docker file, we set a default parameter to '5'. If we didn't specify any parameters in the command line the parameter will be '5'. If we did then that will override the command instruction and remember for this to happen we should always specify the entry point and command instructions in a JSON format.
	- To modify the entry point during runtime say from sleep to an `imaginary sleep 2.0` command. In this case we can override it by using the entry point option in the docker run command. 
		- For example: `docker run --entrypoint sleep2.0 ubuntu-sleeper 10`
- docker networking 
	- when you installed docker it creates 3 networks automatically.
		1. Bridge
			- a default network that a container get attached to.
			- a private and internal network created by docker on the host.
			- all containers attached to this network by default and they get an internal ip address usually in the range `172.17.0.x` series.
			- the container can access each other using this internal ip.
			- to access any of these containers from the outside world, map the port of these containers to port on the docker host.
		2. none
			- the containers are not attacked to any network and doesn't have any access to the external network or other containers. they run in an isolated network.
		3. host
			- another ways to access containers externally is to asscociate the container to the host network. this take out any network isolation between the docker host and the docker container meaning that if you would like to run a web server on port 5000 in the webapp container it is automatically accessible on the same port externally without requiring any port mapping as the web container uses the hosts network.
			- this would also mean that unlike before you will now not be ablue to run multiple web containers on the same host on the same port as the ports are now common for all containers in the whole network.
	- if you would like to associate a container with any other network, we specify a network information using the network command parameter like this `docker run ubuntu --network=none` or `docker run ubuntu --network=host`
- User-defined networks
	- by default docker only create one internal bridge network (`172.17.0.1`), we could create our own internal network using the command docker network create and specified the driver which is bridge in this case and the subnet for that network followed by the custom isolated network name.
	```docker
	docker network create \
		--driver bridge \
		--subnet 182.18.0.0/16
		custom-isolated-network
	```
	- run the docker network 'ls' command to list all networks.
- Inspect Network -> to see the network settings and the IP address assigned to an existing container.
	- run the docker inspect command with the ID or name of the container and look for the 'bridge' section in the 'NetworkSetting' section-> `docker inspect <container id or container name>`
- Embedded DNS
	- containers can reach each other using their names. 
	- For example, in this case I we have a web server and mysql database container running on the same node. How can I get my web server to accesss the database on the database container?
		- one thing that we could do is using the internal ip address signed to the mysql container but that is not very ideal because it is not guaranteed that the container will get the same ip when the system reboots.
		- the right way to do it is to use the container name. all containers in a docker host can resolve eacho other with the name of the container. docker has built-in DNS server that helps the containers to resolve each other using the container name.
		- Note: the built-in DNS server always runs at address '127.0.0.11'.
- How are the containers isolated within the host?
	- docker uses network namespaces that create a separate namespace for each container. It then uses a virtual ethernet pairs to connect containers together.
- docker storage and file systems -> where and how docker stores data and how it manages file systems of the containers.	
	- How docker stores data on the local file system?
		- when you install docker on a system it creats this folder structure `/var/lib/docker`. this is where docker stores all its data by default.
		- there are multiple folders under it called a 'ufs', 'containers', 'image', 'volumes', and etc. any volume that created by the docker containers are created under the volumes folder.
	- Layered architecture
		- when docker builds images it builds these in a layered architecture. 
		- if both applications have the same layers, docker is not going to build these layers instead it uses the same layers its built before from the cache and only creates another different layers. This way make docker build images faster and efficiently saves disk space.
		- all of these layers are created when we run `docker build` command to form the final docker iamge. All of these are the docker image layers. Once the build is completed you cannot modify the contents of these layers. They are read-onyl and you can only modify them by initiating a new build.
		- when you run a container based off of an image docker creates a container based off of this image layers and creates a new writable layer on top of the image layers. 
		- The wirtable layer is used to store data created by the container such as log files written by the applications, temporary files generated by the container or just any file modified by the user on that container. this layer though is only as long as the container is alive. when the container is destroyed this layer and all of the changes stored in it are also destroyed.
		- The same image layer is sharesd by all containers created using this image. If I were to log in to the newly created container and say create a new file called 'temp.txt', it will create that file in the container layer which is 'read-write'able.
		- After running a container what if I wish to modify the source code. I can still modify the source code even the container share the same image with any other container. But before I save the modified file, docker automatically creats a copy of the file in the read/write layer and I will then be modifying a different version of the file in ther read/write layer. All futuer modifications will be done on this copy of the file in the read/write layer. This is called `copy-on-write` mechanism.
		- The image layer being read-only just means that the files in these layers will not be modified in the image itself so the image will remain the same all the time until you rebuild the image using the docker build command.
		- When we get rid of the container all of the data that was stored in the container layer also gets deleted. 
	- Volumes -> to persist the data 
		- For example: if we are working with a database and we would like to preserve the data created by the container.
			- we could add a persistent volume to a container by using docker volume create command -> `docker volume create <volume name>`
			- then the docker will create a folder called `<volume name>` under the `/var/lib/docker/volumes` directory.
			- to run a docker container we have to mount this volume inside the docker containers read/write layer using the `-v' option` like this `docker run -v <volume name>:/var/lib/mysql mysql`. ( `/var/lib/mysql` is the location inside the container which is the default location where mysql stores data)
		- if we run the docker run command with the slash v option without creating a volume that we want to mount to the container before, the docker will automatically create that volume and mounted to the container.
		- `volume mounting` -> mounting a volume created by docker under the `/var/lib/docker/volumes/` folder.
		- if we had our data already at another location which is not the default docker's volume location (`/var/lib/docker/volumes`), we will provide the complete path to the docker run command like this `docker run -v /data/mysql:/var/lib/mysql mysql`. This is called `bind mounting`.
		- So, there are 2 types of mounts
			1. volume mounting -> mount the volume from the 'volumes' directory
			2. bind mounting -> mount a directory from any location on the docker host 
		- using the `-v` in the docker run command is an old style, the new way is to use the `--mount` option. The `--mount` is the preferred way as it is more verbose so you have to specify each parameter in a key equals value format.
			- For example: `docker run --mount type=bind, source=<absolute path of source data>, target=<absolute path of destination location inside the container> <docker image>`.
		- Who is responsible for doing all of these above operations? (maintaining the layered architecture, creating a writable layer, moving files across layers to enable copy and write, etc.)
			- It is the `storage driver`. Docker uses storage drivers to enable layered architecture.
			- Some of the common storage drivers are:
				- AUFS
				- ZFS
				- BTRFS
				- Device Mapper
				- Overlay
				- Overlay2
			- The selection of the storage driver depends on the underlying OS being used. Docker will choose the bast storage driver available automatically based on the operating system.
			- The diffent storage driver also provide different performance and stability characteristics. So you need to choos one that fits the needs of your application and your organization.
- docker compose 
	- if we need to setup a complex application or running multiple services, a better way to do it is to use docker compose.
	- with docker compose, we can create a configuration file in `yaml` format called `docker-compose.yml` and put together the different services and the options specific to this to running them in this file. Then, we could simply run a docker compose up command to bring up the entire application stack.
	- this is easier to implement, run and maintain as all changes are always stored in the docker compose configuration file. However, this is all only applicable to running containers on a single docker host.

	```yml
	# version 1
	redis:
		image: redis
	db:
		image: postgres:9.4
	vote:
		image: voting-app
		ports:
			- 5000:80
		links: 
			- redis:redis
	result:
		image: result or build: ./result (the directory of the project including with Docker file)
		ports:
			- 5001:80
		links:
			- db:db
	workder:
		image: worker
		links:
			- db
			- redis
	```
- Docker compose - versions
	- docker compose evolved over time and now supports a lot more options than it did in the beginninig.
	- the example code above is the trimmed down version of the docker compose file. this is in fact the original version of docker compose file known as version 1. this had a number of limitations for example if you wanted to deploy containers on a different network other than the default bridge network there was no way of specifying that in this version of the file. also say you have a dependency or startup order of some kind for example your database container must come up first and only then and should the voting application be started there was no way you could specify that in the version 1 of the docker compose file.
	- support for these came in version 2, with version 2 and up the format of the file also changed a little bit. you no longer specify your stack information directly as you did before. it is all encapsulated in services section.

	```yml
	version: 2
	services:
		redis:
			image: redis
		db:
			image: postgres:9.4
		vote:
			image: voting-app
			ports:
				- 5000:80
			depends_on:
				- redis
		result:
			image: result or build: ./result (the directory of the project including with Docker file)
			ports:
				- 5001:80
		workder:
			image: worker
	```
	- How does docker compose know what version of the file you're using?
		- you are free to use version 1 or version 2 depending on your needs. so how does the docker compose know what format you are using for version 2 and up you must specify the version of docker compose file you are intending to use by specifying the version at the top of the file.
	- another different is with networking. in version 1, docker compose attaches all the containers it runs to the default bridge network and then use links to enable communication between the containers as we did before. with version 2, docker compose automatically creates a dedicated bridge network for this application and then attaches all containers to that new network. all containers are then able to communicate to each other using each other service name. so you basically don't need to use links in version 2 of docker compose.
	- finally, the version 2 also introduces it depends on feature. if you wish to specify a start-up order for instance say the voting web application is dependent on the redis service. so you need to ensure that redis container is started first and only then the voting web application must be started. we could add a depends on property to the voting application and indicate that it is dependent on redis.
	- docker compose version 3 similar to version 2 in the structure meaning. it has a version specification at the top and the services section under which you put all our services just like in version 2. version 3 comes with support for `docker swarm`. there are some options that were removed and added.
		- more details: https://docs.docker.com/compose/
- docker registry
	- a docker registry is a place where docker images are stored.
	- for docker run command: `docker run <docker image>`
		- if `docker image` is not exist on the machine, docker will pull its repository from docker hub (docker registry). In this case, the docker image is in `registry/user account/image repo` format e.g. `docker.io/nginx/nginx` or just `nginx` if both parts are the same.
	- where are images stored and pulled from?
		- if we do not specify the location where these images are to be pulled from, it is assumed to be on docker default registry (docker hub) which its dns name is `docker.io/`.
		- the registry is where all the images are stored.
		- there are many other popular registries as well:
			- `gcr.io` -> google registry where a lot of `kubernetes` related images are stored like the one used for performing end-to-end tests on the cluster.
	- when you have applications build in-house that shouldn't be made available on public. hosting an internal private registry may be a good solution.
	- Private Registry
		- Many cloud services providers such as 'AWS' or 'GCP' provide a private registry by default when you open an account with them.
		- any of these solutions be a docker hub or google registry or your internal private registry, you may choose to make a repository private so that it can only be accessed using a set of credentials.
		- from docker perspective, to run a container using an image from a private registry you first log into your private registry using the docker login command. (`docker login private-registry.io`)
			- once successful, run the application using private registry as part of the image name like this `docker run private-registry.io/apps/internal-app`.
			- so remember to always login before pulling or pushing to a private registry. if you did not log into the private registry, it will come back saying that he image cannot be found.
		- cloud providers like AWS or GCP provide a private registry when you create an accout with them. But, what if you are runnint your application on-premise and don't have a private registry. How do you deploy your own private registry within your organization?
			- the `docker registry` is itself another application and of course is available as a docker image. the name of the image is `registry` and it exposes the API on port '5000'. -> `docker run -d -p 5000:5000 --name registry registry:2`
			- now that you have your custom registry runnint at port '5000' on this docker host. How do you push your own image to it?
				- use the image tag command to tag the image with a private registry URL in it. In this case, since it's running on the same docker host I can use 'localhost:5000' followed by the image name. -> `docker image tag my-image localhost:5000/my-image`
				- I can then push my image to my local private registry using the docker command docker push and the new image name with the docker registry information in it. -> `docker push localhost:5000/my-image`
				- from there, I can pull my image from anywhere within this network using either localhost if you're on the same host or the IP or domain name of my docker host if I'm accessing from another host in my environment. -> `docker pull 192.168.56.100:5000/my-image`
- docker engine -> take a deeper look at dockers architecture, how it actually runs application in isolated containers? and how it works under the hoods?
	-  