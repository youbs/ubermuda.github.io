---
layout: post
tags: [docker]
title: Dockerfiles maintainability pro-tips
---

Writing a basic Dockerfile might be easy, but as your requirements gradually complexify, so do your Dockerfiles. Until the point where it can be a bit hard to get it right. Here are a few advices, common-sense wisdom, and pro tips to write efficient and solid Dockerfiles.

This post is an excerpt from my book, [Discovering Docker](http://discoveringdocker.com/). The chapter original chapter has 2 more sections: **Optimization** and **Runnability**. I'm leaving the entire table of contents as-is so you can get a glimpse at all the great content that's waiting for you in the book!

1. [Maintainability](#maintainability)
    1. [Use inheritance](#use-inheritance)
    2. [Only one process per container](#only-one-process-per-container)
    3. [Split arguments, and sort them](#split-arguments-and-sort-them)
    4. [Use absolute paths with `WORKDIR`](#use-absolute-paths-with-workdir)
    5. [Pin versions when possible](#pin-versions-when-possible)
    6. [Use `COPY` instead of `ADD`](#use-copy-instead-of-add)
2. Optimization
    1. Use a `.dockerignore` file
    2. Choose a lightweight base image
    3. Avoid extra packages
    4. Understand the layering system
    5. Know your tools
    6. Know the Dockerfile DSL
    7. Optimize file downloading with curl
3. Runnability
    1. Use `EXPOSE`
    2. Expandable containers
    3. `ENTRYPOINT` vs `CMD`
    4. `ENTRYPOINT` vs `RUN`
    5. Run things as an unprivileged user
    6. Use an orchestration tool

As usual with *best practices*, they are to be taken with a grain of salt. Not all of them will apply to every case, and you should always question the relevance of an advice before applying it.

## Maintainability

### Use inheritance

Inheritance is the only mechanism at your disposal for Dockerfile re-usability, so use it. Do you have a set of packages you include in all your dev images, build a *devbase* image or something:

    FROM debian:jessie

    RUN apt-get update
    RUN apt-get install -y git vim curl

Do you build a lot of nodejs applications, write a base nodejs image and re-use it everywhere. Having a lot of images is not a bad thing. Build small, specialised images that you can re-use and build upon.

### Only one process per container

Remember the *Separation of Concerns* principle? It also applies to containers, and for the same reasons. Running only one process per container makes the Dockerfile simpler, easier to maintain and less error-prone. Whenever you feel like adding a new process in a container, ask yourself if it's really worth it or if this particular process wouldn't be better in its own container.

If you're afraid of the runtime complexity ("*damn, it's gonna be a mess to link properly*"), don't forget tools like fig exist, and you should definitely leverage them.

### Split arguments, and sort them

No biggie here, it just makes things more readable and maintenable.

Consider this instruction:

    RUN apt-get install -y nginx php5-cli php5-json git mysql-client php5-mysqlnd

Versus:

    RUN apt-get install -y \
        git \
        mysql-client \
        nginx \
        php5-cli \
        php5-json \
        php5-mysqlnd

Which is more readable and editable? Did you even notice git was being installed in the first example?

### Use absolute paths with `WORKDIR`

Use absolute paths in your `WORKDIR` instructions. It makes it easier to read and maintain your Dockerfile.

### Pin versions when possible

If you've ever used configuration management tools, or managed any kind of server, or dependency to an app, you know this one too. Pinning versions avoids bad surprises when all of a sudden a new version of some critical software is released that is not compatible with your stack.

You can use the `ENV` instruction to easily pin version and keep your Dockerfile readable:

    FROM ubermuda/nodejs

    ENV LESS_VERSION 1.7.5
    ENV PG_VERSION 9.1+134wheezy4

    RUN npm install -g less@$LESS_VERSION
    RUN apt-get install -y postgresql=$PG_VERSION

Of course, you could also pin versions directly in the configuration files of the corresponding tools.

### Use `COPY` instead of `ADD`

The `COPY` instruction has a lot less magic than `ADD`. Use it in priority.

{% include part_of_book_discovering_docker.html %}