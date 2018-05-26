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
- Images - The blueprints of our application which form the basis of containers. In the demo above, we used the docker pull command to download the busybox image.
- Containers - Created from Docker images and run the actual application. We create a container using docker run which we did using the busybox image that we downloaded. A list of running containers can be seen using the docker ps command.
- Docker Daemon - The background service running on the host that manages building, running and distributing Docker containers. The daemon is the process that runs in the operating system to which clients talk to.
- Docker Client - The command line tool that allows the user to interact with the daemon. More generally, there can be other forms of clients too - such as Kitematic which provide a GUI to the users.
- Docker Hub - A registry of Docker images. You can think of the registry as a directory of all available Docker images. If required, one can host their own Docker registries and can use them for pulling images.


