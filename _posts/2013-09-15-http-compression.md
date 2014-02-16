---
layout: post
title: "HTTP Compression"
description: "About HTTP Compression, and enabling it in NGINX and Tomcat servers"
tags: ["nginx","server","Tomcat", "HTTP"]
---
HTTP Compression
=============================

The user experience is now becoming a very important factor in the e-commerce industry and faster page loads is also a key to improve it (I have heard feedbacks for faster page loads like, this site knows telepathy, you just think of it and there it is ;) you can guess the UX about it). HTTP compression enabling on hosted servers helps one to get better page speed. To understand the process in simplest terms is when a browser accepts gzip compressed content, when making request to server notifies through header that it accepts compressed content Accept-Encoding: gzip, deflate and server when sending response gzips content and adds tag to notify client Content-Encoding: gzip and so saves bandwidth and helps in faster page loads. What happens when a client does not accept compressed content? Nothing, its just normal request response.

Now how do you do it,

***Nginx :***

just go to ` /etc/nginx/nginx.conf `
and in http module add following lines, and reload/restart the server

{% highlight bash %}
# enable compression
gzip  on;
# This sets the response header Vary
gzip_vary on;
# compression level (between 1 and 9) where 1 is the least compression (fastest) and 9 is the most (slowest)
gzip_comp_level 6;
# configures how requests coming from a proxy should be handled. any means enable compression for all requests.
gzip_proxied any;
# MIME types to compress
gzip_types text/plain text/html text/css application/json application/x-javascript text/xml application/xml application/xml+rss text/javascript application/javascript text/x-js;
# the number and the size of the compression buffers
gzip_buffers 16 8k;
# disables compression for browsers that do not support it (here, MS Internet Explorer before version 6 SV1)
gzip_disable "MSIE [1-6]\.(?!.*SV1)";
{% endhighlight %}

***Tomcat :***
go to tomcat folder, `open conf/server.xml` , you will find `Connector` tag there, update it by adding attributes `compression`, `noCompressionUserAgents`, `compressionMinSize` and `compressableMimeType`. 

we would have to add following attributes in connector tag user conf/server.xml
compression="on"
compressionMinSize="2048" (min. file size to be able to be compressed)
noCompressionUserAgents="gozilla, traviata"
compressableMimeType="text/html,text/xml" (add other MIME type as required)

sample connector tag would look like -
{% highlight bash %}
<Connector port="8080" protocol="HTTP/1.1"
               connectionTimeout="20000"
               redirectPort="8443"
                compression="on"
                compressionMinSize="2048"
                noCompressionUserAgents="gozilla, traviata"
                compressableMimeType="text/html,text/xml" />
{% endhighlight %}

Add the tag and restart the server, the HTTP compression should be on now and you may test it.

