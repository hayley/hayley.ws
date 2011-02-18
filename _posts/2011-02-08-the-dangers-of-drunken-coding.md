---
layout: post
---

> Friends don't let friends drink and deploy.

In my case, you can substitute drunk with sleepy, but in either case, you can code like there's no tomorrow. *Just don't deploy that code until you're sober.* Or awake.

I woke up this morning and checked the [severe weather outlook](http://wickedwx.com) on my site. I was mortified to find that the site was down.

In all the time that the new site has been up, I screwed up on the day that I actually promoted it.

That was part of the embarrassment. The other part was just the fact that "how did I miss this?" I was embarassed that this didn't get caught.

## what actually happened

Though I didn't do a screenshot, I believe the error was an uninitialized constant twitter.

*The entire site was down*. Ouch.

So what caused it:

* I have a bunch of passwords and API codes in a file outside of source control (for good reason)
* the feature update was split between the secret twitter info in the non source-controlled file and the rest of the ruby code that *was* in source control
* I committed the new twitter code to the git repository and then manually updated the secret file on the server
* Somehow I *forgot* to push the new code to the remote repository, which would've been fine except:
* I did a ``cap deploy``. There was no new code to deploy, but it did restart the server.
* the secret file contained code that required that the twitter gem be installed. Ya know, something that would've happened on the ``cap deploy`` **if I had remembered to push the code first**.

## tl; dr - I had server crashing code in a private file

I consider the *core* cause to be how I structured the private file. See, it *had* been a lot of stuff like this:

{% highlight ruby %}
ENV['MONGOHQ_URL'] = "<url>"
ENV['TUMBLR_EMAIL'] = "<email>"
{% endhighlight %}

But I broke the pattern to do the twitter changes by adding this style:

{% highlight ruby %}
Twitter.configure do |config|
  config.consumer_key = 'blah'
  config.consumer_secret = 'blah'
  config.oauth_token = 'blah'
  config.oauth_token_secret = 'blah'
end
{% endhighlight %}

Had I been declaring the twitter information in the like before by just using variables, things would've been fine. The server would've restarted, seen the new variables and loaded them, and then not used them because *I hadn't pushed the new code*.

But instead, I created my own little Charlie Foxtrot.

## lessons learned

### only place harmless code in files outside of source code control

I'm actually kinda glad this happened. Though I've known for awhile that I really needed to have some sort of post-deploy tests in place, this specific issue taught me that **I should only have harmless variables outside of source code control**.

So if instead of my ``Twitter.configure`` block, I had just declared stuff like ``ENV['TWITTER_CONSUMER_KEY'] = 'blah'``, things would've been fine. Oh, I still would've woken confused as to why no tweets had been sent, but I *wouldn't have crashed my whole site*.

### at the very least, hit your main page after you deploy

I almost always do this, which is one of the reasons it was so embarassing that the site was down as long as it was. *It should've been caught within minutes. Not hours.*

Ideally though, I'd like to write a set of automatic tests that would happen anytime new code gets deployed to the server. *I'm a human; I can't be trusted.*

### don't deploy drunk

Or high. Or sleepy. Or impaired in any way. See, that coding tear you went on last night may have resulted in some awesome code. But all it takes is *one tiny bug* to make your code come crashing down.

Alright, so if I had had the proper tests in place, that tiny bug should've been caught much much quicker. But it doesn't change the fact that it's no fun to troubleshoot when you're impaired. I have far too much personal experience with that from a gig where the company insisted that all of us *day-shifters* make major changes in the middle of our sleep schedules. Brilliant!

Upon further reflection, I think my problem was that I wanted to go to bed with a sense of accomplishment. Deploying the code was the natural end to a good day of programming. Because, it didn't really count until it was on the server.

And I've noticed that desire since then. Frankly, I'm just having to force myself not to do any more late night code deploys. There's still a 50/50 chance that I will do something stupid come morning, but at least, I'll have all day to fix it.

So my parting advice?

> Only deploy big changes when you have a stretch of time to just sit and watch the server logs.

Yeah. Sit back and watch it die.

