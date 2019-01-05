---
layout: post
title:  "Blocking Sites Using Mac Terminal"
date:   2016-06-08 11:24:14 -0400
categories: jekyll update
comments: true
tags: unix terminal
---
<div style="text-align:center">
<img src="/assets/post02/main.jpg"  width="750">  
</div><br>
<!--excerpt.start-->
This is a quick way to block sites that cause distraction in workspace.
I find it useful to stop myself from habitually checking websites at work.

### 1. Launch Terminal

### 2. Type the following to Terminal
```
sudo vim /etc/hosts
```

Type in your password when prompted.

### 3. Press `i`  and you'll be in editing mode.
At the bottom of the list, add lines in the following format:

```
127.0.0.1 www.facebook.com
127.0.0.1 facebook.com
127.0.0.1 www.youtube.com
127.0.0.1 youtube.com
```

### 4. Press `ESC` then `:` then type `wq!` to save and exit.

### 5. Lastly, flush cache by typing in the terminal:
```
sudo dscacheutil -flushcache
```

### 6. Now when you try to access Facebook, you should get this.
<figure>
  <img src="/assets/post02/No-Facebook.png" width="1300" height="300">
</figure>


This method makes your computer look for the domains you listed (e.g., facebook.com) on your local machine instead of the web.

p.s. If you want to regain access, just follow to step 3 and delete the lines you added.
