---
layout: post
title: A base image to build ebooks
tags:
- docker
---

Today, I finished writing a new chapter for my [Discovering Docker book](https://geoffrey.io/books/discovering-docker.html) and happily went off to build the new ePub, Mobi and PDF files. There was one problem though: the PDF build was no longer working. Pandoc was throwing a weird error about a media file not being found. The really weird thing is everything went well on my mac (except, I don't have `pdflatex` on my mac, which is why I don't build the PDF on my mac).

After a bit of investigation, I found that the versions of Pandoc I use on my mac and on the PDF build machine are not the same, which got me thinking. Dude. You're writing a book on Docker. And you don't even build it in a container.

Well, no longer. I am pleased to present you `ubermuda/pandoc-ebook`, a Docker image with Pandoc, pdflatex and kindlegen installed:

* [Source at GitHub](https://github.com/ubermuda/dockerfiles/tree/master/pandoc-ebook)
* [Image at Docker Hub](https://registry.hub.docker.com/u/ubermuda/pandoc-ebook/)

This is nothing to brag about really, it's a fairly simple Docker image. There's a little something though that I'd like to share with you. If you already went to read the Dockerfile, you might have noticed and find this a bit odd:

```
RUN apt-get install -y texlive-latex-base texlive-fonts-recommended && \
    apt-get clean

RUN apt-get install -y curl pandoc && \
    apt-get clean
```

There are two things to notice here.

## Cache optimization

First, there are two `RUN apt-get install` instructions, where only one would suffice. This is done to take advantage of `docker build`'s caching system. The thing is, `texlive-latex-base` has **a lot** of dependencies, and installing them is by far the most time consuming task in this Dockerfile. Imagine for a second there's only one `RUN apt-get install` instruction:

```
RUN apt-get install -y curl pandoc texlive-latex-base texlive-fonts-recommended && \
    apt-get clean
```

What happens if I want to install a new package? I need to change the `apt-get install` call like this:

```
RUN apt-get install -y NEWPACKAGE curl pandoc texlive-latex-base texlive-fonts-recommended && \
    apt-get clean
```

Which will, of course, invalidate the cache for this line. In the next build, I get to re-download and re-install every dependency of `texlive-latex-base`.

By splitting this in two instructions, I can easily add new packages to install without invalidating the cache for `texlive-latex-base`:

```
RUN apt-get install -y texlive-latex-base texlive-fonts-recommended && \
    apt-get clean

RUN apt-get install -y NEWPACKAGE curl pandoc && \
    apt-get clean
```

Only the cache for the second `RUN` is invalidated. How neat is that?

## Image size optimization

The second thing to notice is that I run `apt-get clean` in the same `RUN` as `apt-get install`. This is done to reduce the size of the base image. Imagine I run `apt-get clean` in a separate `RUN` instruction:

```
RUN apt-get install -y texlive-latex-base texlive-fonts-recommended
RUN apt-get clean
```

When `docker build` encounters the first `RUN` instruction, it runs it in a new container from whatever image was produced by the previous instruction (we will call it image `A`), and commits it to a new image that we will call `B`. When it gets to the second `RUN`, it creates a new container based on image `B`, and runs `apt-get clean`, effectively cleaning apt's package cache, and commits this to image `C`. So what's the problem here? Simply put, **apt's package cache is still present in image `A`**.

Putting `apt-get clean` in the same `RUN` as `apt-get install` reduces the base image size by effectively

{% include see_also_book_discovering_docker.html %}