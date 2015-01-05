---
layout: post
title: The builder container
tags:
- docker
---

If building containers has taught me one thing, it's that the simpler the container, the better. Indeed, complex containers, just as any complex system, come with a handful of problems:

1. they take longer to build
2. they are more difficult to maintain
3. their execution – and manipulation in general – is more error-prone.

These are all caracteristics of a complex system, so nothing really new here.

Now how do you *simplify* a container. It's simple, if you are a developer, you already have half of the answer: separation of concerns. Yes, your old *SoC* friend, that helped you build robust code, is now going to help you build robust containers!

The idea is simple, yet brilliant: move the building part of your containers to a dedicated container. Say you're dockerizing a Symfony2 application. Most of the time, your container has things such as [composer](https://getcomposer.org/) or [bower](http://bower.io/) in it, and the wrapper script runs commands like `composer install` and `bower install`. That's some complexity that need not be in the app container. Consider instead moving all of this in a dedicated container:

    FROM debian:jessie

    ADD https://deb.nodesource.com/setup /root/nodejs-setup
    RUN /root/nodejs-setup
    RUN apt-get update && apt-get install -y nodejs php5-cli php5-json
    RUN php -r "readfile('https://getcomposer.org/installer');" | php
    RUN npm install -g bower

    COPY build.sh /root/build.sh

    CMD ["/root/build.sh"]

The `build.sh` build script can look something like:

    #!/bin/bash -ex

    if ! mountpoint -q /srv; then
        echo "No application detected in /srv"
        exit 2
    fi

    cd /srv

    bower install --allow-root
    /composer.phar install

I usually store specific Dockerfile's in subdirectories of a `docker` directory in my project, so for example, I'd store my builder Dockerfile and its context in `docker/builder/Dockerfile`.

You now just have to build your container, and run it:

    $ docker build -t acme/builder docker/builder
    $ docker run -v $PWD:/srv acme/builder

Wow that's cool! Now I can remove composer and bower from my app container!

If you're lazy, you can even write a small Makefile to help you type less:

    .PHONY: build builder

    build:
        docker run -v $(shell pwd):/srv acme/builder

    builders:
        docker build -t acme/builder docker/builder/

You can then easily rebuild the container and your project:

    $ make builder
    $ make build