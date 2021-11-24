---
layout: post
title:  "Building PaperWiki: An encyclopedia of scientific journal articles"
date:   2021-11-20 11:24:14 -0400
categories: jekyll update
comments: true
tags: app flask python better-science
---

<figure>
  <div style="text-align:center">
  <img src="/assets/post20211124/paperwiki0.png" width="550">
  <figcaption><p align="right"><i><a href="http://www.paperwiki.org/">Paperwiki.org</a> logo</i></p></figcaption>
  </div><br>
</figure>

[*This post was also published in Bootcamp on Medium*](https://bootcamp.uxdesign.cc/case-study-a-crowdsourced-online-encyclopedia-for-scientific-journal-articles-c671b5ba29f)<br>


<!--excerpt.start-->
### Building PaperWiki: A crowdsourced online encyclopedia for scientific journal articles

Back in summer of 2018 when I was still an aspiring neuroscientist, I attended a [summer program](https://neurohackademy.org/) at the University of Washington in Seattle. The first week was devoted to learning everything coding and data related. The second week was a hackathon — time reserved for building new tools and packages that would help us do better science.

# The pain points in doing science
Thinking about what I could contribute to the scientific community, I thought about my biggest challenges in doing science. Reading. It’s not that I hate reading but reading scientific journal articles can be difficult and lonely. Journal articles are dense, often superfluous, and packed with data & ideas. Unfortunately, you have very little access to what other people think. If you are lucky, maybe you’ll do a journal club where half the attendees haven’t read the paper or you’ll see some chat about it on academic Twitter until it’s quickly displaced by other new papers.

Back in high school when I was stuck reading books or papers for classes, [Wikipedia](https://en.wikipedia.org/wiki/Main_Page) and [SparkNotes](https://www.sparknotes.com/) were my saving grace. SparkNotes had amazing summaries that really helped a casual reader like me better understand a book to its entirety. Wikipedia proved the power of crowdsourced knowledge where people collaborate and correct each other to explain even the most complex theories and concepts in a digestible manner.

I thought it would be great to have something like this for academic papers. The idea was simple: Create a platform where people can share and collaborate summaries of scientific articles.

<br>

___

<br>

<div style="text-align:center">
<img src="/assets/post20211124/paperwiki1.png" width="600">  
</div>
<figcaption><p align="center"><i>Screenshot of <a href="http://www.paperwiki.org/">paperwiki.org</a>
</i>
</p></figcaption>

# The features
Paperwiki required two key features: 1) search for a science article, and 2) create or edit notes attached to that article.

# Feature 1. Searching for a science article
I thought that the first feature would be very easy to implement. I thought I’d be able to use Google Scholars which is astonishingly good at finding the exact article with just a few names and keywords. Turns out, Google Scholars don’t have an API 😡. So I tried to use a wrapper to access their website only to find myself blocked after a few queries.

Eventually, I landed on using [Crossrefapi](https://www.crossref.org/documentation/retrieve-metadata/rest-api/) which is a publicly available REST API. The search performance was not ideal but it had a good [Python library](https://github.com/fabiobatalha/crossrefapi) which would play well with the Flask framework I was using.


<div style="text-align:center">
<img src="/assets/post20211124/paperwiki2.png" width="600">  
</div>
<figcaption><p align="center"><i>Search results shown with abstracts.
</i>
</p></figcaption>

# Feature 2. Creating and editing summaries of scientific articles
I wanted the editing experience to be lightweight and user friendly. It should also be able to save images that could help with comprehension. I looked around and found [Flask-PageDown](https://github.com/miguelgrinberg/Flask-PageDown), which was [Markdown](https://en.wikipedia.org/wiki/Markdown) for Flask which was the backend I was using to develop Paperwiki. So when a user found the article they were looking for, they could click “Create Wiki” to create a new summary or “See Wiki” if a page already exists for that article, that would lead them to a page like the [one shown below](http://www.paperwiki.org/see_wiki?id=10.31234/osf.io/bd9wn).

<div style="text-align:center">
<img src="/assets/post20211124/paperwiki3.png" width="600">  
</div>
<figcaption><p align="center"><i>Screenshot of the editing experience.
</i>
</p></figcaption>

<br>

___

<br>


# Next steps for improving how we do science
PaperWiki is in its infancy, but I really think this could help bring science to more people. I hope to add additional functionalities in the coming months such as 1) Personalized lists of articles so that you can keep track of all the wiki’s you contributed to, and 2) group reading lists so instructors can create a collection of articles with a blank page for their students to fill out together.

Looking back, building PaperWiki was an exciting project and I’m passionate about making it better. Please feel free to reach out to me or open a pull request on the project repository if you share my passion for open science.

And finally, here is the link to the [Paperwiki.org](http://paperwiki.org/)

