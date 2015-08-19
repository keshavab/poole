---
layout: post
title: "Converting windows line delimiters into unix delimiters"
comments: true
---

The problem of working in windows and later transferring files to unix is that it will have a "^M" character at end of each file. This is because in Windows, lines 
end with both the line feed and carriage return ASCII characters, but Unix uses only a line feed.

To convert them there are umpteen ways, dos2Unix is one such handy way.

Install cygwin on windows. u'll get all linux "Type" utility commands on windows.
(if u r already on a *nix system, most of them will already have it.)

use "find" command to find list of files and run dos2Unix on each of them to correct all files.

One problem before that. windows too has a find command and to have our find command take precedence over windows one,
Edit your path (System Properties -> Advanced -> Environment Variables -> System Variables -> Path) and move the entry for cygwin to the front of the path.

Then run the foll command in directory where u need to convert files.
{% highlight bash %}
$find . -name "*.java" -o -name "*.mxml" -o -name "*.xml" -o -name "*.as" | xargs -I {} dos2Unix {}
{% endhighlight %}

