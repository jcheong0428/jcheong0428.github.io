---
layout: post
title:  "Summer school comparison: Neurohackademy vs Methods in Neuroscience at Dartmouth (MIND)"
date:   2018-07-29 11:24:14 -0400
categories: jekyll update
comments: true
tags: grad-school life tips
---

For graduate students, postdocs, and young faculty, attending a summer program is a
great opportunity learn something new and make new friends. But there are many
summer programs that neuroscience researchers can choose from and deciding which one
to attend can be difficult.

Here I outline my experience with two of the summer schools that I had the privilege
of attending: [Neurohackademy at University of Washington in Seattle](https://neurohackademy.org/)
and [Methods in Neuroscience at Dartmouth (MIND)](https://summer-mind.github.io/).

TL;DR;
**Check out Neurohackademy if you want to focus on learning about good
programming practices, wide range of analytic tools, and an introduction to hacking.
I would recommend MIND if you want to focus more on learning advanced analytic methods
that you were curious about or wanted to apply to your own data. The topics offered
at MIND change from year to year but a core theme seems to be methods useful
for analyzing naturalistic and multidimensional data with temporal, linguistic,
and interpersonal dynamics. Personally, I would recommend go to Neurohackademy in
the first/second year of grad school and then check out MIND in later years.**

<br>

___

<br>



<figure>
<div style="text-align:center">
  <img  src="/assets/post15/neurohackademylogo.png" width="560">
</div>
</figure>

# Neurohackademy

Neurohackademy is a two-week program focused on neuroimaging and data science
hosted by the University of Washington eScience Institute in Seattle. Participants come from a wide
range of backgrounds including graduate students (master, phd), post-docs, as well as
those who work in the industry. A detailed course schedule can be found [here](https://neurohackademy.org/neurohack_year/2018/)
but in summary the first week is devoted to introductory tutorials & lectures
and the second week is reserved for hackathon projects.

Lectures in the first week covered introductions to common programming languages
including Python, R, and Matlab. Tutorials for each language covered both basic concepts
and use in advanced analyses. For example, tutorials in R included data munging, visualizations,
as well as using [Neuropointillist](http://ibic.github.io/neuropointillist/) for
fMRI analysis in R. Topics in Python were the most extensive including data munging,
visualizations, packaging, software testing, fMRI analysis using [nipype](https://nipype.readthedocs.io/en/latest/),
and deep learning using [Keras](https://keras.io/). There is also a big focus
on open and reproducible science, and thus covers using open science tools such as Docker containers.

My favorite tutorials were [deep learning with Keras](https://neurohackademy.org/course/deep-learning-with-keras/)
by [Ariel Rokem](https://arokem.org/) and [introduction to web applications](https://neurohackademy.org/course/introduction-to-web-technologies/) by [Anisha Keshavan](https://anisha.pizza/#/). Ariel did an amazing job explaining the fundamental building blocks of a
neural network and how they come together to form more complicated models such as DCNNs with an
easy to follow tutorial. Anisha's web application tutorial described the
fundamental building blocks in building a web app while also touching on popular
tools such as [Vue.js](https://vuejs.org/) and [Firebase](https://firebase.google.com/).

The hackathon hosted more than a dozen [projects](https://github.com/neurohackademy/2018_projects)
with diverse topics using web applications, cloud computing, open datasets, and deep learning.
I participated in two projects: hacking the [Study Forrest](http://studyforrest.org/about.html) data and building [PaperWiki](https://github.com/jcheong0428/paperwiki). Using the
Study Forrest dataset, we looked at how brain representations of characters evolve
over time across individuals. We tested whether participants generated different
representations of characters over time due to personal preferences or identical
representations over time due to shared information. On a whole brain level, we
found evidence for for the latter for the main characters Forrest and Jenny
but not for the peripheral characters. Check out the presentation video below:

<p align="center"><iframe width="560" height="315" src="https://www.youtube.com/embed/Vn0ebJ-ZhGU?start=3076" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe></p>

[PaperWiki](https://paperwiki.herokuapp.com/) was designed to be like Wikipedia
for research articles. Whenever I read academic papers, I wanted to know and discuss what other
people thought about the paper, the history of the paper (e.g., why is this paper meaningful?),
and any updates regarding the contents of the paper (e.g., was there any issues in the paper?).
This kind of information is often scattered and surprisingly difficult to get
and that is why I think PaperWiki could be a useful tool for researchers as well as students.
Currently, one can search for articles and create a Wikipedia-like article for each paper in markdown. There is currently a Disqus commenting board which I hope to replace with a reddit-like board in the future.

Overall, Neurohackademy is a great opportunity where you can learn a ton about programming,
neuroimaging research, and the open science community by meeting and interacting with core developers of the Python scientific computing ecosystem such as numpy, sklearn, nipype, and Jupyter. If you are interested in
what the lectures are like, checkout the lecture videos [here](https://www.youtube.com/playlist?list=PLO3l0PnUGHYEqA7rFQT2jM6jxsaC2XiHh).

<figure>
<div style="text-align:center">
  <img  src="/assets/post15/neurohackweek18.jpg" width="560">
</div>
</figure>
Lastly, summer in Seattle is beautiful and here is a picture of the attendees
on a cruise to prove it.

<br>

___

<br>

<figure>
<div style="text-align:center">
  <img  src="https://raw.githubusercontent.com/Summer-MIND/Summer-MIND.github.io/master/images/mind_logo.png" width="200">
</div>
</figure>

# Method In Neuroscience at Dartmouth (MIND)

The MIND summer school lasts about a week and a half but the shorter duration
still packs a tons great lectures and tutorials. The hackathon at MIND begins
in the first few days, and the days are structured to have lectures in the morning,
tutorials in the afternoon, and working on hackathon projects in the evening.
I would say that the MIND schedule is a bit more intense than Neurohackademy
as you end up working on projects in the evening whereas evenings in Neurohackademy
didn't really get fired up until the second week of hackathons.

There is a day devoted to learning about open science tools such as
Git/Github, containers ([Docker](https://www.docker.com/)/[Singularity](https://www.sylabs.io/)),
and [Jupyter Notebooks](http://jupyter.org/) but the time spent on these topics is
relatively less than what you would get from Neurohackademy. An interesting comparison
is that MIND uses the [MIND-Docker container](https://github.com/Summer-MIND/mind-tools)
for tutorials whereas Neurohackademy uses the Jupyter hub. I think both options are
great for creating an easy-to-use and uniform environment for tutorials but the hub
approach seems a little more accessible because you merely need to enter the website address
without having to install a ~10gb docker container.  

The best part of MIND were the lectures and specific tutorials aimed at
data analysis. When I attended in 2017, we learned about spike decoding, fMRI decoding,
network dynamics, spatiotemporal models, HMMs, and much more. This year included
additional topics such as RSA, natural language processing, and conversation analyses
which I was sad to miss. As the focus is on learning new analytic tools and methods, the hackathons
projects also lean more towards applying these methods to your own or public data (in contrast
to Neurohackademy where more people focused on building and improving tools
for analysis and visualizations).

When I attended, I participated in applying a [negative affect classifier, PINES,](http://journals.plos.org/plosbiology/article?id=10.1371/journal.pbio.1002180)
to neural data from [Aaron Heller's](http://www.manateelab.org/) [study](http://www.manateelab.org/publication/the-face-of-negative-affect-trial-by-trial-corrugator-responses-to-negative-pictures-are-positively-associated-with-amygdala-negatively-associated-with-ventromedial-prefrontal-cortex-activity/) on looking at corrugator activity while viewing negative/positive images. We found that PINES
predictions correlated well with arousal and negatively correlated with valence
although the predictions weren't so good when we tried to corrguator activity or win/loss trials.
Also, after training a whole-brain ridge prediction model for valence and arousal
we compared the cross prediction accuracies which turned out to be fairly high suggesting that
these two modalities may be similarly represented in the brain.

Overall, I think MIND is a great summer program especially for individuals who
have some experience in programming that want to focus on learning and applying
new methods in a short amount of time. You get to interact with instructors to learn
those new methods from the experts who use them. You can check out the MIND lecture videos [here](https://www.youtube.com/channel/UCFiU9ZsUybQPq14MgvZJZSQ/videos).

<blockquote class="twitter-tweet" data-lang="en" width="500" align="center" right="10"><p lang="en" dir="ltr"><a href="https://twitter.com/hashtag/MIND17?src=hash&amp;ref_src=twsrc%5Etfw">#MIND17</a> summer school group photo on dartmouth green <a href="https://t.co/5yJg7atRYw">pic.twitter.com/5yJg7atRYw</a></p>&mdash; MIND Summer School (@comp_mind_ss) <a href="https://twitter.com/comp_mind_ss/status/898243028753580032?ref_src=twsrc%5Etfw">August 17, 2017</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

Also summer in Dartmouth is not too bad either!

# The verdict

If you are early in your graduate program or if you would like to get a
birds-eye view of the programming landscape used in neuroimaging analyses with a
focus on open data science, I highly recommend checking out Neurohackademy first.
If you are a more seasoned programmer or have specific topics you would like to
learn about such as spatiotemporal analyses, natural language processing, RSA,
neural decoding, and network analyses, I would highly recommend checking out MIND.
