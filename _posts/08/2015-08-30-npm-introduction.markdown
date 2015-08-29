---
layout: post
title:  npm Introduction
date:   2015-08-30 00:49:00
categories: programming npm
tags: programming npm 'package manager' js node
---

Today I started with a tutorial to get a brief introduction about MEAN stack. The first thing you need with MEAN stack is node.js. With node.js you get `npm` node package manager. You might have used it in the past to install some other tools which you might be using. I have used it many times before. Still till now I only knew it's a package manager. Also I knew that it has capability to install global and local modules. Local is something like you can do with `python env`. But nothing was clear. So as I read [Anna Debenham said in her github-pages article](https://24ways.org/2013/get-started-with-github-pages/) it I also vowed to myself that I will figure it out and will write a blog post.

## Introduction

[A Beginner’s Guide to npm — the Node Package Manager](http://www.sitepoint.com/beginners-guide-node-package-manager/) is a perfect guide anyone can have for npm. Hats off to [Peter Dierx](https://twitter.com/peterdierx) for writing it.

So lets get started. npm is node package manger i.e. it lets you install, upgrade, remove/uninstall, search node packages. One more thing to notice is npm is now not limited only to `node`. You can also use npm with browserify, express, js, cordova, coffe-script, gulp, less, grunt and [many more](https://www.npmjs.com/#explicit).

## Package scope in npm
You can install package globally or locally with npm. Global packages are installed in `{prefix}/lib/node_modules/` where `{prefix}` is usually  `/usr/` or  `/usr/local`. Local packages are installed inside a `node_modules` directory where you ran the `npm install` command. This means global scope will need `sudo` access to install.

This made me think when should I install things as local and when should I install them globally. If I install package globally wouldn't it be bad as someone might also have to install them globally  if s/he is using my code. As stated above for global `sudo` access is also needed. Peter Dierx also provided a solution to this `sudo` problem which is explained below. But still what is the use of global modules?

## When to use which scope
I got the answer to when use the global from one of the [node.js's blog post](http://blog.nodejs.org/2011/03/23/npm-1-0-global-vs-local-installation). According to it:

**Global**: If you’re installing something that you want to use in your shell, on the command line or something, install it globally, so that its binaries end up in your PATH environment variable.

**Local**: If you’re installing something that you want to use in your program, using require('whatever'), then install it locally, at the root of your project.

But sometimes you can't choose then:

1. Install it in both places. Seriously, are you that short on disk space? It’s fine, really. They’re tiny JavaScript programs.
2. Install it globally, and then npm `link coffee-script` or `npm link express` (if you’re on a platform that supports symbolic links.) Then you only need to update the global copy to update all the symlinks as well.

## Getting started

All the other things which you want to know about npm like:

1. how to install/update/remove/search globally/locally
2. how to change the default global installation directory
3. how to install a specific version
4. what is npm cache
5. managing dependencies

are covered in [A Beginner’s Guide to npm](http://www.sitepoint.com/beginners-guide-node-package-manager/).

## npm vs bower
As define in a [SO question](http://stackoverflow.com/questions/18641899/what-is-the-difference-between-bower-and-npm):

npm is most commonly used for managing Node.js modules, but it works for the front-end too when combined with `Browserify` and/or `$ npm dedupe` where as Bower is created solely for the front-end and is optimized with that in mind.

The biggest difference is that npm does nested dependency tree (size heavy) while Bower requires a flat dependency tree (puts the burden of dependency resolution on the user).

A nested dependency tree means that your dependencies can have its own dependencies which can have their own, and so on. This is really great on the server where you don't have to care much about space and latency. It lets you not have to care about dependency conflicts as all your dependencies use e.g. their own version of Underscore. This obviously doesn't work that well on the front-end. Imagine a site having to download three copies of jQuery.

Also this is explained in a great detail in [Nested Dependencies](http://maxogden.com/nested-dependencies.html). It also covers all the pros and cons of both (for eg. `dll hell` in case of bower).

## Conclusion
This just covers the intro to npm. It is much more than this. To get a complete info follow the [official docs](https://docs.npmjs.com/). I will cover them as I use them as I feel knowing this much is enough for now.

## References

1. [npm](https://www.npmjs.com/)
2. [A Beginner’s Guide to npm — the Node Package Manager](http://www.sitepoint.com/beginners-guide-node-package-manager/)
3. [npm 1.0: Global vs Local installation](http://blog.nodejs.org/2011/03/23/npm-1-0-global-vs-local-installation)
4. [Nested Dependencies - Insight into why node_modules works the way it does](http://maxogden.com/nested-dependencies.html)