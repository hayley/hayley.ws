---
layout: post
title: tumblr downtime
---
It's not the tumblr outage itself that bothers me. Downtime seems to be somewhat inevitable for large sites.

It's how they handled it.

## what bothers me

What bothers me most is how little communication there was throughout the ordeal.

Sure, *today* there's the [apology on their tumblelog][tumblr-staff-outage] which has things like this:

> While you might feel like you've gotten used to seeing errors on Tumblr recently, know that this is absolutely unacceptable to our team, and unacceptable for a platform determined to be the best place in the world for your creative expression.

and this:

> We can't apologize enough [...] But please always know that we truly care about your work as much as you do

To me, their actions say otherwise.

Sure, I'm probably being harsh, but it takes nearly zero effort to write an apology and post it on your company's web site *after* the fact.

You know what does take effort? Having someone actively communicating during the outage. For instance, [@posterous](http://twitter.com/posterous) was insanely active during their [August outage](http://blog.posterous.com/the-path-ahead-after-a-tough-week). Even if they didn't have any new information, they were still @replying to all sorts of people. I was even shocked to see them reply to me when I made some offhand remark, considering I hadn't even @'d them directly. They obviously cared enough that they were monitoring what was being said about them on twitter and more importantly, actually responding to those comments.

Contrast that to [@tumblr](http://twitter.com/tumblr). For most of the outage, the account was just *crickets*. And I think it's important to note that in their downtime apology, they even said:

> P.S. If our staff blog is ever unreachable, you can check our [Twitter stream](http://twitter.com/tumblr) for updates.

To me, this sets the expectation that their twitter account is their official backup form of communication. They just can't be bothered to use it when it's most important, apparently.

> We can't apologize enough

Tell that to [@DHH](http://twitter.com/dhh), who, in the wake of the 37signals' Campfire outage, has a ton of @replies to Campfire customers apologizing for the downtime.

So to summarize, what bothers me about tumblr's lack of communication is that:

1. they barely communicated during the outage itself
2. writing one blog post (no matter how sincere it may be) hardly qualifies as "we can't apologize enough"

## this wasn't the first time

Companies stumble. Sh\*t happens. But for me, this only further cemented my attitude that tumblr either doesn't care about *individual* users, or has grown too big to be able to care.

Granted, I'll admit that *a lot* of this was perception but my dealings with posterous support vs. tumblr support couldn't have been more different.

I got the impression from posterous that they were overly eager to help and actually gave a damn, whereas tumblr's response felt somewhat canned. For one, my issue actually got passed on to one of the devs at posterous and I got a direct response back from one of them. Whereas, with tumblr, their frontline support passed my bug report onto the development team and that was the last I heard of it. And that bug still exists to this day.

Granted, the bug probably only burns a small percentage of users *some* of the time, but I feel like it should be a quick fix, something trivial. So based on this experience, it leaves me with the impression that they either just don't care, or they just don't have the resources to fix it.

## complaining about a free service

The biggest opposition I see to all of the complaints is that "you get what you pay for".

It's human nature to want to complain when something goes wrong regardless of whether you forked over cash in the process. And to just say "you get what you pay for" is a cop-out.

Furthermore, the argument could be made that with tumblr, you're not paying cash for the service, but you are paying with your content.

For instance, until last week, tumblr was the *only* source for about ~200 of my songs. And I'd suspect that, for many people, tumblr is the only source for *all* of their content.

So sure, we're not paying tumblr actual money, but instead we're paying with *exclusive* content. And sure, a lot of that content may be endless reblogs, and though that particular content may not be valuable to you or me, it's obviously valuable to someone out there because tumblr is growing like crazy.

I have no idea how tumblr is going to *sustainably* monetize their site (I tend to doubt that the premium themes and $9 featured blog promotions would keep the lights on), but all of that content is bringing them a ton of eyeballs and new users in their quest to build:

> a platform determined to be the best place in the world for your creative expression

There's a lot of competition out there. Granted, tumblr is somewhat unique in that it's somewhat of a hybrid of traditional blogging platforms and of social services like twitter and facebook.

Still though, if they want to keep their edge, they're going to have to get way more transparent about what's going on behind the scenes when things like this happen.

## how I'm moving forward

I made my first tumblr post on August 10th, 2007. And I've posted new music to my tumblelog every single day since February 1st.

I don't plan to stop.

But this latest experience has really reinforced my growing paranoia that trusting all of your content to one 3rd party site is a bad idea.

I think (hope) that incidents like [ma.gnolia's total loss of user data](http://en.wikipedia.org/wiki/Gnolia#January_2009_total_data_loss) are few and far between. Somehow, I had the good fortune to decide to scrape all of my tumblr posts a week ago. Had I not done that, I would've absolutely been freaking during the outage. If tumblr had never recovered, I still would've lost the weeks' worth of posts since then, but a week is way easier to cope with than the 350+ posts that I would've lost had I not backed up *anything*.

On the plus side, this was the kick-in-the-butt motivation I needed to finally start moving on a project I've been intending to implement for ages. For quite some time, I've wanted to build a system where I have one centralized copy of every piece of content that I post. A copy that *I* control. And via scripting magic, I can easily take that one piece of content and have it posted to any of the 3rd party sites I choose.

So I'll get the best of both worlds: all of my content will be handled directly by me, but I'll still get to easily stay active on sites like tumblr without a whole lot of extra hassle.

## what you can do

If you have a mac, you can use the [official tumblr backup tool](http://staff.tumblr.com/post/286303145/tumblr-backup-mac-beta), though I must say it's sad to see that it hasn't seen an update for nearly a year. And though I'm a mac user almost exclusively, it's also sad that they're not supporting linux or windows.

If you like Ruby, you can take a look at [the script I wrote to do something useful with my tumblr audio posts and turn them into jekyll posts](https://github.com/hayley/yelyah.com/blob/master/_util/tumblr_scraper.rb).

But even if it comes down to having to manually copy/paste everything you write, if this is content that you want to keep, content that you want to be able to read in a few years' time, *won't you be glad that you did*?

[tumblr-staff-outage]: http://staff.tumblr.com/post/2127872280/downtime
