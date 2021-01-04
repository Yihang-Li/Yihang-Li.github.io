---
title: 'Summary: Recent Advances and Trends in Large-Scale Kernel Methods'
date: 2020-02-24 15:54:58
tags: [Kernel Methods, summary]
categories: Notes
mathjax: true
typora-copy-images-to: ..\assets
typora-root-url: ..
---

[RAandTinLSKM](https://www.jstage.jst.go.jp/article/transinf/E92.D/7/E92.D_7_1338/_pdf/-char/ja)

### 摘要部分

1. 核方法优势：通过使用核技巧可以把线性模型直接拓展到非线性应用场景

2. 然鹅， 核方法的naive使用的计算复杂度是很高的，scales cubically w.r.t. 训练样本的数量

   > <span style='color:red'>表示不赞同，我现有层面的知识恰恰是能避免高维计算</span> <span style="color:green">(或许是不同的角度？)</span>

3. 这篇文章回归了最近的处理大规模数据的核方法

***

### 引言部分

1. 核方法的关键：核函数——使得计算复杂度独立于特征空间的维数
2. *Vapnik* 的重大工作使得很多线性的监督和非监督学习算法能够应用核方法（<span style="color:blue">Kernelized</span>)。包括岭回归、感知机、费希尔判别分析、主成分分析、K均值聚类和独立成分分析。
3. 这篇文章的目的：回顾各种加速核方法的技巧
4. 这篇文章的布局：
   - *Sect. 2* : 复习核方法的基础思想，回顾最近大数据核方法的进展，一个计算瓶颈是核矩阵K的计算
   - *Sect. 3* : 回顾核矩阵能怎样被低秩矩阵逼近（考虑incomplete Cholesky decomposition、Lanczos approximation、Nystrom method and Gaussian transform）
   - *Sect. 4* : 回顾监督核方法。回归部分：怎样用低秩逼近方法来加速相应核算法诸如RR、PLS、GP。分类部分：用cutting-plane或者dual coordinate descent 方法解决支持向量算法中的优化问题。
   - Sect. 5 : 关注非监督方法。怎样用拉普拉斯特征映射有效降低稀疏数据的维数，应用到谱聚类算法中
   - Sect. 6 : 面对structured 数据，怎样有效计算核函数。怎样从多个kernel构造出一个kernel。
   - Sect. 7 : 总结。

***

***

### Sect. 2 核方法基础

考虑线性参数模型：

> $$ f(\mathbf{x};\mathbf{w}) \equiv <\mathbf{w},\mathbf{x}> \tag{1}$$

其中，$$ \mathbf{x} \in \mathcal{R^d}$$是输入变量， $$\mathbf{w} \in \mathcal{R^d}$$是参数向量，$$<\dot{},\dot{}>$$表示内积。

给定训练数据$$\{(\mathbf{x}_i,y_i)|\mathbf{x_i}\in \mathcal{R^d}, y_i \in \mathcal{R}\}_{i=1}^{\mathcal{l}}$$, 为了确定参数$$\mathbf{w}$$，需要最小化经验风险项和正则项的线性和：

$$ J(\mathbf{w}) \equiv R_{emp}(\mathbf{w})+\lambda \Omega(\mathbf{w})$$, 其中 $$\lambda > 0$$ 为正则化参数。



以岭回归为例，使用平方损失函数来度量经验风险，$$L_2$$范数作为正则项：

> $$R_{emp}(\mathbf{w}) \equiv \sum_{i=1}^{\mathcal{l}}(y_i-f(\mathbf{x_i};\mathbf{w}))^2 \tag{2}$$

> $$\Omega(\mathbf{w}) \equiv || \mathbf{w}||_{2}^{2} \tag{3}$$

输入样本构成的数据矩阵记作：$$\mathbf{X} = (\mathbf{x_1}|\mathbf{x_2}|...|\mathbf{x_\mathcal{l}})$$，则岭回归的解析解为：

> $$\widehat{\mathbf{w}} = (\mathbf{X}^T\mathbf{X}+\lambda\mathbf{I})^{-1}\mathbf{X}^T\mathbf{y} \tag{4}$$

(4) 表明岭回归求解的计算复杂度为$$O(d^3)$$，依赖于输入空间的维数。

***

> 假设参数$$\mathbf{w}$$可以写成训练样本的线性组合：
>
> $$\mathbf{w} \equiv \sum_{i=1}^{\mathcal{l}}\alpha_i\mathbf{x_i} = \mathbf{X}\mathbf{\alpha} \tag{5}$$



则 Eq. (1) 可以表示成：$$ f(\mathbf{x};\mathbf{\alpha}) \equiv \sum_{i=1}^{\mathcal{l}}\alpha_i<\mathbf{x}_i,\mathbf{x}> $$

