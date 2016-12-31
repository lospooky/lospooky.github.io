---
layout: post
title: "HowTo: Blur an Image on GPU with Torch"
---
{% include JB/setup %}

[Torch](http://torch.ch) is an amazing LuaJit/C/CUDA framework for scientific computing and it's mainly
used to develop and train neural networks.

Now, in order to blur an image we have to convolve it with a blurring kernel (also called point-spread-function).
Convolutions usually are Torch's bread and butter.<br>
Unfortunately though, the <code>image.conv2</code> function we would normally use to convolve an image with a specified kernel does not support performing the operation on GPU, using CUDA. This could be an issue since pretty much any meaningful Torch setup runs in GPU and CPU <--> GPU memory transfers are very slow.

So what to do in case we want to blur, or otherwise convolve an image with a given kernel, on GPU?

We'll just have to repurpose a <code>nn.SpatialConvolution</code> layer, normally used as a building block for convolutional neural networks, and perform a forward pass! The general procedure is as follows:

- Instantiate a <code>nn.SpatialConvolution</code> layer with input and output channels matching the number of channels in the input image, so 1 for Grayscale and 3 for RGB.

- Generate our convolution kernel, Gaussian or otherwise

- Set the <code>nn.SpatialConvolution</code> weights relative to matching input/output channels to our convolution kernel; set all other weights and biases to 0

- Move the now ready <code>nn.SpatialConvolution</code> layer to GPU

- Perform a forward pass on the input image

<br>
Here's a sample Gist for your convenience! 

<div style="margin-bottom: 20px;">
	<a data-toggle="collapse" data-target="#gist" href="#gist" class="btn btn-info"><span class="fa fa-github  fa-lg"></span>&nbsp;&nbsp;Show Gist</a>
</div>

<div class="collapse" id="gist">
	<script src="https://gist.github.com/lospooky/e4ef2c1a58b8634bfaf8d6dac4e3f6a7.js"></script>
</div>