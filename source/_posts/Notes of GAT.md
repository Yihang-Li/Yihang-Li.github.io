---
title: Notes of GAT
date: 2021-08-31T13:35:36+08:00
tags:
---
# Notes of GAT

`youngyhli`

## 1. Details ------ Single Graph Attention Layer

**Input**: a `set of node features`, $\mathbb{h} = \{\vec{h}_1, \vec{h}_2, \dots, \vec{h}_N\}, \vec{h}_i \in \mathbb{R}^F$

* N : number of nodes     - F : number of the features in each node

**Output**: a `new set of node features` (with cardinality $F^{\prime})$, $\mathbb{h}^{\prime}= \{\vec{h}^{\prime}_1, \vec{h}^{\prime}_2, \dots, \vec{h}^{\prime}_N\}, \vec{h}^{\prime}_i \in \mathbb{R}^{F^{\prime}}$

**Shared Linear Transformation**: (At least one learnable linear transformation is required)

* Parametrized by a weight matrix, $\mathbf{W} \in \mathbb{R}^{F^{\prime}\times F}$ (applied to every node)

**Shared Attentional Mechanism**: $a: \mathbb{R}^{F^{\prime}} \times \mathbb{R}^{F^{\prime}} \rightarrow \mathbb{R}$

* For attention coefficients: $e_{ij} = a(\mathbf{W}\vec{h}_i, \mathbf{W}\vec{h}_j)$
* Indicate the importance of node j's features to node i
* Most general form: allows every node attend on every other node, dropping all structural information

**Masked Attention**: injecting the graph structure into the the attention mechanism

* Only compute $e_{ij}$ For nodes $j \in \mathcal{N}_i$

**Comparable Coefficients**: (across different nodes)

* Normalize them across all choices of j using softmax function: $\alpha_{ij} = \operatorname{softmax}_j(e_{ij}) = \frac{\operatorname{exp}(e_{ij})}{\sum_{k\in \mathcal{N}_i}\operatorname{exp}(e_{ik})}$

**Settings in the Paper**: single-layer feedforward neural network

* Parametrized

<!-- more -->

## 2. Implementation

## 3. Application

## Reference

1. [Graph Attention Networks](https://arxiv.org/abs/1710.10903)
