---
title: MDS6222 Dynamic Programming (Notes 1)
date: 2021-01-22T15:09:58+08:00
tags: [DP]
categories: Notes
mathjax: true
---
> Note: Only write key points here, other things see other materials.

# Table of contents

1. [Where and Why to Use Dynamic Programming](#paragraph1)
3. [Basic Structure of Stochastic Dynamic Programming](#paragraph2)
4. [Value of Information ~ Two Game Chess](#p3)
5. [DP Algorithm](#P4)
6. [State Augmentation](#p5)


<!-- more -->

## Where and Why to Use Dynamic Programming <a name="paragraph1"></a>

👀️ What if there exists some optimization problems such that

* Decisions can be ***Sequentially made in stages*** and only deepend on the state in the current stage.  (`Memoryless`)
* Stochastic Information can be ***gradually disclosed in stages*** and only depends on the state and the decision(action, control) in the current stage. (`Memoryless`)
* Total costs (Rewards) can be ***accumulated in stages***. (`Additive`)

🚀️ Opportunities for more efficient and effective methods: Dynamic Programming (`DP`)

> DP can deal with ***complex stochastic problems*** where information about $\omega  $ becomes available in stages, and the decisions are also made in stages and make use of this information.

## Basic Structure of Stochastic Dynamic Programming <a name="paragraph2"></a>

### ***2*** Principle Features of DP

* Discrete-time Dynamic Systems
* Cost Function is Additive over Time

### ***6*** Key Elements of DP Formulation

* ***Stage***: $k$

> Finite Horizon, Total Number of Horizon $N$ , time/space/etc.

* ***State***: $x_k$

> Decision Base

- ***Control***: $u_k \in U_k(x_k)$

> Action, Decision, $u_k$ is selected with knowledge of $x_k$

- ***Disturbance***: $\omega_k \sim P(\omega_k|x_k, u_k)$

> Value of Information, probability distribution of $\omega_k$ does not depend on past values $\omega_{k-1}, \dotsm, \omega_0$, but may depend on $x_k$ and $u_k$

- ***`Memoryless` System Dynamics***: $x_{k+1} = f_k(x_k, u_k, \omega_k)$

> $k$: time-varied, if removed, then stationary system.  `Memoryless` $f_k$ does not depend on $x_{k-1}, \omega_{k-1}$ and so on.

- ***`Additive` Stage Cost***: $g_k(x_k, u_k, \omega_k), k = 0, 1, \dotsm, N-1$
  ***Terminal State***: $g_N(x_N)$

### ***2*** Aims

- ***Optimal Policy***:  $\pi^* = \left(\mu_0^*, \mu_1^*, \dotsm, \mu_{N-1}^* \right)$

> ***Policy***: $\pi = \left(\mu_0, \mu_1, \dotsm, \mu_{N-1}\right)$ where $\mu_k$ maps states $x_k$ into controls $u_k = \mu_k(x_k)$ and is such that $\mu_k(x_k) \in U_k(x_k)$ for all $x_k$

- ***Optimimal Cost-to-go Function***: Tail Subproblems Defined Recursively

> $J_N(x_N) = g_N(x_N)$
>
> $J_k(x_k) = \min\limits_{u_k \in U_k(x_k)}E_{\omega_k}\{g_k(x_k, u_k, \omega_k)+J_{k+1}(f_k(x_k, u_k, \omega_k))\}$,  $\forall k = 0, 1, \dotsm, N-1$

## Value of Information ~ Two Game Chess <a name="p3"></a>

## DP Algorithm

### Optimality of DP Algorithm

### Application of DP Algorithm

## State Augmentation <a name='p5'></a>

### Time Lags

### Correlated Disturbance
