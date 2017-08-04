---
layout: post
title:  "Using OpenFace-Cambridge Face Expression Toolbox"
date:   2017-08-03 11:24:14 -0400
categories: jekyll update
---
[OpenFace](https://github.com/TadasBaltrusaitis/OpenFace) is a powerful toolkit that provides facial landmark detection, pose tracking, action unit recognition, gaze tracking, and facial feature extractions. Here I share the simple way to setup OpenFace using a Docker container. 

# Install Docker 
Install docker at [https://docs.docker.com/docker-for-mac/install/#download-docker-for-mac](https://docs.docker.com/docker-for-mac/install/#download-docker-for-mac)

# Download the Docker container 
The Docker container is at [https://hub.docker.com/r/benbuleong/openface-cambridge/](https://hub.docker.com/r/benbuleong/openface-cambridge/) but you essentially just have to type the following command into the terminal.
```
docker pull benbuleong/openface-cambridge
```


# Run the Docker container
```
docker run -dit --name openface -v /Users/jinhyuncheong/Dropbox/facecam_exp:/opt/OpenFace/build/bin/facecam_exp benbuleong/openface-cambridge

```
-dit : 
--name : rename your running image or Docker will name it for you
-v : attach a directory for your data <Your Path>:<Docker Path>
The final argument is the name of the downloaded docker container. 


# Access the Docker container
```
docker attach openface
```

# Do some feature extraction 
```
./FeatureExtraction -f facecam_exp/Data/Jin/2_720_trimmed.MP4 -outroot facecam_exp/Data/Jin -of 2_Jin_OF.csv -q
```
See the full argument and other methods [here](https://github.com/TadasBaltrusaitis/OpenFace/wiki/Command-line-arguments)

The output file is a csv file in which Facial Action Units are listed in terms of presence and intensity. 

Action units covered are:   
| Action Units | Description |  
| --- | --- |  
| AU01 | Inner Brow Raiser |  
| AU02 | Outer Brow Raiser |  
| AU04 | Brow Lowerer |   
| AU05 | Upper Lid Raiser |  
| AU06 | Cheek Raiser |  
| AU07 | Lid Tightener |  
| AU08 | Lips toward |  
| AU09 | Nose Wrinkler |  
| AU10 | Upper Lip Raiser |  
| AU12 | Lip Corner Puller (smile) |  
| AU14 | Dimpler (contempt) |  
| AU15 | Lip Corner Depressor |  
| AU17 | Chin Raiser |  
| AU20 | Lip Stretcher |  
| AU23 | Lip Tightener |  
| AU25 | Lips part |  
| AU26 | Jaw Drop (surprise) |  
| AU45 | Blink |  

<figure>
  <img src="http://what-when-how.com/wp-content/uploads/2012/06/tmp7527313.png
" width="500">
</figure>



