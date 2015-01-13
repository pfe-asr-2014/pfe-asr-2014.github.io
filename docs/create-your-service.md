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

Docker is designed to run a single process per container, that's why the container
stop when the process of the `CMD` instruction die. So how can we have a system
with multi process ? There is many possibility: you can use a [process manager](http://ddollar.github.io/foreman/),
use a bash script with the `&` symbol (as long as the last process isn't killed)
or you can stick to the docker philosophy: one process per container. In that
case we need a way to make our container communicate between them. Docker provide
us a way to link our container together with a functionnality called… link !

To demonstrate the concept of link we will take the example of a database (MySQL)
and a managing tool for that database (phpMyAdmin). Luckily for us, there already
are images of these software.

_Again, this is a basic introduction to link. For a more advanced version,
[see the official one](https://docs.docker.com/userguide/dockerlinks/)._

```sh
# Download the base images
docker pull mysql
docker pull corbinu/docker-phpmyadmin

# Run the MySQL server
docker run -d \
  –name mysqlcontainer \
  -e MYSQL_ROOT_PASSWORD=password \
  –expose 3306 \
  -v /var/lib/mysql \
  mysql
```

So with these commands we have downloaded phpmyadmin and mysql, and there are
ready to use ! The third command is the one to run a MySQL database with the root
password set to `password`, the name `mysqlcontainer`, we expose the port 3306 to allow
connection to the database and the argument `-v` tell docker to store the content
of the specified directory on the host machine (it will not be deleted with the
container).
Note that the name of this container is mandatory. We need it to be able to refer
to it from our other container.

Now, we are going to run the phpmyadmin container:

```sh
# Run the
docker run -d \
  –name phpmyadmin \
  –link mysqlcontainer:mysql \
  -e MYSQL_USERNAME=root \
  -e MYSQL_PASSWORD=password \
  -p 80 \
  corbinu/docker-phpmyadmin
```

So what have we done with this command ? We are running the container in the
background (`-d`), with the name of phpmyadmin (`-name phpmyadmin`), pass to the
container two environment variable: `MYSQL_USERNAME` and `MYSQL_PASSWORD`, expose
the port 80 to the host and all of that with the image `corbinu/docker-phpmyadmin`.
And we have linked the container `mysqlcontainer` to our phpmyadmin container with
the alias `mysql`. This alias is used by docker to create two principals things:

* an entry in the `/etc/hosts` file. It means that from the container, we can now
  use the `mysql` uri to link to our mysql container (eg. you can `ping mysql`).
* a bunch of environment variable that can help you to programmaticaly connect to
  the linked container (see [the docker docs](https://docs.docker.com/userguide/dockerlinks/#environment-variables)).

That's all, you now know how to make containers communicate between them !

## Introduce services
