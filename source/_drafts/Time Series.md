---
title: Time Series
tags: [Notes]
---
## Lecture 1

Dependency
Bayes rule
Regression and Variable selection
Permutation
Bootstrap
Receiver operating characteristics

## Lecture 2

Overview of Time Series Data

### Intro

- Example time series data：Variables measured sequentially in time(regular time points assumed in this course)
- Our main interest: predict what is going to happen in the near future
  based on dependencies:
  - this variable’s conditional distribution on it’s own past
  - This variable’s conditional distribution given some other variables’ past

### Important Terms

- Understand the data
  - `Trend`: a systematic change in a time series that does not appear to be periodic may or may not be linear; sometimes approximated by linear reasonably well
  - `forecasting`: forecasting is an `extrapolation`  (going outside the range of current data)
  - `seasonal variation`: a repeating pattern, not necessarily with a 1-year periodicity
    		example: weekly variation of restaurant bookings
  - `cycles`: A pattern with a rough periodicity, but not a fixed one 
            example: El Nino effect, economic cycles
  - `aggregate`, `taking a subset of specific month/range`, `multiple time series data`, `Intersection between time series`, `Stochastic trend`, `Partial trend`

### Decomposition

- Decomposition of series

  - `Additive Model`: $x_t = m_t + s_t + z_t$ 
    where $m_t$ is the trend, $s_t$ is the seasonal effect, $z_t$ is an error term (a sequence of correlated random variables with mean zero)
    Note: 

    - If the seasonal effect tends to increase as the trend increases: $x_t = m_t \times s_t + z_t$
    - If the random variation is modelled by a multiplicative factor and the 
      variable is positive, we can use: $\operatorname{log}(x_t) = m_t + s_t + z_t$  (`Multiplicative`decomposition)

  - Local weighted average with proper window size

    - wide enough to average out seasonal effects
    - not too wide to cause bias

    Note: close to a kernel smoother with box kernel

  - Classical decomposition: Additive decomposition; Multiplicative decomposition

  - `STL`: Seasonal and Trend decomposition using Loess

    - Handles any type of seasonality, not only monthly and quarterly data.
    - The seasonal component is allowed to change over time, and the rate of change can be controlled by the user.
    - The smoothness of the trend-cycle can also be controlled by the user.
    - It can be robust to outliers.

    



## Lecture 3

### Stationary and ergodic time series

- Understanding a time series observation as a realization of a stochastic process

- `Stationary in the mean`: mean doesn't change over time. i.e. $E[Y_t] = \mu$ does not depend on $t$

- `Covariance (weak) stationary`: variance-covariance structure doesn't change over time i.e. $E[Y_t] = \mu$ does not depend on $t$; $var[Y_t] = \sigma^2$ does not depend on $t$; $cov(Y_t, Y_{t-j}) = \gamma_j$ exists, is finite, and depends only on $j$ but not on $t$, for $j=0, 1, 2, \dots$

- `Strict stationary`: the distribution does not change over time

  e.g. Gaussian White Noise (GWN) process is covariance stationary (also strictly stationary)

- Two related terms: 

  - Independent White Noise (IWN): independent with mean zero  and variance $\sigma^2$;
  - Weak White Noise (WN): uncorrelated with mean zero and variance $\sigma^2$

  e.g. Deterministic trending process is not stationary because the mean depends on $t$;  Random walk is not stationary because its variance depends on $t$, however, its first difference is covariance stationary.

- `Ergodicity`: 遍历性，各态遍历性；指统计结果在时间和空间上的统一性，表现为时间均值等于空间均值。
  $Y_t$ and $Y_{t-j}$ are essentially independent if $j$ is large enough
  e.g. of covariance stationary but not ergodic



### Auto correlation and Partial auto correlation

- variance function of a stationary time series: 
  - population version - $\sigma^2 = E[(x_t - \mu)^2]$
  - sample version - $\operatorname{Var}(x) = \frac{\sum_{i=1}^{n}(x_t-\bar{x})^2}{n-1}$

- Covariacne: $\gamma(x, y) = E[(x-\mu_x)(y-\mu_y)]$(Population Version); $\operatorname{Cov}(x, y) = \sum(x_i-\bar{x})(y_i-\bar{y})/(n-1)$ (Sample Version)

- Correlation: $\rho(x, y) = \frac{\gamma(x,y)}{\sigma_x\sigma_y}$

- Auto-CoVariance Function (ACVF): P-version: $\gamma_k = E[(x_t-\mu)(x_{t+k}-\mu)] $; S-version: $c_k = \frac{1}{n}\sum_{t=1}^{n-k}(x_t-\bar{x})(x_{t+k}-\bar{x})$

