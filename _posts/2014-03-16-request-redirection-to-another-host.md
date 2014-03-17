---
layout: post
title: "Request redirection to another host"
description: "request redirection to another host on an nginx sever"
category: "server"
tags: ["server","Nginx","request","redirection"]
---
REQUEST REDIRECTION
===============================

Sometimes it might be required that a request has to be redirected to another host, and that too if the request is a POST request, simple request redirection won't work there. A better way of doing it on Nginx is described below, by this way, you would be able to redirect all kinds of requests, to required host.

go to required server module in your nginx configurations, and define the rules as below,

{% highlight bash %}
################### REDIRECTIONS  ###########################
        location /xyz/index.html {
                proxy_set_header Host your.required.host.com;
                proxy_pass https://your.required.host.com; # here the request would be redirected to https://your.required.host.com/xyz/index.html
        }

        location /abc/xyz/ {
                proxy_set_header Host my.host.net;
                proxy_pass https://my.host.net/pqr; # here the request would be redirected to https://my.host.net/pqr/abc/xyz/
        }
####################################################
{% endhighlight %}

By this example, you would be able to redirect any HTTP request, to the required host on the request url, your request data is preserved. After making changes you need to reload server config for the changes to take effect.

One thing to note here is, if for some reason your up-stream host (to where you are redirecting request) is down, for a while, the requests will start failing which is obivious, but the requests continue to keep failing even after up-stream host comes up. I had faced this issue once and I did not have time to debug it, probably I will debug it soon and share the findings. So the temprorary solution to this problem is that you just go and reload/restart the nginx server and everything starts working fine.

