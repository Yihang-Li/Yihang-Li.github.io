---
title: Time Series
tags: [Notes]
---
## Lecture 3

### Stationary and ergodic time series

Stationary in the mean: mean doesn't change over time. i.e. $E[Y_t] = \mu$ does not depend on $t$

Covariance (weak) stationary: variance-covariance structure doesn't change over time i.e. $E[Y_t] = \mu$ does not depend on $t$; $var[Y_t] = \sigma^2$ does not depend on $t$; $cov(Y_t, Y_{t-j}) = \gamma_j$ exists, is finite, and depends only on $j$ but not on $t$, for $j=0, 1, 2, \dots$

Strict stationary: the distribution does not change over time

e.g. Gaussian White Noise (GWN) process is covariance stationary (also strictly stationary)

Two related terms: Independent White Noise (IWN): independent with mean zero  and variance $\sigma^2$; Weak White Noise (WN): uncorrelated with mean zero and variance $\sigma^2$

e.g. Deterministic trending process is not stationary because the mean depends on $t$; Random walk is not stationary because its variance depends on $t$, however, its first difference is covariance stationary.


Ergodic: 定义？



### Auto correlation and Partial auto correlation



variance function of a stationary time series: population version; sample version

Covariacne:

Correlation:

Auto-CoVariance Function (ACVF):
P-version: $\gamma_k = E[(x_t-\mu)(x_{t+k}-\mu)] $; S-version: $c_k = \frac{1}{n}\sum_{t=1}^{n-k}(x_t-\bar{x})(x_{t+k}-\bar{x})$

Auto-Correlation Function (ACF):

P: $\rho_k = \frac{\gamma_k}{\sigma^2}
$; S: $r_k = \frac{c_k}{c_0}$




### Cross correlation



## Lecture 5

### White Noise

### Random Walks

$x_t = x_{t-1} + \omega_t$, where $\{\omega_t\}$ is a white noise series

when $x_0 = 0$,  we have $x_t = w_1 + w_2 + \dots + w_t$

$\mu_x = 0$, $\gamma_k(t) = cov(x_t, x_{t+k}) = t\sigma^2$

$\rho_k(t) = \frac{1}{\sqrt{1+\frac{k}{t}}}$


### The backward shift and difference operator


### Autoregressive process


Stationary and non-stationary AR processes:
