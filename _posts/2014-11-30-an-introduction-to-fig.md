---
layout: post
title: An introduction to Fig
tags:
- docker
- fig
---

## What is orchestration?

Orchestration is the act of managing multiple containers at once. Most of the time when you start playing with Docker, you only handle one container at a time. Then you learn about networking and how putting all your processes in the same container is inconvenient, and in the blink of an eye you find yourself building multi-containers infrastructures. Your first tries might not be really complicated, two containers, maybe three, but it already feel cumbersome. Linking containers by hand, managing volumes, very quickly gets confusing, and there's obviously something more practical to do.

## Introducing Fig

That more practical something is called [Fig](http://www.fig.sh/). Fig started as a product from the guys at [Orchard](https://www.orchardup.com/), and soon became the de-facto standard for automated docker containers orchestration. It has since been acquired by Docker Inc. and went from de-facto standard to officially supported solution.

### Installing Fig

Fig is available as a Python package that you can install with the following comand:

    $ sudo pip install -U fig

It's that easy. If it doesn't work for you, there is more information [in Fig's official documentation](http://www.fig.sh/install.html).

### Using Fig

To orchestrate an infrastructure with Fig, you first describe it in an YAML configuration file. The syntax is straightforward and tries to stick as much as possible to things you already know from docker.

Here's an example Fig configuration for the [Pagekit](http://www.pagekit.com/) CMS:

    web:
        image: ubermuda/pagekit
        ports:
            - 80
        links:
            - db:pagekit_db_1
        volumes_from:
            - data
    db:
        image: orchardup/mysql
        environment:
            MYSQL_ROOT_PASSWORD: changethis
            MYSQL_DATABASE: pagekit
    data:
        image: busybox
        command: /bin/true
        volumes:
            - /pagekit/storage
            - /pagekit/app/cache

This configuration file declares three different containers.

The `web` container will be the web-facing container. It is built from the `ubermuda/pagekit` image – of which you can find [the source at ubermuda/pagekit](https://github.com/ubermuda/docker-pagekit) on GitHub. This container will expose port `80` (through the `ports` option), get linked to the `db` container with the alias `pagekit_db_1` (`links`) and the volumes from the `data` container will be mounted into it (`volumes`).

With the `db` container we can see how easy it is to declare environment variables inside a container: just use the `environment` configuration key. In our case, we declare all the values in the configuration file, but you could also omit the value and it would be inferred from the host's environment. For example:

    db:
        environment:
            MYSQL_ROOT_PASSWORD

The `MYSQL_ROOT_PASSWORD` environment variable would then be populated from the environment variable with the same name from the host machine.

Lastly, the `data` container declares all the directories we're going to use as volume data sharings through the `volumes` configuration key.

With that configuration written, you're just a `fig up` away from booting up your infrastructure:

    $ fig up
    Creating dockerpagekit_db_1...
    ...
    Creating dockerpagekit_data_1...
    Creating dockerpagekit_web_1...
    ...
    Attaching to dockerpagekit_db_1, dockerpagekit_web_1
    ...
    db_1  | 141110  4:14:02 [Note] /usr/sbin/mysqld: ready for connections.
    db_1  | Version: '5.5.38-0ubuntu0.12.04.1-log'  socket: '/var/run/mysqld/mysqld.sock'  port: 3306  (Ubuntu)
    ...
    web_1 | 2014-11-10 04:15:20,750 INFO success: nginx entered RUNNING state, process has stayed up for > than 1 seconds (startsecs)
    web_1 | 2014-11-10 04:15:20,750 INFO success: php5-fpm entered RUNNING state, process has stayed up for > than 1 seconds (startsecs)

The complete boot log can be a bit too long, so I only included relevant parts. As you can see, three containers are created, and `dockerpagekit_db_1` is linked to `dockerpagekit_web_1`, just like we wanted.

You might also notice that there's no log lines for the `data` container. That is because the `/bin/true` command does not output anything.

You can now check that everything is running fine by running the `docker ps` in another terminal, and checking out the web container with your web-browser (you will need to find out the mapped port before, either with `docker ps` or `docker port`).

### Limitations

As of the time of this writing, Fig does not support remote orchestration. That means you can only orchestrate an infrastructure on a single host. It's still useful because you want to get it right as soon as possible.

{% include part_of_book_discovering_docker.html %}