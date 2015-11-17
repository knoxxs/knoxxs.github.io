---
layout: post
title:  "Understanding SOP & CORS"
date:   2015-11-17 07:39:00
categories: programming javascript cors
tags: cross-domain cross-origin cross-origin-resource-sharing jsonp
---

I was doing [FreeCodeCamp](http://www.freecodecamp.com/) and in one of the task I ahve to use an API to pull in some quotes. While working on it I was getting `Cross origin request are not allowed error`. After spending about an hour or so I was able to make it work using `jsonp` but haven't understood anything under the hood. So I decided to dig a bit deeper and this post covers that. Also I was shocked while researching about this as there is no resource on the web which is covering the complete story on cross origin. Hope I cover it completely.

## What is same-origin policy (SOP)?
Its a policy defined by W3C which at least I all the major browsers implements. Under the policy, a web browser permits scripts contained in a first web page to access data in a second web page, but only if both web pages have the same origin. An origin is defined as a combination of `URI scheme`, `hostname`, and `port number`. This policy prevents a malicious script on one page from obtaining access to sensitive data on another web page through that page's Document Object Model. If you are not satisfied then follow [Why is the same origin policy so important?](http://security.stackexchange.com/questions/8264/why-is-the-same-origin-policy-so-important) to understand its importance. 

## What is the problem then?
JavaScript and the web programming has grown by leaps and bounds over the years, but the same-origin policy still remains. This prevents JavaScript from making requests across domain boundaries, and has spawned various hacks for making cross-domain requests. In some circumstances the same-origin policy is too restrictive, posing problems for large websites that use multiple subdomains.

## Either follow same-origin policy or don't?
The above two sections talks about the need of having same-origin policy and need of removing it. What should be a better solution and the answer is do not remove the same-origin policy but define some standard and rules for secure cross-origin access. This resulted int birth of CORS (Cross Origin Resource Sharing). But as `CORS` took its time to come, people found many workarounds for same-origin policy. One of them is `jsonp`.

## What a p can do to json?
`jsonp` (stands for _JSON with Padding_) takes the leverage of the fact that though cross-origin calls are not allowed but cross-origin `script import` is allowed in html like:
  
```html
<script src="http://server2.example.com/RetrieveUser?UserId=1234&jsonp=methodName"></script>
```

To understand it completely follow:

1. [Confused on how a JSONP request works](http://stackoverflow.com/questions/10193085/confused-on-how-a-jsonp-request-works)
2. [How JSONP Works](http://schier.co/blog/2013/09/30/how-jsonp-works.html)
3. [Is JSONP safe to use?](http://stackoverflow.com/questions/613962/is-jsonp-safe-to-use)
4. [Are there reasons not to use JSONP for AJA~X requests?](http://stackoverflow.com/questions/19173/are-there-reasons-not-to-use-jsonp-for-ajax-requests)

## What is CORS?
CORS introduces a standard mechanism that can be used by all browsers for implementing cross-domain requests. The spec defines a set of headers that allow the browser and server to communicate about which requests are (and are not) allowed. CORS continues the spirit of the open web by bringing API access to all.

To understand it completely see:

1. [Using CORS](http://www.html5rocks.com/en/tutorials/cors/)
2. [Cross-domain Ajax with Cross-Origin Resource Sharing](https://www.nczonline.net/blog/2010/05/25/cross-domain-ajax-with-cross-origin-resource-sharing/)
3. [Enable CORS](http://enable-cors.org/index.html) - This will give you everything on CORS.

## Other Solutions
There are many other hacks which people have found to work around same origin policy:

#### 1. Server-Side Proxy
This is the most basic and one of the first hack people did. An alternative to circumventing the Same-Origin Policy to perform Cross-Domain requests is to simply not make any Cross-Domain requests at all! If you use a proxy that resides in your domain, you can simply use this to access the external service from your back-end code and forward the results to your client code. Follow [JavaScript: Use a Web Proxy for Cross-Domain XMLHttpRequest Calls](https://developer.yahoo.com/javascript/howto-proxy.html) and [Using XMLHttp to Make Calls With a Proxy](http://support.brightcove.com/en/video-cloud/docs/using-xmlhttp-make-calls-proxy) for more.

#### 2. `document.domain` Property
If two windows (or frames) contain scripts that set domain to the same value, the same-origin policy is relaxed for these two windows, and each window can interact with the other. For example, cooperating scripts in documents loaded from orders.example.com and catalog.example.com might set their document.domain properties to “example.com”, thereby making the documents appear to have the same origin and enabling each document to read properties of the other. This might not always work as the port stored in the internal representation can become marked as null. In other words example.com port 80 will become example.com port null because we update document.domain. Port null might not be treated as 80 (depending on your browser) and hence might fail or succeed depending on your browser.

#### 3. Cross-document messaging
It allows a script from one page to pass textual messages to a script on another page regardless of the script origins. Calling the postMessage() method on a Window object asynchronously fires an "onmessage" event in that window, triggering any user-defined event handlers. A script in one page still cannot directly access methods or variables in the other page, but they can communicate safely through this message-passing technique.

## What about WebSockets?
Modern browsers will permit a script to connect to a WebSocket address without applying the same-origin policy. However, these browsers recognize when a WebSocket URI is used, and insert an Origin: header into the request that indicates the origin of the script requesting the connection. To ensure cross-site security, the WebSocket server must compare the header data against a whitelist of origins permitted to receive a reply.

## Disabling Same-Origin policy
In case you are doing some testing or proof of concept thing and you don't want to worry about the SOP, you can disable it:

1. [Disabling on Chrome](http://stackoverflow.com/questions/3102819/disable-same-origin-policy-in-chrome)
2. [Disabling on Firefox](http://stackoverflow.com/questions/17088609/disable-firefox-same-origin-policy)
3. [Disabling on Safari](http://stackoverflow.com/questions/4556429/disabling-same-origin-policy-in-safari)
4. [Disabling on IE](http://stackoverflow.com/questions/20947359/disable-same-origin-policy-internet-explorer)

## References

1. [Why is the same origin policy so important?](http://security.stackexchange.com/questions/8264/why-is-the-same-origin-policy-so-important)
2. [Confused on how a JSONP request works](http://stackoverflow.com/questions/10193085/confused-on-how-a-jsonp-request-works)
3. [How JSONP Works](http://schier.co/blog/2013/09/30/how-jsonp-works.html)
4. [Is JSONP safe to use?](http://stackoverflow.com/questions/613962/is-jsonp-safe-to-use)
5. [Are there reasons not to use JSONP for AJA~X requests?](http://stackoverflow.com/questions/19173/are-there-reasons-not-to-use-jsonp-for-ajax-requests)
6. [Using CORS](http://www.html5rocks.com/en/tutorials/cors/)
7. [Cross-domain Ajax with Cross-Origin Resource Sharing](https://www.nczonline.net/blog/2010/05/25/cross-domain-ajax-with-cross-origin-resource-sharing/)
8. [Enable CORS](http://enable-cors.org/index.html) - This will give you everything on CORS.
9. [JavaScript: Use a Web Proxy for Cross-Domain XMLHttpRequest Calls](https://developer.yahoo.com/javascript/howto-proxy.html)
10. [Using XMLHttp to Make Calls With a Proxy](http://support.brightcove.com/en/video-cloud/docs/using-xmlhttp-make-calls-proxy)

