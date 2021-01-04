---
title: Random Features for Large-Scale Kernel Machines
date: 2020-03-09 14:27:49
tags: [kernel, random features, large scale, translation]
categories: Notes
mathjax: true
typora-copy-images-to: ..\assets
typora-root-url: ..
---



> 翻译总结自 [Ali Rahimi, Ben Recht](https://people.eecs.berkeley.edu/~brecht/papers/07.rah.rec.nips.pdf)著



# 用于大规模核机器的随机特征

### Ali Rahimi and Ben Recht

## 摘要

为了加速核机器的训练，我们提出将输入数据映射到随机化的低维特征空间，并随后应用已有的快速线性方法。我们通过构造随机化特征以使得转换后的数据的内积近似于特征空间中自定义的平移不变核。我们探索了两类随机特征，提供了他们估计不同径向基核的收敛界，并且证明了在大规模分类和回归任务中，使用这些特征的线性机器学习算法更优于目前的最新大规模核机器模型。

## 1. 引言

核机器，诸如支持向量机等，因其能在充足的训练数据下以任意精度逼近任何函数或决策边界而引人注目。不幸的是，对数据的核矩阵（格拉姆矩阵）进行操作的方法，根据训练数据集的大小表现得不尽如人意。比如，一个有50万数据的数据集可能需要在现代的工作站上花费数天来训练。另一方面，针对线性支持向量机和正则化回归的特定算法在数据维数很小时会很快，因为他们操作训练数据的协方差矩阵而不是核矩阵<span style=color:red>(?)</span>，[1,2]。我们提出了能够结合线性和非线性优点的方法。受到逼近核矩阵的随机算法的启发（e.g.,[3,4]），我们通过将数据映射到相对低维的随机特征空间中，将任何核机器的训练和评估有效地转换为线性机器的相应操作。我们的实验表明，随机特征与非常简单的线性学习技术相结合，可以与基于核方法的最新分类和回归算法相媲美。随机特征显著减少了训练需要的计算，并且获得相似的甚至更好的测试误差。

对于只依赖于输入点对之间内积的算法，核技巧是一种能够为其生成特征的简单方法。它依赖于以下结论：任何正定函数 $k(\mathbf{x},\mathbf{y}), \mathbf{x},\mathbf{y} \in \mathcal{R}^d$定义了一个内积和一个映射<span style=color:red>(?lifting)</span>$\phi$，以使得映射后的数据点能被快速计算为$<\phi(\mathbf{x},\phi(\mathbf{y})>=k(\mathbf{x},\mathbf{y})$。这种便利的代价是算法仅通过评估$k(\mathbf{x},\mathbf{y})$或通过将$k$应用于所有数据组成的核矩阵来处理数据。最终，大量的训练集会导致大量的计算和存储成本。

> 逐字逐句 stop here...



## 2. 相关工作

## 3. 随机傅里叶特征

随机傅里叶基函数：$cos(\omega^{\prime}\mathbf{x}+b)$，其中$w \in \mathcal{R}^d， b \in \mathcal{R}$为随机变量。

这些映射将数据点投影到随机选择的直线上，然后将得到的标量（投影点的值）通过正弦函数传递。从适当的分布中绘制这些线的方向可确保两个变换点的乘积将近似于所需的平移不变核。

![](/assets/screenshot-people.eecs.berkeley.edu-2020.03.09-17_44_39.png)



## 4. 随机Binning特征

## 5. 实验

## 6. 结论

## 参考文献

## A 证明

