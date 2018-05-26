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



