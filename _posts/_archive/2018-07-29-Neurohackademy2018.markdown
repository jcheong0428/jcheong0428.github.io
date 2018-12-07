---
layout: note
title:  "What I learned from Neurohackademy and MIND (Methods in Neuroscience at Dartmouth)"
date:   2018-07-29 11:24:14 -0400
categories: jekyll update
---

# Day 0 - 2018.07.29
7:30am woke up. ran to Gas Works Park and Fremont. Explored neighborhood.
11am took Link Light Rail with Scott Cole to downtown.
Scott works with Brad Voytek on oscillation signals, authored pacpy package.
Question about using phase coupling methods on fMRI data.
3:30pm got back to campus
6:15pm went to DinTaiFung with James Kent and Daniel Low.
James from Iowa U works with old people exercise intervention for cognitive
improvement and fMRI. Daniel Low been to Trento (Jim Haxby) but works in
Netherlands with NLP and brain imaging for sentence coding/decoding models.
9pm met Peer and Aurelio. Peer from Germany but really likes Germany.

# Day 1 - 2018.07.30
Christian - UofW, functional parcellation with HCP resting state and DTI.
package that does nonparametric parcellation
Can use this parcellation on a subject level, compare number of parcellation
across subjects across tasks. 	

meet DR Jeffrey Cheong who had to retract three science papers.

Chris - Docker, made mydockerimage
Testing
https://neurohackweek.github.io/software-testing-for-scientists/
singularity shell docker://jcheong0428/mydockerimage

Peer told me about:
openknowledgemaps.org
scholia
deepneuron

Claudio Toro - BU Joe Mcguire
vmPFC slow scale.

Lee - from freesurfer labs, freesurfer getting FDA approved for usage in hospitals.

# Day 2
## Fernando Perez - Jupyter notebook creator.
Jupyter = julia python and r was the first.
built a platform, protocol, document it, others buitl on top of it.
jupyter homage to astronomy that funded and gave credence to python.
galileo - built notebooks
Cython - highlights of where C code is created.

sklearn neural network
http://scikit-neuralnetwork.readthedocs.io/en/latest/guide_model.html

reproducible research
An article about computational science in a scientific publication is not the
scholarship itself, it is merely advertising of the scholarship. The actual
scholarship is the complete software development environment and the
complete set of instructions which generated the figures.
Buckheit and Donoho, WaveLab and Reproducible Research 1995

Turning this into a way you work. do it from day 1 .
https://github.com/berkeley-stat159-f17/stat159-f17
should be a pattern: version control: git, github; programming: python;
process automation: make; data analysis: np, pd, mpl, nltk ,sklearn;
documentation: sphinx; software testing(untested code is broken code): pytest;
continuous integration: Travis; reproducible containers: Binder.

computational hygiene : a daily habit; github classroom.
https://classroom.github.com/
Using Make.
https://swcarpentry.github.io/make-novice/
Ligo Binder
https://github.com/minrk/ligo-binder

## JB Poline - Numpy tutorial
`np.full((3,3),np.nan)`

## Image Processing Tutorial
skimage -
feature.hog - to process hog
edge detection and hough_circle tried but doesn't work well on cell images.

nitime : timeseries analysis in nipype by Ariel

## Ariel - high performance computing.
```
%%timeit
```

```
pip install line_profiler
%load_ext line_profiler
%lprun -f functionname functionname()
```

Cython library.
```
%%cython
def my_poly(double a, double b):
	return 10.5 ** a + (b**2)
```

```
from distutils.core import setup
from distutils.extension import Extension
from Cython.Distutils import build_ext
ext = Extension("fib",sources=["fib.pyx"])
setup(ext_modeules=[ext], cmdclass={'build_ext':build_Ext})
```

```
%%cython -a
some code
```
the more yellow it is, c doesn't know what to do with it
will be slow

numba:
```
from numba import jit
jit(fib)
```

dask to speed up file loading.
https://dask.pydata.org/en/latest/

## Poster session
mihael - global signal is respiration + hr
you can still regress out despite sampling rate concern.


# Day 3
## Deep learning with KERAS
https://neurohackademy.github.io/convolutional-neural-networks/

Use npz format instead of csv!
http://www.dinesh.xyz/benchmarking-data-formats.html

## Sklearn tutorial
function?? -> shows code
https://jakevdp.github.io/PythonDataScienceHandbook/

radial basis function using kernel trick, SVM calculation over large dimension.
https://en.wikipedia.org/wiki/Kernel_method
C : small c value let margins bleed into distribution.
large C value tight margin between clusters.


## Anisha web technology
mindbogle
afq-browser
https://yeatmanlab.github.io/AFQBrowser-demo/?table[prevSort][count]=2&table[prevSort][order]=ascending&table[prevSort][key]=&table[sort][count]=2&table[sort][order]=ascending&table[sort][key]=
MindControl-herokuapp

