---
layout: pages
title: Docker 101
---

The Docker 101 book is the perfect book to get started with Docker. With it, you will understand what is Docker, how it works, and how it can be useful for you. The book covers everything you need to know about docker, from building images to orchestrating remote container.

The Docker 101 book is still being written but you can already [pre-order it at a special price](https://gumroad.com/l/Docker101)! Additionnally, I will publish some of the chapters on [my blog](/), so stay tuned!

### Table of contents

Please be aware that since the book is still not finished, this table of contents is for informational purpose only and might change without notice before the release of the book. This should however be a good indication of the content of the book.

* [What is Docker?]({% post_url 2014-08-24-what-is-docker %})
  * Founding blocks
  * Why containers?
  * The LXC instruction set
  * Execution drivers
  * Union filesystems
  * Storage drivers
  * Docker's client/server model
  * What is boot2docker?
* First steps
  * Container vs Image, what's the difference?
  * Creating a container
  * Committing a container
  * Re-using an image
  * Managing containers and images
* Dockerfile
  * Introduction to the Dockerfile
  * Syntax and DSL explanation
  * Dockerfile and build context
  * Limitations
  * Image overloading
  * Leveraging the build cache
* Volumes and file sharing
  * What are volumes? How are they useful?
  * Sharing files between host and container
  * The Data Container design pattern
  * Sharing volumes between containers
* Managing logs
  * With the host's syslog
  * Via a volume share
  * Other solutions (Remote Api, logspout, ...)
* Pet vs Cattle
  * Problem explanation
  * Self-container multi-process containers with daemontools
  * Multi-containers architecture and orchestration
* Networking
  * How networking works inside Docker
  * Linking containers to each others
  * The Ambassador design pattern
  * Service Discovery
    * Built-in
    * Using third-party tools (etcd, consul, ...)
* Orchestration
  * Introduction to libswarm
  * Third-party tools (Gaudi, Fig, Vagrant, ...)
* The Remote API
  * Introduction to the Remote API
  * How to use the Remote API
  * Implementation examples
  * Third-party client libraries
* The Registry
  * Introduction to Docker Hub
  * Trusted builds
  * Setting up a private registry
  * Private registry and authentication
* Bonus
  * CoreOS / Project Atomic
  * Homebrew PaaS with Flynn, Mesos / Marathon
  * Reverse proxying with nginx or hipache
  * 0 downtime deployment howto