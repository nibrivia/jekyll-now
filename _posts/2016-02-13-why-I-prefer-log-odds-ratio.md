---
layout: post
title: "Why I prefer log odds ratios."
author: "Olivia Brode-Roger"
date: "February 13, 2016"
---



Imagine that high school graduation rates went from 75% to 80% in Obama's presidency.
Regardless of truth, lets try to get a measure of how impressive this is.
How would it compare to going from 45% to 55%? how about 94% to 99%? and 1% to 6%? and 99.9% to 99.9999%?
I find the extremes a lot more impressive than the middle, simply because the population which doesn't graduate got multiplied by a large factor!


{% highlight r %}
dates <- as.Date(c("2009-01-01", "2016-01-01"))
rates_label <- c("Real", "Half", "High", "Low", "Extra High")
rates <- c(75, 80, 45, 55, 94, 99, 1, 6, 99.9, 99.9999)
graduation_rates = data.frame(dates=rep(dates, 5),
                              label=rep(rates_label, each=2),
                              rates=rates)
ggplot(graduation_rates, aes(x=dates, y=rates, color=label)) + geom_line()
{% endhighlight %}

![center](/../figs/2016-02-13-why-I-prefer-log-odds-ratio/unnamed-chunk-1-1.png)

So how can we bring some math to this intuition?
Well, the intuition comes from the fact that some population got multiplied (or divided) by a large factor.
In the case of 1% to 6%, it's the graduating population, for 94% to 99% it's the dropout population.
Odd ratios are a nice way to measure that, formally `odds of winning:odds of losing`.
In these cases, we get `1:99` to `6:94=3:47`(and the inverses for the other way around).

However, given two fractions, I have a hard time comparing them.
We could compute the values, giving us for low `0.010101...` to `0.0638...`, and for high `15.666...` to `99`.
I still have trouble getting much meaning from them, but let's plot what we have first (please note the y-scale changes).


{% highlight r %}
ggplot(graduation_rates, aes(x=dates, y=rates/(100-rates), color=label)) + geom_line()
{% endhighlight %}

![center](/../figs/2016-02-13-why-I-prefer-log-odds-ratio/unnamed-chunk-2-1.png)

{% highlight r %}
ggplot(graduation_rates, aes(x=dates, y=rates/(100-rates), color=label)) + geom_line() + ylim(-1, 100)
{% endhighlight %}

![center](/../figs/2016-02-13-why-I-prefer-log-odds-ratio/unnamed-chunk-2-2.png)

{% highlight r %}
ggplot(graduation_rates, aes(x=dates, y=rates/(100-rates), color=label)) + geom_line() + ylim(-1,5)
{% endhighlight %}

![center](/../figs/2016-02-13-why-I-prefer-log-odds-ratio/unnamed-chunk-2-3.png)

The Very High change is clearly very impressive, but Low and High don't look comparable at all.
Due to the huge range, these graphs are also very hard to read and we might as well stay with percentages.

In addition, I would like both changes to be as impressive as each other.
In other words, I don't want the non-graduation rate change to look more or less impressive than the graduation rate.

This is where the `log` comes in.
`log(0:100)= -infinity`, `log(1:1)=0` and `log(100:0) = infinity` (if the first and last ones made you cringe, just read them as limits).
We now have symmetry appears: `log(a/b)=-log(b/a)`.


{% highlight r %}
ggplot(graduation_rates, aes(x=dates, y=log(rates/(100-rates)), color=label)) + geom_line()
{% endhighlight %}

![center](/../figs/2016-02-13-why-I-prefer-log-odds-ratio/unnamed-chunk-3-1.png)

Now this graph might not look impressive, but give it a chance.
Before going any further, keep in mind that the impressiveness of a change of +1 is constant, this means that going from 50% to 75%, is comparable to going from 90% to 96%.
This embodies the intuition we were trying to formalize.

Now, to the interesting features!
First of all, the extra high line swoops up, corresponding to a reduction of the dropout rate by a factor of 1,000, that is *very* impressive, but invisible in our first graph.
Secondly, the high and low lines are visibly comparable in terms of their slope: they both had a similar impact.
Finally, the 10% change around the median is centered at 0, looking a slightly more impressive than the real change, again corresponding to our intuition.

So which is more impressive?
Since the difference in log space is sometimes misleading, I'm also going to compute the corresponding multiplying factor.

{% highlight r %}
#I'm sure there is a prettier way of doing this, I'm still learning, sorry :S
lor_rates_2009 <- log(graduation_rates[dates == as.Date("2009-01-01"), ][["rates"]]/(100-graduation_rates[dates == as.Date("2009-01-01"), ][["rates"]]))
lor_rates_2016 <- log(graduation_rates[dates == as.Date("2016-01-01"), ][["rates"]]/(100-graduation_rates[dates == as.Date("2016-01-01"), ][["rates"]]))

lor_rates_diff <- data.frame(label=rates_label, lor_rate_diff=lor_rates_2016 - lor_rates_2009)
lor_rates_diff$factor <- exp(lor_rates_diff$lor_rate_diff)
lor_rates_diff[order(lor_rates_diff$lor_rate_diff), ]
{% endhighlight %}



{% highlight text %}
##        label lor_rate_diff      factor
## 1       Real     0.2876821    1.333333
## 2       Half     0.4013414    1.493827
## 3       High     1.8435845    6.319149
## 4        Low     1.8435845    6.319149
## 5 Extra High     6.9087548 1001.000000
{% endhighlight %}
Which now clearly corresponds with our intuition that the real change, and the middle change are fairly comparable, that low and high have equivalent changes, and that the very high is extremely impressive.
