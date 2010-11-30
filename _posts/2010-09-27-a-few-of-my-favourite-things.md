---
layout: post
title: A few of my favourite things
---


A few of my favourite things
----------------------------

## FFR

One plus side of switching my gaming from PC-based to console-based has been the fact that I don't feel like I have to keep chasing computer upgrades. Oh sure, I still *want* upgrades, but it generally ends up being more about some new feature (e.g. the portability of a Macbook Air) than about upgrading the specs themselves.

So one thing I've used to prolong the useful life of a system has been the clean install. It's done wonders for my music studio mac which is something like 4 years old at this point, but still running strong (it also definitely helps that my current setup doesn't involve loading tons of virtual instruments).

As for my main mac, it's desperately needed a reinstall for *at least* a year. I know this because it was already "dirty" when I upgraded to Snow Leopard, when it came out. The upgrade didn't help.

I've had some random things that haven't worked quite right and I've also been getting erratic system slowdowns. Obviously it wasn't anything so bad that I actually did anything about it. Until now.

I signed up to participate in [Rails Rumble](http://railsrumble.com/), and it became increasingly clear that I needed to do a clean install *now*. Cause I'm going to need as aggravation-free of a development environment as possible for such a compressed deadline.

This weekend I finally got the bee in my bonnet I needed to get me to actually do the reinstall.

## Backing everything up

To be clear, when I talk about a "clean install", I'm talking about a full hard drive wipe. The equivalent of what I used to hear in the windows world as the FFR - fdisk, format, reinstall.

I knew I wanted to make a backup disk image, but I wanted to keep it as small as possible (and yet somehow, it still ended up in the neighborhood of 130 gigs).

I copied files off to one hard drive, and then for the things that I *really* wanted to keep, I copied those to a second hard drive as well. Now might be a good time to mention that, though I've been burned before, I still don't have a proper backup plan in place.

For creating the disk image, I used [Carbon Copy Cloner][carbon-copy-cloner]. This was by far the largest part of the reinstall process, taking something like 7+ hours to make a disk image (it would've been nice if CCC had shown an estimated time remaining during the process, though I realize those things can be wildly inaccurate).

Afterwards, I crossed my fingers and hoped for the best.

## The clean install

First, there is no "erase and install" in 10.6. I didn't realize this and charged into a standard install. 

There was no stop/cancel button so I assumed I was stuck waiting for the wrong install to finish. I realized like 20 minutes into it, that you could quit the installer though, effectively getting your stop/cancel.

So instead of the built-in "erase and install" option, you now have to manually wipe the hard drive. Which I believe was basically:

* boot off the snow leopard install disk
* use the disk utility app in the utilities menu
* erase the hard drive
* do the install as normal

## the "new" computer

I was originally going to say that the first thing I did was to install a launcher. Then I realized it was the Dock. *That* was the first thing I did when the computer was fresh.

I move the dock to the left and then hide it, then remove all of the built-in icons (so the only icons that ever show up in my dock are the currently running apps). It's how I roll.

So then next was installing an app launcher. I've used a few launchers in the past (and now I'm auditioning another one) and I currently have no favorite, so here's a [wikipedia page with a comparison of OS X launchers][wikipedia-launchers]. Honestly, given how few features I tend to use, I might be perfectly happy with Spotlight if I ever gave it a chance.

## A few of my favourite things

In no true order, here are the other things I've installed *so far*. Perhaps, you'll find this useful, but I mostly doing this for myself, to make it easier for the next time. Ya know the next time I dirty my system up enough that it needs a complete reinstall. Which will probably be next month, but I'll procrastinate for at least another year.

* My browser of choice is currently [Chrome][chrome]. Though ask me again in 6 months and it may well have changed. Things just feel snappier in it. Otherwise, I'd still be using Firefox.

* I've recently fallen for [Visor][visor]. I launch the Terminal app, and then the terminal is always a control+backtick away. And Visor requires [SIMBL][SIMBL].

* I've also recently started using zsh instead of bash. Mostly because of the RPROMPT (a right aligned prompt).

* I used [oh-my-zsh][oh-my-zsh] to get some good zsh defaults.

* I currently am digging the "dst" theme.

* I used [Steve Losh's color scheme][steve-losh-colors], which is from the [Monokai TextMate theme][monokai]. And for reference, here are the colors:

{% highlight bash %}
Black: 0, 0, 0
Red: 229, 34, 34
Green: 166, 227, 45
Yellow: 252, 149, 30
Blue: 196, 141, 255
Magenta: 250, 37, 115
Cyan: 103, 217, 240
White: 242, 242, 242
{% endhighlight %}

* I used a dmg to install [git][git-mac]. I've used Mercurial too, but basically github.com is the killer "app" that sold me on using git.

* I installed [Homebrew][homebrew], but have yet to actually use it to install anything.

* I installed [Ruby Version Manager (rvm)][rvm]. I had never used it before, but I had the impression that this would be the cleanest way to get Ruby 1.9.2 and Rails 3.0.0 installed on my mac. Installing ruby/rails with it was pretty painless:

{% highlight bash %}
rvm install 1.9.2
rvm 1.9.2 #switch to 1.9.2 (applies to current session only)
gem install rails
{% endhighlight %}

* [Perian][perian] is my choice for quickly getting the necessary codecs for divx/xvid playback installed.

* I hide the system clock and display [FuzzyClock][fuzzyclock] instead. I've got it set to display the time in Svenska, which I can basically read natively now (not the whole language, just the time). However, there's a big problem (that I haven't bothered to google), *is that really how they say the time?*. For instance, in English, it's currently "twentyfive past two". In Svenska, it's "fem i halv tre", which I interpret to literally mean "5 until half 3". I know we'd sure be saying "two twenty five", so there's no guarantee that the Swedish isn't similarly also "fuzzied". Again, it's something I could google and find out once and for all. But haven't.

* I installed [TextMate][textmate], but I'm wondering if it's time to take the plunge and learn emacs. Ya know, so that I can find partake in the vi <> emacs war. 2 things forced me to learn vi back in the 90s. 1) A box where pico wasn't installed at all. 2) Having a freebsd box where pico was randomly adding junk to the end of files that I edited. It's time to learn emacs. Yeah. Or something.

* I installed [Burn][burn] for guess what, burning disks.

* I installed [The Unarchiver][unarchiver] to get support for stuff like rar and all the other file types that aren't supported by the OS.

## the end

So that's what I have installed *so far*. To me, this indicates the most important pieces. The pain points. The must haves.

There are still a ton of apps on my paper list.

One app I'm probably avoiding though is my to-do list software. I just don't want to know.


[carbon-copy-cloner]: http://www.bombich.com/
[wikipedia-launchers]: http://en.wikipedia.org/wiki/Comparison_of_application_launchers#Mac_OS_X
[chrome]: http://www.google.com/chrome/
[visor]: http://visor.binaryage.com/
[SIMBL]: http://www.culater.net/software/SIMBL/SIMBL.php
[steve-losh-zsh]: http://stevelosh.com/blog/2010/02/my-extravagant-zsh-prompt/
[steve-losh-colors]: http://stevelosh.com/blog/2009/03/candy-colored-terminal/
[oh-my-zsh]: http://github.com/robbyrussell/oh-my-zsh
[monokai]: http://www.monokai.nl/blog/2006/07/15/textmate-color-theme/
[git-mac]: http://code.google.com/p/git-osx-installer/downloads/list?can=3
[homebrew]: http://mxcl.github.com/homebrew/
[rvm]: http://rvm.beginrescueend.com/
[perian]: http://perian.org/
[fuzzyclock]: http://www.objectpark.org/FuzzyClock.html
[textmate]: http://macromates.com/
[unarchiver]: http://wakaba.c3.cx/s/apps/unarchiver.html
[burn]: http://burn-osx.sourceforge.net/Pages/English/home.html
