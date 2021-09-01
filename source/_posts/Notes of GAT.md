---
title: Notes of GAT
date: 2021-08-31T13:35:36+08:00
tags:
---
# Notes of GAT

`youngyhli`

**GAT：首次将图注意力机制引入到图神经网络中的工作**

<!--more-->

## 1. Details ------ Single Graph Attention Layer

**Input**: a `set of node features`, $\bold{h} = \{\vec{h}_1, \vec{h}_2, \dots, \vec{h}_N\}, \vec{h}_i \in \mathbb{R}^F$

 - $N$ : number of nodes     - $F$ : number of the features in each node

**Output**: a `new set of node features` (with cardinality $F^{\prime}$), $\bold{h}^{\prime}= \{\vec{h}^{\prime}_1, \vec{h}^{\prime}_2, \dots, \vec{h}^{\prime}_N\}, \vec{h}^{\prime}_i \in \mathbb{R}^{F^{\prime}}$



**Shared Linear Transformation**: (At least one learnable linear transformation is required)

 - Parametrized by a weight matrix, $\mathbf{W} \in \mathbb{R}^{F^{\prime}\times F}$ (applied to every node)

**Shared Attentional Mechanism**: $a: \mathbb{R}^{F^{\prime}} \times \mathbb{R}^{F^{\prime}} \rightarrow \mathbb{R}$

 - For attention coefficients: $e_{ij} = a(\mathbf{W}\vec{h}_i, \mathbf{W}\vec{h}_j) \quad (1)$

 - Indicate the importance of node $j$'s features to node $i$

 - Most general form: allows every node attend on every other node, dropping all structural information



**Masked Attention**: injecting the graph structure into the the attention mechanism

 - Only compute $e_{ij}$ For nodes $j \in \mathcal{N}_i$

**Comparable Coefficients**: (across different nodes)

- Normalize them across all choices of $j$ using softmax function:$\alpha_{ij} = \operatorname{softmax}_j(e_{ij}) = \frac{\operatorname{exp}(e_{ij})}{\sum_{k\in \mathcal{N}_i}\operatorname{exp}(e_{ik})} \quad (2)$



**Settings in the Paper**: single-layer feedforward neural network

- Parametrized by a weight vector $\vec{a} \in \mathbb{R}^{2F^{\prime}}$ (`2` due to the concatenation in the following)

- Applying the LeakyReLU nonlinearity (with  negative input slope $\alpha = 0.2$)

- Fully expanded out, the coefficients computed by the attention mechanism: $\alpha_{ij} = \frac{\operatorname{exp}(\operatorname{LeakyReLU}(\vec{a}^{T}[\mathbf{W}\vec{h}_i\|\mathbf{W}\vec{h}_j]))}{\sum_{k \in \mathcal{N}_i}\operatorname{exp}(\operatorname{LeakyReLU}(\vec{a}^{T}[\mathbf{W}\vec{h}_i\|\mathbf{W}\vec{h}_k]))} \quad (3)$

  - $\|$ is the concatenation operation
  - 值得注意：这里的拼接导致$e_{ij} \neq e_{ji}$, 从而注意力系数是非对称的
  - 而归一化也进一步导致了注意力权重的非对称性：$e_{ij}$是针对节点$i$的所有邻居进行归一化，而$e_{ji}$是针对节点$j$的所有邻居

  > 这种非对称性在图数据上有什么用呢？一个简单的例子：在社交网络中，有一个大 V 和一个普通用户互相关注。但是，大 V 对于普通用户的重要性和普通用户对大 V 的重要性明显是不一样的。

  - 注意到非线性激活函数的存在是合理的：若省略它，基于指数运算的性质，分子分母可以同时约去带$i$的这项，从而$\alpha_{ij}$实际变成了 $\alpha_j$，节点对$(i, j)$之间的注意力权重就没有考虑节点$i$的表示了



**Once Obatained**: the normalized attention coefficients are used to compute a linear combination of the features corresponding to them, to serve as the final output features for every node (after potentially applying a nonlinearity, $\sigma$): $\vec{h}_i^{\prime} = \sigma(\sum_{j\in \mathcal{N}_i}\alpha_{ij}\mathbf{W}\vec{h}_j) \quad (4)$



**Multi-Head Attention**: to stabilize the learning process of self-attention

- K independent attention mechanisms execute the transformation of Equation $(4)$, and their features are concatenated, resulting in the following output feature representation: $\vec{h}_i^{\prime}= \|_{k=1}^{K} \sigma(\sum_{j\in \mathcal{N}_i}\alpha_{ij}^{k}\mathbf{W}^k\vec{h}_j) \quad (5)$

  > If we perform multi-head attention on the final (prediction) layer of the network, concatenation is no longer sensible ------ instead we employ `averaging` and delay applying the final nonlinearity (softmax or logistic for classfication problems) until then: $\vec{h}_i^{\prime} = \sigma(\frac{1}{K}\sum_{k=1}^{K}\sum_{j\in \mathcal{N}_i}\alpha_{ij}^{k}\mathbf{W}^k\vec{h}_j) \quad (5)$





## 2. Implementation

[paperswithcode: Graph Attention Networks](https://paperswithcode.com/paper/graph-attention-networks)



## Reference

1. [Graph Attention Networks](https://arxiv.org/abs/1710.10903)
2. [深入理解图注意力机制（Graph Attention Network）](https://blog.csdn.net/c9Yv2cf9I06K2A9E/article/details/105548217)

