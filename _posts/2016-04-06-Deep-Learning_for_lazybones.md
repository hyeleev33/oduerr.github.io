---
layout: post
title: "Deep learning for lazybones"
---

In this blog I explore the possibility to use a trained CNN on one image dataset (ILSVRC) as feature extractor for another image dataset (CIFAR-10). The code using TensorFlow can be found at [github](https://github.com/oduerr/dl_tutorial/tree/master/tensorflow/inception_cifar10).


## Background
Image classification has made astonishing progress in the last 3 years. While 2012 a computer could hardly distinguish a cat from a dog, things have dramatically changed after [[Alex Krizhevsky et al., 2012]](https://papers.nips.cc/paper/4824-imagenet-classification-with-deep-convolutional-neural-networks) won the Imagenet classification competition using a deep learning method called convolutional neural network (CNN). 

They achieved a top-5 error rate of 17% compared to 26% of the second best approach. While back in 2012 one had to be a hard core C++ programmer to build and train these models, recently several new deep learning frameworks have emerged: e.g. Theano, Lasagne, Torch. They make it possible to use deep learning if you know just some python. In November 2015 Google released their own framework called TensorFlow with much ado. Recently, a network termed inception-v3 trained on the ILSVRC-2012 dataset has been made publicly available for TensorFlow [[Szegedy et al, 2015]](http://arxiv.org/abs/1512.00567). This network achieves an astonishing top-5 error rate of 3.6%. Given the fact that humans achieve approximately 5% (partly due to the 120 different breeds of dogs in the data set) this is very amazing. 



## The test data:

![My helpful screenshot]({{ site.url }}/imgs/dl_lazybones/cifar_examples.jpg)

Note that the ‘inception v3’ can deal with images of size 299x299, without the need of downscaling them. So using the tiny 32x32 images from CIFAR-10 is quite an overkill.  

## Extracting the features using the CNN network


<tt>
	(venv)dueo@deepfish:~/dl_tutorial/tensorflow/inception_cifar10 python cifar-10_experiment.py 
</tt>

This script will download the TensorFlow model and does the feature extraction for the CIFAR images. This script needs about 0.03 seconds per image on a Titan X GPU, and on my 13’’ Late-2013 MacBook Pro it takes approximately 0.5 seconds on the CPU. I guess the code could be further enhanced using batches instead of feeding in one image after another, but I want to keep it simple.

## Doing the classification


If we compare this with the [state-of-the-art results](http://rodrigob.github.io/are_we_there_yet/build/classification_datasets_results.html), the 87% we achieve are not bad at all, considering the fact that we did not need to train the network but just a SVM. If you used, for example HOG-features, you would get about 59%. In my opinion this shows that using hand-crafted features like HOG does not make much sense anymore, we reduced the error from 41% down to 23% by simply using a trained network for feature generation. For illustration, here are some examples for which we have been wrong, some of these mistakes are quite obvious:

![My helpful screenshot]({{ site.url }}/imgs/dl_lazybones/cifar_misclassified.jpg)

## Bonus: t-SNE visualization


<a href="https://dl.dropboxusercontent.com/u/9154523/blog/inception_cifar10/outfile_3000.jpg">
<img border="0" alt="W3Schools" src="{{ site.url }}/imgs/dl_lazybones/combine.jpg">
</a>
