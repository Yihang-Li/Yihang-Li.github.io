---
title: Spatial analysis made easy with linear regression and kernels
date: 2020-03-09 16:33:20
tags: [kernel, spatial, regression, translation]
categories: Notes
mathjax: true
typora-copy-images-to: ..\assets
typora-root-url: ..
---



> 总结自[Philip Miltona, Helen Couplanda , Emanuele Giorgib , Samir Bhatta](https://arxiv.org/abs/1902.08679)

# Spatial analysis made easy with linear regression and kernels



## 摘要

核方法是一种流行的技术，可用于通过映射到隐式的高维特征空间来扩展线性模型已处理非线性的空间问题。
尽管核方法在计算上优于显式的特征映射，却仍然受制于样本点的立方级计算复杂度。给定几千个样本点，它的计算成本很快就超过了现有的可获取的算力。
本文旨在提供核方法的概览，从first-principals（着重于岭回归），到对随机傅里叶特征（RFF）的回顾，RFF是一种将核方法应用于大数据集的方法。
我们展示了RFF方法如何能够逼近完整的核矩阵，以微弱地降低准确性为代价提供了显著的计算速度的提高，并且可以仅使用几行代码就可以并入许多现有的空间方法中。
我们给出了在仿真空间数据集上实现RFF的示例，以说明这些属性。 最后，我们总结了RFF的主要问题，并着重介绍了一些旨在改善这些问题的技术。

 ## 1. 引言

## 2. 线性模型

## 3. 非线性特征的线性模型

## 4. 过拟合和岭回归

## 5. 对偶和格gram矩阵

## 6. 核函数，核矩阵和核技巧



## 7. 大N问题

## 8. 随机傅里叶特征

## 9. 线性模型的变体

## 10. 空间分析的随机傅里叶特征玩具实例

## 11. 随机傅里叶特征的高等方法

### 11.1 RFF的局限

### 11.2 拟蒙特卡洛特征（QMC RFF）

### 11.3 杠杆值采样

### 11.4 正交随机特征

### 11.5 不稳定和任意核函数

## 12. 结论









 