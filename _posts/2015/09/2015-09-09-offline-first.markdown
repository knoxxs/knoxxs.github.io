---
layout: post
title:  Offline first web applications
date:   2015-09-09 20:10:00
categories: programming offline
tags: offline web-app app 
---

Now a days *Offline First* is the new revolution going on the web. First I heard about `UpUp` a library which helps you make your website offline available. I got curious how. Here what I found.

## Offline First

Its the community which got together to support Offline First. According to them they want to make the Offline First the same thing as Mobile First. Some things defined on their website which may help you understand this movement:

1. We can’t keep building apps with the desktop mindset of permanent, fast connectivity, where a temporary disconnection or slow service is regarded as a problem and communicated as an error - mobile network is not stable.
2. A brief intro about offline first : [Say hello to offline first](http://hood.ie/blog/say-hello-to-offline-first.html)
3. This complete thing is very well defined in [an article by A List Apart](http://alistapart.com/article/offline-first). Do read the [Taking stock](http://alistapart.com/article/offline-first#section1) section.

## UpUp

A tiny script that makes sure your site is always there for your users. An offline first library that hides the complexity of using Service Workers and the new Caching API, and lets developers easily create sites that are accessible both online and offline. Sample code you have to insert:

```html
 <script src="/upup.min.js"></script>
  <script>
    UpUp.start({
      'content-url': 'offline.html' // show this page to offline users
    });
  </script>
```

### How it works
As defined in their [tutorial]():

>UpUp uses ServiceWorkers to determine when network requests fail. A ServiceWorker is a special script that runs in the browser in the background, and can see the status of network requests.

>When the user first visits your site, UpUp registers a ServiceWorker with your browser, and gives it a list of files to cache for later.

>The next time the user visits your site, the ServiceWorker listens for network errors. If a network request fails, and the ServiceWorker finds that file in the cache, it will return that file, as if it came from the network.

### Online like version
The next best thing we can do is to keep the data also cached based on the user and keep updating this cache based on [`Caching Headers`](http://diveintohtml5.info/offline.html)

## References

1. [Offline First](http://offlinefirst.org/)
2. [UpUp](https://www.talater.com/upup/)
3. [Designing Offline-First Web Apps](http://alistapart.com/article/offline-first)
4. [Getting Started with Offline First using upup](https://www.talater.com/upup/getting-started-with-offline-first.html)
5. [Let’s take this offline](http://diveintohtml5.info/offline.html)