---
layout: post
title:  Scope and this in javascript"
date:   2015-10-01 11:29:00
categories: programming js
tags: javascript scope this _this that
---

This post is about this and that.

Yesterday while learning Ember, I came across `var _this = this;`. I have seen it many times in UI code. There is also a different version of it: `var that = this;`. Though I knew a bit about this (i.e. JS is function scoped) but still to be completely sure I wanted to know the exact reasons for this pattern. Many times I asked my UI friends but every time I got `closures` in reply. But as a beginner I felt that I should not know about `closures`. There should be a better explanation. So I found [`Scope and this in JavaScript`](http://javascriptplayground.com/blog/2012/04/javascript-variable-scope-this/) and its amazingly written that I don't need to make any notes of it. Also there is one more benefit of using it: `this` doesn't get minified.