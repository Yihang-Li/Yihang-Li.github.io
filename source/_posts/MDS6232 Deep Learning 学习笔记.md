---
title: MDS6232 Deep Learning 学习笔记(持续更新)
tags: [Deep Learning]
categories: Notes
mathjax: true
---

# Introduction - 01/11/2021

## Deep belief net 2006-Hinton
- **RBM(Restricted Boltzmann Machines)**: A restricted Boltzmann machine (RBM) is a generative stochastic artificial neural network that can learn a probability distribution over its set of inputs. See [Wiki](https://en.wikipedia.org/wiki/Restricted_Boltzmann_machine) & [towardsdatascience](https://towardsdatascience.com/restricted-boltzmann-machines-simplified-eab1e5878976)
  > R(Restricted): The two layers are connected by a **fully bipartite graph**.

  > **Unsupervised**

  This restriction allows for more efficient training algorithms than what is available for the general class of Boltzmann machines, in particular, the gradient-based contrastive divergence algorithm.

- **CD algorithms(contrastive divergence)**: a recipe for training undirected graphical models (a class of probabilistic models used in machine learning). It relies on an approximation of the gradient (a good direction of change for the parameters) of the log-likelihood (the basic criterion that most probabilistic learning algorithms try to optimize) based on a short Markov chain (a way to sample from probabilistic models) started at the last example seen.See [Quora](https://www.quora.com/What-is-contrastive-divergence)
  > I like Geoff Hinton's description of CD: He called it "the Microsoft Algorithm:" It asks, "where do you want to go today?" and then doesn't let you go there... [by Gary Cottrell, Professor of Computer Science and Engineering, UCSD]

- **Gibbs Sampling**: In statistics, Gibbs sampling or a Gibbs sampler is a Markov chain Monte Carlo (MCMC) algorithm for obtaining a sequence of observations which are approximated from a specified multivariate probability distribution, when direct sampling is difficult. See [Wiki](https://en.wikipedia.org/wiki/Gibbs_sampling)

