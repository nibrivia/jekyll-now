---
layout: page
title: "Target tracking"
author: "Olivia Brode-Roger"
date: "February 16, 2016"
permalink : /targets/
---



States above the diagonal are where Clinton exceeded targets, below Sanders is exceeding target, more in my (first!) [blog post]({% post_url 2016-02-10-targets-v-results %})
Arbitrarely, positive indicates "Clinton-ness", 0 is neutral and negative is "Sanders-ness".


{% highlight text %}
## Error in `$<-.data.frame`(`*tmp*`, "result", value = numeric(0)): replacement has 0 rows, data has 51
{% endhighlight %}



{% highlight text %}
## Error: id variables not found in data: result
{% endhighlight %}



{% highlight text %}
## Error in ggplot(state_targets, aes(x = value, y = result, color = variable)): object 'state_targets' not found
{% endhighlight %}

Because the distance from the diagonal is what we care about, here's a change of coordinates:

{% highlight text %}
## Error in ggplot(state_targets, aes(x = value, y = result - value, color = variable)): object 'state_targets' not found
{% endhighlight %}
The targets used are:

- [Cook Political Report, Jan 21](http://cookpolitical.com/story/9179).
- [Cook Political Report, Feb 12](http://cookpolitical.com/story/9258).
- [my first attempt]({% post_url 2016-02-15-targets---FiveThirtyEight-edition %}) (`white*2012 Democratic vote share`)
- [FiveThirtyEight's targets](http://fivethirtyeight.com/features/bernie-sanderss-path-to-the-nomination/)
