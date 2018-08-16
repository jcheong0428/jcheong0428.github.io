---
layout: post
title:  "Tipping in NYC taxis"
date:   2016-05-04 11:24:14 -0400
categories: jekyll update
comments: true
---

<figure>
  <img src="/assets/post01/nycab.jpeg" width="1300">
  <figcaption><p align="right"><i>http://becomeanewyorker.com/</i>
  </p></figcaption>
</figure>

This is a quick analysis on how people tipped NYC cabs in 2013.
The data was retrieved from [Chris Whong's blog](http://chriswhong.com/open-data/foil_nyc_taxi/comment-page-1/). It's a sizeable dataset and an amazing visualization came out of it called [NYC Taxis: A Day in the Life](http://nyctaxi.herokuapp.com/). I got interested in the dataset and wanted to look at how people tip in NYC cabs.

# Tips by payment type

After cleaning up the dataset, I first noticed that more than half the rides were paid by credit card.

<figure>
  <img src="/assets/post01/CashCreditTipPercent.png" width="1300">
  <figcaption><p align="right"><i></i>
  </p></figcaption>
</figure>

When people pay by cash, it turns out tip is not recorded for 99% of the rides. I'm not sure if this is because drivers can't record tip into their meter or if they just don't bother. One thing I do know is that there are at least 6639 cases of tip recorded when paid in cash.

When people pay by card, the tip is automatically recorded. and it shows a nice distribution of tip percentage with about 40% of passengers paying tips of 20%.    
<br>

# Tips by group size
The main question I wanted to examine was whether people's tip change as a function of group size.

This is a question that one can easily see going two ways. I might be inclined to tip more in a group than alone because 1) I feel social pressure, or 2) I feel like I owe more service because the driver dealt with more people.

Alternatively, I might tip less in a larger group because 1) diffusion of responsibility, or 2) I feel less connected to the driver since the service is distributed to the group than concentrated on me.

Here is the result:

<figure>
  <img src="/assets/post01/TipPercentByGroupSize.png" width="1300">
  <figcaption><p align="right"><i></i>
  </p></figcaption>
</figure>

Consistent with the second idea, tip percentage tends to decrease as group size gets larger.

This is consistent with findings in [restaurants](http://scholarship.sha.cornell.edu/cgi/viewcontent.cgi?article=1036&context=articles), where larger groups tend to tip less than smaller groups.

But why is this the case? It's difficult to look at the social computations going on in an individual's head in this kind of data. One possibility cited in the above article referencing [Elman(1976)](http://psp.sagepub.com/content/2/3/307.abstract) is that larger groups tend to have larger bills where the financial burden increases holding tip percentages. I find that this is not the case below.

<br>

# Total Fare by group size

The average fare fluctuates across group sizes. For sanity, I capped the max fare at $500 but I think it's very rare for people to travel that far. You can see that fares are higher when people travel as a group of three or four but lower when they travel alone or in a group of six.

<figure>
  <img src="/assets/post01/TotalFareByGroupSize.png" width="1300">
  <figcaption><p align="right"><i></i>
  </p></figcaption>
</figure>

<br>

# Tip as function of total fare

A [Bloomberg article](http://www.bloomberg.com/news/articles/2014-08-07/tipping-taxi-drivers-data-analysis-cant-explain-these-puzzles) explored an interesting phenomenon. That is, people tend to tip less on whole numbers when fares end in 5s or 0s. I also find that this is the case, with a slight caveat that it occurs right before the numbers end in 0s or 5s. For instance, if your bill is $27 you are more likely to pay just $3 to round up your total cost to a whole number than to tip the customary 20%.  

Also in the article, they note that tip rates drop drastically when the total fare is $100. I don't see that in my data but I suspect that might be due to some filtering or averaging artifact.

<figure>
  <img src="/assets/post01/TipPercentByFare.png" width="1300">
  <figcaption><p align="right"><i></i>
  </p></figcaption>
</figure>

<br>

# Conclusions

1. People in larger groups tip less.
2. Larger groups don't necessarily spend more.
3. Tips tend to decrease as total fare increases but drops more heavily near fares close to ending in 0s.
