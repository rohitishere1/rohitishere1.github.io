---
layout: post
title: "More on Request rate limiting on Nginx"
description: "Tricks on requests rate limiting on Nginx"
category: "web servers"
tags: ["server","Nginx","request","web servers","rate limit"]
---
REQUEST RATE LIMITING
===============================

I wanted to cover more on request rate limiting than I had last time. I had covered [rate limiting per ip and white-listing](http://rohitishere1.github.io/2013/06/27/rate-limit-per-ip---nginx/). But what if you had to limit the overall requests being served. Imagine a case where my server is working happily at 30 requests per second and it could survive the load of 100 requests per second, suddenly gets 120 requests per second for some fraction of time, it could potentially put the service on un-recoverable loss. A minor spurt of requests for a fraction of time could be lethal, unless we handle it gracefully, so in that case we put a limit on overall requests being served. Sample config below.

{% highlight bash %}
        ..
            limit_req_zone $server_name  zone=test1:10m   rate=5r/s;
        ..
    server{
        ..
            limit_req zone=test1 burst=10 nodelay;
        ..
        }
{% endhighlight %}

I am chosing server name as the key since I wish to limit the number of requests being served by server so it will keep count of requests being served. We can also chose to have server port as key (`$server_port`) if we have multiple server names.

Now, if I wish to have rate limiting on any particular url, I would just specify a different location for it and specify rate limit for it. Sample config below,

{% highlight bash %}
        ..
            limit_req_zone $server_name  zone=test1:10m   rate=5r/s;
        ..
    server{
        ..
            location /v1/search {
              limit_req zone=test1 burst=10 nodelay;
              proxy_pass ......
            }
        ..
        }
{% endhighlight %}

 As per above config, rate limiting is restricted to /v1/search url. Similary, I can have a similar set of urls for which I can rate limit, I would have to define the location accordingly, I can also share the rate limit zone, so overall rate limit would be shared.
Also, if you wish to send specific response code when rate limit is hit, you can set it by limit_req_status directive `limit_req_status 429;`

I guess this covers everything, that I had missed out from my previous post on request rate limiting. Do let me know in comments if you want me to cover anything more.

