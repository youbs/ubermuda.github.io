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

This is nothing to brag about really, it's a fairly simple Docker image. There are three things I'd like to tell you about though.

## Base image inheritance

As you might have noticed already if you went to read the sources, the `ubermuda/pandoc-ebook` actually depends on the `ubermuda/pandoc` image. This is base image inheritance in action, and this is a good thing. You might already know inheritance from your programming background, and Dockerfile inheritance looks – and behave – a lot like it.

The main reason for using inheritance here is that I – and other people – also need Pandoc for purposes other than eBook generation. Having to use an image with kindlegen installed would make no sense in that case. It also makes the images simpler.

## Cache optimization

Then there's this:

    RUN apt-get install -y texlive-latex-base texlive-fonts-recommended
    RUN apt-get install -y cabal-install zlib1g-dev

Why the two `apt-get install` calls, you might ask. The reason is *cache optimization*. The `texlive-latex-base` package has **a lot** of dependencies, and installing them is by far the most time consuming task in this Dockerfile.

Imagine for a second there's only one `RUN apt-get install` instruction:

    RUN apt-get install -y cabal-install zlib1g-dev texlive-latex-base texlive-fonts-recommended

What happens if I want to install a new package? I need to change the `apt-get install` call like this:

    RUN apt-get install -y NEWPACKAGE cabal-install zlib1g-dev texlive-latex-base texlive-fonts-recommended

Which will, of course, invalidate the cache for this line. In the next build, I get to re-download and re-install every dependency of `texlive-latex-base`.

By splitting this in two instructions, I can easily add new packages to install without invalidating the cache for `texlive-latex-base`:

    RUN apt-get install -y texlive-latex-base texlive-fonts-recommended
    RUN apt-get install -y NEWPACKAGE cabal-install zlib1g-dev

Only the cache for the second `RUN` is invalidated. How neat is that?

## Image size optimization

Finally, this:

    RUN apt-get install -y curl && \
        curl http://kindlegen.s3.amazonaws.com/$KINDLE_ARCHIVE | tar xzC /kindle && \
        apt-get remove -y curl

This one is here to optimize the resulting image size. There are two other obvious ways to achieve the same. The first would be to have the `apt-get remove` on its own line:

    RUN apt-get install -y curl
    RUN curl http://kindlegen.s3.amazonaws.com/$KINDLE_ARCHIVE | tar xzC /kindle
    RUN apt-get remove -y curl

Or I could leverage the `ADD` instruction and get rid of the `apt-get` commands:

    ADD http://kindlegen.s3.amazonaws.com/$KINDLE_ARCHIVE
    RUN tar xzf $KINDLE_ARCHIVE -C /kindle
    RUN rm -f $KINDLE_ARCHIVE

Both are unfortunately less then optimal when it comes to image size, because of how the image layer system works. Let's focus on the first example and deconstruct it step by step:

1. `RUN apt-get install -y curl`
    
    Docker runs this command in a container created from whatever image resulted from the previous step. Let's call the 
    resulting image of this step `A`

2. `RUN curl http://kindlegen.s3.amazonaws.com/$KINDLE_ARCHIVE | tar xzC /kindle`
    
    Docker creates a new container from image `A`, runs the `curl | tar` command, and commits the result to image `B`

3. `RUN apt-get remove -y curl`

    Once again, Docker creates a new container from image `B`, runs the command, and commits the result as image `C`.

Now what, exactly, is image `C`? It is the composition of images `A`, `B` and `C`. That means that anything that is in images `A` and `B` are also in `C`. And what is in `A`? That's right, `curl`. So even though we uninstalled `curl` in step 3, the files are still present in `C`, because of `A`!

This easily verifiable using `docker images` (there's a `SIZE` field) and `docker history` to check the layers of an image.

This might not amount to much in this particular case, especially given the already huge size of `ubermuda/pandoc`, but in order to optimize your images, it's important to understand and keep in mind how the layer system works when you write your own Dockerfiles.

{% include see_also_book_discovering_docker.html %}