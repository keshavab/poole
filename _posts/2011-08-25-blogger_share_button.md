---
layout: post
title: "Blogger share button does not work, not visible"

comments: true
redirect_from: "/2011/08/25/blogger_share_button"
permalink: blogger_share_button
---

If google share button on blogger does not work with older templates, then add foll piece of css onto ur design template -
{% highlight css %}
.post-share-buttons a
{
position: relative;
display: inline-block;
}
{% endhighlight %}

