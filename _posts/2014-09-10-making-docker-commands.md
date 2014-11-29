---
layout: post
title: Making Docker commands
tags:
- docker
- command
- alias
category: tech
---

[Docker](http://docker.com/) is awesome to package, ship and run daemon applications, but you sometime wish you could package binaries the same way, to avoid, for example, installing a shipload of crap on your workstation. This is very doable, and actually pretty easy. Let's see how we can dockerize [composer](http://getcomposer.org/), a PHP package manager, and then use it seamlessly on any PHP-less system.

First thing to do is to build a composer docker image. It's dead simple, because composer has very few dependencies.

Here's an example of Dockerfile that does just that:

    FROM debian:wheezy
    RUN apt-get update -qqy
    RUN apt-get install -qqy php5-cli php5-json curl
    RUN curl -sS https://getcomposer.org/installer | php
    VOLUME ["/srv"]
    WORKDIR /srv
    ENTRYPOINT ["/composer.phar"]

What happens here? Let's review this Dockerfile line by line.

    FROM debian:wheezy

This is the *base image*. The foundation upon which we're going to build our container. I like to use debian as a base because it's SO MUCH MORE lightweight than ubuntu, and yet still has apt and such niceties.

    RUN apt-get update -qqy
    RUN apt-get install -qqy php5-cli php5-json curl

Did I say "review line by line"? I lied. These two lines respectively update apt-get's index and install composer's dependencies. We could skip `curl` by using `php` to download composer's install script, I'll let this decision to your discretion, because I don't want to take this kind of decisions.

    RUN curl -sS https://getcomposer.org/installer | php

Standard composer installation procedure, nothing fancy here.

    VOLUME ["/srv"]
    WORKDIR /srv

The `/srv` directory will be the working directory of our container, so let's make it a volume, and change the default working directory of the image to it.

    ENTRYPOINT ["/composer.phar"]

Finally, run `/composer.phar` when we run the container. We use `ENTRYPOINT` here instead of `CMD` because it allows us to pass arguments to the command.

Put all this in a Dockerfile, and run this command from the same directory:

    $ docker build -t composer .

Boum. Just like that, you can now enjoy the awesomeness of composer, right from your PHP-less workstation.

Let's test this and create a Symfony project. Navigate to a directory you want to create the project in (`/tmp` could be a good place right now), and run composer's `create-project` command:

    $ docker run -v $PWD:/srv composer create-project symfony/framework-standard-edition AcmeDemo

*Holy macaroni!*, I hear you think – because yes, I can hear your thoughts, *am I really gonna type all this crap each time I want to run composer?!*. And you're right to ask this, because no you won't.

The awesome guys at [Marmelab](http://marmelab.com/) just published [a very interesting way to make *docker commands* using make](http://marmelab.com/blog/2014/09/10/make-docker-command.html), but that's not how we're going to do it (read it anyway, it's **really** very interesting). Instead, we'll do something much simpler (but less powerful): use aliases.

Most likely you're using bash, and if you're not, chances are you're a zsh hipster in which case I don't really like you, so I'll only cover bash here.

An alias is basically a command that, when ran, runs something else. You can use it to set default parameters for critical commands (for example, a lot of people alias `rm` to `rm -i` – I'll let you read `rm`'s man to see why), or you can use it to do awesome things, like make runing `composer` actually run `docker run -i -t -v $PWD:/srv composer`. Here's how:

    $ alias composer="docker run -i -t -v \$PWD:/srv composer"

That's it. You might want to notice that we escaped the `$` in `$PWD`. This is because if we don't, it will be evaluated by `alias` right now and we will end up with whatever is our current directory hardcoded in the alias, just as if we typed this instead (given `/some/path` is your current working directory):

    $ alias composer='docker run -i -t -v /some/path:/srv composer'

What makes aliases awesome, apart from their simplicity, is that anything you append to the aliased command will be appended to the resolved command too. So `composer create-project` will resolve as `docker run -i -t -v $PWD:/srv composer create-project`.

You can check that it works by running `composer create-project symfony/framework-standard-edition AcmeDemo` again, and noticing that composer now tells you the `AcmeDemo` directory already exists – because we created it earlier. Great.

Just running `alias` in your shell creates the alias locally. It means that the alias is not accessible from your other shells, and that it will be removed from existence the second you logout from this shell. Not nice. To make it persistent, you have to add it to your `~/.profile` file (at the end would be a good place), then logout and open a new shell.

You can check that it worked by running `alias` without argument to get the list of declared aliases in your shell:

    $ alias
    alias composer='docker run -i -t -v $PWD:/srv composer'

One last thing. At the begining of this post, I said docker was great to *package*, *ship* and *run*. We packaged and ran, and since I like you (yes, even if you're using zsh), I *shipped* too.

You can use the [`ubermuda/composer`](https://registry.hub.docker.com/u/ubermuda/composer/) image in an alias to do just what we described in this post, without the hassle to actually build the container. How cool is that? I'll save you the trouble to answer, *a lot* is how cool it is.

{% include see_also_book_discovering_docker.html %}