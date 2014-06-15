---
layout: post
title: "3 things the authors of this picture have misunderstood"
date: 2014-05-28
author: Max Isacson
categories: linux
---

So this picture recently showed up on my facebook feed:

![scr](/assets/2014-05-28-3-things-the-authors-of-this-picture-have-misunderstood/scr_resize.jpg)

However it is complete nonsense. Let's see why.

##1. ROOT/CINT is a C/C++ interpreter
First of, [ROOT is a software package](http://root.cern.ch) developed by a team at CERN used extensively for data analysis in particle physics. Mainly it is used as an included library in compiled C/C++ code but when run from the terminal as in the picture above it acts as a C/C++ interpreter, much like the Python interpreter. However *it does not execute shell commands*. This is what it looks like when running the command inside the interpreter:

![root1](/assets/2014-05-28-3-things-the-authors-of-this-picture-have-misunderstood/root1.png)

##2. Wait, what directory are we trying to delete?
To me it seems that the authors are toying with the idea of deleting the root (`/`) directory present on \*NIX machines. However the command

```console
sudo rm -rf *
```

will not do that unless executed from the `/` directory, which the authors from the looks of the picture are not doing. Instead it will delete all the contents of the `xpeng` directory, which although unwise, will not break the system. The correct way to do it is:

```console
sudo rm -rf /
```

##3. `rm` preserves `/`
Okay, I might say too much here, since the system in use in the picture is OSX and I'm not familiar with the specific implementation of `rm`. But I digress. Most modern \*NIX systems, not all however (I'm looking at you BSD), implements `rm` in such a way so that it does not operate recursively on `/` by default to safe guard against PEBKAC errors. The flag to override this is `--no-preserve-root`. So the full command looks like:

```console
sudo rm -rf --no-preserve-root /
```

##Creative ways to break your system
You have to agree that `sudo rm -rf --no-preserve-root /` is pretty boring. It's very predictable so let's spice things up a bit.

###The russian roulette way
Every one knows how to play russian roulette. You have a six-shooter with a single bullet loaded, you spin the barrel and pull the trigger with the gun against your head. Fun, right? Let's emulate that in `bash`:

```console
sudo -i
[ $[$RANDOM % 6] == 0 ] && rm -rf --no-preserve-root / || echo "click"
```

###The kmem lottery way
Let's write random bits to the kernel memory!

```console
sudo dd if=/dev/urandom of=/dev/kmem bs=1 count=1 seek=$RANDOM
```


