---
layout: post
title: Know your tools
tags:
- docker
- general
---

If you [follow me on GitHub](https://github.com/ubermuda), you might have noticed I just pushed (a few days ago) a global update to all my Dockerfile repositories. In this update, I basically remove all calls to `apt-get clean`, and you may wonder why I do that, since I explained in an earlier post how to use it efficiently. Well, I also [updated that blog post](/a-base-image-to-build-ebooks.html). So what happened? Simply put: I didn't know my tools.

Most of my Dockerfiles are based off the official `debian` image, usually `debian:jessie`, since it's the latest. In order to minimize the size of my images, I figured running `apt-get clean` whenever I `apt-get install` anything would be great. Turns out the folks who maintain the `debian` image though of this too.

If you check apt's configuration in the official `debian` image, you will see that it is already configured to remove the package cache after each `apt-get install` run (the actual configuration is in `/etc/apt/apt.conf.d/docker-clean`).

What does it mean? It means I was not only doing something useless, which is bad, but also advocating it, which is super-bad.

The morale here is **know your fucking tools**. Is there an image that you use as a base for most of your images? Inspect it. Know how it's built. Know what is already configured and what is not. It is only common-sense, but like I said [in another post](/backuping-postgresql-in-docker.html), engineers tend to lose all notion of common-sense in the face of a new shiny tool, so don't be that guy!