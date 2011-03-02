---
layout: post
---

I've run into this problem far too often.

It generally plays out like this:

* put a small portion of some sample text I want to scrape into [rubular](http://rubular.com/) (FYI: this site has been invaluable for testing regular expressions.)
* I formulate a regular expression that gives me exactly what I want
* BUT when I use it for real in a ruby script on the *whole* text, it's suddenly returning far too much (greedy little bastage)

## a real example

I recently decided that I wanted to parse the public weather outlooks that the Storm Prediction Center issues. Here's an example:

<pre>
ZCZC SPCPWOSPC ALL
WOUS40 KWNS 281017
ALZ000-GAZ000-TNZ000-281800-

PUBLIC SEVERE WEATHER OUTLOOK  
NWS STORM PREDICTION CENTER NORMAN OK
0417 AM CST MON FEB 28 2011

...SIGNIFICANT SEVERE THUNDERSTORMS EXPECTED OVER PARTS OF THE
TENNESSEE VALLEY INTO SOUTHERN APPALACHIANS TODAY...

THE NWS STORM PREDICTION CENTER IN NORMAN OKLAHOMA IS FORECASTING
THE DEVELOPMENT OF POTENTIALLY WIDESPREAD DAMAGING WINDS AND A FEW
STRONG TORNADOES OVER PARTS OF THE TENNESSEE VALLEY AND SOUTHERN
APPALACHIANS TODAY.

THE AREAS MOST LIKELY TO EXPERIENCE THIS ACTIVITY INCLUDE

 NORTHERN ALABAMA
 NORTHWEST GEORGIA
 MIDDLE AND EASTERN TENNESSEE

ELSEWHERE...SEVERE STORMS ARE ALSO POSSIBLE FROM THE LOWER
MISSISSIPPI VALLEY AND LOWER OHIO VALLEY TO THE THE MIDDLE AND
SOUTHEAST ATLANTIC COASTS.

AN UPPER-LEVEL STORM SYSTEM OVER THE CENTRAL AND SOUTHERN PLAINS
EARLY THIS MORNING WILL ACCELERATE RAPIDLY EAST THROUGH THE MID AND
LOWER MISSISSIPPI AND TENNESSEE VALLEYS TODAY AND EVENTUALLY OFF THE
ATLANTIC COAST BY TUESDAY MORNING.  A COLD FRONT WILL ACCOMPANY THIS
STORM SYSTEM EAST WITH THIS BOUNDARY SERVING AS THE FOCUS FOR SEVERE
THUNDERSTORM DEVELOPMENT TODAY INTO TONIGHT.

STRONG SOUTHWEST WINDS JUST ABOVE THE GROUND HAVE ALLOWED A WARM AND
HUMID AIR MASS TO PROGRESS NORTH FROM THE GULF OF MEXICO INTO THE
OHIO VALLEY IN ADVANCE OF THIS STORM SYSTEM.  AS A RESULT...
CONDITIONS OVER A LARGE AREA HAVE BECOME FAVORABLE FOR THE
DEVELOPMENT OF STRONG TO SEVERE THUNDERSTORMS.  THE GREATEST RISK
FOR POTENTIALLY WIDESPREAD DAMAGING WINDS AND A FEW STRONG TORNADOES
IS EXPECTED TO DEVELOP ACROSS THE TENNESSEE VALLEY AND THE SOUTHERN
APPALACHIANS TODAY AS THE UPPER-LEVEL STORM SYSTEM AND SURFACE COLD
FRONT INTERACT WITH THE UNSTABLE ATMOSPHERE.

A RISK FOR SEVERE THUNDERSTORMS CAPABLE OF MAINLY DAMAGING WINDS 
WILL CONTINUE LATER TODAY INTO TONIGHT EAST OF THE APPALACHIANS AS
WELL AS ACROSS PORTIONS OF THE CENTRAL AND EASTERN GULF STATES.

STATE AND LOCAL EMERGENCY MANAGERS ARE MONITORING THIS DEVELOPING
SITUATION. THOSE IN THE THREATENED AREA ARE URGED TO REVIEW SEVERE
WEATHER SAFETY RULES AND TO LISTEN TO RADIO...TELEVISION...AND NOAA
WEATHER RADIO FOR POSSIBLE WATCHES...WARNINGS...AND STATEMENTS LATER
TODAY.

..MEAD.. 02/28/2011

$$  </pre>

**My goal: to get that summary paragraph that's wrapped by "...".**

So, doing my typical thing, I only pasted a portion of the text into rubular and came up with this:

{% highlight ruby %}
overview = fixed_text.match(/^\.\.\.(.*)\.\.\.$/m)[1]
{% endhighlight %}
demo: [http://rubular.com/r/L4OdNuAvKh](http://rubular.com/r/L4OdNuAvKh)

<a href="http://www.flickr.com/photos/hayley/5492433814/" title="Rubular_ ^_._._.(.*)_._._.$ by h a y l e y, on Flickr"><img src="http://farm6.static.flickr.com/5258/5492433814_1296fea301.jpg" width="500" height="323" alt="Rubular_ ^_._._.(.*)_._._.$" /></a>

Fantastic it works!

Wait, what?

<a href="http://www.flickr.com/photos/hayley/5492445216/" title="Rubular_ a Ruby regular expression editor and tester by h a y l e y, on Flickr"><img src="http://farm6.static.flickr.com/5255/5492445216_a6856bba6c.jpg" width="347" height="500" alt="Rubular_ a Ruby regular expression editor and tester" /></a>

demo: [http://rubular.com/r/pkV0KmKP47](http://rubular.com/r/pkV0KmKP47)

I seem to run into this every time I use ``/m`` to make the dot character match new lines. Honestly, I don't actually remember how I overcame this in the past (programmer's amnesia), but I finally learned how to make the query less greedy.

So in this example. Here's how you fix it:

{% highlight ruby %}
overview = fixed_text.match(/^\.\.\.(.*)\.\.\.$/m)[1]
overview = fixed_text.match(/^\.\.\.(.*?)\.\.\.$/m)[1]
{% endhighlight %}

without the question mark: [http://rubular.com/r/pkV0KmKP47](http://rubular.com/r/pkV0KmKP47)

with the question mark: [http://rubular.com/r/XIf9yX2vwI](http://rubular.com/r/XIf9yX2vwI)

Since I've been burned by this so many times and my programmer's amnesia is quite strong, I figured I better officially explain this to my future self. *\*waves to future self*

And assuming there's a public weather outlook in effect, you can see this in action on the [wickedwx.com](http://wickedwx.com) site (the public weather outlook overview will appear at the top of the page).
