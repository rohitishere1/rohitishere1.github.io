---
layout: post
title: "Play with locate"
description: "A minor tweak with locate which one might require"
category: "OS"
tags: ["OS","Linux","MacOS"]
---
locate command
========================================

We are going to talk about locate utility commonly available on Linux and MacOS.
I happen to be a frequent user of locate command and have figured that its data does not get updated that frequently so, if you are using it for any critical purpose, you might have erronous responses.
So the way out is manually updating its database, how to do it is below.

***MacOS :***

{% highlight bash %}
sudo /usr/libexec/locate.updatedb
{% endhighlight %}

***Linux :***

{% highlight bash %}
sudo updatedb
{% endhighlight %}
