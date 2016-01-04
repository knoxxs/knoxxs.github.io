---
layout: post
title:  Chat Architecture Everywhere
date:   2015-11-23 10:02:00
categories: chat architecture real-time
tags: websockets chat xmpp gcm google-cloud-messaging socket.io stanza.io firebase parse smack sse server-sent-events long-polling polling BOSH comet android slack telegram quickblox
---

Till now [Supertext](http://supertextnow.com/) is working on popular chat apps like Facebook Messenger, WhatsApp etc. Now we want to make our on hat app in which we will be having many customized experience. As in past everywhere I have seen the chat example, I thought it would be really simple to make a chat app as there will be a standard defined on web to make one. But on first glance I was swirled with number of awailber options to make the chat app. The only thing came to my mind was:

![Chat architecture everywhere]({{ site.url }}assets/2015/11/chat-architecture/chat-architecture-everywhere.jpg)

There were many things to thing off like Real Time using WebSockets / Long Polling / Server Sent Events / Comet / BOSH, Jabber Server + XMPP + socket.io, Firebase + Parse / Firechat, Slack / Telegram / Actor, stanza.io / Google Cloud Messaging / OneSignal, Quickblox / SupportKit. I will try to answer all these different questions in different posts.

## Introduction 
### Comet / BOSH
Polling / Long Polling / SSE / WebSockets  all lies under [comet](http://knoxxs.github.io/http/architecture/communication/2015/11/18/real-time-web-base-technologies/). [WebSocket](http://knoxxs.github.io/programming/real-time-web/2015/11/20/understanding-websockets/) is the latest tech under the comet umbrellan which trying to solve the problem of real time communication by providing server push. 

[BOSH](https://www.wikiwand.com/en/BOSH) is bidirectional communication protocol written for the XMPP.

### Jabber + XMPP + stanza.io
[Extensible Messaging and Presence Protocol (XMPP)](https://www.wikiwand.com/en/XMPP) is a set of application protocol based on XML (Extensible Markup Language) for doing real-time chat (and many other things, for that matter) - it then has to be transported across the network somehow, so you need a transport binding.

XMPP used to be called Jabber initially. [eJabber](https://www.ejabberd.im/) and [OpenFire](http://www.igniterealtime.org/projects/openfire/) are two famous out of the box XMPP servers.

[stanza.io](https://github.com/otalk/stanza.io) Modern XMPP in the browser, with a JSON API

### Firebase + Parse / Firechat 
[Firebase](https://www.firebase.com/) can power your app's backend, including data storage, user authentication, static hosting, and more.

[Parse](https://www.parse.com/) Instantly add a powerful cloud database, push notification services, and analytics tracking to your app.

[Firechat](https://firechat.firebaseapp.com/) is an open-source, real-time chat widget built on Firebase.

### Slack / Telegram / Actor
[Slack](https://slack.com/) is a messaging app for teams who are changing the world

[Telegram](https://telegram.org/) is a cloud-based mobile and desktop messaging app with a focus on security and speed

[Actor](http://actorapp.github.io/platform/) Including modern applications for Web, iOS, iPad, Android and SDK for building your own WhatsApp.
 
### socket.io / Google Cloud Messaging / OneSignal 
[socket.io](http://socket.io/) is the fastest and most reliable real-time engine. To understand how `socket.io` works follow [this](http://stackoverflow.com/questions/16719282/how-does-socket-io-work).  

[Google Cloud Messaging (GCM)](https://developers.google.com/cloud-messaging/) is a simple and reliable messaging to reach over a billion devices.
 
[OneSignal](https://onesignal.com/) is a high volume, cross platform push notification delivery.

There are also many other options to choose from like Pusher or PubNub.

### Quickblox / SupportKit
[Quickblox](https://quickblox.com/) is ready-to-go modules add new functionality such as video calling, instant messaging and push notifications.
 
[SupportKit](https://smooch.io/) is a mobile and web messaging API & SDKs for apps and businesses.

## Solutions

So that now I have covered all the top techs which you can use to make chat application. Lets compare them. These are the some popular combinations I am comparing. You can find an alternative to any tech or tool mentioned here. I think from the birth of digital age people are making chat apps so there are so many tools & tech available out there.

### Using XMPP lib

XMPP is just a protocol which brings a standard in chat servers so that a chat client/server can talk to any other chat/server. With XMPP you need a network binding ans server/client libs implementing XMPP using the selected network-binding.
 
You can find XMPP list of XMPP libs [here](https://xmpp.org/xmpp-software/libraries/), XMPP servers [here](http://xmpp.org/xmpp-software/servers/) and ready to use clients [here](http://xmpp.org/xmpp-software/clients/). These different solutions uses different base tech underneath like network bindings: BOSH, long polling, websockets etc.

In this solution you have to setup a XMPP server, use server XMPP lib and client XMPP lib.

You might also want to consider [Google moves away from the XMPP open-messaging standard](http://www.zdnet.com/article/google-moves-away-from-the-xmpp-open-messaging-standard/).

### Firebase + Parse
Firebase and Parse both are [Backend as a Platform](https://www.wikiwand.com/en/Mobile_Backend_as_a_service) providers and both are real time. There are some differences as mentioned [here](https://www.quora.com/Cloud-Services/How-does-Firebase-differ-from-Parse) & [here](http://stackoverflow.com/questions/27716333/which-one-is-better-to-user-between-parse-firebase-and-aws-cognito).

There are many examples of making chat app using firebase: [using ionic](https://www.airpair.com/angularjs/posts/build-a-real-time-hybrid-app-with-ionic-firebase), [using ember](https://medium.com/@sterlingcobb/how-to-build-a-realtime-chatting-app-with-firebase-and-emberjs-from-scratch-8ef614acf820#.j2724ezab) , [firechat](https://firechat.firebaseapp.com/) and you can find an example with any popular tech present out there. Parse example is [here](https://maniacdev.com/2014/07/example-full-featured-chat-app-with-a-great-interface-using-parse-for-the-backend).

I am unable to find anyone implemented XMPP over Parse or Firebase. According to me that is also possible.

You might also want to consider [Is it possible to use Parse.com for realtime chat (like Socket.io)?](https://www.quora.com/Is-it-possible-to-use-Parse-com-for-realtime-chat-like-Socket-io)

### Slack / Telegram / Actor
I don't know ahy it not mentioned anywhere but according to me we can use slack /telegram or actor to implement the chat application.

With slack you get complete slack backend along with a REST and real time API.

With telegram you are getting everything you get with slack except the real time API but you get open source telegram implementations for all the major platforms which internally should be using real time techs to connect to telegram server. Though you might also face some problems as mentioned [here](https://yalantis.com/blog/whats-wrong-telegram-open-api/).

Actor is another alternative to telegram but there are some improvements as mentioned [here](https://www.reddit.com/r/scala/comments/3c9blu/messaging_platform_with_actors_in_its_heart/).

### socket.io / Google Cloud Messaging / OneSignal
In case you want to have your own server but you only want a real time tech to communicate then you can choose any of socket.io / GCM etc. There are other alternatives too like [Real-time framework](http://framework.realtime.co/messaging/).

[Strophe.js](http://strophe.im/strophejs/) and [stanza.io](https://github.com/otalk/stanza.io) are js lib which combines socket.io and XMPP.

### Quickblox / SupportKit
Also you can use a third party services which does anything for you using any of the above techs but it will come with a price. There are many alternatives on web like [Pepper Talk](http://getpeppertalk.com/), [Jiver](http://jiver.io/), [layer](https://layer.com/), [buddy](http://buddy.com/) 

## Other considerations

### Why XMPP is good
You can't use just websockets or anything for chat - leaving XMPP. This really means is that you're inventing your own application-layer protocol for chat, and the odds are you're going to save a lot of time and headaches by taking advantage of the work that's already gone into writing one with useful properties (security, identity, extensibility etc.) and for which there are existing libraries and servers by going XMPP instead.

### Mobile battery usage considerations
As suggested in [this SO answer](http://stackoverflow.com/a/19950258/742173) Do not use sockets. That means you will need to keep a service running and maintain a connection at all times on each client device. That will drain the battery like crazy and no one will use it. What every-single-one of those apps use including WhatsApp, Hangout, Gmail, Facebook messenger is the Google cloud messaging (GCM). Follow [this](http://stackoverflow.com/questions/15617615/websockets-versus-gcm-under-android-battery-usage) & [this](http://stackoverflow.com/questions/11508613/how-does-push-notification-technology-work-on-android/11508811#11508811) for more details.

Most of push services like OneSignal uses GCM underneath when used on mobile. So in most cases you are safe to go but do check for this.
 
### XMPP BOSH vs COMET
[This SO answer](http://stackoverflow.com/a/7332789/742173) perfectly sums it up:

1. BOSH and Comet are the same thing.
2. The reason I say that they are the same thing, at a fundamental level, is that they both originally (see below) relied upon two HTTP connections. The first is a back channel which is either a long-held HTTP Streaming connection or use for HTTP Long-Polling. The second connection is short-lived and used for sending commands such as subscription requests. These two connections allowed for the bi-directional communication to be simulated.
3. BOSH differs is it offers added detail within it's protocol and was developed with XMPP in mind. Any Comet server could be developed to adhere to the standards defined in the BOSH spec.
4. Existing technology stack mostly use WebSockets as they offer a standardised and efficient bi-directional communications mechanism.

![Realtime web technology stack diagram]({{ site.url }}assets/2015/11/chat-architecture/realtime-web-technology stack-diagram.png)

## References:

1. [Understanding WebSockets](http://knoxxs.github.io/programming/real-time-web/2015/11/20/understanding-websockets/)
2. [BOSH](https://www.wikiwand.com/en/BOSH)
3. [Extensible Messaging and Presence Protocol (XMPP)](https://www.wikiwand.com/en/XMPP)
4. [eJabber](https://www.ejabberd.im/)
5. [OpenFire](http://www.igniterealtime.org/projects/openfire/)
6. [Firebase](https://www.firebase.com/)
7. [Parse](https://www.parse.com/)
8. [Firechat](https://firechat.firebaseapp.com/)
9. [Slack](https://slack.com/)
10. [Telegram](https://telegram.org/)
11. [Actor](http://actorapp.github.io/platform/)
12. [stanza.io](https://github.com/otalk/stanza.io)
13. [socket.io](http://socket.io/)
14. [Google Cloud Messaging (GCM)](https://developers.google.com/cloud-messaging/)
15. [OneSignal](https://onesignal.com/)
16. [Real time web - base technologies](http://knoxxs.github.io/http/architecture/communication/2015/11/18/real-time-web-base-technologies/)
17. [XMPP - Libraries](https://xmpp.org/xmpp-software/libraries/)
18. [XMPP - Servers](http://xmpp.org/xmpp-software/servers/)
19. [XMPP - Libraries](http://xmpp.org/xmpp-software/clients/)
20. [Cloud Services: How does Firebase differ from Parse?](https://www.quora.com/Cloud-Services/How-does-Firebase-differ-from-Parse)
21. [Which one is better to user between Parse, Firebase and AWS Cognito?](http://stackoverflow.com/questions/27716333/which-one-is-better-to-user-between-parse-firebase-and-aws-cognito)
22. [Build a Real Time Hybrid App with Ionic & Firebase](https://www.airpair.com/angularjs/posts/build-a-real-time-hybrid-app-with-ionic-firebase)
23. [How to build a realtime chatting app with Firebase and EmberJS from Scratch](https://medium.com/@sterlingcobb/how-to-build-a-realtime-chatting-app-with-firebase-and-emberjs-from-scratch-8ef614acf820#.j2724ezab)
24. [Example: Full Featured Chat App With A Great Interface Using Parse For The Backend](https://maniacdev.com/2014/07/example-full-featured-chat-app-with-a-great-interface-using-parse-for-the-backend)
25. [Firechat](https://firechat.firebaseapp.com/)
26. [What's Wrong With Open Source Telegram?](https://yalantis.com/blog/whats-wrong-telegram-open-api/)
27. [Messaging Platform with Actors in its heart](https://www.reddit.com/r/scala/comments/3c9blu/messaging_platform_with_actors_in_its_heart/)
28. [Strophe.js](http://strophe.im/strophejs/)
29. [Google moves away from the XMPP open-messaging standard](http://www.zdnet.com/article/google-moves-away-from-the-xmpp-open-messaging-standard/)
30. [Is it possible to use Parse.com for realtime chat (like Socket.io)?](https://www.quora.com/Is-it-possible-to-use-Parse-com-for-realtime-chat-like-Socket-io)
31. [Xmpp Vs Websocket](http://stackoverflow.com/questions/26549010/xmpp-vs-websocket)
32. [XMPP, WebSocket, and other questions](http://stackoverflow.com/questions/19949995/xmpp-websocket-and-other-questions)
33. [Websockets versus GCM under Android: battery usage?](http://stackoverflow.com/questions/15617615/websockets-versus-gcm-under-android-battery-usage)
34. [How does push notification technology work on Android?](http://stackoverflow.com/questions/11508613/how-does-push-notification-technology-work-on-android/11508811#11508811)
35. [XMPP BOSH vs COMET](http://stackoverflow.com/questions/7327153/xmpp-bosh-vs-comet)
36. [An XMPP Subprotocol for WebSocket](https://datatracker.ietf.org/doc/rfc7395/)
37. [Real-time framework](http://framework.realtime.co/messaging/)
38. [Pepper Talk](http://getpeppertalk.com/)
39. [Jiver](http://jiver.io/)
40. [layer](https://layer.com/)
41. [buddy](http://buddy.com/)
42. [how does socket.io work?](http://stackoverflow.com/questions/16719282/how-does-socket-io-work)