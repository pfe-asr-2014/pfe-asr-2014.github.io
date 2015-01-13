---
layout: default
title: Create your service/image
category: docs
---

# Create your image (and service)
_This article is a work in progress. Please open an issue if you have a remark or
suggestion._

## Introduction to docker (and Dockerfile)

_Nota: This is a quick and volontary incomplete description of what is docker and
how to easily create a basic image. For a more complete understanding please visit
[the docker user guide](https://docs.docker.com/userguide/)_

This platform is based on docker so to be able to understand how you can contribute
to the platform you need to understand some basis about docker.
In a nutshell, docker is a "chroot on steroids". It means that you isolate each
process from in the other in what is called a container (basically its a part of
the OS that is completely isolated from the OS itself).
The TSP platform is composed of these containers to provide to the student reusable,
ready-to-use environment for practical work; for example, in a relational database
course we could provide a database with some data already present to the student.
We could also provide a GUI to let the student learn easily the SQL syntax without
having to install, manage or upgrade its own database and tools.
Back to docker, there is two kind of things that you need to know to be able to
provide these services:

* **images**: an image is the file system of the system that you want to provide.
  It already exist many images like debian or ubuntu for a base OS, but you can
  also found some service already installed and configured like MySQL or WordPress.
  (You can browse them on the [Docker Hub Registry](https://registry.hub.docker.com/))
* **containers**: a container is a running instance of an image. An image is just
  a file system while the container is the piece of OS that run on that fs (this
  distinction permit to easily create a distributed cluster of the same software).

Okay, now how do I create my own image ? To do that, docker introduce a file: the
Dockerfile.
A Dockerfile is a list of instruction that are executed sequentially to create
an image. For example, if I want to create an image with apache installed on a
debian jessie system I would create the following `Dockerfile`:

```Dockerfile
FROM debian:jessie

RUN apt-get update && apt-get install apache2
```

In this example, we create our image on top of the `debian:jessie` image (**FROM**)
and run a simple command `apt-get update && apt-get install apache2` with the
**RUN** instruction. You'll find a complete list of the `Dockerfile` available
commands at [https://docs.docker.com/reference/builder/](https://docs.docker.com/reference/builder/).

We now know how to describe how to create an image, we have to effectively create
this image. Do to that there is single command:

```sh
docker build path/to/Dockerfile

# You can also tag your image to easily find it afterwards
# The version is optionnal, if not specified it will be mark as latest
docker build -t image-tag-name[:version] path/to/Dockerfile
```

And voilà ! You now have a docker image ready to be distributed to your student
(or anyone that use docker really).

## Separate your container by process (and link them instead)

## Introduce services
