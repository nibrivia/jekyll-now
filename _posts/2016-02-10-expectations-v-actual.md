---
title: "Democratic primary expectations v actual"
author: "Olivia Brode-Roger"
date: "February 10, 2016"
---

The data for the expectations come from http://cookpolitical.com/story/9179. At some point I will get around to computing these myself.
The delegate data is (so far) manually entered.


{% highlight r %}
delegates <- data.frame(State = c("Iowa", "New Hampshire"),
                        Clinton_percentage=c(23, 9), Sanders_percentage=c(21, 15),
                        Clinton_expect=c(13, 9), Sanders_expect=c(31, 15))
{% endhighlight %}

We can plot these against each other and see how well we do!
This is using log-odds (more on that at later date).
The more positive the number, the more the state is in support of Clinton and vice versa. 0 is perfectly equal. Above the line means Clinton is exceeding expecations, below Sanders, regardless of who "won" the state.


{% highlight r %}
require(ggplot2)
{% endhighlight %}



{% highlight text %}
## Loading required package: ggplot2
{% endhighlight %}



{% highlight text %}
## Loading required package: methods
{% endhighlight %}



{% highlight r %}
plot <- ggplot(delegates, aes(x = log(Clinton_expect/Sanders_expect), y = log(Clinton_percentage/Sanders_percentage),
                              fill = log(Clinton_expect/Sanders_expect) - log(Clinton_percentage/Sanders_percentage))) +
  #geom_text( aes(label=State), hjust = 0, vjust = 0 ) +
  geom_point( aes(size = Clinton_expect+Sanders_expect), shape=21) + scale_size_continuous(range = c(3,7)) +
  scale_fill_gradientn(colours = c("blue", "grey", "red"), name="Candidate ahead", limits=c(-1,1)) +
  labs(title = "Expectations v Results", x = "Expectations", y = "Results", size = "Total delegates") +
  geom_abline(intercept = 0, slope=1, alpha = 0.5) +
  xlim(-1,1) + ylim(-1,1)
plot
{% endhighlight %}

![center](/../figs/2016-02-10-expectations-v-actual/unnamed-chunk-2-1.png)

This is interesting since it shows that Sanders is winning (points are y-negative), but is also shows that, at this point, Clinton is meeting or exceeding expectations. This disagrees the mainstream media narrative, unfortunately these narratives (apparently) have some influence on the future primaries, so we'll see.
Another interesting thing that this might shed some light as to if momentum really exists.