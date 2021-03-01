---
title: CNN
tags: [Deep Learning]
categories: Notes
mathjax: true
---
`卷积神经网络`：含有卷积层的神经网络

* Translation invariance in images implies that all patches of an image will be treated in the same manner.
* Locality means that only a small neighborhood of pixels will be used to compute the corresponding hidden representations.
* In image processing, convolutional layers typically require many fewer parameters than fully-connected layers.
* CNNS are a special family of neural networks that contain convolutional layers.
* Channels on input and output allow our model to capture multiple aspects of an image at each spatial location.

在深度学习中核数组都是学习出来的：卷积层无论使用互相关运算或卷积运算都不影响模型预测时的输出

可以通过更深的卷积神经网络使特征图中单个元素的感受野变得更加广阔，从而捕捉输入上更大尺寸的特

`填充（Padding）`是指在输入高和宽（二维情形）的两侧填充元素（通常是0元素，zero-padding）

`步幅（Stride）`卷积核窗口每次滑动的行数和列数（No-Stride: Stride = 1）
