---
layout: default
title: TSP MOOC Platform
---

# Welcome to the Télécom SudParis MOOC Platform.
_This article is a work in progress. Please open an issue if you have a remark or
suggestion._

_**TODO** Short summary of the project_

# Usage

_This section describe how to use the platform as a user (if you want to know
 how to create new services or modify the present ones, you can
 [found out the documentation](http://localhost:4000/docs/create-your-service.html))._

To be able to use this platform you need to have [docker](https://docker.com)
installed (You'll find the [instruction on their site](https://docs.docker.com/installation/)).

Then you have to download and run the overview software:

```sh
git clone git@github.com:pfe-asr-2014/tsp-mooc-overview.git

docker build -t tsp-mooc-overview tsp-mooc-overview

# With boot2docker
docker run -it --env HOST_IP=$(ip route|awk '/192/ { print $9 }') debian bash

# Or without b2d
docker run -it debian bash
```

Et voilà, you can now visit the [http://localhost:4000](http://localhost:4000)
if you don't use boot2docker, or if you use it, type `boot2docker ip` and
visit http://<_the previous IP_>:4000.

# Contact
Having trouble with the platform? [Check out the documentation](/docs) or
[open an issue](https://github.com/pfe-asr-2014/pfe-asr-2014.github.io/issues)
and we’ll help you sort it out.
