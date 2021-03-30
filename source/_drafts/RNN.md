---
title: 循环神经网络 (RNN)
tags:[Notes]
---
## What is Recurrent Neurak Network

区别于前馈神经网络

是一类用于处理`序列数据`的网络结构，输入通常是连续的、长度不固定的序列数据

注：

1. 传统的机器学习方法中，序列建模常用隐马尔可夫模型（HMM）和条件随机场（CRF）
2. 近年来，RNN应用领域：机器翻译、序列标注、图像描述、推荐系统、智能聊天机器人、自动作词作曲等
3. 就像几乎所有函数都可以被认为是前馈网络，本质上任何涉及循环（$s^{(t)}=f(s^{(t-1)};\theta)$, $s$在t时刻的定义需要参考t-1时刻同样的定义）的函数都可以视为一个循环神经网络

## Why

传统的前馈神经网络一般的输入都是一个定长的向量，无法处理变长的序列信息，即使通过处理变成定长的的向量，也很难捕捉到序列中的长距离依赖关系

RNN能够较好地处理序列信息，并能捕获长距离样本之间的关联信息，此外还能用隐结点状态保存序列中有价值的历史信息，使得网络能够学习到整个序列的浓缩的、抽象的信息

## How

> 1. Instead of making independent predictions on samples, assume the dependency among samples and make a sequence of decisions for sequential samples
> 2. It is expected that recent observations are more informative than more historical observations in predicting future values

RNN通过将神经元串行起来处理序列化的数据，每个神经元能用它的内部变量保存之前输入的序列信息，整个序列被浓缩成抽象的表示，并可以据此进行分类或生成新的序列

## Markov -> Hidden Markov -> RNN

- Markov Models assume dependence on most recent observations
  First-oder MM: $p(x_1, \dots, x_T) = \Pi_{t=1}^{T}p(x_t|x_{t-1}) $
  Second-order MM: $p(x_1, \dots, x_T) = \Pi_{t=1}^{T}p(x_t|x_{t-1}, x_{t-2})$
- HMM: hidden variables $h_t$ and observations $x_t$

  $$
  p(x_1, \dots, x_T, h_1, \dots, h_T, \theta) = p(h_1)\Pi_{t=2}^{T}p(h_t|h_{t-1})\Pi_{t=1}^{T}p(x_t|h_{t})

  $$



- RNN: Model a dynamic system driven by an external signal $x_t$: $h_t = F_{\theta}(h_{t-1}, x_t)$.
  $h_t$ contains info about the whole past sequence. The equation above implicitly defines a function which maps the whole past sequence $(x_t, \dots, x_1)$ to the current state $h_t = G_t(x_t, \dots, x_1)$.
  注：

  1. The summary is lossy. $h_t$ keeps some important aspects of the past sequence.
  2. Sharing parameters: from different time steps
  3. Share a similar idea with CNN: replacing a fully connected network with local connections with parameter sharing
  4. It allows to apply the network to input sequence of different lengths and predict sequence of different lengths

循环神经网络的设计思路和卷积神经网络类似。
为了处理序列数据，RNN设计了循环/重复的结构，这部分结构通常称为事件链
事件链间存在依赖关系，即t时刻的定义及计算需要参考t-1时刻的定义及计算

## Long Short-Term Memory

LSTM （长的短期记忆，注意这里dash - 是在short跟term之间的）

【参考】

1. [李宏毅](https://www.youtube.com/watch?v=OP5HcXJg2Aw)
2. [D2L](https://d2l.ai/chapter_convolutional-neural-networks/conv-layer.html)
3. 花书
4. 百面机器学习
5. 百面深度学习
6. 课件
