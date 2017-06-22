---
layout: post
title: On Keras, Theano, Tensorflow and Transfer Learning
---


*The thread for this issue on github can be found [here](https://github.com/fchollet/keras/issues/6989).*

If you're hearing it for the first time, [Keras](https://keras.io/) is a wrapper that is built on top of [Theano](http://deeplearning.net/software/theano/) and [Tensorflow](https://www.tensorflow.org/), two of the most popular **Deep Learning** libraries for Python. It makes it really easy to build and train Deep Neural Networks without letting us worry about a ton of different issues that might creep up while coding explicitly in Theano or Tensorflow.

Today, I was trying to compare the time taken by Python's Keras module and Matlab's [matconvnet](http://www.vlfeat.org/matconvnet/) package (which is not so widely used) to extract features for a gray-scale image from one of VGG-16's fully connected layers for **Transfer Learning** (more on this later).

**VGG-16** is one of the Deep CNN architectures used by the VGG group for the ImageNet ILSVRC-2014 competition where it won the top honors.
You can read more on that [here](http://www.robots.ox.ac.uk/~vgg/research/very_deep/). The authors have made the models and the weights trained on the ImageNet data readily available and hence VGG16 implementations can now be used out-of-the-box.

Ok, coming to the main topic of this blog post, after I ran my tests it was clear that matconvnet was behaving better than Keras running on Tensorflow Backend. So, I tried running Keras with Theano Backend and the results were even worse, but almost every [issue](https://github.com/fchollet/keras/issues/2156) out there on github pointed out that Tensorflow with Keras was slower than Theano. 

Well, so now I got down to checking which performed better on Keras on my machine, Tensorflow or Theano. I recorded how long it took for Keras to load the model, preprocess the image, and extract features from it.

First Tensorflow Backend, `$ python extractvgg.py`
```
{
  "image_data_format": "channels_last",
  "predict time": 1.0201201179997952,
  "preprocessing time": 0.012538709999716957,
  "load time": 2.1383959619997768,
  "image_dim_ordering": "tf"
}
```

Now with Theano Backend, `$ KERAS_BACKEND="theano" python extractvgg.py`
```
{
  "image_data_format": "channels_last",
  "predict time": 3.185495815000195,
  "preprocessing time": 0.016582568000103493,
  "load time": 8.029607248000048,
  "image_dim_ordering": "tf"
}
```

Clearly, Theano is slower. But there's a catch. If you observe, the `image_data_format` and the `image_dim_ordering` are set to **"channel_last"** and **"tf"** respectively, because my default backend is Tensorflow.

Now what's this?

Basically how Tensorflow reads data is *`(N, height, width, channels)` (i.e **channels_last**) and how Theano reads it is `(N, channels, height, width)` (**channels_first**) and because the backend is set to Theano but the image orderings don't match, Keras has to perform extra work shuffling the data dimensions everywhere, and maybe that's what's causing the time burden.

<sub> * N - number of images,  channels = no. of color channels </sub>
 
So I explicitly set these values.

```python
from keras import backend as K
K.set_image_data_format("channels_first")
K.set_image_dim_ordering("th")
```

Now on running `$ KERAS_BACKEND="theano" python extractvgg.py`, it should've performed better than Tensorflow, but alas, what I got was a long list of errors. 

But all works fine when I don't explicitly set the values. I haven't been able to figure out what's causing it, I'll try looking into it when I'm free over the next few days. I've raised an [issue on Github](https://github.com/fchollet/keras/issues/6989), we'll wait and see if someone can reproduce the error and find a solution,or if you can go through it and give it a try, that'd be great too!

The code to the experiments can be found [here](https://gist.github.com/kaushiksk/e6975b9afdff5bbe73e7a703c715b8c6) and I ran these on a CPU with 8GB RAM inside an anaconda environment.

And I just realized I haven't spoken about Transfer Learning here. Well, I'll try to do that in detail in the coming posts! I realize this post might seem erratic to a few but I just thought I'll put it out there. See you next time.