- Auto-Correlation Function (ACF): P: $\rho_k = \frac{\gamma_k}{\sigma^2}$; S: $r_k = \frac{c_k}{c_0}$
  Some points for interpretations:
  - The lag 0 autocorrelation is always 1
  - statistically significant values at specific lags that have some practical meaning 
    	Ex: For monthly series, a significant autocorrelation at lag 12 might indicate that the seasonal adjustment is not adequate. 
  - Squaring the autocorrelation  -> percentage of variability explained by a linear relationship between the variables. 
    	Ex: a lag 1 autocorrelation of 0.1 implies that a linear dependency of $x_t$ on $x_{t−1}$  would only explain 1% ($0.1\times 0.1 $)of the variability of $x_t$. 
- Patial Correlation
  Correlation between two random variables, with the effect of a set of controlling random variables removed. 
  - `Correlation`: The length of projection of $x$ onto $y=cos(\phi)$
    Standardize doesn't change the correlation
  - `Partial correlation`: First project $x$ and $y$ onto the subspace orthogonal to $z$,
    (or take residual against $z$), and then find correlation between the residuals
  - The `partial autocorrelation` of a time series for a given lag is the partial correlation of the time series with itself at that lag given all the information between the two points in time. 

### Cross correlation

- Cross Covariance function (ccvf): $\gamma_k(x, y) = E[(x_{t+k} - \mu_X)(y_t - \mu_y)]$
  Note: $\gamma_k(x, y) = \gamma_{-k}(y, x)$
  Sample Version: $c_k(x, y) = \frac{1}{n}\sum_{t=1}^{n-k}(x_{t+k}-\bar{x})(y_t-\bar{y})$
- Cross Correlation function (ccf): $\rho_k(x, y) = \frac{\gamma_k(x, y)}{\sigma_x\sigma_y}$





## Lecture 4

Some general forecasting strategies:

### Leading Variable Approach

- Find a related variable that leads the variable of interest by one or more time intervals. 
  The closer the relationship and the longer the lead time, the better the forecast. 

  What if a leading variable cannot be found?
  	Find a variable that is associated with the variable of interest, and is easier to predict. 

Example: 
	Interested in predicting: value of building work done - “Activity” 
	Potential leading variable: total dwellings approved - “Approvals” 

- Naive approach to predict
  $y_t = m_{y_t} + s_{y_t} + r_{y_t}, \quad x_t = m_{x_t} + s_{x_t} + r_{x_t}, \quad r_{y_t} = \beta_0+\beta_1r_{x_{t-1}} + \epsilon_t $

  At time t, use the following information to forecast $y_{t+1}$ :

  ​			the previous trend/seasonal variation up to $y_t$, 

  ​			the current $x_t$ value


  Do the prediction for one future time point only.

### Bass Model

- Theory of adoption and diffusion of a new product by society:
  Among all eventual adopters of the product, F(t) and f(t) are the cumulative and probability density function of the distribution of time until purchase.

- the probability that a random purchaser who has not yet made the purchase will do so in the next small time increment: $\frac{f(t)}{1-F(t)} = p + qF(t)$
  where $p$ is the coefficient of innovation and $q$ is the coefficient of imitation
- The intuition of the model: 
  Initial sales will be to people who are interested in the novelty of the product, whereas later sales will be to people who are drawn to the product after seeing their friends and acquaintances use it. 
- The discrete time version
- Solution

### Exponential Smoothing

- Assume there is no systematic trend or seasonal effects in the process, or that these have been identified and removed
- The mean of the process can change from one time step to the next, but we have no information about the likely direction of these changes. 
- Solution: `borrow information from history`: $x_t = \mu_t + w_t$ , where $\mu_t$ is the non-stationary mean of the process at time $t$ and $w_t$ are independent random deviations with a mean of 0 and a standard deviation $\sigma$
- Let $a_t$ be the estimate of $\mu_t$; estimate of the mean at time $t$ by a weighted average of observation at time $t$ and our estimate of the mean at time $t − 1$.
  $a_t = \alpha x_t + (1-\alpha)a_{t-1}, 0 < \alpha < 1$
-  Set: $a_1=x_1$
  $a_t$  is a linear combination of the current and past observations, with more weight given to the more recent observations
  these weights form a geometric series, and the sum of the infinite series is unity 
- Forecasting: $\hat{x}_{n+k|n}=a_n, k=1, 2, \dots$ （由n处的值来预测n+k处的值）
- Exponential smoothing is a special case of the Holt-Winters algorithm 

