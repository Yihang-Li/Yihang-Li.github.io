---
title: Notes about Focal Loss
date: 2021-09-02T14:30:49+08:00
tags:
---
# Notes of Focal Loss

`youngyhli`

## What，Why and How?

### What

Focal Loss：Kaiming He 团队在论文[《Focal Loss for Dense Object Detection》](https://arxiv.org/abs/1708.02002)基于目标检测场景提出的损失函数，用于解决分类问题中类别不平衡、分类难度差异的问题

> 虽然是基于目标检测场景提出的，但是在NLP等其他任务中，也存在着大量的类别不平衡、正负标签比例差异过大的问题


<!--more-


### Why

一方面，标签失衡使得占比大的样本占据主导（表现为梯度大多数时候是基于那些占比大的样本的更新）

另一方面，从分类难易程度来说，对于一个问题，大多数样本都是简单易分的，难分的只占少数，从而导致easy problem dominating问题（即大多数简单样本对loss起主要贡献，模型难以关注到少量的难分样本）

### How

Focal Loss正是基于此出发，降低easy samples的比重，使得模型更多地关注到hard sample上

> 在此之前的思路大都集中在采样样本，使hard sample在数据中所占的比重更大，而Focal Loss没有进行采样，选择了按照loss对easy sample降权的处理

## Details

### CE Loss

对于分类问题，常采用CE Loss：(以二分类为例)

$$
\operatorname{CE}(p, y) = -log(p) \text{ if } y=1 \text{ else } -log(1-p)
$$

> In the above, $y \in \{-1, +1\}$ specifies the ground-truth class and $p \in [0, 1]$ is the model's estimated probability for the class with label $y=1$

为了后续描述的方便，这里定义$p_t = p \text{ if } y=1 \text{ else } 1-p$ , 从而有$\operatorname{CE}(p, y) = \operatorname{CE}(p_t) = -log(p_t)$

> 实际上$p_t$反映了$p$与$y$的接近程度，$p_t$越大，说明分类越好

### First Version

最初作者通过加上一个参数$\alpha$来平衡CE，对于占比小的样本增大该参数即可：

$$
\operatorname{CE}(p_t) =-\alpha_tlog(p_t)
$$

这样虽然能平衡正负样本，却没有能处理难易样本的区分

> While α balances the importance of positive/negative examples, it does not differentiate between easy/hard examples.

### Focal Loss Definition

为了能够处理难易样本的区分，作者在CE上加入了另一个因子，可以动态调节权重：

$$
\operatorname{FL}(p_t) = -(1-p_t)^\gamma log(p_t)
$$

$\gamma=0$时退化为CE Loss，作者通过实验表明，$\gamma=2$时，一个样本被分类的$p_t=0.968$对应的Focal Loss比CE Loss小1000多倍，从而增加了那些误分类的重要性。

### Final Version

最后作者把上述两个参数结合起来，

$$
\operatorname{FL}(p_t) = -\alpha_t(1-p_t)^\gamma log(p_t)
$$

得到了最后的公式，并指出：

> In general, $\alpha$ should be decreased slightly as $\gamma$ is increased (for $\gamma=2, \alpha=0.25$ works best)

## Implementation

udf:

```python
def focal_loss(gamma=2., alpha=0.25):

    def focal_loss_fixed(y_true, y_pred):
        bce = tf.losses.binary_crossentropy(y_true, y_pred)
        p_t = (y_true * y_pred) + ((1 - y_true) * (1 - y_pred))
        alpha_factor = y_true * alpha + (1 - y_true) * (1 - alpha)
        modulating_factor = tf.pow(1.0 - p_t, gamma)
        loss = tf.reduce_sum(alpha_factor * modulating_factor * bce,axis = -1 )
        return loss
    return focal_loss_fixed
```

pkg:

[https://focal-loss.readthedocs.io/en/latest/](https://focal-loss.readthedocs.io/en/latest/)

## Reference

[Focal Loss for Dense Object Detection](https://arxiv.org/abs/1708.02002)

[Focal Loss以及其在NLP领域运用的思考](https://ldzhangyx.github.io/2018/11/16/focal-loss/)

[对Focal Loss的认识](http://skyhigh233.com/blog/2018/04/04/focalloss/)

[Focal Loss论文阅读笔记](https://blog.csdn.net/qq_34564947/article/details/77200104)
