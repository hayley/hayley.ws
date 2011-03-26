---
layout: post
category: snippet
title: the comma that ruined my life
teaser: "tl;dr: beware the trailing comma"
---

Okay, not really, but here's another tale of Javascript *despair*.

I've been dissecting Paul Nicholls' [Christchurch quake map](http://www.christchurchquakemap.co.nz/) in an attempt to learn coffeescript and, uck, javascript.

I had a big ole array like this (but with several hundred more entries).

{% highlight javascript %}
quakes = [
{"lon":"-123.1710","m":250,"d":1,"s":"2011/03/24 14:45 UTC","lat":"39.1937","t":1300977950},
{"lon":"99.7386","m":540,"d":10,"s":"2011/03/24 15:54 UTC","lat":"20.6819","t":1300982075},
{"lon":"144.0058","m":490,"d":33,"s":"2011/03/24 15:58 UTC","lat":"36.7604","t":1300982300},
{"lon":"-64.8280","m":390,"d":97,"s":"2011/03/24 16:13 UTC","lat":"18.3579","t":1300983192},
];
{% endhighlight %}

Do you see the problem?

Chrome didn't. Nor did Firefox nor Safari.

But IE. Oh IE. It was complaining about null objects. (Disclaimer: I only tested in IE 7, because that's what a friend had tried and it was failing. And, okay, so the friend had originally tried in Firefox and the site was breaking there too (though I could never recreate it on my virtualized windows install) so I guess the problem is larger than just IE.)

Anyways.

After much heartache, I discovered that IE was going out of bounds reading through the list of earthquakes and was going totally kaputt when it would hit the null entry.

The fix?

Remove one freaking trailing comma.
