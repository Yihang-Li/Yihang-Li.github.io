---
title: 卷积神经网络 （CNN）
tags: [Deep Learning]
categories: Notes
mathjax: true
---
## What is CNN？

前馈神经网络的一种，主要由卷积层构成，具有`局部连接（稀疏交互）`和`权值共享（参数共享）`等特性，主要用于图像识别领域

<!-- more -->

## Why CNN？

### Problems of fully connected neural networks

- 输出层的每个结点会与输入层的每个结点相连
- 参数数量随着输入图片尺寸的增大而大大增加

### Why CNN?

* 通过图片的一些pattern来识别整张图片，而识别这些pattern并不需要看整张图片
* 相同的pattern可能会出现在图片的不同位置
* 对图片进行降采样（池化）不会改变图片的特征，带来的好处是需要的参数量大大减小

## Some Basic Ideas

`卷积运算（Convolutional Operator）`

$$
s(t) = (x*w)(t) = \int x(a)w(t-a)da=\sum_{a=-\infty}^{\infty}x(a)w(t-a)
$$

第二个等号为连续情形；第三个等号为离散情形
$x$称为输入，$w$称为核函数，也叫filter，输出有时称作特征映射(feature map)

注：

- 卷积是可交换的，这是因为将核相对于输入进行翻转(flip)；
- 通常使用的互相关函数(cross-crorrelation)，和卷积运算几乎一样但是没有对核进行翻转
- 实际中，对应项相乘相加
- 在深度学习中核数组都是学习出来的：卷积层无论使用互相关运算或卷积运算都不影响模型预测时的输出

`通道（channel）` 例如对RGB图片来说，channel的数量是3（分别对应R、G、B），而对于单色图片来说channel的数量是1. 总体而言，channel可以分为下述三种：

- 最初输入图片样本的in_channel，取决于图片类型
- 卷积操作完成后输出的out_channel, 取决于卷积核的数量（此时的out_channel也会作为下一次卷积时卷积核的in_channel）
- 卷积核中的in_channel. 即上一次卷积的out_channel, 或者当第一次卷积时，为对应图片样本的channel

> 具体来说，卷积层是通过特定数目的卷积核（filter）对输入的多通道（channel）特征图进行扫描和运算，从而得到多个拥有更高层语义信息的输出特征图（通道数目等于卷积核个数）



`感受野（Receptive Field)` 对于*某层输出特征图*上的某个点，在卷积神经网络的*原始输入数据*上能影响到这个点的取值的区域

> 可以通过更深的卷积神经网络使特征图中单个元素的感受野变得更加广阔，从而捕捉输入上更大尺寸的特征

`步幅（Stride）`卷积核窗口每次滑动的行数和列数（No-Stride: Stride = 1）

`填充（Padding）`是指在输入高和宽（二维情形）的两侧填充元素（通常是0元素，zero-padding）

## Motivation

### Sparse Interactions

`稀疏交互` 卷积核尺度远小于输入的维度，每个输出神经元仅与前一层特定区域内的神经元存在连接权重（即产生交互）
注：让网络变得简单，可以缓解过拟合；物理意义：先学习局部特征，再把局部特征组合起来形成更复杂和抽象的特征

### Parameter Sharing

`参数共享` 在同一个模型的不同模块中使用相同的参数（即卷积核），卷积运算的固有属性
注：大大降低了模型的存储需求；物理意义：参数共享使得卷积层具有平移等变性（equivariance）



## Normalization

### [Batch Normalization](https://nealjean.com/ml/neural-network-normalization/)

`Covariate Shift`: 模型的训练集和测试集的分布不一致，或者模型在训练过程中输入数据的分布发生变化

对于一个复杂的机器学习系统，在训练过程中一般会要求系统里各个子模块的输入分布是稳定的，如果不满足，则称为内部协变量偏移（Internal covariant shift），网络越深，这种现象越明显

The goal of BN is to reduce `internal covariate shift` by normalizing each mini-batch of data using the mini-batch mean and variance.

