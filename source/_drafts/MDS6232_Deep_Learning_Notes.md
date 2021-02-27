---
title: MDS6232 Deep Learning Course Notes(Updating)
date: 2021-01-11 17:13:14
tags: [Deep Learning]
categories: Notes
mathjax: true
---
# Introduction

> Introduction - 01/11/2021

## Deep belief net 2006-Hinton

- **RBM(Restricted Boltzmann Machines)**: A restricted Boltzmann machine (RBM) is a generative stochastic artificial neural network that can learn a probability distribution over its set of inputs. See [Wiki](https://en.wikipedia.org/wiki/Restricted_Boltzmann_machine) & [towardsdatascience](https://towardsdatascience.com/restricted-boltzmann-machines-simplified-eab1e5878976)

<!-- more -->

> R(Restricted): The two layers are connected by a **fully bipartite graph**.

> **Unsupervised**

This restriction allows for more efficient training algorithms than what is available for the general class of Boltzmann machines, in particular, the gradient-based contrastive divergence algorithm.

- **CD algorithms(contrastive divergence)**: a recipe for training undirected graphical models (a class of probabilistic models used in machine learning). It relies on an approximation of the gradient (a good direction of change for the parameters) of the log-likelihood (the basic criterion that most probabilistic learning algorithms try to optimize) based on a short Markov chain (a way to sample from probabilistic models) started at the last example seen.See [Quora](https://www.quora.com/What-is-contrastive-divergence)

  > I like Geoff Hinton's description of CD: He called it "the Microsoft Algorithm:" It asks, "where do you want to go today?" and then doesn't let you go there... [by Gary Cottrell, Professor of Computer Science and Engineering, UCSD]
  >
- **Gibbs Sampling**: In statistics, Gibbs sampling or a Gibbs sampler is a Markov chain Monte Carlo (MCMC) algorithm for obtaining a sequence of observations which are approximated from a specified multivariate probability distribution, when direct sampling is difficult. See [Wiki](https://en.wikipedia.org/wiki/Gibbs_sampling)

> Introduction - 01/13/2021

## Feature Engineering vs Feature Learning

> ***[WiKi](https://en.wikipedia.org/wiki/Feature_learning)***:
>
> - In machine learning, **feature learning** or representation learning is a set of techniques that allows a system to automatically discover the representations needed for feature detection or classification from raw data. This replaces **manual feature engineering** and allows a machine to both learn the features and use them to perform a specific task.
> - Feature learning is motivated by the fact that machine learning tasks such as classification often require input that is mathematically and computationally convenient to process. However, real-world data such as images, video, and sensor data has not yielded to attempts to algorithmically define specific features. An alternative is to discover such features or representations through examination, without relying on explicit algorithms.


| Feature Engineering | Feature Learning |
| :-: | :-: |
| Rely on human domain knowledge much more than data | Make better use of big data, data-driven way |
| If handcrafed features have multiple parameters, it is hard to manually tune them | Learn the values of a huge number of parameters in feature representations |
| Feature design is separate from training the classifier | Jointly learning feature transformations and classifiers makes their integration optimal |
| Developing effective features for new applications is slow | Faster to get feature representations for new applications |

> Introduction - 01/18/2021

### Design Cycle

<img src="/assets/Feature_Engineering_Design_Cycle.png" alt="Feature Engineering Design Cycle" style="zoom:80%;" />

> ***For Feature Engineering Design Cycle***:
>
> - **Preprocessing and Feature Design** may lose useful information and not to be optimized, since they are not parts of an end-to-end learning system
> - **Preprocessing** could be the result of another pattern recognition system

<img src="/assets/Deep_Learning_Design_Cycle.png" alt="Deep Learning Design Cycl" style="zoom:60%;" />

> ***For Deep Learning Design Cycle***:
>
> - Learning plays a bigger role in the design cycle
> - Feature learning becomes part of the end-to-end learning system and makes the key difference
> - Preprocessing becomes optional means that several pattern recognition steps can be merged into one end-to-end learning system
> - However, we may underestimated the importance of data collection and evaluation
> - Preprocessing** could be the result of another pattern recognition system

### Learning features and classifiers separately

> **Not all the datasets and prediction tasks are suitable for learning features with deep models**

<img src="/assets/LFCS.png" alt="Learning features and classifiers separately" style="zoom:80%;" />

As above, we learn features on Task A, instead learn classifier for task B.

## Deep Structures vs Shallow Structures

> The former usually relate to global or non-local while the latter to local

> Shallow models divide the feature space into regions and match templates in local regions
>
> Shallow models increases the model capacity tipically by increasing the size of feature vectors

> Human Understand the World through Multiple Levels of Abstractions
>
> - Humans can imagine new pictures by re-configuring these abstractions at multiple levels. Thus, our brain has good **generalization**(`脑补`) can recognize things never seen before.

## Joint Learning vs Separate Learning

<img src="/assets/JSL.png" alt="Joint Learning vs Separate Learning" style="zoom:80%;" />

> Deep Learning is a framework/language but not a black-box model.
>
> Its power comes from joint optimization and increasing the capacity of the learner (`depth&width`, `trade off between capacity&convergence`)
>
> Large learning capacity makes **high dimensional data transforms**(`width`) possible, and makes better use of contextual information( **Hierarchical nonlinear representations**(`depth`))

<img src="/assets/widthdepth.png" alt="Width&Depth" style="zoom:80%;" />

## Sparsify (Pruning)

<img src="/assets/Sparsify.png" alt="sparsify" style="zoom:80%;" />

Red part: progressively pruning | Green part: directly pruning

> ***Dropout!***  See [D2L](https://d2l.ai/chapter_multilayer-perceptrons/dropout.html)

## Conclusion

- Deep learning = Machine learning with big data, feature learning, joint learning (end-to-end learning), contextual learning (depth & width)
- Deep feature presentations are: Sparse & Selective
  - Robust to data corruption
