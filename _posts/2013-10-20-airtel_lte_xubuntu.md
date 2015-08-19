---
layout: post
title: "Airtel 4G LTE card + Xubuntu - Making it work"
comments: true
---

 I have Xubuntu 12.04 LTS (64-bit) and got a new Airtel 4G LTE usb Card. ( ZTE MF 825a).
Airtel has provided linux drivers and an install script in a folder called 'Linux'

Copy the contents onto home dir and extract the contents. It would look something like this
{% highlight bash %}
keshava Linux$pwd
/home/keshava/Linux
keshava Linux$ls
Airtel_4G_Internet.tar.gz  install.sh  PCL_AIRTEL.tar.gz  zr
{% endhighlight %}

But, when you run 
{% highlight bash %}
sudo ./install.sh
{% endhighlight %}

The program fails to run. Now you start pulling your hair !!
Now, you can stop that since i've already done it and below is solution.
This is because Airtel by default ships 32 bit libraries and if you are on a 64 bit system, 32-bit libraries would not be installed by default.
{% highlight bash %}
 keshava Linux$file /opt/Airtel_4G_Internet/Airtel\ 4G\ Internet 

/opt/Airtel_4G_Internet/Airtel 4G Internet: setuid ELF 32-bit LSB executable, Intel 80386, version 1 (SYSV), dynamically linked (uses shared libs), for GNU/Linux 2.6.15, BuildID[sha1]=0xf4d3b440e68b7292416cc47c2f09c63ed74c7e6e, not stripped
{% endhighlight %}

So, now install 32-bit libraries.
{% highlight bash %}
sudo apt-get install ia32-libs
{% endhighlight %}

This would install 32 bit libraries, and the program , when now run from main menu would run - smooth as butter.

Voila !!! 
