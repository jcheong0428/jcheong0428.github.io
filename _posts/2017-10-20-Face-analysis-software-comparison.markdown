---
layout: post
title:  "Face analysis software comparison: OpenFace vs iMotion-Emotient (FACET)"
date:   2017-10-20 11:24:14 -0400
categories: jekyll update
comments: true
tags: openface imotions emotient facet facial-expressions emotions
---

# Facial expression analysis software comparison: OpenFace vs iMotions-Emotient (FACET)

This post provides a comparison between two facial expression analysis platforms OpenFace and iMotion-Emotient (FACET engine).   
OpenFace is a open source platform available at [https://github.com/TadasBaltrusaitis/OpenFace](https://github.com/TadasBaltrusaitis/OpenFace), see [previous post](http://jinhyuncheong.com/jekyll/update/2017/08/03/Using-OpenFace-Cambridge-Face-Expression-Toolbox.html) for help on how to use the OpenFace expression software on Dockers.   

[iMotions](https://imotions.com/) provides two facial expression engines Emotient and AFFECTIVA.  

Here we compare face detection and facial expression extraction performance of the two softwares.   

#### TLDR:
iMotions Emotient (FACET) seems to provide a more accurate extraction of Action Unit features than OpenFace.   
However, OpenFace has a superior face detection algorithm and can possibly be improved in AU extractions with further training.  


# Comparison of face detection on rotating head.
First, I compare the Face Detection performance of two softwares.  
I recorded a video in which a face was rotated 90 degrees to the left, returned to center, then 90 degrees to the right, then returned to center.  
OpenFace was much better in face detection than FACET (**OpenFace 92% success vs FACET 58% success**).  
As can be seen in the left panel of graph below, FACET shows much more face detection dropouts compared to OpenFace.  
This suggests that OpenFace is far superior in detecting faces on profile views compared to FACET.

<figure>
  <img src="/assets/post10/fig1.png" width="1200">
</figure>

One thing to note however is that OpenFace success does not necessarily mean that it's doing a perfect job.   
The OpenFace provides confidence measure for its face detection success showing that confidence does suffer in profile head views.

<figure>
  <img src="/assets/post10/fig2.png" width="1200">
</figure>

The importance of face detection is that if the algorithm cannot find a face, it cannot provide facial expression predictions.

In OpenFace, face detection failure does not provide NAN values for predictions but instead 0s.   
Therefore one must be careful when interpreting the absence of AUs as a true absence or a result of face detection failure.   
This is represented around frame 150 in the graph below.

In contrast, FACET records NAN values for AU predictions when it cannot identify a face.
This is represented by the discontinued green lines between frames 100 - 200 in the plot below.

<figure>
  <img src="/assets/post10/fig3.png" width="1200">
</figure>

### Conclusion
OpenFace provides a better face detection algorithm.

# AU predictions in genuine facial expressions of emotion

Here I compare the AU predictions between OpenFace and FACET on a facial expression recording we conducted while a participant watched a TV drama.
OpenFace provides both AU intensity (e.g., AU12_r) and AU presence (e.g., AU12_c).  
The intensity is on a **5 point scale** (0:not present to 5:maximum intensity) representing the degree of activation.
AU presence indicates whether an AU is visible on the face as a binary outcome.

FACET deprecated providing intensity or probabilities of AUs in recent versions and now provides Action Unit Evidence (e.g., AU12 Evidence).  
The evidence represents a **logarithmic odds of an action unit or expression being present** and can be considered as Z-scores on which statistical analyses should be performed. The Z-scores can be baselined and/or be transformed to probabilities using the following softmax function:

$$ P(AU) = \frac{1}{ 1 + 10 ^ {Evidence}} $$

With these differences in mind, we can compare how similar the AU predictions are between the two software.

First, comparing the AU prediction similarity shows that the *prediction similarity between the two softwares is low* (mean r = .28; blue bars on figure below). This however includes the average negative correlations of AU17 (Chin Raiser). **The more widely used and distinct expressions AU10 (upper lip raiser), AU12 (lip corner puller), AU23 (lip tightener), AU25 (lips part) are more correlated around r = .5**.   

Low correlations could result from that they operate on different scales.  
OpenFace predictions have a minimum value of 0 while FACET provides continous predictions.   
Recalculating the correlations after removing the bottomed-out zero values does not result in huge correlation increases (green bars on bottom graph).  

However **converting FACET's AU Evidence to Probabilities using a softmax function increases the correspondence between the two prediction algorithms to r = .6 for AUs 10, 12, and 25.**  

<figure>
  <img src="/assets/post10/fig4.png" width="1200">
</figure>

We can also compare the facial expression predictions from each software with the actual facial expression images.
Here is a participant smiling at two different timepoints which provides a few interesting insights.

## Comparing AU12 (Lip Corner Puller)
AU12 prediction is important because it is indicative of a smile and experience of joy.  

Comparing the two instances of AU12 activation at 17:10 (left image) and at 20:00 (right image), **FACET seems to provide a more accurate characterization of activity** as the lip corners are more pulled in the left image (at 17:10) than on the right image (at 20:00).   

<figure>
  <img src="/assets/post10/fig5.png" width="500" align="left">
  <img src="/assets/post10/fig6.png" width="500" >
  <figcaption><p align="left"><i>Images generated using <a href="https://github.com/jcheong0428/facesync">FaceSync</a>; OpenFace y-axis is intensity and the FACET y-axis is the probability of activation.</i></p>
  </figcaption>
</figure>


## Comparing AU17 (Chin Raiser):   
Looking at the left image (at 19:39) in which the chin raise is clear, both algorithms accurately predict AU17 activation.

However in the right image (at 10:30), **OpenFace detection is more noisy** and suggests the chin is raised whereas FACET provides a more stable prediction and does not predict a chin raise which seems correct. Also the tonic increase in AU17 predictions at around 35,000 frame is also puzzling and seem to be less stable than FACET's predictions.  

<figure>
  <img src="/assets/post10/fig7.png" width="500" align="left">
  <img src="/assets/post10/fig8.png" width="500" >
   <figcaption><p align="left"><i>Images generated using <a href="https://github.com/jcheong0428/facesync">FaceSync</a>; OpenFace y-axis is intensity and the FACET y-axis is the probability of activation.</i></p>
   </figcaption>
</figure>


# Conclusion
1. OpenFace has better face detection and can provide data even for profile view of faces.
2. FACET, however, seems to have a more accurate representation of Action Unit activations.

# Coming next
1. Comparison with Affectiva.
2. Training OpenFace with additional data.
