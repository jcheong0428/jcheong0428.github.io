---
layout: post
title:  "Keeping a job alive from remote session (SSH)"
date:   2017-02-15 11:24:14 -0400
categories: jekyll update
---

# How to leave a session open so that the job
you started running on SSH continues even when you are disconnected. 

### Start a `screen` session 

```
screen
```

### Start the job you want to run 
```
python keep_it_running.py
```

### Detach `screen` using the following key combination
Press `ctr + a` then `d`


### To resume screen session 
```
screen -r
```


* The screen session can use a different bash_profile than the original computers. Source bash_profile to load the same environment. 
```
source .bash_profile
```