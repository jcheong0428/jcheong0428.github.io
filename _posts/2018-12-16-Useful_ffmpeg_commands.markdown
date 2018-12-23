---
layout: post
title:  "Useful ffmpeg commands for video processing"
date:   2018-12-16 11:24:14 -0400
categories: jekyll update
comments: true
tags: ffmpeg code videos tips python archive
---

<figure>
<div style="text-align:center">
  <img src="/assets/post20181216/ffmpeg.png" width="750">
  <figcaption><p align="right"><i><a href="movieconverter-studio.com/ PUBLIC/ffmpeg/logo-new/ffmpeg-logo-src/">Source</a></i>
  </p></figcaption>
  </div><br>
</figure>

# Useful ffmpeg commands for video processing

ffmpeg is the powerhouse for simple processing of videos.
You can use simple commands to resample, resize, cut, append, and combine videos.
You can also break a video into frames or create a video from multiple images.
Here are some basic useful commands to get started.

## Converting frame rate of videos.
```
import subprocess
command = f"ffmpeg -i {input_video} -r 30 -c:v libx264 -preset ultrafast -crf 0 {output_video}"
subprocess.call(command, shell=True)
```

## Extracting frames from a video.
Using the fps to skip frames and scaling the screenshot
```
import subprocess
command = f"ffmpeg -i {input_video} -vf fps=.5 -vf scale=320:-1 screenshot_%04d.png"
subprocess.call(command, shell=True)
```

## Create a video based on images
```
import subprocess
command = f"ffmpeg -r 30 -start_number 0 -i screenshot_%04d.png -pix_fmt yuv420p -y {output_video}"
subprocess.call(command, shell=True)
```

## Trim video
Trimming videos are tricky if you want to be super accurate, see these references:
https://stackoverflow.com/questions/12208639/ffmpeg-convert-video-from-specified-time-period-slowly/12208967#12208967
https://superuser.com/questions/499380/accurate-cutting-of-video-audio-with-ffmpeg
```
# More accurate but slower
ffmpeg -i Input.mp4 -ss 00:17:00 -t 15 -async 1 TrimmedVideo.mp4

# Less accurate but faster
ffmpeg -ss 00:00:02 -i Input.mp4 -t 00:00:30 TrimmedVideo.mp4
```

## Stitch videos side-by-side
```
ffmpeg -i LeftVideo.mp4 -i RightVideo.mp4 \
  -r 24 \
  -crf 30 \
  -filter_complex hstack \
  -preset ultrafast \
  CombinedVideo.mp4
  ```

## Concatenate videos together
```
ffmpeg -i FirstVideo.mp4 -c copy -bsf:v h264_mp4toannexb -f mpegts intermediate1.ts
ffmpeg -i SecondVideo.mp4 -c copy -bsf:v h264_mp4toannexb -f mpegts intermediate2.ts
ffmpeg -i "concat:intermediate1.ts|intermediate2.ts" -c copy -bsf:a aac_adtstoasc CombinedVideo.mp4
```

# Convert video to standard MP4 format - to read in jupyter notebook
```
ffmpeg -i Input.mov Output.mp4
```
