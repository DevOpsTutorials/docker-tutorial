# docker-tutorial

## Docker concepts

### What is Docker?
Wikipedia defines Docker as:

an open-source project that automates the deployment of software applications inside containers by providing an additional layer of abstraction and automation of OS-level virtualization on Linux.

- Docker is a tool that allows developers, sys-admins etc. to easily deploy their applications in a sandbox (called containers) to run on the host operating system i.e. Linux.
- The key benefit of Docker is that it allows users to package an application with all of its dependencies into a standardized unit for software development.

### Packaging applications

- JAR, WAR etc.
- Linux packages - rpm, Debian etc.
- VM from host image formats like AMI
- Container from Docker image

## Tutorial

Checking if the Docker installation on the host is good:
```
$ docker run hello-world
```

Download a Docker image from Docker Hub to the host:
```
$ sudo docker pull busybox
```

List the Docker images already available on the local host:
```
$ docker images
```

Run `busybox` image in a container:
```
$ docker run busybox echo "hello from busybox"
hello from busybox
```

Check the history of running containers:
```
$ docker ps -a
```

### Docker terminology
- `Image` - The blueprint of an application which forms the basis of a container. Use command `docker pull` to download the  image from a Docker registry.
- `Docker Registry` - Where docker images are stored centrally for deployment.
- `Docker Hub` - is a public registry managed by Docker, Inc.
- `Container` - Created from a Docker image which runs the actual application. A container is spun up using `docker run` command.
- `Docker Daemon` - The background service running on the host that manages building, running and distribution of Docker images and containers. The daemon is the process that runs in the operating system to which clients talk to.
Try these commands to see the details:
```
$ sudo service docker status
$ ps -ef |grep -i docker
```
- `Docker Client` - A tool that allows the user to interact with the daemon. `docker` is a CLI client.

### Running and accessing container

Run a container from an image publicly available and try to access it locally.

```
$ docker run -d -P --name static-site prakhar1989/static-site
Unable to find image 'prakhar1989/static-site:latest' locally
latest: Pulling from prakhar1989/static-site

$ docker ps
CONTAINER ID        IMAGE                     COMMAND             CREATED             STATUS              PORTS                                           NAMES
61183d5d3bce        prakhar1989/static-site   "./wrapper.sh"      45 seconds ago      Up 44 seconds       0.0.0.0:32769->80/tcp, 0.0.0.0:32768->443/tcp   static-site

$ docker port static-site
443/tcp -> 0.0.0.0:32768
80/tcp -> 0.0.0.0:32769

$ curl -I http://localhost:32769
HTTP/1.1 200 OK
Server: nginx/1.9.9
Date: Sat, 26 May 2018 04:09:39 GMT
Content-Type: text/html
Content-Length: 2041
Last-Modified: Sun, 03 Jan 2016 04:32:16 GMT
Connection: keep-alive
ETag: "5688a450-7f9"
Accept-Ranges: bytes
```

Map a container port to the host externally accessible from Internet. Port `9090` is configured in AWS to access from the Internet.
```
$ docker ps
CONTAINER ID        IMAGE                     COMMAND             CREATED             STATUS              PORTS                                           NAMES
61183d5d3bce        prakhar1989/static-site   "./wrapper.sh"      6 minutes ago       Up 6 minutes        0.0.0.0:32769->80/tcp, 0.0.0.0:32768->443/tcp   static-site

$ docker stop 61183d5d3bce
61183d5d3bce

$ docker run -p 9090:80 prakhar1989/static-site
Nginx is running...
```

Check the application as `http://IP-ADDRESS:9090/` (example: `http://52.89.218.0:9090/`). You will see a message like:
```
Hello Docker!
This is being served from a docker container running Nginx.
```

### Building and running a Docker image
> ***GROUPLABEL*** below is your Docker username (see examples below the commands)
```
$ git clone https://github.com/kurianinc/docker-curriculum.git

$ cd docker-curriculum/flask-app

$ docker build -t GROUPLABEL/catnip .
# example: $ docker build -t thomastk/catnip .

$ docker run -p 9090:5000 GROUPLABEL/catnip
# example: docker run -p 9090:5000 thomastk/catnip
```

The app running in the container can be accessed as `http://IP-ADDRESS:9090/` (example: `http://52.89.218.0:9090/`)

### Push image to Docker Hub

If you don't have an account on Docker Hub, create one at https://hub.docker.com/

Login to Docker Hub using your account. A sample login session below:
```
$ docker login
Login with your Docker ID to push and pull images from Docker Hub. If you don't have a Docker ID, head over to https://hub.docker.com to create one.
Username: thomastk
Password:
Login Succeeded
```

Push image to Docker Hub, a sample session below:
```
$ docker push thomastk/catnip
The push refers to repository [docker.io/thomastk/catnip]
118f7100ddfc: Pushed
e377d4e34361: Pushed
1f50a9b9b2c7: Pushed
55fe2eed468c: Layer already exists
0e4f7fa7eb06: Layer already exists
ac0e7b8ba9e8: Layer already exists
b57c982f5768: Layer already exists
7ad7ab2d3895: Layer already exists
23044129c2ac: Layer already exists
8b229ec78121: Layer already exists
3b65755e1220: Layer already exists
2c833f307fd8: Layer already exists
latest: digest: sha256:73da305e4f34e322c2b0961a1f24bd0aded629448a70cf936e1764545efbfe3b size: 2840
```

This image is now available anywhere in the Internet to download and run as container as in the following example:
```
$ docker run -p 9090:5000 thomastk/catnip
```
