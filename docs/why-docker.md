---
layout: default
title: Why choosing docker ?
category: docs
---

# Why choosing docker ?
_This article is a work in progress. Please open an issue if you have a remark or
suggestion._

## The Origin

Before explaining why we have choose docker as base for our platform, we need to
explain the origin of this project.
The need to have a platform dedicated to the student have two origins: the first
one is from the TSP school and more specificaly the need to have an environment
for practical work (student are not here to learn how to install a specific
software but to learn to use it) ready to use without the need to install many
software; the second reason, which is more or less linked to the previous one,
come from the importance given to online courses. Teachers wants to continue to
make student practicing what they learn, but while for in-school student they can
rely on the school IT department to prepare machine with the required software
it something that is not possible for online student.

A first solution was designed based on Vagrant to let online student experiment
(this solution is available at http://insert-the-good-address-here). There were
some problems with this solution:

1. It requires one virtual machine per course.
2. Vagrant is a great tool when you can use the command line, but for a beginner
   student working on Windows it can be a no-go factor.
3. A complete virtual machine consume a lot of resources and, while it is not a
   problem on high-end computer, it can limit some old/cheap computer in emerging
   countries.

So we needed a tool with a graphical user interface that doesn't consume much
resources and with which we can run many courses on a basic computer.

## Enter docker

How to conserve the power of virtual machine without their weight ? The solution
is well known: container. _**TODO** describe what a container is_.
Lucky we are, we have a hype product that promese to let you easily use container:
docker. From a technical aspect, docker is based on the Linux Container (LXC) and
thus, only work on Linux. Fortunately the docker folk have created boot2docker,
a small virtual machine that run Linux in VirtualBox (available on all major OS).
With that, we have the power of virtualization without the cost of having too many
VMs running on the student computer (in fact only one will be running, or none if
the student use a Linux kernel).

## Comparison docker and vagrant

Okay, we have choose docker over vagrant but can we have some real info as why
docker is better (or as default not worse) ?

Sure, there you are:

* _**TODO** CPU_
* _**TODO** Start time_
* _**TODO** Memory_
