---
title: MDS6222 Dynamic Programming (Notes 1)
tags: [DP]
categories: Notes
mathjax: true
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
- ***Control*** $u_k \in U_k(x_k)$: Action, Decision, each $u_k$ is selected with knowledge of current state $x_k$
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

* Decisions can be ***Sequentially made in stages*** and only depend on the state in the current stage.  (`Memoryless`)
* Stochastic Information can be ***gradually disclosed in stages*** and only depends on the state and the decision(action, control) in the current stage. (`Memoryless`)
* Total costs (Rewards) can be ***accumulated in stages***. (`Additive`)

🚀️ Opportunities for more efficient and effective methods: Dynamic Programming (`DP`)
>DP can deal with ***complex stochastic problems*** where information about $\omega  $ becomes available in stages, and the decisions are also made in stages and make use of this information.


### Example: Inventory Control
- ***Stage***: Ordering Period: $k$
- ***State***: Inventory Level at Period $k$ : $x_k$
- ***Control***: Stock ordered at Period $k$ : $u_k \ge 0$
- ***Disturbance***: Demand at Period $k$ with given probability distribution: $\omega_k$
- ***System Dynamics***: $x_{k+1} = f_k(x_k, u_k, \omega_k) = x_k + u_k - \omega_k$
- ***Stage Cost***: $g_k(x_k, u_k, \omega_k) = kI_{u_k>0} + cu_k + h(x_k + u_k - \omega_k)^{+} + p(x_k + u_k - \omega_k)^{-}$
  Note: costs = setup + ordering + holding + shortage
- ***Terminal Cost***: $g_N(x_N) = 0$
- ***Cost Fuction is Additive over Time***: $E_{\omega}\{g_N(x_N) + \sum_{k = 0}^{N-1}g_k(x_k, u_k, \omega_k)\}$
- ***Optimization over Policies/Rules(closed-loop)***: $u_k = \mu_k(x_k), \text{for} \ k=0, 1, \cdots, N-1$

### Open-loop vs Closed-loop (Feedback Control)
- In open-loop minimization, we select all controls $u_k$ at once at time 0, without waiting to see the subsequent disturbance $w_k$
- In closed- loop minimization, we postpone selecting control $u_k$ until the last possible moment (time $k$) when the current state $x_k$ will be known, thus can make use of the information of $w_k$ that becomes available in stages

### Examples
1. Schedualing (A Deterministic Finite-State Problem);
2. Two Game Chess


## 1.2 The Basic Problem
Given a discrete-time dynamic system
$$
x_{k+1} = f_k(x_k, u_k, \omega_k), \quad k = 0, 1, \cdots, N-1,
$$
where the state $x_k \in S_k,$ the control $u_k \in U_k(S_k) \sub C_k,$ and the random disturbance $\omega_k \in D_k = P(\dot\ |x_k, u_k).$
And given an initial state $x_0$ and an admissible policy $\pi = \{\mu_0, \cdots, \mu_{N-1}\},$ for given stage cost function $g_k,$ the expected cost of $\pi$ starting at $x_0$ is
$$
J_\pi(x_0) = E\left\{g_N(x_N) + \sum_{k = 0}^{N-1}g_k(x_k, \mu_k(x_k), \omega_k)\right\}
$$
An optimal policy $\pi^*$ is one that minimizes this cost:
$$
J_{\pi^*}(x_0) = min_{\pi\in\Pi}J_\pi(x_0),
$$
where $\Pi$ is the set of all admissible policies.
> Note: When produced by DP, $\pi^*$ is independent of $x_0.$


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
The DP Algorithm rests on the principle of optimality:
Let $\pi^* = \{\mu_0^*, \cdots, \mu_{N-1}^*\}$ be optimal policy. Consider the "tail subproblem" whereby we are at $x_i$ at time $i$ and wish to minimize the "cost-to-go" from time $i$ to time $N$
$$
E\left\{g_N(x_N) + \sum_{k = i}^{N-1}g_k(x_k, \mu_k(x_k), \omega_k)\right\}
$$
Then the tail policy is optimal for the tail subproblem. (Optimization of the future does not depend on what we did in past)

- The DP algorithm is based on this idea: it proceeds sequentially, by solving all the tail subproblems of a given time length, using the solution of the tail subproblems of shorter time length.
  - e.g. deterministic scheduling example: length-2-tail -> length-3-tail -> length-4-original
  - e.g. stochastic inventory control example: length-1-tail -> length-2-tail ->  length-$N-k$-tail

- > 'One-at-a-time version of Divide and Conquer'
-  While the original problem requires an optimization over the set of policies, the DP algorithm decomposes the problem into a sequence of minimizations carried out over the set of controls. Each of these minimizations is much simpler than the original problem.



### The DP Algorithm
{% note [info] %}
**Proposition 1.3.1**: Start with $J_N(x_N) = g_N(x_N)$ and go backwards using :
$$
J_{k}\left(x_{k}\right) =\min _{u_{k} \in U_{k}\left(x_{k}\right)} \underset{\omega_{k}}{E}\left\{g_{k}\left(x_{k}, u_{k}, \omega_{k}\right) + J_{k+1}\left(f_{k}\left(x_{k}, u_{k}, \omega_{k}\right)\right)\right\}, \quad k=0,1, \ldots, N-1
$$
Then $J_0(x_0),$ generated at the last step, is equal to the optimal cost $J^*(x_0)$. Also, the policy $\pi^* = \left\{\mu_0^*, \ldots, \mu_{N-1}^* \right\}$, where $\mu_k^*(x_k)$ minimizes in the right side above for each $x_k$ and $k$, is optimal
{% endnote %}
> Proof by induction that $J_k(x_k)$ is equal to $J_k^*(x_k)$, defined as the optimal cost of the tail subproblem that starts at time $k$ at state $x_k$.

>Note:
>    - All the tail subproblems are solved (in addition to the original problem)
>    - Intensive computational requirements
>



- $J_k(x_k)$ : the cost-to-go at state $x_k$ and time $k$, and refer to $J_k$ as the cost-to-go function at time $k$
- e.g. 1.3.1 two ovens


## 1.4 State Augmentation and Other Reformulations
Deal with situations where some of the assumptions of the basic problem are violated ---> reformulate into the basic problem format.

### Case1: Time Lags
### Case2: Correlated Disturbances















