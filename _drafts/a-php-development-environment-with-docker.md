---
layout: post
title: A PHP development environment with Docker
tags:
- docker
- PHP
- development
---

A lot of developers nowadays use Vagrant to manage their development environment through Virtual Machines. Vagrant is awesome, but VMs have a number of drawbacks (mainly, it uses a lot of resources). So with the emergence of container technologies, more particularily Docker, we have a simple solution to this problem. Let's see how we can setup a development environment using Docker.

## What is a good development environment

First, we need to define what we consider a good development environment. For me, it has the following qualities:

1. **Disposable**. I must be able to trash the environment and spawn a new one at will.
1. **Fast to start**. When I want to work, I want to work *now*.
1. **Easy to update**. Things go very fast in our industry, and I must be able to update my development environment with new version of software easily.

Docker allows all of this, and a bit more. You can destroy a container and re-create it almost instantly, and updating the environment is just a matter of rebuilding the image(s) you're using.

## What is a PHP development environment

Given the complexity of today's web applications, a PHP development environment can be a lot of things. To keep things simple, we are going to limit ourselves. Our environement will be able to run a Symfony project through Nginx and PHP5-FPM, with MySQL and Redis. It will also allow us to easily run CLI commands.

## Pet vs Cattle

Another decision we have to make upfront is whether our development environment will be single or multi-container. There are advantages in both:

* single-containers are easier to pass around, and manipulate
* multi-containers allows for greater modularity when adding components to your stack

Because I'm lazy, and I need to keep some content for my book, we'll only see the single-container approach in this post.

## Initializing a project

First thing is to initialize a new Symfony project. The recommended way to do this is to use [composer](http://getcomposer.org/) with the `create-project` command. We could install composer on our workstation, but that would be too easy, so instead we're going to use it through docker too.

Luckily, I published an article on how to do just that: [make docker commands](http://geoffrey.io/make-docker-commands.html/) (Ok I'm cheating, I initially wrote it as part of this very article, then decided that it would make a good standalone).

Anyway, go read it, then create yourself a `composer` alias if don't have one already:

    $ alias composer="docker run -i -t -v \$PWD:/srv ubermuda/composer"

You can now initialize a Symfony project like a boss:

    $ composer create-project symfony/framework-standard-edition SomeProject

Awesome. Give yourself a high-five, get a cup of coffee or whatever is your liquid drug of choice, and get ready for the real work.

## Single-container environment

Building a self-sufficient container to run a standard Symfony project is pretty easy. All we have to do is to install our usual stack of nginx, php5-fpm, mysql-server and redis, throw in a pre-made nginx vhost configuration, tweak some config files, and we're good to go.