采用批量归一化后，深度神经网络的训练过程更加稳定、对初始值不再那么敏感、可以采用较大的学习率来加速收敛

BN可以看作是带参数的标准化：$y^{(k)} = \gamma^{(k)}\frac{x^{(k)}-\mu^{(k)}}{\sqrt{(\sigma^{(k)})^2+\epsilon}}+\beta^{(k)}$. 其中，$x^{(k)}, y^{(k)}$分别是原始输入数据和BN后的输出数据，$\mu^{(k)}, \sigma^{(k)}$分别是输入数据的均值和标准差（在mini-batch上），$\beta^{(k)}, \gamma^{(k)}$分别是可学习的平移参数和缩放参数，上标$k$表示数据的第$k$维（BN在数据各个维度上是独立进行的），$\epsilon$是为防止分母为0的一个小量

BN 在网络中的位置有争议

## Pooling

`池化` 本质是降采样，能显著降低参数量（降维），还能保持对平移、伸缩、旋转操作的不变性。
注：池化主要是从降低计算复杂度的角度来考虑，但是当计算资源充沛的时候，有些任务不进行池化会有更好的效果，而且对另一些任务来说，是不能进行池化操作的，i.e. 下围棋，Alpha Go（详见李宏毅）

常用池化：

1. Mean Pooling（对池化窗口求均值）: 能够抑制由于领域大小受限造成的估计值方差增大的现象，特点是对背景的保留效果更好
2. max pooling（取池化窗口内的最大值）：能够抑制网络参数误差造成估计均值偏移的现象，特点是更好地提取纹理信息



## Computation

假设一个卷积层输入特征图的尺寸为$l_w^{(i)}\times l_h^{(i)}$，卷积核大小为$k_w \times k_h$，步长为$s_w \times s_h$，则输出特征图的尺寸$l_w^{(o)}\times l_h^{(o)}$ 如何计算？

Note：假设在卷积核的滑动过程中，我们对输入特征图的左右两侧分别进行了$p_w$列填充，上下两侧分别进行了$p_h$行填充，填充后的特征图尺寸为$(l_w^{(i)}+2p_w)\times (l_h^{(i)}+2p_h)$，则输出特征图的尺寸为：

$$
l_e^{(o)} = \left\lfloor\frac{l_e^{(i)} + 2p_e - k_e}{s_e}\right\rfloor + 1, e \in \{w, h\}
$$

如果输入特征图的通道数为$c^{(i)}$，输出特征图的通道数为$c^{(o)}$，在不考虑偏置项（bias）的情况下，卷积层的参数量和计算量是多少？

参数量：卷积层的参数量，主要取决于每个卷积核的参数量以及卷积核的个数。这里，每个卷积核含有$c^{(i)}k_wk_h$个参数，而卷积核的个数即输出特征图的通道个数$c^{(o)}$，因此参数总量为：

$$
c^{(i)}c^{(o)}k_wk_h
$$

计算量：卷积层的计算量，由卷积核在每个滑动窗口内的计算量以及整体的滑动次数决定。在每个滑动窗口内，卷积操作的计算量大约为$c^{(i)}k_wk_h$，而卷积核的滑动次数即输出特征图的数据个数，也就是$c^{(o)}l_w^{(o)}l_h^{(o)}$，因此整体的计算量为：

$$
c^{(i)}c^{(o)}l_w^{(o)}l_h^{(o)}k_wk_h
$$

## Typical architecture of CNN

Convo + Normal + Pooling


## BP on CNN

[或许看看这个](https://medium.com/@pavisj/convolutions-and-backpropagations-46026a8f5d2c)：The backward pass during Backpropagation also uses convolutions!






【参考】

1. [李宏毅](https://www.youtube.com/watch?v=OP5HcXJg2Aw)
2. [D2L](https://d2l.ai/chapter_convolutional-neural-networks/conv-layer.html)
3. 花书
4. 百面机器学习
5. 百面深度学习
6. 课件
7. [cs231n](https://cs231n.github.io/convolutional-networks/)

