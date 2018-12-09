---
layout: post
title:  "Keeping a job alive from remote session (SSH)"
date:   2017-02-15 11:24:14 -0400
categories: jekyll update
comments: true
tags: unix terminal session
---

# How to leave a session open so that the job

When using a cluster, I sometimes like to take advantage of the interactive nodes. Especially when prototyping, it is much better to debug in an interactive session rather than submitting a bunch of jobs and later realizing that it crashed. There are several options to do this such as using __screen__ or __tmux__. Screen has been around since 1987 so it can be considered the tried and true way and more stable. Tmux is a more modern solution with more bells and whistles such as multiple panes or an IDE look. I don't necessarily need a fancy solution so here is a simple tutorial on how to use screen.

### 0. Install `screen`
Screen comes with most terminals (I think) but if not you can look up how to install it for your operating system. If you already have homebrew it could be a simple `brew install screen` to install it.

### 1. Start a `screen` session
```
screen
```

You can give it a name with

```
screen -t MyScreenSession
```

### 2. Start the job you want to run
```
python keep_it_running.py
```

### 3. Detach `screen` using the following key combination
Press `ctr + a` then `d`


### 4. To resume screen session
```
screen -r
```

### 5. To list your current screen sessions.
You can make as many screen sessions as you'd like. To list them:
```
screen -ls
```


* The screen session can use a different bash_profile than the original computers. Source bash_profile to load the same environment.
```
source .bash_profile
```

# Scrolling in a screen session.
You'll notice that the scroll bar does not work like usual. To scroll in a screen session, use the following.
```
ctr + a, ESC (becomes copy mode), scroll up/down, ESC to return
```
