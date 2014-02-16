---
layout: post
title: "strip server info"
description: "stripping server info from response headers"
category: "server" 
tags: ["server","Nginx","Tomcat","response","security"]
---
SERVER RESPONSE
===============================

Have you ever wondered, what information you are giving away in response to each request? By default, a server adds several info to response headers. Most of which is helpful and mandatory while other info makes you vulnerable at system security. Like if you are using an older version of a server which has some security flaw which is well known to the world, you are just broadcasting your security lapses, request by request. Since this behaviour of server is by default, one needs to turn this off manually. How to do it is very simple and described below.

***Nginx :***

just go to ` /etc/nginx/nginx.conf `
and in http module, search for server_tokens, if not present add it.

{% highlight bash %}
# hide nginx version in res headers
server_tokens off;
{% endhighlight %}

The valid context for server_tokens is http, server and location, so it can be added anywhere depending on the requirement.
Just reload/restart Nginx for this configuration to come into effect. This disables emitting Nginx version in error messages and in the "Server" response header field. Should you wish to enable it, replace ` off ` with ` on `.

***Tomcat :***

To hide server version in Tomcat, add ` server ` keyword in your ` Connector ` tag in ` CATALINA_HOME/conf/server.xml `, illustrated below

{% highlight bash %}
<Connector port="8080" ...
            server="Apache" />  <!-- server header is now Apache -->
{% endhighlight %}

To remove, server version from error messages, we need to update it in ` catalina.jar `.
Firstly, unpack catalina.jar

{% highlight bash %}
cd CATALINA_HOME/server/lib
jar xf catalina.jar org/apache/catalina/util/ServerInfo.properties
{% endhighlight %}

Open ServerInfo.properties and change server.info line to server.info=Apache Tomcat.
and then repack catalina.jar

{% highlight bash %}
cd CATALINA_HOME/server/lib
jar uf catalina.jar org/apache/catalina/util/ServerInfo.properties
{% endhighlight %}
 
Restart server for changes to take effect.

` P.S. one of possible issue that can be faced post this changes is, `
` some test scripts or probes that identify server version may not work. ` 

