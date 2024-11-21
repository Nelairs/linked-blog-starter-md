Docker is software container platform that allows to create, distribute and execute applications in to isolated enviroment

  

Some of the advantages are:

- Isolation: Docker containers are isolated from each other, this means that if a system gets compromised, the host system will not suffer this.
- Portability: The containers can be moved easily from system to system, this is perfect for deploying vulnerable enviroments for pentesting.
- Reproducibility: This containers can be configured easily and reproducible, this lets us recreate attack scenarios.

---

### Installing Docker

To install docker use the command `apt install` [`docker.io`](http://docker.io) which will install from the OS repo

Once installed the daemon needs to be initiated, use `service docker start`

---

### Dockerfile

To start using docker and deploy the images its needed a file called **Dockerfile,** this file containes the configurations to build the container.

An example of this file would be:

```YAML
FROM ubuntu:latest
MANTAINER Santiago Etchenique "santiago.etchenique@enterprise.com"
```

This would be the base config file.

---

### Building

Now to start the build of a image we use `docker build` command

```Bash
docker build -t my_first_image .
```

This using the last example would use ubuntu image in the latest version

![[Untitled 19.png|Untitled 19.png]]

To see the images we use `docker images`

![[Untitled 1 2.png|Untitled 1 2.png]]

We can pull images from the repository using `docker pull debian:latest`

At the time we only have configured images that can be used for deploying containers

---

### Docker Run

Now I will be using the images that we have in the local repository to deploy a container.

Using `docker run` Ill specify with some parameters how i want my container to ble deployed

`docker run -dit --name myContainer my_first_image`

> -d is for leaving a daemon, or in second plane  
> -i is for making the container interactive so i can connect to it  
> -t is for telling the container to use a terminal  
> the name of the container is setted and the name of the image that we want  

Now with `docker ps` we can list our container that are deployed

![[Untitled 2 2.png|Untitled 2 2.png]]

### To connect

We need to use `docker exec -it myContainer bash`

![[Untitled 3 2.png|Untitled 3 2.png]]

---

### Updating Dockerfile

```Docker
FROM ubuntu:latest
MANTAINER Santiago Etchenique "santiago.etchenique@enterprise.com"
RUN apt update && apt install -y net-tools \
	iputils-ping \
	curl \
	git \
	nano
```

This new file will make an image with the specified tools already installed

Now using `docker build -t my_first_image:v2 .` i can build the new image with v2 tag usign the new configuration

![[Untitled 4 2.png|Untitled 4 2.png]]

Now `docker run -dit --name mySecondContainer my_first_image:v2`

---

### Port forwarding

  

For this example Ill use Apache and PHP for demonstration pourposes.

  

```JavaScript
FROM ubuntu:latest

MANTAINER Santiago Etchenique "santiago.etchenique@enterprise.com"

ENV DEBIAN_FRONTEND noninteractive

RUN apt update && apt install -y net-tools \
	iputils-ping \
	curl \
	git \
	nano \
	apache2 \
	php

EXPOSE 80

ENTRYPOINT service apache2 start && /bin/bash
```

  

`docker build -t webserver .`

  

`docker run -dit -p 80:80 --name myWebserver webserver`

![[Untitled 5 2.png|Untitled 5 2.png]]

Now we have a webserver deployed and forwarded via host IP

---

### MOUNTS

  

Now the mounts are similar as port forwarding, what you do is forward a host directory to be used by the container, in this way you can leave .html files to interpreted by the webserver container.

  

```Docker
FROM ubuntu:latest

MANTAINER Santiago Etchenique "santiago.etchenique@enterprise.com"

ENV DEBIAN_FRONTEND noninteractive

RUN apt update && apt install -y net-tools \
	iputils-ping \
	curl \
	git \
	nano \
	apache2 \
	php

EXPOSE 80

ENTRYPOINT service apache2 start && /bin/bash
```

  

`docker build -t webserver .`

  

`docker run -dit -p 80:80 -v /home/nelairs/Desktop/docker/:/var/www/html/ --name myWebserver webserver`