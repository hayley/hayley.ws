---
layout: post
title: getting jekyll running on nearlyfreespeech.net
---

This is the short guide largely based off of [my original post about yelyah.com](http://hayley.ws/2010/12/03/yelyah.com-redux.html) on how to get a jekyll site up and running on nearlyfreespeech.net.

Given that the [github pages](http://pages.github.com) system does not support [kramdown](http://kramdown.rubyforge.org/), I decided it was enough of a reason to move over to [nearlyfreespeech.net](http://nearlyfreespeech.net) for this site and so I decided to go through and document *all* of the steps, so that I can be extra lazy the next time that I deploy a jekyll site to NFSN.

## why nearlyfreespeech.net

Covered in more length in [my original post about yelyah.com](http://hayley.ws/2010/12/03/yelyah.com-redux.html), the tl;dr is that NFSN should be pretty cheap for a jekyll site because jekyll generates static files. And on NFSN, static sites only charged for storage and transfer.

I'm hosting my DNS elsewhere, so transfer and storage are all I'm paying for.

## initial setup / gotcha

At some point, I might do something more detailed here about how to create the site through NFSN's web interface, but for now, I'll just say that the one gotcha is that you'll want to make sure you "change server type" to just static. Unless you plan on having some PHP pages, you'll want static because otherwise, you'll be paying a penny/day for features that you don't need for a jekyll site.

## configuring yourdomain.com

As part of creating your site, you'll end up with SOMETHING.nfshost.com. But I'm assuming you'll want to point yourdomain.com at it instead. You can choose to have NFSN do this for you, but it's an extra cost (which may well be worth it to you).

However, I'm the kind who just loves that word *free*, so the rest of this assumes that you're going to use someone else's DNS service (which is probably whoever you registered yourdomain.com through).

*And as a heads up, if you already have something hosted at yourdomain.com, you can just skip this section for now and use the SOMETHING.nfshost.com for testing and then come back to this section once you verify that everything is working on the SOMETHING.nfshost.com site.*

On the NFSN side, you'll need to "Add a New Alias" for each domain you want NFSN to answer for. This will generally mean *two* aliases, one for yourdomain.com and another for www.yourdomain.com.

With your 3rd party DNS provider, you'll want to do 2 things:

1. Create separate A (address) records for your main domain (usually called "``@``") for every IP listed at the bottom of your site page on the NFSN member site admin page. These will probably be addresses that look like 208.94.X.X.
2. Create a CNAME (canonical name) record for ``www`` and point it to either your domain (usually called "``@``") or to SOMETHING.nfshost.com. I think the reasoning behind pointing to your nfshost.com name instead of @ is that, if NFSN needs to change what IPs your site is hosted, the www address will automatically get those changes. And it should be noted, that if this situation were to happen, you'd have to manually change the IPs associated with the @ regardless of what you choose for the www record, because those records *will not* automatically update.

## installing jekyll on NFSN

Granted, you have the option of just manually copying the contents of _site over to your ``/home/public`` directory, but my current preference is to have the server generate the files itself (which is similar to how github pages behaves). So for that, you'll need to have the proper gems installed. Plus, we're going to setup a git repository, so that you can have the site be automagically regenerated every time you push to the repository.


{% highlight sh %}
ssh USERNAME@NFSNSERVER # substitute with the values found on your site page
# setting up the environment variables for gem install to work
export GEM_HOME=/home/private/gems
export GEM_PATH=/home/private/gems:/usr/local/lib/ruby/gems/1.8/
export PATH=$PATH:/home/private/gems/bin
export RB_USER_INSTALL='true'

# installing jekyll
gem install jekyll
gem install rdiscount # if you're using rdiscount for markdown
gem install kramdown # if you're using kramdown for markdown
{% endhighlight %}

## getting the remote git repo setup on NFSN

On the remote server:

{% highlight sh %}
mkdir -p ~/git/REPONAME.git 
cd ~/git/REPONAME.git
git --bare init
{% endhighlight %}

On your local system and you may choose to substitute "``nfsn``" for "``origin``" or whatever you'd like:

{% highlight sh %}
git remote add nfsn ssh://USERNAME@NFSNSERVER/home/private/git/REPONAME.git
git push nfsn master
{% endhighlight %}

## configuring a git hook for automatic site deployment

My script is almost entirely based on the [post-receive script shown on jekyll's deployment page](https://github.com/mojombo/jekyll/wiki/Deployment).

On the remote server:

{% highlight sh %}
cd ~/git/REPONAME.git/hooks/
touch post-receive
chmod 775 post-receive
vi post-receive # or substitute your favorite editor
{% endhighlight %}

{% highlight sh %}
#!/bin/sh
REPONAME='hayley.ws' # substitute the appropriate name

export GEM_HOME=/home/private/gems
PATH=$PATH:/home/private/gems/bin

GIT_REPO=$HOME/git/$REPONAME.git
TMP_GIT_CLONE=$HOME/tmp_deploy/$REPONAME
PUBLIC_WWW=/home/public

git clone $GIT_REPO $TMP_GIT_CLONE
jekyll --no-auto $TMP_GIT_CLONE $PUBLIC_WWW
rm -Rf $TMP_GIT_CLONE
exit

{% endhighlight %}

At this point, if you're more confident than I, you could push an update to the remote git repository to test the post-receive script, but I found this to be more efficient. Run this on your local system:

{% highlight sh %}
ssh USERNAME@NFSN /home/private/git/REPONAME.git/hooks/post-receive
{% endhighlight %}

Once you get the script working properly (hopefully it will work for you on the first go!), you can just do a ``git push nfsn`` and the script will kick off and update your site *automagically*.

## outtro

Did I miss anything? Want me to cover something in further detail?

Eventually, I'll have comments configured so you can tell me. Until then, I'll adapt the answering machine joke:

*Hi, I'm a telepathic comment system. Just think about your comment and I'll think about responding to it.*