---
layout: post
title: "NPM behind firewall"

comments: true
redirect_from: "/2012/01/19/npm_behind_firewall"
permalink: npm_behind_firewall
---

If you are behind a corporate firewall and want to use [npm](https://www.npmjs.com/) , then you need to configure with foll options.
{% highlight bash %}
#npm config set registry http://registry.npmjs.org/ 
#npm config set proxy http://<my proxy>  
{% endhighlight %}

