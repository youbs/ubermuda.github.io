---
layout: pages
title: Discovering Docker
category: Books
---

<br />

<img class="book-cover" src="/images/discovering-docker/book.png" alt="Discovering Docker" />

*Discovering Docker* is the perfect book to get started with Docker. With it, you will understand what is Docker, how it works, and how it can be useful for you. The book covers everything you need to know about Docker, from building images to orchestrating remote containers.

You will then be able to leverage the power of Docker for your every day tasks, like setting up development environment, easily running continuous integration in separated containers, deploy to any Docker capable cloud, scale your infrastructure, you name it.

*Discovering Docker* is due for release late October, but you can already pre-order at a special price for a limited time!

<div class="pre-order">
    <a href="http://gum.co/discovering-docker/pre-order?wanted=true" class="btn btn-success">Pre-order now for $9</a>
</div>

Not sure just yet? [Read the first chapter online]({% post_url 2014-08-24-what-is-docker %}). More excerpts will be added soon. 

## Who is this book for?

The *Discovering Docker* book is aimed at people who wants to understand what is Docker, how it works and what can be done with it. It features explanations of how Docker works internally, comprehensive coverage of its API, and practical examples what can be done with it.

## Do I need knowledge of Docker to read this book?

No, you do not. The book is self-sufficient and will teach you everything you need to know to get started with Docker.

## Are there translations planned?

A french translated version will be released soon after the english one, and downloadable for free for buyers of the english version, so do not hesitate to pre-order now. After that, I'd like to organize community-driven translations whose revenues will be reversed to the translators and/or opensource projects.

{% include see_also_my_docker_training.html %}

---

### Table of contents

Please be aware that since the book is still not finished, this table of contents is for informational purpose only and might change without notice before the release of the book. This should however be a good indication of the content of the book.

* [What is Docker?]({% post_url 2014-08-24-what-is-docker %})
  * [The building blocks of Docker]({% post_url 2014-08-24-what-is-docker %}#the-building-blocks-of-docker)
  * [LXC and execution drivers]({% post_url 2014-08-24-what-is-docker %}#lxc-and-execution-drivers)
  * [Union filesystems and storage drivers]({% post_url 2014-08-24-what-is-docker %}#union-filesystems-and-storage-drivers)
  * [Docker's client/server model]({% post_url 2014-08-24-what-is-docker %}#dockers-clientserver-model)
  * [Running Docker on non-Linux hosts]({% post_url 2014-08-24-what-is-docker %}#running-docker-on-non-linux-hosts)
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
  * What is orchestration?
  * Third-party tools (Gaudi, Vagrant, ...)
  * More with Fig
  * Introduction to libswarm
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

---
Feel free to discuss the content of this book and suggest improvements, I'm listening and continually updating the book. Also, all purchases qualify for free lifetime updates, so do not hesitate and <a href="https://gum.co/discovering-docker/pre-order?wanted=true">pre-order now</a>!

{% include disqus.html %}