---
layout: post
title:  "Reverse Proxy"
date:   2016-07-21 18:03:00 -0600
categories:
---
When setting up my personal website, I had a temporary server running for an Alexa project I was working on. Two applications cannot bind to the same port. To solve this I used a reverse proxy.

To best understand a [reverse proxy server](https://en.wikipedia.org/wiki/Reverse_proxy), it helps to first understand a [forward proxy server](https://en.wikipedia.org/wiki/Proxy_server). In a client-server relationship, a forward proxy acts in place for the client. A reverse proxy acts in place for the server.

The basic idea is the reverse proxy binds to port 80 and 443. When a request arrives the server evaluates some rules. Those rules are used to determine if the request is to be forwarded to another server. In this case another server running on the same machine.

To create the reverse proxy I used [ngnix](http://nginx.org). There are plenty alternatives. I chose nginx since it's what I know best.

The first configuration is very simple. It just serves static content.

```
# Tricks of the Trade
server {
  listen tricksofthetrade.pro:80;
  server_name tricksofthetrade.pro;

  root /var/www/tricksofthetrade.pro/public_html;
  index index.html;
}
```

The second configuration is more complicated. For clarity, I left out the SSL handling details. Simply put, any requests on port 443 with a path that starts with `/alexa` are forwarded to my Alexa server on port 6443.

```
# Alexa Server
server {
  listen 443 ssl;
  server_name tricksofthetrade.pro;
  # ...
  proxy_redirect off;

  location /alexa {
    proxy_pass https://localhost:6443;
  }
}
```

Note, an added benefit of running the Alexa server on port 6443 is that it no longer needs to run with super user permissions.

Each configuration is stored in a separate file. It allows me to easily enable and disable individual servers.

The Alexa server is no longer running, but building the reverse proxy was a good exercise. Now that it's in place, setting up and tearing down other servers will be easy.
