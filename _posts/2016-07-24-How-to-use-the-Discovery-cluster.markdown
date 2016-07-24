---
layout: post
title:  "How to use the Discovery cluster at Dartmouth"
date:   2016-07-24 11:24:14 -0400
categories: jekyll update
---
<figure>
  <img src="/assets/post05/Discovery.png" width="966">
  <figcaption><p align="right"><i>http://techdoc.dartmouth.edu/discovery/</i>
  </p></figcaption>
</figure>

Here is a quick memo on how to use the [Discovery Cluster](http://techdoc.dartmouth.edu/discovery/) at Dartmouth. 

couple ways to check jobs  

```
showq -u <username>
qstat -u <username>
myjobs -r
myqstat
```

how to submit a job  

```
qsub <job script>
qsub -N <name of job> -M <my email> -m bea -l walltime=<hh:mm:ss> <job script>
```

how to make a job script
Sample bash script that calls a python script to do things  

```
#!/bin/bash -l
#PBS -N <name of job>
#PBS -M <my mail address>
#PBS -m bea
#PBS -l nodes=1:ppn=1
#PBS -l walltime=72:00:00
cd <working directory>
module load python/2.7.11
python <python job script>
```

Sample python script that generates multiple bash scripts to distribute jobs onto separate cores (originally by [Luke Chang](cosanlab.com)).
The module designates what version of python + packages you are using. 


```
#!/usr/bin/env python
import os
import subprocess

base_dir = '/ihome/jcheong/FNL_bootstrap'

# set parameters
qp = dict()
qp['nodes'] = 1
qp['cores'] = 4
qp['walltime'] = '24:00:00'
qp['email'] = 'jcheong0428@gmail.com'
qp['logs'] = os.path.join(base_dir,'logs')

for epn in [1,2,3,4]:
# This part generates the script for each epn
    with open(os.path.join(base_dir,'Execute_Bootstrap.pbs'),"w") as text_file:
        text_file.write("#!/bin/bash -l \n\
#PBS -N bootstrap" + str(epn) + " \n\
#PBS -q default \n\
#PBS -l nodes=" + str(qp['nodes']) + ":ppn=" + str(qp['cores']) + " \n\
#PBS -l walltime=" + qp['walltime'] + " \n\
#PBS -M " + qp['email'] + " \n\
#PBS -m bea \n\
cd " + os.path.join(base_dir) + " \n\
module load python/2.7.11 \n\
python FNL_DRSA_bootstrap_episode.py " + str(epn) + " \n\
")
# This part executes the script for each epn
    subprocess.call('chmod +x ' + os.path.join(base_dir,'Execute_Bootstrap.pbs'),shell=True)
    qsub_call = 'qsub ' + os.path.join(base_dir,'Execute_Bootstrap.pbs')
    subprocess.call(qsub_call, shell=True)

```