brainderless
draw.io
http://anisha.pizza/nha2018_web/

# Day 4
## Cloud computing.
https://openmri.github.io/ocra/
sudo apt install awscli
aws s3 mb s3://neurohack-jcheong0428
aws s3 ls s3://neurohack-jcheong0428
aws s3 cp s3://neurohack-amandatan s3://neurohack-jcheong0428

## Gael Varoquaz
sklearn, joblib, nilearn. mayavi.
choose battle - where to innovate.
win them : manage complexity.
Debugging is twice as hard as writing code in the first place.

## Satra
https://www.reprozip.org/
neurodocker
https://github.com/kaczmarj/neurodocker

# Day 5
## Eva Dyer
PCA, Manifold.
Decoding Movement
https://github.com/KordingLab/DAD

## Allen Brain Institute
https://www.brain-map.org/
https://github.com/AllenInstitute/AllenSDK

## JB Poline
shown p <.05 what has been shown?
h0 false, h1 true, h0 prob false, h1 prob true
None of the above: - correct answer.

Westover 2014.

# Day 6 - Week 2 - Monday
## Gael Varoquaz - encoding/decoding ML in neuroscience
### Encoding
CNN networks explain the visual system pretty well.
not the exact biological replica, but still a good model.
(we don't know what activation function brain uses etc)
Yamins 2014 - Jim Dicarlo group - Gael's favorite paper.
Eickenberg ... 2017 -
higher CNN layer explains higher order brain regions.

### Decoding
Reverse inference fallacy
all dogs eat meat, gael eats meat, therefore gael is a dog.

find regions that predict observed cognition.

DM(brain)  x coefficients (brain map) = Target.

gramfort et al 2013
Add in a implicit prior of the region - total variation lasso.
TV-L1 predicts better, and encompass face region.
Different decoders give different regions - ill posed problem.

Hoyos-Idrobo 2017.
Ensemble model - slightly better.

right tool for right question: beyond encoding versus decoding dichotomy
https://arxiv.org/abs/1704.08851

Varoquaz 2017 - cross validation can also fail - not enough data
binomial n=30 is fat. width go down as number of samples increase.
when n=30 LOO accuracy +- 20% : nothing you can do about it.
larger sample - you get smaller prediction accuracy b/c heterogeneity.

### Rest fMRI
population imaging - 100,000 subjects with resting state.
research agenda of resting state fmri: grounding inter-subject psychology on brain measurements.
challenge - no salient features in resting fmri. structured brain noise.

lifestyle to brain activity - Smith 2015
autism - 2017

k-means clustering. nilearn.
Ward agglomerative clustering.
Decomposition models
matrix factorization - decomposition model ICA (seek independence of maps),
sparse dictionary learning.

functional better than structural.
BASC functional atlas - suggested by Gael if not using sparse dictionary.

Parametrizing connectivity. - Tangent space embedding
varoquaz 2010 - large lesion create different connectivity.

# Sanmi Koyejo - synthetic fMRI data.
https://openreview.net/pdf?id=BJaU__eCZ
training set image ->
two CNNs, random noised -> generator -> fake image
Both go in to the discriminator categorize real or fake.
https://www.quora.com/What-are-some-recent-and-potentially-upcoming-breakthroughs-in-deep-learning
GANs for fMRI
BrainPedia - neurovault

Implicit generative models are difficult to evaluate.
inception score.
issues:
https://arxiv.org/pdf/1710.10196.pdf?__hstc=200028081.1bb630f9cde2cb5f07430159d50a3c91.1524009600081.1524009600082.1524009600083.1&__hssc=200028081.1.1524009600084&__hsfp=1773666937

https://github.com/BlissChapman/ICW-fMRI-GAN
https://github.com/BlissChapman/SyntheticStatistics


# BrainHack -Cameron Craddock
http://www.brainhack.org/
http://academickarma.org/openreviews
http://fcon_1000.projects.nitrc.org/indi/enhanced/
http://fcon_1000.projects.nitrc.org/indi/cmi_healthy_brain_network/
https://www.biorxiv.org/content/early/2018/06/18/347765
http://preprocessed-connectomes-project.org/abide/
PEER - eyetracking fmri
https://www.biorxiv.org/content/early/2018/06/18/347765

# Softwares
https://help.github.com/articles/removing-sensitive-data-from-a-repository/
OpenNeuro, fmriprep, mriqc: Russ Poldrack, analyzing, sharing neuroimaging data.
neuroscout: Tal Yarkoni, reproducible analyses.
http://grantome.com/: search NIH grants
pavlovian - psychopy experiment scripts.
http://open.semanticscholar.org/

# Hackathon Ideas
blockchain citations.
parcellation pattern to results.
commenting on DOI, follow papers.
crown cell counting.
Create a FACS json - interactive FACS viewer.

# Research Ideas
Tal's paper of blogs, use word predicting personality with eating disorder data.
