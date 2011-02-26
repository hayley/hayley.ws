---
layout: post
title: how I turned 40 lines of buggy code into 2 that just work
---

## aka how to compare two arrays of objects in ruby

**[The 2 line solution is at the bottom of the article.]**

Sometimes I think I've gone stupid.

And the code that I wrote around the 16th doesn't do anything to calm those fears.

## the beginnings of what should've been a simple feature

On the old python-based [wickedwx.com](http://wickedwx.com), I wanted to be able to send out tweets when my scripts detected that there were new storm reports.

Given how I had structured my storm reports data, it was pretty painless code to determine which storm reports were new. Don't quote me since it's been so long, but this seems to be the magic right here:

{% highlight python %}
diff_list = [item for item in storm_reports_list if not item in old_storm_reports]
{% endhighlight %}

This worked because I had a list/array that was full of dictionary/hash objects. I could compare the two easily.

But in Ruby, my storm reports data is an array full of *objects*.

Consider this:

{% highlight ruby %}
r1 = Report.new #=> #<Report:0x102839188>
r1.comments = "TREES DOWN ACROSS HIGHWAY 82 AT THE COLUMBIA AND UNION COUNTY LINE (SHV)" #=> [...]
r2 = Report.new #=> #<Report:0x1027b02c0>
r2.comments = "TREES DOWN ACROSS HIGHWAY 82 AT THE COLUMBIA AND UNION COUNTY LINE (SHV)" #=> [...]
r1 == r2 #=> false
{% endhighlight %}

See, they should be equal. The problem is that they are indeed two distinct objects in the Ruby system, but for my purposes, *r1 and r2 are the same exact thing*.

## how I ended up with 40 lines of convoluted code

I'll detail it further down, but I eventually solved the problem *cleanly*, by using to_json. I did even consider this initially, but I couldn't think through how to solve the problem in its entirety, so that attack path was abandoned for something that proved to be far far far more inelegant.

Instead, I took the separate CSV files for the tornado, hail, and damaging wind reports and then compared those to what the script had just pulled. To do the actual compare, I turned the files into arrays and then did array subtraction to find the new reports.

Part of the problem was that this subtraction would also strip out the CSV header which was needed to be able to properly parse the reports later on. Furthermore, the CSV library was expecting a string, not an array, so I had to convert the array back to a string. And finally, I'd take that mangled ball of data and send it through the CSV parser.

## post mortem

The really frustrating thing was that the code would break unpredictably. There were all sorts of different conditions where the code would work and then suddenly wouldn't. The most recent behaviour was that the code would break on the first run after midnight UTC.

And going back through the code *now*, I think I can see the problem.

Basically, I think it all boils down to one decision in the code:

If the previous CSV file was nil (which would happen when this was the first database run of the day), then the CSV file was assigned to the new reports BUT it was still getting treated in the same way as the results that were processed by array subtraction. Which meant:

* I was adding the CSV header back in unnecessarily
* more damning is that **I was taking a string object that already had proper newlines, turning it into an array, then rejoining it with more newlines**

I didn't understand it at the time, but I remember being perplexed by the fact that *some of the time*, the CSV file would basically end up looking double spaced. But now it's clear to me.

I'm not sure if the extra CSV header was causing a problem, but the *freshly added blank lines definitely were*.

And this also explains why the script would run just fine until after midnight UTC (and this also is indicative of something else I need to fix). It's kinda convoluted but here's the scenario:

* on the first run after midnight, there are no storm reports. Hence, the buggy "find me new reports" code isn't even touched.
* but the severe weather picks up and throughout the rest of the day, there are storm reports coming in.
* on the first run where it goes from knowing about 0 storm reports to 1 or more, it works just fine because *the original CSV is blank and not ``nil``*. It goes through the array subtraction, the re-adding of the CSV header, and the rejoining by new lines A-OK!
* but here's where the trouble is: on the first run after midnight of the next day, there *are* storm reports. But because the previous CSV of the day doesn't exist (because this is the first run, remember), the CSV file is ``nil`` and so it gets set verbatim to the new reports. But then, **it goes through the post-processing that was designed for the array subtracted results, so it gets horribly mangled**.

So here's what I'm talking about:

{% highlight ruby %}
hail_csv = """Time,Speed,Location,County,State,Lat,Lon,Comments
1450,UNK,NE SAN FRANCISCO,SAN FRANCISCO,CA,37.78,-122.41,VERY HEAVY RAINFALL,.11 INCHES IN 5 MIN TIME FRAME ACCOMPANIED BY 35 MPH WIND. (MTR)
1810,UNK,PULASKI,PULASKI,VA,37.05,-80.8,TREE BLOWN DOWN BLOCKING THE INETRSECTION OF SNYDER LANE AND ALUM SPRING ROAD. TREE ALSO DOWN BLOCKING THAXTON ROAD NEAR VIRGINIA COURT. (RNK)
1815,UNK,WOODBURY HEIGHTS,GLOUCESTER,NJ,39.82,-75.15,TREES DOWN. (PHI)
1820,UNK,WESTAMPTON TWP,BURLINGTON,NJ,40.02,-74.83,LARGE TREE KNOCKED DOWN (PHI)
1825,UNK,MIDDLETOWN,NEW CASTLE,DE,39.45,-75.71,WIRES DOWN AND POWER OUTAGES (PHI)
1827,UNK,4 N RAGLAND,ST. CLAIR,AL,33.81,-86.14,SEVERAL TREES DOWN ALONG AND NEAR COUNTY ROAD 22 (BMX)
1832,65,1 E GEORGETOWN,ANNE ARUNDEL,MD,39.14,-76.75,ESTIMATED 60 TO 70 MPH WINDS (LWX)
1850,58,SANDTOWN,KENT,DE,39.03,-75.72,(PHI)
1855,60,2 S CLINTON,PRINCE GEORGES,MD,38.74,-76.9,(LWX)
1856,UNK,AUDUBON,CAMDEN,NJ,39.89,-75.07,TREE FELL INTO A TRAFFIC LIGHT WHICH THEN TOOK DOWN POWER LINES. (PHI)
1900,UNK,LAKEWOOD,OCEAN,NJ,40.1,-74.22,SMASHED WINDOWS... SHINGLES OFF ROOFS... GARBAGE CANS TOSSED... MICROBURST SIGNATURE ON RADAR... 1000 FT ESTIMATED WINDS 75 MPH. (PHI)
1930,UNK,SEASIDE HEIGHTS,OCEAN,NJ,39.94,-74.08,ROOF PARTIALLY BLOWN OFF A BUILDING ON SHERMAN AVENUE... SHINGLES BLOWN OFF OTHER BUILDINGS. FENCE BLOWN DOWN. (PHI)"""

# NOW A DEMONSTRATION OF THE POST PROCESSING THAT SHOULDN'T HAVE OCCURRED
hail_csv_header = "Time,Speed,Location,County,State,Lat,Lon,Comments"
new_reports = [hail_csv_header] + hail_csv.to_a
# can you see what's about to happen:
pp new_reports
# ["Time,Speed,Location,County,State,Lat,Lon,Comments",
# "Time,Speed,Location,County,State,Lat,Lon,Comments\n",
# "1450,UNK,NE SAN FRANCISCO,SAN FRANCISCO,CA,37.78,-122.41,VERY HEAVY RAINFALL,.11 INCHES IN 5 MIN TIME FRAME ACCOMPANIED BY 35 MPH WIND. (MTR)\n",
# "1810,UNK,PULASKI,PULASKI,VA,37.05,-80.8,TREE BLOWN DOWN BLOCKING THE INETRSECTION OF SNYDER LANE AND ALUM SPRING ROAD. TREE ALSO DOWN BLOCKING THAXTON ROAD NEAR VIRGINIA COURT. (RNK)\n",
# [...]
new_reports = new_reports.join("\n")
puts new_reports
{% endhighlight %}

Had it not been postprocessed, it would've been fine, but instead we end up with this newly mangled output:

{% highlight text %}
Time,Speed,Location,County,State,Lat,Lon,Comments
Time,Speed,Location,County,State,Lat,Lon,Comments

1450,UNK,NE SAN FRANCISCO,SAN FRANCISCO,CA,37.78,-122.41,VERY HEAVY RAINFALL,.11 INCHES IN 5 MIN TIME FRAME ACCOMPANIED BY 35 MPH WIND. (MTR)

1810,UNK,PULASKI,PULASKI,VA,37.05,-80.8,TREE BLOWN DOWN BLOCKING THE INETRSECTION OF SNYDER LANE AND ALUM SPRING ROAD. TREE ALSO DOWN BLOCKING THAXTON ROAD NEAR VIRGINIA COURT. (RNK)

1815,UNK,WOODBURY HEIGHTS,GLOUCESTER,NJ,39.82,-75.15,TREES DOWN. (PHI)

1820,UNK,WESTAMPTON TWP,BURLINGTON,NJ,40.02,-74.83,LARGE TREE KNOCKED DOWN (PHI)

1825,UNK,MIDDLETOWN,NEW CASTLE,DE,39.45,-75.71,WIRES DOWN AND POWER OUTAGES (PHI)

1827,UNK,4 N RAGLAND,ST. CLAIR,AL,33.81,-86.14,SEVERAL TREES DOWN ALONG AND NEAR COUNTY ROAD 22 (BMX)

1832,65,1 E GEORGETOWN,ANNE ARUNDEL,MD,39.14,-76.75,ESTIMATED 60 TO 70 MPH WINDS (LWX)

1850,58,SANDTOWN,KENT,DE,39.03,-75.72,(PHI)

1855,60,2 S CLINTON,PRINCE GEORGES,MD,38.74,-76.9,(LWX)

1856,UNK,AUDUBON,CAMDEN,NJ,39.89,-75.07,TREE FELL INTO A TRAFFIC LIGHT WHICH THEN TOOK DOWN POWER LINES. (PHI)

1900,UNK,LAKEWOOD,OCEAN,NJ,40.1,-74.22,SMASHED WINDOWS... SHINGLES OFF ROOFS... GARBAGE CANS TOSSED... MICROBURST SIGNATURE ON RADAR... 1000 FT ESTIMATED WINDS 75 MPH. (PHI)

1930,UNK,SEASIDE HEIGHTS,OCEAN,NJ,39.94,-74.08,ROOF PARTIALLY BLOWN OFF A BUILDING ON SHERMAN AVENUE... SHINGLES BLOWN OFF OTHER BUILDINGS. FENCE BLOWN DOWN. (PHI)

{% endhighlight %}

And all of those blank lines break the CSV parser. Oops.

## what I should've done in the first place || how to compare objects in Ruby

Going into the convoluted solution above, I wasn't real happy from the get-go. The big thing was that, for every new report, it was going through the CSV parser *twice* and I figured that was asking for trouble. But on the first attempt, I hadn't figured out how I'd work around that.

So to reiterate, here's the problem:
{% highlight ruby %}
r1 = Report.new #=> #<Report:0x102839188>
r1.comments = "TREES DOWN ACROSS HIGHWAY 82 AT THE COLUMBIA AND UNION COUNTY LINE (SHV)" #=> [...]
r2 = Report.new #=> #<Report:0x1027b02c0>
r2.comments = "TREES DOWN ACROSS HIGHWAY 82 AT THE COLUMBIA AND UNION COUNTY LINE (SHV)" #=> [...]
r1 == r2 #=> false
{% endhighlight %}

So how do we fix it? With to_json:

{% highlight ruby %}
r1.to_json == r2.to_json #=> true
{% endhighlight %}

So ultimately, here is how I turned about 40 lines of buggy code into just two (or five if you count the surrounding stuff):

{% highlight ruby %}
def findnewreports(old_reports, reports)
  jsonified_old_reports = old_reports.map { |x| x.to_json }
  new_reports = reports.reject { |x| jsonified_old_reports.include?(x.to_json) }
  return new_reports
end
{% endhighlight %}

First, it takes all of the elements of the old_reports array (which was initially created by loading a marshal'd object from the database) and turns each individual element into a jsonified string (as opposed to turning the array itself into a jsonified object, which wouldn't work for the next line).

Next, it iterates over all of the storm reports that *it has just found from the most recent web page scrape* and looks at the jsonified array to see if they are included already. If they are, it drops that array element, because that means that it already knows about that storm report.

And the best thing? It was easy to write a test around this new code, whereas, I had continued to scratch my head as to how I was properly going to test the old code.

I'll still admit that maybe I'll run into problems with these 2 lines, but you know what? It's way easier to diagnose something that happens in 2 lines, than in 40.

So it's a win.
