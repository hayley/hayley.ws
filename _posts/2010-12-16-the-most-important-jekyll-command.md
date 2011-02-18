---
layout: post
category: snippet
title: the most important jekyll command
teaser: "tl;dr: auto regen hides failures"
---
*Update: per the [jekyll commit log](https://github.com/mojombo/jekyll/commits/master), a mid-December update seems to have fixed this.*

More than anything, I seem to get bitten by jekyll *silently* failing on syntax errors when I have auto set to true.

The most important flag you can learn is `--no-auto`. Like:

{% highlight bash %}
jekyll --server --no-auto
{% endhighlight %}

With auto turned on, instead of alerting you to errors immediately, presumably jekyll is just assuming you'll fix it.

You could decide to just set auto to false in `_config.yml` but I often make tiny edits as I'm proofreading something and that would mean a whole bunch of killing/restarting the server.

So training myself to remember `--no-auto` anytime there's unexplained weirdness, seems to be the best compromise.
