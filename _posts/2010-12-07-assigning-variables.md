---
layout: post
category: snippet
title: assigning default values in ruby
teaser: "tl;dr: I was over obsessed with ternary operators"
---

My obsession with trying to find the ternary operator solution led to doing things the least obvious way.

These all seem to do the same thing but which one do *you* prefer to look at?

{% highlight ruby %}
post_type = !yaml_data['tumblr_type'].nil? ? yaml_data['tumblr_type'] : 'audio'
post_type = yaml_data['tumblr_type'].nil? ? 'audio' : yaml_data['tumblr_type']
post_type = yaml_data['tumblr_type'] ? yaml_data['tumblr_type'] : 'audio'
post_type = yaml_data['tumblr_type'] || 'audio'
{% endhighlight %}

Since the lines are in chronological order, I suppose I should be proud. I got *less* insane as time went on.

It could've easily gone the other way though.
