---
layout: post
title:  "Code repo for Git"
date:   2016-12-15 11:24:14 -0400
categories: jekyll update
comments: true
tags: git github
---

<div style="text-align:center">
<img src="/assets/post20161215/main.jpg" width="750">  
</div><br>

# creating a new branch for making pull requests

```
git stash

git branch <branch name>

git checkout <branch name>

git push origin <branch name>

git checkout master
```

# Updating toolbox on PyPi
Info from [http://peterdowns.com/posts/first-time-with-pypi.html](http://peterdowns.com/posts/first-time-with-pypi.html)

```
# change tag version to upload to pip
git tag 0.7 -m "Adds a tag so that we can put this on PyPI."

# change __version__ and tarball version on setup.py
sublime setup.py
git push --tags origin master
python setup.py register -r pypitest
python setup.py sdist upload -r pypitest
python setup.py register -r pypi
python setup.py sdist upload -r pypi
```
