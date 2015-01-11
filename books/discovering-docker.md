---
layout: pages
title: Discovering Docker
category: Books
---

<br />

<img class="big pull-right book-cover" src="/images/discovering-docker/book.png" alt="Discovering Docker" />

*Discovering Docker* is the perfect book to get started with Docker. With it, you will understand what is <a href="http://docker.com/">Docker</a>, how it works, and how it can be useful for you. The book covers everything you need to know to get started with Docker, from building images to orchestrating containers, including basic networking and service discovery.

You will then be able to leverage the power of Docker for your every day tasks, like building your own Docker images, setting up development environment, easily running continuous integration in separated containers, etc.

{% include buy_discovering_docker_button.html %}

<div class="post-global-note"><p>Not sure just yet? <a href="/what-is-docker.html">Read the first chapter online</a>.<br />More excerpts will be added soon.</p></div>

### Who is this book for?

The *Discovering Docker* book is aimed at people who wants to understand what is Docker, how it works and what can be done with it. It features explanations of how Docker works internally, comprehensive coverage of its API, and practical examples what can be done with it.

### Do I need knowledge of Docker to read this book?

No, you do not. The book is self-sufficient and will teach you everything you need to know to get started with Docker.

### In what format is the book available?

*Discovering Docker* is available out of the box in PDF, ePub and MOBI formats, and on demand in any format supported by [Pandoc](http://johnmacfarlane.net/pandoc/).

I will try my best to provide printed copies (probably through [Lulu](http://lulu.com)), but can't make any promise about this as of now.

### Are there translations planned?

Yes. French and german are on their way. If you're willing to help in translating Discovering Docker in your own language, please [send me an email](mailto:geoffrey.bachelet@gmail.com) so we can work out the details.

---

## Table of contents

Because Docker is evolving so fast, Discovering Docker has to evolve fast too. Please be aware that the following Table of Content is susceptible to change. [More information on the lifecycle of this book](/discovering-docker-update-and-plan-for-the-future.html).

* **[What is Docker?]({% post_url 2014-08-24-what-is-docker %})**
  * [The building blocks of Docker]({% post_url 2014-08-24-what-is-docker %}#the-building-blocks-of-docker)
  * [LXC and execution drivers]({% post_url 2014-08-24-what-is-docker %}#lxc-and-execution-drivers)
  * [Union filesystems and storage drivers]({% post_url 2014-08-24-what-is-docker %}#union-filesystems-and-storage-drivers)
  * [Docker's client/server model]({% post_url 2014-08-24-what-is-docker %}#dockers-clientserver-model)
  * [Running Docker on non-Linux hosts]({% post_url 2014-08-24-what-is-docker %}#running-docker-on-non-linux-hosts)
* **First steps**
  * Installation
  * Container vs Image, what's the difference?
  * Creating a container
  * Committing a container
  * Managing containers
  * Managing images
* **Managing containers**
  * Listing containers
  * Inspecting a container
  * Container lifecycle
  * Entering a container
  * Checking processes
  * Exporting and importing containers
  * Restart policies
* **Automating builds with the Dockerfile**
  * What is a Dockerfile?
  * The `docker build` command
  * Build context
  * Context-less builds
  * Cache management
  * Limitations
  * Syntax and grammar of the Dockerfile
  * Instructions
* **Volumes**
  * What are volumes and how do they work
  * Creating a volume
  * Sharing files between host and container
  * Sharing volumes through a Data Volume Container
  * Backing volumes up
* **Managing application logs**
  * The `docker logs` command
  * Using syslog
  * Using a volume share
  * Using an ELK stack
  * With the Remote API
* **Interlude, the Pet vs Cattle metaphor**
* **Pet containers**
  * Running multiple processes in a container
  * Building a Wordpress container
  * Using a volume
* **Multi-containers architectures and orchestration**
  * Role-based containers
  * Process-based containers
  * A simple init script
* **Networking**
  * How networking works in Docker
  * Linking containers
  * Service discovery
  * The Ambassador design pattern
* **Orchestration**
  * What is orchestration?
  * Introduction to fig
    * Installing fig
    * Using fig
  * Limitations
* **The Remote API**
  * What is the Remote API?
  * How to use the Remote API
  * Remote API reference
  * Gotchas
* **The Registry**
  * Automated builds
  * Setting up a private registry

---
Feel free to discuss the content of this book and suggest improvements, I'm listening and continually updating the book. Also, all purchases qualify for free lifetime updates, so do not hesitate!

{% include buy_discovering_docker_button.html %}

{% include disqus.html %}