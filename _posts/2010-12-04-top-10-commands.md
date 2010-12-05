---
layout: post
category: snippet
title: my top 10 commands
teaser: "tl;dr: I &hearts; git"
---

``history | awk '{a[$2]++}END{for(i in a){print a[i] " " i}}' | sort -rn | head``

{% highlight sh %}
622 git
398 ls
367 cd
143 rvm
95 rails
77 jekyll
69 ssh
62 gem
52 irb
49 mate
{% endhighlight %}

I'm pretty shocked to see git so high. And apparently I do a lot of ruby development.