---
title: Dynamic Programming (CH4)
tags: [DP]
categories: Notes
mathjax: true
---
## Ch4 Problems with Perfect State Information

### 4.1 Linear Systems and Quadratic Cost

#### Basic

- System Dynamics (linear):
- Stage Cost (Quadratic):
- $\omega_k$ are independent and zero mean
- DP algorithm:
- Key facts:
  - $J_k(x_k)$ is quadratic
  - Optimal policy is linear

#### The Riccati Equation and Its Asymptotic Behavior

- Riccati equation (like DP, starts at the terminal time $N$ and proceeds backwards)
- Assume controlability of $(A, B)$ and observability of $(A, C)$ where $Q = C^{\prime}C$
- The Riccati equation converges to a pos. defi. $K$, which is a unique solution of the algebraic Riccati equation
- The corresponding steady-state controller $\mu^*(x) = Lx$, where

  $$
  L = -(B^{\prime}KB+R)^{-1}B^{\prime}KA,

  $$

  is stable in the sense that the matrix $(A+BL)$ of the closed-loop system $x_{k+1}=(A+BL)x_k+\omega_k$ satisties $\lim_{k \rightarrow \infty} (A +BL)^k=0$

#### Certainty Equivalence

- Certainty equivalence holds (optimal policy is the same as when $\omega_k$ is replaced by its expected value $E\{\omega_k\} = 0$)

#### Random System Matrices

- Suppose $\{A_k, B_k\}$ unknown but independent
- Certainty equivalence may not hold
- Riccati equation may not converge to a steady-state


### 4.2 Inventory Control

- Stage: week $k$
- State: Inventory level
- Control: Orders
- Disturbance: Demand
- System Dynamics: $x_{k+1} = x_k + u_k - \omega_k$
- Stage Cost
- DP algo
- Optimal Policy
- $J_k$ is convex: prove either inductively(like in book) or by definition (See Notes in P164)
- Base Policy: one threshold
- Positive Fixed Cost (the Base Policy no longer optimal)  -> multiperiod (s, S) policy
- K-convex (convex is 0-convex)

### 4.3 Dynamic Portfolio Analysis

Skip

### 4.4 Optimal Stopping Problems



### 4.5 Scheduling and the Interchange Argument

### 4.6 Set-Membership Description of Uncertainty


$$
$

$$
