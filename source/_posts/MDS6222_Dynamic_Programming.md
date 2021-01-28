---
title: MDS6222 Dynamic Programming (Notes 1)
tags: [DP]
categories: Notes
m
---

# Ch1 The Dynamic Programming Algorithm
>Life can only be understood going backwards,
>but it must be lived going forwards.
>—— Kierkegaard

<!-- more -->

## 1.1 Introduction

### Basic Structure of Stochastic Dynamic Programming

#### **2** Principle Features of DP 👀
* Discrete-time Dynamic Systems
* Cost Function is Additive over Time

#### **6** Key Elements of DP Formulation 👀
* ***Stage***  $k$: Finite Horizon, Total Number of Horizon $N$ , time/space/etc.
* ***State*** $x_k$: Decision Base
- ***Control*** $u_k \in U_k(x_k)$: Action, Decision, each $u_k$ is selected with knowledge of current st$x_k$
- ***Disturbance***  $\omega_k \sim P(\omega_k|x_k, u_k)$: Value of Information, probability distribution of $\omega_k$ does not depend on past values $\omega_{k-1}, \dotsm, \omega_0$, but may depend on $x_k$ and $u_k$
- ***`Memoryless` System Dynamics*** $x_{k+1} = f_k(x_k, u_k, \omega_k)$: $k$ is time-varied, if removed, then stationary system.  `Memoryless` is that $f_k$ does not depend on $x_{k-1}, \omega_{k-1}$ and so on.
- ***`Additive` Stage Cost***: $g_k(x_k, u_k, \omega_k), k = 0, 1, \dotsm, N-1$
     ***Terminal State***: $g_N(x_N)$

#### **2** Aims 👀

- ***Optimal Policy*** $\pi^* = \left(\mu_0^*, \mu_1^*, \dotsm, \mu_{N-1}^* \right)$: ***Policy*** ~ $\pi = \left(\mu_0, \mu_1, \dotsm, \mu_{N-1}\right)$ where $\mu_k$ maps states $x_k$ into controls $u_k = \mu_k(x_k)$ and is such that $\mu_k(x_k) \in U_k(x_k)$ for all $x_k$
- ***Optimimal Cost-to-go Function***: Tail Subproblems Defined Recursively

> $J_N(x_N) = g_N(x_N)$
>
> $J_k(x_k) = \min\limits_{u_k \in U_k(x_k)}E_{\omega_k}\{g_k(x_k, u_k, \omega_k)+J_{k+1}(f_k(x_k, u_k, \omega_k))\}$,  $\forall k = 0, 1, \dotsm, N-1$


### Where and Why to Use Dynamic Programming

👀️ What if there exists some optimization problems such that

* Decisions can be ***Sequentially made in stages*** and only deepend on the state in the current stage.  (`Memoryless`)
* Stochastic Information can be ***gradually disclosed in stages*** and only depends on the state and the decision(action, control) in the current stage. (`Memoryless`)
* Total costs (Rewards) can be ***accumulated in stages***. (`Additive`)

🚀️ Opportunities for more efficient and effective methods: Dynamic Programming (`DP`)
>DP can deal with ***complex stochastic problems*** where information about $\omega  $ becomes available in stages, and the decisions are also made in stages and make use of this information.


### Example: Inventory Control
- ***Stage***: Ordering Period: $k$
- ***State***: Inventory Level at Period $k$ : $x_k$
- ***Control***: Stock ordered at Period $k$ : $u_k \ge 0$
- ***Disturbance***: Demand at Period $k$: $\omega_k$
- ***System Dynamics***: $x_{k+1} = f_k(x_k, u_k, \omega_k) = x_k + u_k - \omega_k$
- ***Stage Cost***: $g_k(x_k, u_k, \omega_k) = $
- ***Terminal Cost***:
- ***Cost Fuction is Additive over Time***:
- ***Optimization over Policies/Rules***:

> Revisited Later and fullfill above things! 2021-01-27

### Open-loop vs Closed-loop (Feedback Control)
- In open-loop minimization, we select all controls $u_k$ at once at time 0, without waiting to see the subsequent disturbance $w_k$
- In closed- loop minimization, we postpone selecting control $u_k$ until the last possible moment (time $k$) when the current state $x_k$ will be known, thus can make use of the information of $w_k$ that becomes available in stages

### Examples
1. Schedualing (A Deterministic Finite-State Problem);
2. Two Game Chess


## 1.2 The Basic Problem
> Need to Add the content of basic problem here --- 2021-01-27
### Value of Information
-  In Deterministic Problem, Open-loop is as good as Closed-loop
-  Value of Information = $J^*_{\text{closed-loop}} - J^*_{\text{open-loop}}$  (The reduction in cost)
-  Example: Two-Game Chess

### Encoding Risk in the Cost Function
- As above, one important characteristic of stochastic problems is the possibility of using information with advantage
- The other distinguishing characteristic is the need to take into account `risk` in the problem formulation.
  e.g. Expectation and Variance in investment decision
  e.g. St. Petersburg paradox


## 1.3 The Dynamic Programming Algorithm
### Principle of Optimality
> Need whole illustration here --- 2021-01-27

- The truncated policy is optimal for the tail problem.
- The DP algorithm is based on this idea: it proceeds sequentially, by solving all the tail subproblems of a given time length, using the solution of the tail subproblems of shorter time length.
  - e.g. deterministic scheduling example
  - e.g. stochastic inventory control example

-> 'One-at-a-time version of Divide and Conquer'

### The DP Algorithm
> Need whole illustration here for Proposition1.3.1 and its proof detail --- 2021-01-27

- $J_k(x_k)$ : the cost-to-go at state $x_k$ and time $k$, and refer to $J_k$ as the cost-to-go function at time $k$
- e.g. 1.3.1 two ovens


## 1.4 State Augmentation and Other Reformulations
Deal with situations where some of the assumptions of the basic problem are violated ---> reformulate into the basic problem format.

### Case1: Time Lags
### Case2: Correlated Disturbances















