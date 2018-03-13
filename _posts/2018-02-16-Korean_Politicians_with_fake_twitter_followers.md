---
layout: post
title:  "Korean Politicians with Fake Twitter Followers"
date:   2018-02-15 11:24:14 -0400
categories: jekyll update
---

Inspired by the New York Times article [The Follower Factory](https://www.nytimes.com/interactive/2018/01/27/technology/social-media-bots.html)
 I decided to investigate if Korean politicians also bought followers. Twitter is very [influential](https://www.reuters.com/article/net-us-korea-politics-socialmedia/south-koreas-twitter-generation-may-give-liberals-upset-win-idUSBRE8380BI20120409) in Korean politics and has even been [scrutinized](https://ko.wikipedia.org/wiki/%EA%B5%AD%EA%B0%80%EC%A0%95%EB%B3%B4%EC%9B%90_%EC%97%AC%EB%A1%A0_%EC%A1%B0%EC%9E%91_%EC%82%AC%EA%B1%B4) for involvement of Korean
 CIA agents who created fake accounts to swing elections by mass retweeting of propaganda.
 Even though [its influence has dwindled over the years](http://www.koreaherald.com/view.php?ud=20161012000804) many Korean politicians still heavily use Twitter with even the current President, Moon Jae In, boasting over 1.74 million followers.


The NYT identified fake followers by looking at the Twitter **Join Dates**, i.e., when the followers created their accounts. In this figure from the NYT article, the streaky pattern at which there was a rapid increase of followers around Jan 2013, Sept 2014 and at Nov 2015 is very noticeable. And that these followers's accounts were created at similar times is clear indication that they are fake accounts.

<figure>
<div style="text-align:center">
  <img src="/assets/post12/chefsymon_nyt.png" width="800">
  <figcaption><p align="right"><i>The New York Times</i>
  </p></figcaption>
</div>
</figure>

Taking this approach, I investigate the fake followers on Korean politician Twitter accounts.

<br><br>

---
### Data Collection
To gather a list of followers and their join dates,
I needed to loop through each follower and request their user info.
Unfortunately, Twitter API  imposes a limit of 1 request per second,
which means that to scrape an account with about 300K followers would take about 4 days.
I started collecting the data on February 1st, 2018.  

Here is the code I wrote using [Twython](https://github.com/ryanmcgrath/twython) which grabs the info and saves it to a text file.

```python
#!/usr/bin/env python
from twython import Twython
import time
# SET CREDENTIALS
APP_KEY = 'YOURAPPKEY'
APP_SECRET = 'YOURAPPSECRET'
OAUTH_TOKEN = 'YOUR-OAUTHTOKEN'
OAUTH_TOKEN_SECRET = 'YOUROAUTHTOKENSECRET'

# INITIALIZE TWITTER API
twitter = Twython(APP_KEY, APP_SECRET, oauth_version=2)
ACCESS_TOKEN = twitter.obtain_access_token()
twitter = Twython(APP_KEY, access_token=ACCESS_TOKEN)

# SET SCREEN NAMES YOU WOULD LIKE TO SCRAPE
screen_names = ['jcheong0428']
for screen_name in screen_names:
    next_cursor = -1
    page_num = 0
    with open(screen_name+'.txt', 'w') as f:
        f.write('screen_name\tid_str\tcreated_at\tfollowing\tfriends_count\tfollowers_count\tstatuses_count\n')
    while next_cursor != "0":
        print(page_num)
        page_num += 1
        followers_list = twitter.get_followers_ids(screen_name=screen_name,cursor=next_cursor)
        next_cursor = followers_list['next_cursor_str']
        for follower_id in followers_list['ids']:
            try:
                user = twitter.show_user(id=follower_id)
                with open(screen_name+'.txt', 'a') as f:
                    # SAVE USER INFO
                    for info in ['screen_name','id_str','created_at','following','friends_count','followers_count','statuses_count']:
                        infotosave=user[info]
                        if infotosave==None:
                            infotosave = 0
                        f.write(str(infotosave)+'\t')
                    f.write('\n')
                time.sleep(1.01) # limit is 900 per 15 min.
            except:
                time.sleep(1.01)
```

<br><br>

---
### Analyzing follower data
#### Park Geun-Hye ([@GH_PARK](https://twitter.com/GH_PARK), 385K followers)
The first politician I scraped was the former 18th president Park Geun-Hye who took office on December 19th 2012. She joined Twitter on April 2010, has 385K followers, and was possibly the most influential conservative politician on Twitter. Here is the plot of the followers' join dates by the order in which they started following the account.

<iframe width="800" height="800" frameborder="0" scrolling="no" src="//plot.ly/~jcheong0428/90.embed"></iframe>

At first glance, it is difficult to see much of a fake follower pattern here as seen in the [NYT article](https://www.nytimes.com/interactive/2018/01/27/technology/social-media-bots.html).
Maybe you see some streaks spanning across around beginning of 2010, 2012, and 2013. There is an awkward streak of followers created around May 15, 2017 - May 29, 2017, that spans maybe 300 followers but nothing strongly evident. Perhaps all of Park's followers were legit but I thought if there is an imbalance of followers, you should be able to see it it in a histogram of followers binned by join date.

<iframe width="800" height="500" frameborder="0" scrolling="no" src="//plot.ly/~jcheong0428/100.embed"></iframe>

If you assume that Twitter join dates may follow a uniform distribution
(approximately similar number of people join twitter on each Month) and that they
would have equal probability of following an account, this histogram looks surprising.
There are at least four distinct peaks, first at Jul 2010 (14K followers),
second at Jan 2012 (9K followers),third at Dec 2012 (12K followers), and fourth at Jan 2016 (8K followers).
Although these times coincide with crucial political events for Park (she announced to run for president in Jul 2010, became head of the party in Dec 2011, and won the election in Dec 2012),
it still seems odd that mass amounts of people would **create accounts** within a sort period of time
to start following her over time.

**It wouldn't be surprising to see increases in followers happen in
waves at times when the politician is receiving a lot of attention. HOWEVER, it is
unusual and surprising if a distinct portion of your followers created their accounts at almost the same time.**  
And this is exactly the pattern we see in the histogram.

<br><br>

#### Lee Jung-Hee ([@heenews](https://twitter.com/heenews), 271K followers)
Here is a look at [Lee Jung-Hee (@heenews)](https://twitter.com/heenews) who was
one of the candidates for the 18th presidential election and was also a member of the
National Assembly. She and her party has been removed from the assembly in December 2014
after being criticized for being [pro-North Korean](https://en.wikipedia.org/wiki/Lee_Jung-hee).
She joined Twitter on January 2010 and still has 271K followers. From here on I will start plotting
the cumulative follower plot and the histogram side by side with the histogram rotated 90 degrees.

<iframe width="800" height="600" frameborder="0" scrolling="no" src="//plot.ly/~jcheong0428/120.embed"></iframe>

Streaks of followers are more noticeable here that begins around late 2011 to
early 2012. There is also another distinct horizontal streak around Dec 2012.
The histogram confirms that there is an imbalance in the join dates of followers
with huge peaks at Dec 2011 and Dec 2012 that add up to 24K followers.


<br><br>

---

### Other politicians
Here are some other politicians with the two kinds of plots side by side.
I rotated the histogram so it is easier to compare the streaky pattern to the histograms.

#### Moon Jae-In ([@moonriver365](https://twitter.com/moonriver365), 1.76M followers)
Moon is the current president of South Korea and is active on Twitter with  1.76M followers.
However, we can suspect that some portion of his followers are fake followers or bots as indicated by numerous horizontal streaks in the graph below. These horizontal patterns indicate tens of thousands of followers created in a very short time from each other which is very unnatural.

<figure>
<div style="text-align:center">
  <a href="https://www.dropbox.com/s/i19t9hl8hffjovx/moonriver365.html?dl=1">
  <img src="/assets/post12/moonriver365.svg" width="800">
  <figcaption><p align="right"><i>Click image to download interactive graph</i>
  </p></figcaption></a>
</div>
</figure>

#### Rhyu Si-min ([@u_simin](https://twitter.com/u_simin), 712K followers)
<iframe width="900" height="800" frameborder="0" scrolling="no" src="//plot.ly/~jcheong0428/118.embed"></iframe>

#### Han Myeong-Sook ([@HanMyeongSook](https://twitter.com/HanMyeongSook), 240K followers)
<iframe width="800" height="600" frameborder="0" scrolling="no" src="//plot.ly/~jcheong0428/114.embed"></iframe>

#### Chun Jung-bae ([@jb_1000](https://twitter.com/jb_1000), 202K followers)
<iframe width="800" height="600" frameborder="0" scrolling="no" src="//plot.ly/~jcheong0428/104.embed"></iframe>

#### Kang Gi-Gap ([@kanggigap](https://twitter.com/kanggigap), 140K followers)
<iframe width="800" height="600" frameborder="0" scrolling="no" src="//plot.ly/~jcheong0428/106.embed"></iframe>


#### Won Hee-Ryong ([@wonheeryong](https://twitter.com/wonheeryong), 45.1K followers)
<iframe width="800" height="600" frameborder="0" scrolling="no" src="//plot.ly/~jcheong0428/108.embed"></iframe>



# Conclusion
Identifying fake Twitter followers of Korean politicians did not turn out to be as obvious as the NYT article. However a closer look at the distribution of follower join dates suggests traces of fake follower activity in that thousands of accounts created within a short period  follow the account.  


One possible reason the pattern is more distributed could be that these account
creators may be pacing the rate at which the bots follow the account or unfollowing and re-following
an account to avoid suspicion.  

You might think that thousands of followers are not significant when it's only a fraction
of the entire number of followers, but these fake accounts may be professionally managed
by groups with clear aims of manipulating public opinion. This may have been
the [case](https://www.politico.com/story/2018/02/16/text-full-mueller-indictment-on-russian-election-case-415670) in the 2016 US presidential election during which a Russian company [created more than 50K human-coordinated fake accounts and posted more than 2 million tweets](https://www.judiciary.senate.gov/imo/media/doc/Edgett%20Appendix%20to%20Responses.pdf).  

While some may argue that the influence of these malicious attempts at swaying public opinion is [overrated,](https://www.nytimes.com/2018/02/13/upshot/fake-news-and-bots-may-be-worrisome-but-their-political-power-is-overblown.html)
we should perhaps be more cautious and keep in mind that those trending people, topics, and articles on the web may be fake activity.
