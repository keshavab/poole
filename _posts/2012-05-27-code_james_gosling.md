---
layout: post
title: "Code by James Gosling took good three years to mature"
comments: true
redirect_from: "/2012/05/27/code_james_gosling"
permalink: code_james_gosling
---

I was wandering into basics of algorithms and found this [interesting page](http://www.cs.ubc.ca/~harrison/Java/sorting-demo.html) and [this](http://www.cs.ubc.ca/~harrison/Java/QSortAlgorithm.java.html) article. Probably, this was written in the earliest days of Java, when the authors of Java were trying to just prove that code can be written in Java , and they took algorithms as their test bed. Typical for anybody to do so.

That's not a surprise. What caught me by surprise was the comments section. I'll just reproduce the comments section.( leaving out license :) )
{% highlight java %}
/**
 * A quick sort demonstration algorithm
 * SortAlgorithm.java, Thu Oct 27 10:32:35 1994
 *
 * @author James Gosling
 * @version     1.6f, 31 Jan 1995
 */

/**
 * 19 Feb 1996: Fixed to avoid infinite loop discoved by Paul Haeberli.
 *              Misbehaviour expressed when the pivot element was not unique.
 *              -Jason Harrison
 *
 * 21 Jun 1996: Modified code based on comments from Paul Haeberli, and
 *              Peter Schweizer (Peter.Schweizer@mni.fh-giessen.de). 
 *              Used Daeron Meyer's (daeron@geom.umn.edu) code for the
 *              new pivoting code. - Jason Harrison
 *
 * 09 Jan 1998: Another set of bug fixes by Thomas Everth (everth@wave.co.nz)
 *              and John Brzustowski (jbrzusto@gpu.srv.ualberta.ca).
 */
{% endhighlight %}

So, What's the observation ?
It took good ***3 YEARS***  for this 80 lines of antediluvian Quick sort code to mature with bug fixes resolved.

An 80 odd line code written by creator of Java taking  **1000** odd days  to get fixed !.
Today, it convinced me on how the Enterprise software support industry is thriving so long for so many years. Code written by one of the finest taking so long to mature and imagine the code written by noobs, rookies, other ordinary folks that has creeped in to trillions of lines of software universe.
Millions of lines of code written by rookies would take light years to get fixed i believe. Probably at complexity greater than O(n!).

No Wonder [Linus doesn't do github pull requests.](https://github.com/torvalds/linux/pull/17#issuecomment-5654674)
