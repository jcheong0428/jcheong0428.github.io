---
layout: post
title:  "Korean Politicians with Fake Twitter Followers"
date:   2018-02-15 11:24:14 -0400
categories: jekyll update
---

Inspired by the [New York Times article The Follower Factory](https://www.nytimes.com/interactive/2018/01/27/technology/social-media-bots.html)
which investigated how some celebrities have allegedly purchased fake followers with prices as low as 2 cents per follower, I decided to look into detecting similar patterns on Korean politicians. Twitter is heavily used by Korean politicians and it has had powerful influence on [past  elections](https://www.reuters.com/article/net-us-korea-politics-socialmedia/south-koreas-twitter-generation-may-give-liberals-upset-win-idUSBRE8380BI20120409). Perhaps the most notable example of its influence is the [case still under investigation](https://ko.wikipedia.org/wiki/%EA%B5%AD%EA%B0%80%EC%A0%95%EB%B3%B4%EC%9B%90_%EC%97%AC%EB%A1%A0_%EC%A1%B0%EC%9E%91_%EC%82%AC%EA%B1%B4) in which Korea CIA agents allegedly used fake Twitter and other SNS accounts to post propaganda comments on websites. Even though [its influence has dwindled over the years](http://www.koreaherald.com/view.php?ud=20161012000804) many politicians still heavily use Twitter with even the current President, Moon Jae In, boasting over 1.74 million followers.

The NYT identified the amount of fake followers on celebrity accounts by looking at the Twitter **Join Date** at which the followers joined Twitter. In this screenshot from the NYT article, you can quickly notice the streaky pattern at which there was a rapid increase of followers around Jan 2013, Sept 2014 and at Nov 2015.

<figure>
<div style="text-align:center">
  <img src="/assets/post12/chefsymon_nyt.png" width="800">
  <figcaption><p align="right"><i>The New York Times</i>
  </p></figcaption>
</div>
</figure>

The authors argue that this pattern provides clear evidence of bots or fake followers which they later verified purchases.
The pattern is that thousands of bots are created around a short time period (the streaky lines) and then slowly but persistently
start following the account over a period of time. Taking this approach, I investigate the fake followers on Korean politician Twitter accounts.
<br><br>

---
### Data Collection
To gather followers of an account and the followers' join dates,
I needed to access the list of followers, then request the user info for each follower.
Unfortunately, Twitter API basically imposes a limit of 1 request per second. Which means that to scrape an account with 300K followers would take about 4 days. I started collecting the data on February 1st, 2018.  

Here is the code that I used with [Twython](https://github.com/ryanmcgrath/twython) which grabs the info and saves to a text file.

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
#### Park Geun-Hye (@GH_PARK, 385K followers)
The first politician I scraped was the former 18th president Park Geun-Hye who took office on December 19th 2012. She joined Twitter on April 2010, has 385K followers, and was possibly the most influential conservative politician on Twitter. Here is the plot of the followers' join dates.

<iframe width="1100" height="800" frameborder="0" scrolling="no" src="//plot.ly/~jcheong0428/90.embed"></iframe>

At a first glance, I did not see much of a fake follower pattern here as seen in the [NYT article](https://www.nytimes.com/interactive/2018/01/27/technology/social-media-bots.html).
Maybe you see some streaks spanning across around beginning of 2010, 2012, and 2013. There is an awkward streak of followers created around May 15, 2017 - May 29, 2017, that spans maybe 300 followers but nothing strongly evident. Perhaps all of Park's followers were legit but I thought if there is an imbalance of followers, you should be able to see it it in a histogram of followers binned by join date.

<iframe width="1100" height="500" frameborder="0" scrolling="no" src="//plot.ly/~jcheong0428/100.embed"></iframe>

If you assume that Twitter join dates may follow a uniform distribution
(approximately similar number of people join twitter on each Month) and that they
would have equal probability of following an account, this histogram looks surprising.
There are at least four distinct peaks, first at Jul 2010 (14K followers),
second at Jan 2012 (9K followers),third at Dec 2012 (12K followers), and fourth at Jan 2016 (8K followers).
Although these times coincide with crucial political events for Park (she announced to run for president in Jul 2010, became head of the party in Dec 2011, and won the election in Dec 2012),
it still seems odd that mass amounts of people would **create accounts** within a sort period of time
to start following her over time. The more natural process would be to have had created Twitter
accounts at distributed times but start following her around the same time in response to a political event.
From this exercise, we could venture a guess that if Park's account has fake bot followers, they may have been created at those times with histogram peaks.

<br><br>

#### Lee Jung-Hee (@heenews, 271K followers)
Here is a look at [Lee Jung-Hee (@heenews)](https://twitter.com/heenews) who was
one of the candidates for the 18th presidential election and was also a member of the
National Assembly. She and her party has been removed from the assembly in December 2014
after being criticized for being [pro-North Korean](https://en.wikipedia.org/wiki/Lee_Jung-hee).
She joined Twitter on January 2010 and still has 271K followers.

<iframe width="1100" height="800" frameborder="0" scrolling="no" src="//plot.ly/~jcheong0428/96.embed"></iframe>

Streaks of followers are more noticeable here that begins around late 2011 to
early 2012. There is also another distinct horizontal streak around Dec 2012.

<iframe width="1100" height="500" frameborder="0" scrolling="no" src="//plot.ly/~jcheong0428/102.embed"></iframe>

Again, the histogram confirms that there is an imbalance in the join dates of followers
with huge peaks at Dec 2011 and Dec 2012 with 16.4K followers have created their account
in December 2011.


<br><br>

---

### Other politicians
Here are some other politicians with the two kinds of plots side by side.
I rotated the histogram so it is easier to compare the streaky pattern to the histograms.

#### Chun Jung-bae (@jb_1000, 202K followers)
<iframe width="1100" height="600" frameborder="0" scrolling="no" src="//plot.ly/~jcheong0428/104.embed"></iframe>

#### Kang Gi-Gap (@kanggigap, 140K followers)
<iframe width="1100" height="600" frameborder="0" scrolling="no" src="//plot.ly/~jcheong0428/106.embed"></iframe>

Just like Lee Jung-Hee, also has surge of followers created on Dec 2012.

#### Won Hee-Ryong (@wonheeryong, 45.1K followers)
<iframe width="1100" height="600" frameborder="0" scrolling="no" src="//plot.ly/~jcheong0428/108.embed"></iframe>


# Conclusion
Identifying fake Twitter followers of Korean politicians did not turn out to be as obvious as the NYT article. However a closer look at the distribution of join dates among followers suggests that there may have been fake followers created during certain periods that followed the accounts over time.

The crucial reason NYT was able to identify those patterns was because twitter sorts the followers
based on when they started following an account. However, this is easy to trick if you are the
bot owner by simply unfollowing and re-following an account over time. If this is already the case, it may not be possible to confirm the genuineness of followers in this manner.