### Holt-Winters Method

- Models trend and seasonal variation
  $$
  \begin{array}{l}
  a_{t}=\alpha\left(x_{t}-s_{t-p}\right)+(1-\alpha)\left(a_{t-1}+b_{t-1}\right) \\
  b_{t}=\beta\left(a_{t}-a_{t-1}\right)+(1-\beta) b_{t-1} \\
  s_{t}=\gamma\left(x_{t}-a_{t}\right)+(1-\gamma) s_{t-p}
  \end{array}
  $$
  $\mathrm{a}_{\mathrm{t}}$ - estimated level at time $\mathrm{t}$
  $b_{t}$ - slope at time t, change in level from one time period to the next
  $\mathrm{s}_{\mathrm{t}}$ - seasonal level at time $\mathrm{t}$ 

  $p-$ period (known)
  a, $\beta$, and $\gamma$ are the smoothing parameters

- Typical choices of the weights $\beta$ and $y$ are 0.2 . The updating equations can be started with a $=\mathrm{x}_{1}$ and initial slope, $\mathrm{b}_{1}$, and seasonal effects, $\mathrm{s}_{1}, \ldots, \mathrm{s}_{\mathrm{p}}$, reckoned from experience, estimated from the data in some way, or set at 0 .
  Forcasting:
  $$
  \hat{x}_{n+k \mid n}=a_{n}+k b_{n}+s_{n+k-p}
  $$

- Multiplicative seasonal model:
  The Holt-Winters algorithm with multiplicative seasonals is
  $$
  \left.\begin{array}{l}
  a_{n}=\alpha\left(\frac{x_{n}}{s_{n-p}}\right)+(1-\alpha)\left(a_{n-1}+b_{n-1}\right) \\
  b_{n}=\beta\left(a_{n}-a_{n-1}\right)+(1-\beta) b_{n-1} \\
  s_{n}=\gamma\left(\frac{x_{n}}{a_{n}}\right)+(1-\gamma) s_{n-p}
  \end{array}\right\}
  $$
  Forecasting:
  $$
  \hat{x}_{n+k \mid n}=\left(a_{n}+k b_{n}\right) s_{n+k-p} \quad k \leq p
  $$



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



## Lecture 6
### Vector AR Models
- Ljung-Box test: an overall test of the randomness of a time series.
  - $H_0$ : The data does not exhibit serial correlation.
  -  $H_1$ : The data does exhibit serial correlation.
  -  The statistic: $Q=n(n+2) \sum_{h=1}^{H} \frac{\hat{\rho}_{e}^{2}(h)}{n-h}$
     H is chosen somewhat arbitrarily, typically, H=20
    - Under the null hypothesis, asymptotically $(n \rightarrow \infty)$,  for a series, $Q ～\chi^2_H$; for residuals of a model fit with p+q parameters, $Q ～\chi^2_{H-p-q}$

-  Using AR model to predict long into the future will require the use of forecasted future values as inputs into our prediction.
   Predicting too long into the future will produce values that approach the mean value of the series
  - Suppose there is information they can borrow from each other, how to make maximal use of it?
    One way is to generate AR(p) model of multiple variables. (There are other ways we will discuss in state-space models)
    `The formula see PPT`
- The VAR is symmetric in all variables
- The VAR model has many parameters (!): $N+p\times N^2$
  (Use such a model only when we really believe there is such a relationship.)



