---
layout: post
title: Create a branch from a stash
tags:
- git
- stash
category: tech
---

The other day I stumbled upon an interesting Git question on the HumanCoders' forums, [can you push a stash?](https://forum.humancoders.com/t/peut-on-pusher-un-stash/390) (in french).

The question already had an answer (basically, *no you can't*), but even though the answer was technically correct, I felt there's something you can do that's *close enough*: create a branch from a stash, then push it.

Have you ever wondered how a stash is stored in Git? If yes, then you certainly already know what I'm going to describe. If not, well, you're not curious enough.

Git stores stashes as commits. Yes, you read well, commits. From that, it's pretty easy to create a branch from a stash.

To illustrate this, consider an empty repository:

    ~$ git clone git@github.com:ubermuda/test
    Cloning into 'test'...
    warning: You appear to have cloned an empty repository.
    Checking connectivity... done.
    ~$ cd test/

Say you're doing some work, creating and committing files:

    test (master #) $ echo foo > README.md
    test (master #%) $ git add .; git commit -m "initial commit"
    [master (root-commit) 93a1be7] initial commit
     1 file changed, 1 insertion(+)
     create mode 100644 README.md

Ok, that's not very useful, and I'd certainly get fired if that was all I did of my work days. Fortunately, I am self-employed so it's ok.

Now we need some actual material to work on to illustrate the whole *stash branch* concept, so just modify the `README.md` file and stash it:

    test (master) $ echo bar > README.md
    test (master *) $ git stash
    Saved working directory and index state WIP on master: 93a1be7 initial commit
    HEAD is now at 93a1be7 initial commit

Here's the interesting part, check the repository's graph:

    test (master) $ git log --all --graph --oneline --decorate
    * 236ada7 (refs/stash) WIP on master: 93a1be7 initial commit
    |\
    | * 142767c index on master: 93a1be7 initial commit
    |/
    * 93a1be7 (HEAD, master) initial commit

There are two new commits, `142767c` and `236ada7`. The interesting one is `236ada7`, because it contains the modifications in the stash (`142767c` is just a dummy parent for the stash, it has the same content as our initial commit `93a1be7`).

We can now create a branch from that stash with `git branch`:

    test (master) $ git branch bar 236ada7

And it works!

    test (master) $ git checkout bar
    Switched to branch 'bar'
    test (bar) $ cat README.md
    bar

Now to achieve our initial goal, let's push that branch:

    test (bar) $ git push origin bar
    ...
    To git@github.com:ubermuda/test
     * [new branch]      bar -> bar

And that's it. Even though we did not technically push the stash, we created a branch from it and pushed it, achieving the same purpose!

**UPDATE**: [Cedric Exbrayat](https://twitter.com/cedric_exbrayat) [just pointed out](https://twitter.com/cedric_exbrayat/status/520547071569715200) that you can in fact use the `git stash branch` command to achieve just that, with added benefits (automatically dropping the stash for example), which shows just why I love Git (apart from graphs): you never stop learning!