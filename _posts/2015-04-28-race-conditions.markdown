---
layout: post
title:  "Race Conditions"
date:   2015-04-28 8:45:09
categories: "race condition"
tags: [race condition, thread, concurrency, hacking]
---

I came across [Race conditions on Facebook, DigitalOcean and others (fixed)](http://josipfranjkovic.blogspot.co.uk/2015/04/race-conditions-on-facebook.html) from reddit. I never suspected before that using race condition to hack someone will be that easy. So I decided to try the same thing.

After some thinking I realized I first need to find any offer which adds money in my account. Normal discount coupons won't help because there only one call is made, the actual payment call.  
Also first I need to learn how to make multiple request with the same session. I know I can share the same cookie, but I making this an opportunity for me to learn all about sessions.