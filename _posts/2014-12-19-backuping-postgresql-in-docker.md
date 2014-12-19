---
layout: post
title: Backuping PostgreSQL with Docker
tags:
- postgresql
- docker
---

**DISCLAIMER**: I first wrote this blog post and discarded it thinking that there's nothing new in it. It's true that there's nothing new or complicated in this blog post, the only thing you'll learn by reading it is that you can use `pg_dump` to backup a PostgreSQL server running inside a Docker container.

Then one thing struck me. The most common *problem* I notice when explaining and answering questions about Docker to complete beginners is that they are looking for new solutions to existing problems, when really the old solutions still work.

Engineers have a tendancy to forget what they know, and that they can apply most of this knowledge to new paradigms without much change. So eventually, this blog post serves three purposes:

1. Show you an example usage of `docker exec` usage
2. Remind you that the knowledge you gained in the *before-docker* era is still useful
3. Bump you into framing this knowledge into a container mindset

On with the show!

---

Really, this post could have been called *Backuping your RDBMS with Docker*, but I needed some concrete stuff for the examples. Since the question that triggered this post was about PostgreSQL, I decided to go along with it.

Here's the situation. You have a PostgreSQL server running inside a Docker container, and you're pretty happy with the combined powers of these two awesome pieces of software. But you're a smart person, and you know that, sooner of later, you will need backups of this database. Maybe there hardware will fail, or maybe the new intern will fail, you never know. So you decide to backup the database regularly.

But how do you do this. PostgreSQL is running inside a container and there's no obvious way to access the data from the outside.

The problem boils down to running `pg_dump` against your PostgreSQL instance, and you have three ways of achieving this.

First, the insider. Depending on your version of Docker, use either [nsenter](https://github.com/jpetazzo/nsenter) or [docker exec](http://docs.docker.com/reference/commandline/cli/#exec) to gain a shell inside the container, and dump your data (using `pg_dump`) to a shared volume (`postgres` being the name of the container running PostgreSQL):

    docker exec postgres pg_dump -h db -f /shared/backup.sql

But we can do better, right? The ideal workflow would involve being able to connect to the PostgreSQL without needing a shell in its container. There are two ways to do this.

First, the *containerize everything* approach. You build a `pg_dump` container (that's a container that has `pg_dump` as its `ENTRYPOINT`), and link it to the PostgreSQL container at runtime. Here's a Dockerfile for the `pg_dump` container:

    FROM debian:wheezy

    RUN apt-get update -y && \
        apt-get install -y postgresql-client && \
        apt-get clean -y

    ENTRYPOINT ["/usr/bin/pg_dump"]

Build it with `docker build`:

    $ docker build -t pg_dump - < Dockerfile

And run it:

    $ docker run -it --link postgres:db pg_dump -h db

With `postgres` being the container where your PostgreSQL is running.

The last solution, and the less interesting to describe, is simply to expose port `5432` of your container and running `pg_dump` against it, but honestly, where's the fun?