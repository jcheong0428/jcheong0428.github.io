---
layout: post
title:  "Face analysis software comparison: Affectiva (AFFDEX) vs OpenFace vs Emotient (FACET)"
date:   2018-03-13 00:00:00 -0400
categories: jekyll update
comments: true
tags: emotions facial-expressions affectiva affdex openface emotient facet imotions analysis data
---

<figure>
<div style="text-align:center">
  <img src="/assets/post14/main.png" width="750">
  </div><br>
</figure>

In my previous [post](http://jinhyuncheong.com/jekyll/update/2017/10/20/Face-analysis-software-comparison.html),
I compared emotion recognition performance between OpenFace and FACET which suggested that OpenFace provides a better face detection algorithm but slightly less accurate action unit detection.

In this update, I compare Affectiva's emotion recognition algorithm ([AFFDEX](https://affectiva.readme.io/docs/using-the-emotion-sdk)) and look at how
it compares to [OpenFace](https://github.com/TadasBaltrusaitis/OpenFace) and [FACET](https://imotions.com/emotient/).

# Face Detection Success
First of all, I compare the face detection success rate of the algorithms. As mentioned in my [tutorial](http://jinhyuncheong.com/jekyll/update/2018/03/03/Tutorial_extract_fex_using_affectiva.html) on using Affectiva's JavaScript API, Affectiva offers a frame detector and a photo detector. I ran my sample video through both the frame and photo detectors at 1hz using Affectiva's most recent API Affdex 3.2.
Here are the percentages of the video that each algorithm was able to successfully detect a face.

<figure>
<div style="text-align:center">
  <img src="/assets/post14/FaceDetectionSucces.png" width="800">
</div>
</figure>

I was surprised to see that OpenFace and iMotions both outperformed Affectiva in face detection.
Affectiva's success rate of 72% meant that emotion extraction through Affectiva provided
25% less data than through OpenFace or iMotions. Although changing the parameters might improve
Affectiva's detection, the maximum frame rate I could process was 20hz but it seemed to provide worse results.

# Emotion Detection Comparison
Each software provides slightly different emotion predictions but smiling is a universal feature
that can be compared across all three algorithms. From here I only compare the results from Affectiva's frame detector as it provided the better result. Here is a simple timeseries plot
of smile detection for each algorithm (units are standardized for comparison).

<figure>
<div style="text-align:center">
  <img src="/assets/post14/timeseriescomparison.png" width="800">
</div>
</figure>

Here we can see that Affectiva definitely seems to miss out on many timepoints at which OpenFace or FACET predicts a smile. All values are standardize for plotting but it is pretty clear that Affectiva's
algorithm is less continuous than the other algorithm (Affectiva predictions are given on a 0-100 probability scale) suggesting that Affectiva might be missing the nuanced intensity differences of smiles.

<br>  

We can also zoom in on times where the predictions diverge between Affectiva and other algorithms.

<figure>
<div style="text-align:left">
  <img src="/assets/post14/detection0.png" width="350" height="380">
  <img src="/assets/post14/detection1.png" width="350" height="380">
</div>
</figure>

In the figure above, the left plots show a case in which Affectiva detects a smile while OpenFace and FACET predict a moderate smile. This is in contrast to the plot on the right which is clearly a full blown smile that is detected by OpenFace and FACET. Affectiva, does seem to get it right but for some reason cannot continuously detect the smile surrounding the instance.

<br>  

We can also look at times when Affectiva is missing detections.

<figure>
<div style="text-align:left">
  <img src="/assets/post14/nodetection0.png" width="350" height="380">
  <img src="/assets/post14/nodetection1.png" width="350" height="380">
</div>
</figure>

In the left plot, Affectiva fails to detect a face and a smile while a
moderate smile is detected by OpenFace and FACET. In the plot on the right,
Affectiva once again misses out on a subtle smile that OpenFace and FACET detects.


Also see [this article](http://www.schulte-mecklenbeck.com/wp-content/uploads/2017/12/Stockli2017.pdf) in Behavior Research Methods that also compares Affectiva to FACET.

### Conclusion
Overall, it seems clear that Affectiva's JavaScript API does not perform
as well as OpenFace or FACET in emotion and facial expression detection.
This is surprising given that Affectiva is known to have been trained on
much more data and naturalistic data compared to OpenFace or FACET.
However, one possibility is that the way I use their API as outlined in my [tutorial](http://jinhyuncheong.com/jekyll/update/2018/03/03/Tutorial_extract_fex_using_affectiva.html)
may not be the optimal way to use their algorithm.

If you are interested in validation of the algorithms, check out this [paper by Stockli and colleagues (2017)](https://link.springer.com/article/10.3758/s13428-017-0996-1) in Behavior Research Methods. They also find that the FACET engine outperforms Affectiva's AFFDEX.
