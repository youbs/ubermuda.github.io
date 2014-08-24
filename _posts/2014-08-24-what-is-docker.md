---
layout: post
title: What is Docker?
tags:
- docker
---

[In its own words](https://www.docker.com/whatisdocker/), "Docker is an open platform for developers and sysadmins to build, ship, and run distributed applications". More precisely, Docker is a wrapper around a set of technologies that allows easy, portable, fast and reproducible application packaging. Using Docker, you can package an arbitrarily complex application, push it to a Registry (basically, a public database  of applications), and deploy it in a matter of minutes. But Docker is about much much more than just packaging.

## Founding blocks

To achieve great power and flexibility, Docker relies on two major building blocks: images and containers. Images are the equivalent of virtual machines images, while container are the "running" version of an image. We will go into greater length of describing the differences, but for now, suffices to say that you package application into images that you can distribute, and people can use these images to create container that run your applications.

## Containers, the gory details

Simply put, a container is a sandbox where you can run arbitrary processes and store arbitrary files. It is a clean environment you can build upon. More often than not, people compare containers to virtual machines. This is a good analogy, although with a few caveats (most notably, containers share their kernel with the host machine).

## Why containers?

To create re-usable environments, we used to rely on virtualization. It has many advantages, but mostly it allows to systemically build a system from scratch. It is something you can reproduce, and it is relatively easy to share with other people.

Virtualization comes with its set of drawbacks though, most notably:

1. There's a lot of overhead. You have to boot an entire system before being able to do anything. It consumes a lot of RAM and CPU before you can even start to run your application.
1. It uses large amount of data. Most virtualization software can dynamically resize their hard-drive files, but you still have to allocate enough space to begin with. Also, each virtual machine must have its own copy of the operating system.

Docker containers address these problems, and then some.

## The LXC instruction set

To quote [the Debian Handbook](http://debian-handbook.info/browse/stable/sect.virtualization.html):

> LXC is not, strictly speaking, a virtualization system, but a system to isolate groups of processes from each other even though they all run on the same host. It takes advantage of a set of recent evolutions in the Linux kernel, collectively known as control groups, by which different sets of processes called “groups” have different views of certain aspects of the overall system. Most notable among these aspects are the process identifiers, the network configuration, and the mount points. Such a group of isolated processes will not have any access to the other processes in the system, and its accesses to the filesystem can be restricted to a specific subset. It can also have its own network interface and routing table, and it may be configured to only see a subset of the available devices present on the system.

The Docker team has released the result of their work on LXC in the form of a re-usable go library called [`libcontainer`](https://github.com/docker/libcontainer).

## Execution drivers

Execution drivers are the portability layer of Docker. They will eventually permit seamless containerization across systems. As of 1.0, there is only one official execution driver, `libcontainer`, that works using Linux' LXC instructions. Docker 1.0 runs on non-Linux systems using a VM called `boot2docker` that we will cover in a few paragraphs.

## Union filesystems

Docker makes clever use of union capable filesystems to optimize re-usability of images and make sure as little hard disk space as possible is used. To understand this, you first have to picture yourself what a Docker container is: the union of a root filesystem (read-only filesystem that contains all the base operating system – also known as `rootfs`) and a user land read-write filesystem that will contains all your modifications to the base image.

## Storage drivers

There are currently two drivers available for managing filesystems in Docker: `AUFS` – the historic driver – and `devicemapper`.

One driver is not inherently better than the other, and `devicemapper` was developed mainly to make up for the lack of `AUFS` availability in some kernels. If your setup works well with AUFS, you don't have a reason to switch to devicemapper, and vice-versa.

You can change the storage driver by using the `--storage-driver` option in your `/etc/default/docker` configuration file:

    # Use DOCKER_OPTS to modify the daemon startup options.
    DOCKER_OPTS="--storage-driver=devicemapper"

Before switching, be aware that both storage driver are not compatible with each other and you will either have to re-create any image and controller you had or export and re-import them.

For more details on Docker's configuration, see Chapter 2 – First steps.

## Docker's client/server model

One of the most awesome features of Docker, yet often overlooked by beginners, is its client/server model. The Docker binary can run in two distinct modes:

1. The daemon mode sits in the background of the server and exposes a REST(-ish) API to access its core features. It is responsible for creating and starting containers, building images, well, the actual work.
2. The client mode connects to the Docker daemon and sends orders according to options you specified in the command line.

One immediate consequence of this architecture is that you could very well configure your Docker client to connect to a remote Docker daemon, and administrate a remote server's containers all from the comfort of your local command line.

The second most important consequence is that a lot of third-party clients for the API are maintained in various languages by the community (for example, I created and maintain [Docker-PHP](https://github.com/stage1/docker-php)). You thus can connect to the Docker daemon directly from your application, without the need to compile bindings or shell-out commands.

We will see how to start Docker in daemon mode, and how to configure it, in greater lengths in Chapter 2.

## What is boot2docker

If you are not using a Linux-based operating systems, you can't use the `libcontainer` execution driver. Luckily, [boot2docker](https://github.com/boot2docker/boot2docker) has you covered! It is the officially supported way of running Docker on non-Linux operating systems.

When you first issue a Docker command, boot2docker will boot a Linux VM behind the scene and forward all commands to a docker daemon running inside it. This VM is extremely lightweight (about only 24MB), runs entirely from the RAM and boots in about 5 seconds. It also has to boot only once,  all subsequent docker commands will be seamlessly forwarded to the same virtual machine.

You will find detailed installation instructions in Chapters 2, as well as on [boot2docker's GitHub page](https://github.com/boot2docker/boot2docker).