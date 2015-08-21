---
layout: post
title: Ansible - include + with_items back in 2.0
comments: true
tags:
- ansible
- 2.0
- include
- with_items
---

One of the widely used(abused) feature of [Ansible](https://github.com/ansible/ansible) which was taken back in 1.6 is back in 2.0.

* **include:** statement to include a file with variable name
* **include:** statement with *with_items*

Prior to 2.0, any of these operations would always result in the following error:
{% highlight yaml %}
---
- include: {% raw %}"{{ my_var }}.yml"{% endraw %}
{% endhighlight %}
{% highlight bash %}
ERROR: file not found: /path/to/ansible/provision/{% raw %}{{my_var}}{% endraw %}.yml
{% endhighlight %}

{% highlight yaml %}
---
- include: {% raw %}"{{ item }}"{% endraw %}
  with_items:
  - {%raw%}"{{ my_var }}.yml"{%endraw%}
{% endhighlight %}
{% highlight bash %}
ERROR: [DEPRECATED]: include + with_items is a removed deprecated feature. Please update your playbooks.
{% endhighlight %}


With 2.0([currently devel branch](http://docs.ansible.com/ansible/intro_installation.html#running-from-source)), these are possible now:

**include:** statement to include a file with variable name
{% highlight yaml %}
---
- hosts: all
  tasks:
  - include: {% raw %}"{{ my_var }}.yml"{% endraw %}
{% endhighlight %}

**include:** statement with *with_items*
{%highlight yaml %}
---
- hosts: all
  tasks:
  - include: play.yml {% raw %}{{ item }}{% endraw %}
  with_items:
  - var_1
  - var_2
  - var_3
{% endhighlight %}

This would help in dynamically including a file based on a variable or a fact. This would also help to iterate over the same
play/file with different values.