### MA Process
A moving average (MA) process of order q:
$$
x_{t}=w_{t}+\beta_{1} w_{t-1}+\ldots+\beta_{q} w_{t-q}
$$
$\left\{\mathrm{w}_{\mathrm{t}}\right\}$ is white noise with zero mean and variance $\sigma_{\mathrm{w}}^{2}$
Rewritten in terms of the backward shift operator B:
$$
x_{t}=\left(1+\beta_{1} \mathbf{B}+\beta_{2} \mathbf{B}^{2}+\cdots+\beta_{q} \mathbf{B}^{q}\right) w_{t}=\phi_{q}(\mathbf{B}) w_{t}
$$
$\varphi_{q}$ is a polynomial of order $q$
MA processes are stationary and hence have a time-invariant mean and autocovariance.
$E\left(x_{t}\right)=0$, $\operatorname{Var}\left(x_{t}\right)=\sigma_{w}^{2}\left(1+\beta_{1}^{2}+\ldots+\beta_{q}^{2}\right)$, $\rho(k)=\left\{\begin{array}{cc}1 & k=0 \\ \sum_{i=0}^{q-k} \beta_{i} \beta_{i+k} / \sum_{i=0}^{q} \beta_{i}^{2} & k=1, \ldots, q \\ 0 & k>q\end{array}\right.$ where $\beta_0$ is unity

- Invertible
  An MA process is invertible if it can be expressed as a stationary autoregressive process of infinite order
  - In general, an MA(q) process is invertible when the roots of φq(B) all exceed unity in absolute value
  - The autocovariance function only identifies a unique MA(q) process if the condition that the process be invertible is imposed
- MA models cannot be expressed in a multiple regression form
  Parameters are estimated with a numerical algorithm


### ARMA Model
autoregressive moving average (ARMA) process of order (p, q), denoted ARMA(p, q): $\theta_p(B)x_t=\phi_q(B)w_t$
- The AR(p) model is the special case ARMA(p, 0)
  The MA(q) model is the special case ARMA(0, q)
  An ARMA model will often be more parameter efficient (i.e., require fewer parameters) than a single MA or AR model

 - The process is `stationary` when the roots of $θ$ all exceed unity in absolute value.
   The process is `invertible` when the roots of $φ$ all exceed unity in absolute value.
   When $θ$ and $φ$ share a common factor, a stationary model can be simplified. : $(1 - \frac{1}{2}B)(1 - \frac{1}{3}B)x_t=(1 - \frac{1}{2}B)w_t \Rightarrow (1 - \frac{1}{3}B)x_t=w_t$


## Lecture 7
### Differencing
For non-stationary data
Data transformation may be helpful
Seasonal differencing
Sometimes both a seasonal difference and a first difference to obtain stationary data
- Check whether stationary or not
  -  `unit root test`: statistical hypothesis tests of stationarity that are designed for determining whether differencing is required.
  - A linear time series is nonstationary if there is a unit root
  - Dickey–Fuller test with no drift
      The test of whether 𝜙 = 1 is a simple one-sided t-test of whether the parameter on the lagged yt–1 is equal to 0
      H0: the series is NOT stationary (𝜙 = 1)
      H1: the series is stationary (𝜙 < 1)
   - Augmented Dickey-Fuller test (allows other models and shifts)
   - Kwiatkowski-Phillips-Schmidt-Shin (KPSS) test
     H0: the series is stationary

### ARIMA
A series $\{x_t\}$ is integrated(the reverse of differencing
) of order d, denoted as I(d), if the dth difference of $\{x_t\}$ is white noise $\{w_t\}$

AutoRegressive Integrated Moving Average (ARIMA)
$y_{t}=(1-\mathbf{B})^{d} x_{t}$
$\mathrm{y}_{t}=a_{1} \mathrm{y}_{t-1}+a_{2} \mathrm{y}_{t-2}+\ldots+a_{p} \mathrm{y}_{t-p}+w_{t}+\beta_{1} w_{t-1}+\beta_{2} w_{t-2}+\ldots+\beta_{q} w_{t-q}$

- Box-Cox transformation
- Forecasting:
  - Expand the ARIMA equation so that xt is on the left hand side and all other terms are on the right.
  - Rewrite the equation by replacing t with T+1.
  - On the right hand side of the equation, replace future observations with their forecasts, future errors with zero, and past errors with the corresponding residuals.
  - Beginning with h=1, these steps are then repeated for h=2,3,… until all forecasts have been calculated.

## Lecture 8
### Seasonal ARIMA
Seasonal ARIMA model uses differencing at a lag equal to the number of seasons (s)
Reminder: seasonal differencing $(x_t - x_{t-s}) - (x_{t-1} - x_{t-1-s}) = x_t - x_{t-1} - x_{t-s} + x_{t-1-s}$
$\operatorname{ARIMA}(p, d, q)(P, D, Q)_s$ 



### ARIMA with External Predictors
- Extend ARIMA models in order to allow other information
    - forecast the time series of interest y assuming that it has a relationship with other time series x.
    - x may be controllable, easier/earlier to measure, or easier to forecast
      Ex:
      y: daily electricity demand
      x1: temperature (easier to forecast)
      x2: the day of week

      Ex:
      y: monthly sales
      x: total advertising expense (controllable)

  - x can also be time-shifted leading variables

- f we ignore the time-dependency structure, we can use least squares to find the relations by minimizing
  The portion of data unexplained by the external variables can be of ARIMA structure.
  A regression model with ARIMA errors is equivalent to a regression model in differences with ARMA errors

 - Lagged external predictors

## Lecture 9
### More on ARIMA with External Regressors


### ARCH

### GARCH