从而定义 *核函数* 为：$$k(\mathbf{x},\mathbf{x'}) \equiv <\mathbf{x}, \mathbf{x'}>$$. 

进一步参数模型可以表示为：

> $$ f(\mathbf{x}) = \sum_{i=1}^{\mathcal{l}}\alpha_ik(\mathbf{x}_i,\mathbf{x}) \tag{6}$$

基于核化（kernelized）的模型，岭回归的目标函数可以重写为：$$J(\alpha) = ||\mathbf{y}-\mathbf{K}\mathbf{\alpha}||^2+\lambda\mathbf{\alpha}^T\mathbf{K}\mathbf{\alpha}$$，其中$$\mathbf{K}$$是$$\mathcal{l} \times\mathcal{l}$$ 的核矩阵（定义为：$$K_{i,j} \equiv k(\mathbf{x}_i, \mathbf{x}_j)$$）. 可解得：

>  $$\widehat{\mathbf{\alpha}} = (\mathbf{K} + \lambda\mathbf{I})^{-1}\mathbf{y} \tag{7}$$

(7) 表明核岭回归的计算复杂度为$$O（d\mathcal{l}^3)$$，其中$$d$$来自于计算核函数的值，$$\mathcal{l}^3$$来自于计算核矩阵的逆。

因此，现在计算复杂度主要由训练样本的数量决定。

以上的形式是基于（5）这一线性假设，由*representer theorem*可以保证其正确性。

***

为了说明核形式的优越性，考虑变换 $$\Phi: \mathcal{R}^d \rightarrow \mathcal{R}^{d'}$$. 我们假设 $$d' \gg d$$ 而且通过变换后的样本$$\{\Phi(x_i)\}_{i=1}^{\mathcal{l}}$$来学习模型.

在原始形式（4）中， 因为$$d'$$很大，求解可能会比较棘手；而在核形式（7）中，输入样本仅仅在核函数的值的计算中处理：$$k(\mathbf{x},\mathbf{x'})\equiv <\Phi(\mathbf{x}), \Phi({\mathbf{x'}})>$$。

记 $t$ 为计算核函数值的复杂度，则计算解的复杂度为$O(t\mathcal{l}^3)$，不依赖于 $d'$。因此核形式可以让我们在$d'$比较大的时候更有效地计算解。

——这样的计算技巧称为核技巧

反之，如果存在对应于内积$$<\Phi(\mathbf{x}), \Phi({\mathbf{x'}})>$$的核函数，则用核形式会很有帮助。而这里的内积的存在性是有保证的，只需要核函数是半正定的（这样的核函数称为Mercer核或者再生核）。

***

有很多核函数的计算复杂度都是独立于$d'$的：

> 多项式核：$k(\mathbf{x},\mathbf{x'})=(<\mathbf{x},\mathbf{x'}>+1)^c$
>
> 高斯核：$k(\mathbf{x},\mathbf{x'})=e^{-||\mathbf{x}-\mathbf{x'}||^2/\sigma^2}$

***

然而，当训练样本很大的时候，核方法的计算复杂度仍然很高。

***

***

### Sect. 4 监督方法

#### 4.1 回归

##### 4.1.1 核岭回归

从（7）知道，解KRR即相当于求解线性方程：$$(\mathbf{K} + \lambda\mathbf{I}_{\mathcal{l}})\mathbf{\alpha} = \mathbf{y}$$，

> 通常的方法：Cholesky factorization $O(\mathcal{l}^3)$
> 若使用 CG 方法（共轭梯度法），$O(r\mathcal{l}^2)$ ，这里 $r$ 是CG迭代次数
> 若核函数是高斯核，CG方法的计算复杂度会降至 $O(\mathcal(l))$

> 可使用 LOOCV 来决定正则化参数$\lambda$，在KRR中，LOOCV score 的闭形式是：$g_{LOO}(\lambda)=||\mathbf{H}^{-1}(\mathbf{y}-\mathbf{K}\mathbf{\alpha})||^2$，这里$\mathbf{H} \equiv \mathbf{I}_N-diag(\mathbf{K}(\mathbf{K}+\lambda\mathbf{I}_{\mathcal{l}}^{-1}))$。然而，这里计算设计求逆 -> $O(\mathcal{l}^3)$。

##### 4.1.2 偏最小二乘

##### 4.1.3 Lasso

> LARS -> 能对所有的 $\lambda$ 获得Lasso解；但是对单个的 $\lambda$ ，简单的坐标下降法（coordinate descent）更快。

> 尽管Lasso是线性方法，它也可以被核化[38] [39], 一些学者也关注于怎样求解非线性形式的Lasso正则路径(regularization path)



##### 4.1.4 高斯过程回归

##### 4.1.5 相关向量机（Relevance Vector Machines）

> 4.1.4 & 4.1.5 都是贝叶斯回归方法



##### 4.1.6 支持向量回归

SVR问题可形式化为最小化如下目标函数：

> $$J_{SVR}(\mathbf{w})=\frac{C}{\mathcal{l}}\sum_{i=1}^{\mathcal{l}}l_{\epsilon}(y_i-<\mathbf{x}_i, \mathbf{w}>+\frac{1}{2}||\mathbf{w}||_2^2) \tag{17}$$

这里  $l_{\epsilon}$ 称为 $\epsilon-insensitive$ 损失函数：$$
l_{\epsilon}(\eta) \equiv\left\{\begin{array}{ll}
{0} & {\text { if }|\eta|<\epsilon} \\
{|\eta|-\epsilon} & {\text { otherwise }}
\end{array}\right.$$

常规解法是求解二次规划问题获得（17）的对偶形式，但是SVR可以有效地用4.2.1中的方法求解。

#### 4.2 分类

##### 4.2.1 Cutting Plane Algorithm

##### 4.2.2 Dual Coordinate Descent Algorithm

### Sect. 7 总结