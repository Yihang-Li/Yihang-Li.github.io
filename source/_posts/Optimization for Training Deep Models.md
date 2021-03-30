---
title: SGD 和它的兄弟们
tags: [Notes]
---
## SGD 和它的兄弟们

`Batch Gradient Descent`（正常的梯度下降）正常下山

`SGD` 用单个训练样本的损失来近似平均损失    蒙着眼睛下山 （害怕山谷（震荡）和鞍点（停滞））

`Mini-Batch SGD` 为了降低随机梯度的方差，使得迭代算法更加稳定，也为了充分利用高度优化的矩阵运算操作，同时处理若干个（一个小批量）的数据

<!-- more -->

改进随机梯度下降法：`惯性保持` `环境感知`

`Momentum` ： 惯性的获得是基于历史信息 （SGD在山谷震荡、鞍点停止，而加上惯性之后，有助于打破这一困境）

$$
v_t = \gamma v_{t-1} + \eta g_t

$$


$$
\theta_{t+1} = \theta_t - v_t

$$

前进步伐$-v_t$由两部分组成：一是学习率$\eta$乘以当前估计的梯度$g_t$；二是带衰减的前一次步伐$v_{t-1}$


`AdaGrad`:  感知环境才能更好地前进 (根据自变量在每个维度的梯度值大小来调整各个维度上的学习率，从而避免统一的学习率难以适应所有维度的问题)

分量形式：

$$
\theta_{t+1, i} = \theta_{t, i} - \frac{\eta}{\sqrt{\sum_{k=0}^{t}g_{k,i}^2+\epsilon}}g_{t,i}

$$

其中$\theta_{t+1,i}$表示t+1时刻的参数向量$\theta_{t+1}$的第i个参数，$g_{k,i}$表示k时刻的梯度向量$g_k$的第i个维度（方向）

向量形式：

$$
s_t = s_{t-1} + g_t \odot g_t

$$


$$
\theta_t = \theta_{t-1} - \frac{\eta}{\sqrt{s_t+\epsilon}}\odot g_t

$$

其中$\odot$表示按元素相乘，$\eta$是学习率，$\epsilon$是为了维持数值稳定性而添加的常数，这里开方、除法、乘法的运算都是按元素运算的


`RMSProp`： 学会放下，以后才能更好的拿起   (Adagrad + forgetting, Adagrad 一直在按元素累加过去梯度的平方，始终不肯放下，导致在迭代后期学习率过小，可能较难找到一个有用的解，RMSProp学会了放下，带来了改进)

$$
s_t = \gamma s_{t-1} + (1-\gamma)g_t \odot g_t

$$


$$
\theta_t = \theta_{t-1} - \frac{\eta}{\sqrt{s_t+\epsilon}}\odot g_t

$$

因为RMSProp算法的状态变量是对平方项$g_t\odot g_t$的指数加权移动平均，所以可以看作最近$\frac{1}{1-\gamma}$个时间步的小批量随机梯度平方项的加权平均。这样就使得自变量每个元素的学习率在迭代过程中就不再一直降低（或不变）

注：RMSProp源自Coursera上Hinton的一门课


（`AdaDelta`：参加D2L（没有学习率超参数，通过使用有关自变量更新量平方的指数加权移动平均的项来替代RMSProp算法中的学习率））


`Adam`：历史、环境，我都要！（RMSProp + Momentum）

$$
v_t = \beta_1 v_{t-1} + (1-\beta_1) g_t

$$


$$
s_t = \beta_2 s_{t-1} + (1-\beta_2)g_t \odot g_t

$$


$$
\theta_t = \theta_{t-1} - \frac{\eta}{\sqrt{s_t+\epsilon}}\odot v_t

$$

注：还有偏差修正：参见D2L


【参考】

1. [李宏毅](https://www.youtube.com/watch?v=OP5HcXJg2Aw)
2. [D2L](https://d2l.ai/chapter_convolutional-neural-networks/conv-layer.html)
3. 花书
4. 百面机器学习
5. 百面深度学习
6. 课件


Note：Vanilla 一般的
