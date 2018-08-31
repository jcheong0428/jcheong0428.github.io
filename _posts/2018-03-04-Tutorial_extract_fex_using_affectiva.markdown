---
layout: post
title:  "Affectiva JavaScript Emotion SDK Tutorial"
date:   2018-03-03 11:24:14 -0400
categories: jekyll update
comments: true
tags: affectiva facial-expressions emotions analysis data
---
[Affectiva](https://www.affectiva.com/) is one of the leaders in emotion recognition technology.
And they generously provide a version of their emotion recognition software (Affdex) in the form of [Emotion SDKs](https://affectiva.readme.io/docs/using-the-emotion-sdk). This means that people can use their patented technology to develop new apps and websites.
They also have [sample apps](https://github.com/Affectiva/js-sdk-sample-apps)
using the JavaScript SDK such as analyzing a [photo](https://jsfiddle.net/affectiva/h6p64vwg/show/)
or a [camera stream](https://jsfiddle.net/affectiva/opyh5e8d/show/).

Researchers can also use their SDK to extract emotion predictions from videos we recorded in lab settings for further analyses. This tutorial is designed to help those with a similar purpose.

To do this, I wrote a short html file that calls the Affectiva JavaScript Emotion SDK
to extract emotion information from a video stored locally on my computer.
Opening this html file, the app will automatically extract emotion features from the video
and will save the results to my computer.

<br>
# Step 1
Download and unzip the current version of the affectiva-api-app by clicking [here](https://github.com/cosanlab/affectiva-api-app/archive/master.zip) or
clone the [repository](https://github.com/cosanlab/affectiva-api-app).
```
git clone https://github.com/cosanlab/affectiva-api-app.git
```
<br>

# Step 2
Move the video file you would like to process into the `affectiva-api-app/data` folder.  
If you downloaded the zip file, your path would most likely be `Downloads/affectiva-api-app-master/data`  
For simplicity, let's say my video file is called `sample_vid.mp4` which is
already in the data folder.  
<br>  

# Step 3
Open `affectiva_emotion_detector.html` in your favorite text editor (e.g., Atom, Sublime)
Scroll down to line 35, and change `filename` with the name of your video.
To process the sample video, we would leave it as `data/sample_vid.mp4`
<img src="/assets/post13/img1.png" width="715">  
<br>

# Step 4
Modify additional parameters.
Below the filename, you can also adjust the parameters of the app such as
where in the video you want to start/stop processing and how fine-grained
you want the processing to be.  
In this example, we start processing at time 0 (secs) increment by 100ms
(sec_step) until the end of the video.
<img src="/assets/post13/img2.png" width="715">  
<br>

# Step 5
To run this html app, you need to start a webserver on your computer.
On MACs with Python installed, this is a simple process.
Open up [Terminal](https://www.wikihow.com/Open-a-Terminal-Window-in-Mac) and navigate to the 'affectiva-api-app' folder.  
If the folder is in your downloads you can type the following in your Terminal.
```
cd
cd Downloads/affectiva-api-app
```

If you have Python already installed you can start the webserver using this command
```
python -m SimpleHTTPServer 8000
```
If this command replies with `Serving HTTP on 0.0.0.0 port 8000 ...` then you are good to go.  
If this part is not working, you may need to install Python on your system
(I recommend downloading using [Anaconda](https://www.anaconda.com/download))  
<br>

# Step 6
Open your favorite web browser (e.g., Chrome, FireFox) and type in the address line
```
http://localhost:8000/
```
Which should now give you a list of files in that directory
<img src="/assets/post13/img3.png" width="500" border="1">
<br>
# Step 7
Now click on `affectiva_emotion_detector.html` and it will begin the extraction
and auto download the results in a json file as shown in the image below.
<img src="/assets/post13/img4.png" width="500" border="1">  
<br>

# Step 8
Load the emotion predictions and play around!
```
import pandas as pd
df = pd.read_json('~/Downloads/data_sample_vid.json',lines=True)
```
<br>

# Analysis
One *VERY* important thing to keep in mind is that the output does not
save data when face detection fails. Therefore, I highly recommend resetting
the index to make sure you know which timepoints have missing data.

This is easy to do in pandas with the following
```
import pandas as pd
import numpy as np
df = pd.read_json('~/Downloads/data_sample_vid.json',lines=True)
df.index= df.Timestamp.astype(float)
newindex = np.arange(0,df.Timestamp.max(),.1)
df = df.reindex(newindex,method='nearest',tolerance=.0001)
```

In the sample video, I rotated my head left and then right so there is definitely
missing data at those times because face detection often fails on profile views.
This is apparent when I plot one of the predictions such as attention
Which shows missing data around the 5th and 11th second when my head is rotated.
<figure>
<div style="text-align:center">
<img src="/assets/post13/img5.png" width="400">
</div></figure>
Here is the full list of features that the app currently outputs
```
Timestamp
anger
attention
browFurrow
browRaise
cheekRaise
chinRaise
contempt
dimpler
disgust
engagement
eyeClosure
eyeWiden
fear
innerBrowRaise
jawDrop
joy
lidTighten
lipCornerDepressor
lipPress
lipPucker
lipStretch
lipSuck
mouthOpen
noseWrinkle
sadness
smile
smirk
surprise
upperLipRaise
valence
```

# Conclusion
Obviously, this app can be improved with an option for batch processing and GUIs.  
Please let me know if you would like to help out and making the app more
user friendly!
