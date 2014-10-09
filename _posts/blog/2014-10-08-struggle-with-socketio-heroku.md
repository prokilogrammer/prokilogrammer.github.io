---
layout: post
category: blog
tags: [Socket.io, Heroku]
title: Struggle with Socket.io and Heroku
---

Today at work, we used Socket.io to collect real-time usage statistics of our site. All that our client does is send a request to our server when certain events happen. There is no two-way communication nor there is any cross-client collaboration. This worked perfecly fine on our local machine, but after deploying to Heroku hell broke loose. This post is to document the problems we faced and how we solved it:


## Problems: 

  - Most Socket.io's calls would throw this error: `Error during WebSocket handshake: Unexpected response code: 503`
  - Socket.io connection was reset before completing handshake
  - Sporadically establish connection with server and communicate. But it will not last forever

We think this was caused by Heroku's router trying to route Socket.io's requests from the same client to different dynos.


## Solution: 

We downgraded from v1.1 to v0.9.17 and manually selected the list of transports to fallback to. We also set the XHR polling interval to be 10seconds. This seemed to make everything work.

{% highlight js %}
    io.set("transports", [
        "websocket",
        "flashsocket",
        "htmlfile",
        "xhr-polling",
        "jsonp-polling"
    ]);
    io.set("polling duration", 10);
{% endhighlight %}


Even after all this, we ran into a weird issue where one of the clients started polling the server almost every milli-second. This kept happening for more than two hours. We aren't sure why this happened. I filed a ticket with Socket.io asking for help - [https://github.com/Automattic/socket.io/issues/1817](https://github.com/Automattic/socket.io/issues/1817)


