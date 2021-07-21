---
title: Time Series
tags: [Notes]
---
## Lecture 1

#### Dependency

independent iff

-  Discrete Case: $P(X=x_i and Y = y_j) = P(X=x_i)P(Y = y_j)$ 
- Continuous Case: $f_{X, Y}(x,y) = f_X(x)f_Y(y)$

Measuring linear dependency: Pearson's correlation coefficient

- Population Version: $\rho_{X,Y} = \frac{cov(X,Y)}{\sigma_X\sigma_Y}$
- Sample Version: $r=\frac{\sum_{i=1}^{n}\left(x_{i}-\bar{x}\right)\left(y_{i}-\bar{y}\right)}{\sqrt{\sum_{i=1}^{n}\left(x_{i}-\bar{x}\right)^{2}} \sqrt{\sum_{i=1}^{n}\left(y_{i}-\bar{y}\right)^{2}}}$

Measuring monotone dependency: Spearman's rank correlation coefficient

- First take the ranks, and then compute Pearson's correlation using the ranks

Measuring dependency: arbitrary dependency: 

- Brownian covariance

- Mutual information

#### Bayes rule

#### Norms

#### Regression and Variable selection

##### About $R^2$

$R^2$ is defined as Reg SS / Total SS, and can be thought of as the proportion of the variance of y that is explained by x

The issue of overfitting - the smallest Sum of Squared Error is always obtained by using all the predictors at hand.

Adjusted $R^2$: the number of parameters $d$ is penalized ; 

The unadjusted $R^2$ always increases with addition of predictors; while the adjusted $R^2$ may decrease when the new predictor has little contribution to the prediction of y

##### AIC, Akaike's Information Criterion

The better the fit, and the simpler the model (less parameters), the lower the AIC.

Smaller AIC means better model

##### BIC, similar to AIC, but penalizes complex models more heavily than AIC, thus favoring simpler models than AIC

##### Forward Stepwise Selection; Backward; Stepwise(forward-backward); Best subset selection;

#### Validation and Cross-validation(LOOCV, K-fold)

#### Shrinkage Methods: Ridge/Lasso

#### The testing paradigm

- Null and Alternative hypothesis
- Design test statistic that could differentiate the two hypothesis
- Construct a sampling distribution of the test statistic when the null hypothesis is true
- Compare the test statistic against the sampling distribution, compute the p-value (is the probability that the test statistic would be at least as extreme as observed, if the null hypothesis is true)
- If observed statistic deviates from the sampling distribution sufficiently (small enough p-value), we reject the null hypothesis

#### The sampling distribution

#### Permutation test procedure

#### Bootstrap

#### Receiver operating characteristics

## Lecture 2

Overview of Time Series Data

### Intro

- Example time series data：Variables measured sequentially in time(regular time points assumed in this course)
- Our main interest: predict what is going to happen in the near future
  based on `dependencies`:
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

  - `Local weighted average with proper window size`

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

  e.g. Gaussian White Noise (`GWN`) process is covariance stationary (also strictly stationary)

- Two related terms: 

  - Independent White Noise (`IWN`): independent with mean zero  and variance $\sigma^2$;
  - Weak White Noise (`WN`): uncorrelated with mean zero and variance $\sigma^2$

  e.g. Deterministic trending process is not stationary because the mean depends on $t$;  
  Random walk is not stationary because its variance depends on $t$, however, its first difference is covariance stationary.

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

- Assume there is `no systematic trend or seasonal effects` in the process, or that these have been identified and removed
- The mean of the process can change from one time step to the next, but we have no information about the likely direction of these changes. 
- Solution: `borrow information from history`: $x_t = \mu_t + w_t$ , where $\mu_t$ is the non-stationary mean of the process at time $t$ and $w_t$ are independent random deviations with a mean of 0 and a standard deviation $\sigma$
- Let $a_t$ be the estimate of $\mu_t$; estimate of the mean at time $t$ by a weighted average of observation at time $t$ and our estimate of the mean at time $t − 1$.
  $a_t = \alpha x_t + (1-\alpha)a_{t-1}, 0 < \alpha < 1$
-  Set: $a_1=x_1$
  $a_t$  is `a linear combination of the current and past observations`, with `more weight given to the more recent observations`
  these weights form a `geometric series,` and the sum of the infinite series is unity 
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

In relation to model fitting, a white noise series usually arises as a residual series after fitting an appropriate time series model

If a model has accounted for all the serial correlation in the data, the residual series would be serially uncorrelated, so that a correlogram of the residual series would exhibit no obvious patterns - the residuals should be well approximated by white noise. 

### Random Walks

$x_t = x_{t-1} + \omega_t$, where $\{\omega_t\}$ is a white noise series

when $x_0 = 0$,  we have $x_t = w_1 + w_2 + \dots + w_t$

$\mu_x = 0$, $\gamma_k(t) = cov(x_t, x_{t+k}) = t\sigma^2$

$\rho_k(t) = \frac{1}{\sqrt{1+\frac{k}{t}}}$

### The backward shift and difference operator

$\nabla^n = (1-B)^n$

Is an observed series modeled well using random walk? 
The diff series shouldn’t have any autocorrelation structure.

If the diff contains autocorrelation, then there is some structure that is not modeled by random walk.   

### Autoregressive process

- The random walk is the special case AR(1) with $\alpha_1$ = 1
- The exponential smoothing model is the special case $\alpha_i = \alpha(1 −\alpha)^i$  for i = 1, 2, . . . and p → ∞
- The model is a regression of $x_t$ on past terms $\rightarrow$ ‘autoregressive’ 
- The model parameters can be estimated by minimizing the sum of squared errors 

#### Stationary and non-stationary AR processes:

Consider the characteristic equation: $\theta_p(B) = 1 - \alpha_1B - \alpha_2B^2 - \dots - \alpha_pB^p = 0$, Treat B as a number and find roots.

The roots must `all exceed unity in absolute value` for the process to be stationary.

e.g.: - Random walk, B=1 $\rightarrow$ non-stationary

​           AR(1): $x_t = \frac{1}{2}x_{t-1} + w_t \rightarrow 1 - \frac{1}{2}B = 0 \rightarrow B = 2 > 1$: Stationary	

​           AR process: $x_t = -\frac{1}{4}x_{t-2} + w_t \rightarrow 1+\frac{1}{4}B^2=0 \rightarrow B=\pm2i$, and $|B| = 4 > 1$: Stationary

#### AR(1) properties:

A stable AR(1) process ($|\alpha| < 1$)
$x_t = \alpha x_{t-1} + w_t \rightarrow (1-\alpha B)x_t = w_t \rightarrow$ 
$x_t = (1 - \alpha B)^{-1}w_t = (1 + \alpha B + \alpha^2 B^2 + \alpha^3 B^3 + \dots)w_t = \sum_{i=0}^{\infty}\alpha^iw_{t-i}$

- Then, $E(x_t) = E(\sum_{i=0}^{\infty}\alpha^iw_{t-i}) = \sum_{i=0}^{\infty}\alpha^iE(w_{t-i}) = 0$

- Autocovariance:
  $\gamma_k = Cov(x_t, x_{t+k}) = Cov(\sum_{i=0}^{\infty}\alpha^iw_{t-i}, \sum_{j=0}^{\infty}\alpha^jw_{t+k-j}) $ 

  $= \sum_{i=0}^{\infty}\sum_{j=k+i}\alpha^i\alpha^jCov(w_{t-i}, w_{t+k-j}) = \alpha^k\sigma^2\sum_{i=0}^{\infty}\alpha^{2i} = \alpha^k\sigma^2/(1-\alpha^2)$

- Autocorrelation
  $Var(x_t) = \gamma_0 = \sigma^2/(1-\alpha^2) \rightarrow \rho_k=\alpha^k$

- Partial Autocorrelation

#### Fitting of an AR model

The AR model has no deterministic trend component, 

the trends in the data can be explained by serial correlation and random variation, implying that it is possible that these trends are stochastic. 

This does not imply that there is no underlying reason for the trends 

If a scientific reason for a trend is known, it should factor into the modelling/prediction.



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
- The VAR model has many parameters (!): $N+p\times N^2$ ($N:$number of variables; $p:$order of the AR model)
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
  An MA process is `invertible` if it can be expressed as a stationary autoregressive process of infinite order
  - In general, an MA(q) process is invertible when the roots of $\phi_q(B)$ `all exceed unity in absolute value`
  - The autocovariance function `only` identifies a `unique` MA(q) process if the condition that the process be invertible is imposed
- MA models cannot be expressed in a multiple regression form
  Parameters are estimated with a numerical algorithm


### ARMA Model
autoregressive moving average (ARMA) process of order (p, q), denoted ARMA(p, q): $\theta_p(B)x_t=\phi_q(B)w_t$
- The AR(p) model is the special case ARMA(p, 0)
  The MA(q) model is the special case ARMA(0, q)
  An ARMA model will often be more parameter efficient (i.e., require fewer parameters) than a single MA or AR model

 - The process is `stationary` when the roots of $θ$ all exceed unity in absolute value.
   The process is `invertible` when the roots of $φ$ all exceed unity in absolute value.
   When $θ$ and $φ$ share a `common factor`, a stationary model can be `simplified`. : $(1 - \frac{1}{2}B)(1 - \frac{1}{3}B)x_t=(1 - \frac{1}{2}B)w_t \Rightarrow (1 - \frac{1}{3}B)x_t=w_t$


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

- `Box-Cox transformation `
- Forecasting:
  - Expand the ARIMA equation so that xt is on the left hand side and all other terms are on the right.
  - Rewrite the equation by replacing t with T+1.
  - On the right hand side of the equation, replace future observations with their forecasts, `future errors with zero`, `and past errors with the corresponding residuals`.
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
### ARIMA with External Variables

### conditional heteroskedasticity

- In some areas, the daily return seems to be of `higher variance`. This is` not captured in ACF or PACF plots`. If we only observe the ACF plots, it appears as if the white noise model is appropriate 
- This is `NOT addressed by an ARIMA fit`. This is again NOT addressed by an ARIMA fit, this time with differencing.
- “Volatility clustering” 
  - Variance `NOT constant` over time
  - Samples are `NOT independently and identically distributed`, even though they don’t appear to be auto-correlated 
  - The variance is `correlated` in time, the series exhibits volatility and is called `“conditional heteroskedastic”`. 
  -  Volatility can be `detected by looking at the correlogram of the squared values` since the squared values are equivalent to the variance when mean is zero.

### ARCH

An autoregressive model for the variance process - AutoRegressive Conditional Heteroskedastic (ARCH) model 

- ARCH(1) Model: $\epsilon_t = \omega_t\sqrt{\alpha_0+\alpha_1\epsilon_{t-1}^2}$, $\{\omega_t\}$ is white noise with zero mean and unit variance
- The variance at time t depends on variance at time t-1:
  $E[\epsilon_t]=0$, $Var(\epsilon_t) = E(\epsilon_t^2) = E(\omega_t^2)E(\alpha_0+\alpha_1\epsilon_{t-1}^2)=E(\alpha_0+\alpha_1\epsilon_{t-1}^2)$
  Note: $E(XY)=E(X)E(Y)$ holds if $X$ and $Y$ are independent
- Note: 
  The ACF of $\{\epsilon_t^2\}$ behaves just like the ACF of an AR(1) data. 
  $\{\epsilon_t\}$ should be uncorrelated, should contain no trends or seasonal changes – the residual obtained after fitting a satisfactory (seasonal) ARIMA model.

### GARCH [参考布朗尼](https://machinelearningmastery.com/develop-arch-and-garch-models-for-time-series-forecasting-in-python/)

`Generalised ARCH model` - GARCH(q, p): $h_t = \alpha_0 + \sum_{i=1}^p\alpha_i\epsilon_{t-1}^2+\sum_{j=1}^q\beta_jh_{t-j}$, and $\epsilon_t = \omega_t\sqrt{h_t}$

$h$ is the variance, $\epsilon$ is the sample

What exactly is the “residual”?

- The fitted values: 
  	conditional standard deviation $\sqrt{h_t}$
- The residuals: $\epsilon_t/\sqrt{h_t}$

- If all $h_t$ correctly captures the conditional variance, the residuals will be white noise with mean 0 and variance 1. 

With real data, usually a GARCH(1,1) is sufficient

- Forecast from GARCH model?
  - GARCH fits a model to the residual errors of a fitted time series model
  - Its fitted values have zero mean – `no influence on the mean forecast`
  - It could make small changes to the width of the confidence band of the forecast
  - Procedures are available to make `ARIMA+GARCH forecasts`, e.g. forecastGARCH()

- Forecasting is `not` of major interest with GARCH
  - The main application of GARCH models is for` simulation studies`
  - GARCH simulation can make the simulated data `closer to reality`
  - Especially in finance, insurance, teletraffic, and climatology.

## Lecture 10

### Spectral Analysis

Analyze frequency properties of time series - "frequency domain"

Previous methods were operating in the "time domain"

More useful in acoustics, communications engineering, geophysical science, and biomedical science etc.

### Some reminders

- Does STL capture periodic signal? It can, but we have to tell it the periodicity.
- Does Holt-Winters capture periodic signal? It can, but we have to tell it the periodicity.
- `STL and Holt-Winters pretty much “copy” the pattern in the data, and repeat it in the future.`
- Does ARIMA capture periodic signal? Partially. Unless we tell it the period
- Does machine learning methods capture periodic signal? Possibly, by learning nonlinear relations with past time points.
  The model is flexible – more data points are needed to make the model stable.
- In a noisy data, we can’t even see the periodicity by the eye. How do we find out?

### Mixture of sine functions

Joseph Fourier (1768–1830) showed that sums of sine waves can provide good approximations to most periodic signals.

E.G.

- A simple cosine wave: $Rcos(2\pi ft+\Phi)$, where $R(>0)$ is the amplitude, $f$ is the frequency and $\Phi$ is the phase.

- A linear combination of the two waves

How to Figure out which sine waves were used to generate the data?

- $Rcos(2\pi ft+\Phi) = Acos(2\pi ft)+Bsin(2\pi ft)$, where $A=Rcos(\Phi)$, $B=-Rsin(\Phi)$ and $R=\sqrt{A^2 + B^2}$

- For a fixed frequency f, we can use cos(2πft) and sin(2πft) as predictor variables, and apply least squares regression to find A and B. 
- `Fourier Series`

### Periodogram

- The periodogram $I$ ar frequency $f=\frac{j}{n}$:
  - for $j<\frac{n}{2}$, $I(\frac{j}{n}) = \frac{n}{2}(\hat{A}_j^2+\hat{B}_j^2)$
  - for $k=\frac{n}{2}$(if $n$ is even), $I(\frac{1}{2}) = n(\hat{A}_k)^2$ 

- The height of the periodogram shows the relative strength of cosine-sine pairs

- The total sum of squares in Y is attributed to the  individual frequencies: $\sum_{j=1}^n(Y_j - \bar{Y})^2 = \sum_{j=1}^kI(\frac{j}{n})$

- Why limit to $0 ～ \frac{1}{2}$?: 

  - Given limited data, other frequencies are “aliased” with a frequency in this range
  - f within the interval 0 to 1/2 will be aliased with each frequency of the form f + m/2 for any positive integer m
  - The actual calculation is done by Fast Fourier transformation

- What if the true frequency is not a Fourier frequency?: $f=0.088$ is not a Fourier frequency

- Stationary processes may be represented as linear combinations of infinitely many cosine-sine pairs over a continuous frequency band. 
  We use a discrete example for simplicity:Lecture10.pptx page 18

  

### Spectrum of a time series

- The periodogram is a sample estimate of `the population spectral density` - a `frequency domain characterization` of a population stationary time series.
- An observed time series is of limited length $\rightarrow$ it can only estimate a `discrete periodogram`
- The` population` can be thought of as an `infinite number time series` bearing the same properties, each with infinite length $\rightarrow$ the count of f and A/B pairs in the previous page become infinite
- Single realizations of the series yields estimates of the spectral density
- In this class, we are not interested in the theoretical derivation of spectral density. Rather we focus on sample versions.
- Strategies for estimating the spectral density from a single time series
  - Find sample periodogram, and use smoother
    - “modified Daniell smoother” (center-weighted moving average) 
    - Or other kernel smoothers
  - Model the stationary time series as an AR process of some order (might be a high order). 
    - Find the AR model 
    - Generate the theoretical spectral density of the AR model as the estimate. 

- E.G. 
  - wave tank data: The aim of the analysis is to check whether the spectrum is a realistic emulation of typical sea spectra.
  - Fault detection on eletric motors
  - Weather patterns





## Lecture 11

### Markov Chain

#### Purpose

A nonstationary time series with different stationary segments. How do we delineate the segments, and possibly make a judgement of current state?

#### Discrete time finite Markov Chain

Possible states: finite discrete set S {E1, E2, … … , Es}
	From time t to t+1, make stochastic movement from one state to another

The Markov Property: 
	At time t, the process is at Ej,
	Then at time t+1, the probability it is at Ek only depends on Ej

The temporally homogeneous transition probabilities property:
	Prob(Ej → Ek) is `independent of time t.`

A: Transition Probability Matrix

An n step trasition $A_{ij}(n) := p(X_{t+n}=j|X_t=i)$
It can be shown that $A(n)=A^n$
Proof: Consider the two stage transition probability from Ei to Ej:
$A_{ij}(m+n)=\sum_{k=1}^{K}A_{ik}(m)A_{kj}(n)$, the probability of getting from i to j in m + n steps is the probability of getting from i to k in m steps, and then from k to j in n steps, summed over all k. So, $A(m+n)=A(m)A(n)$, thus $A(n) = AA(n-1)=AAA(n-2)=\cdots=A^n$

e.g. language modeling
Define the state space to be all the words in English 
The marginal probabilities  p(Xt = k) are called `unigram statistics` 
If we use a first-order Markov model, then 
p(Xt = k | Xt−1 = j) is called a `bigram model`. 
If we use a second-order Markov model, then 
p(Xt = k | Xt−1 = j,Xt−2 = i) is called a `trigram model`. 
Language models can be used for sentence completion, data compression, text classification, automatic writing etc.

The `stationary state`:
One step transition: $A_{ij}=p(X_t=j|X_{t-1}=i)$
Probability of being at state j at time t (row vector): $\pi_t(j)=p(X_t=j)$
If we can reach this situation, $\pi=\pi A$
The probability of being at each state `stays constant`. 

For `finite, aperiodic, irreducible` Markov chains,  stationary state  `exists` and is `unique`. 
`Periodic`: if a state can only be returned to at t0, 2t0, 3t0,…….. , t0>1
`Irreducible`: any state can be eventually reached from any state

Note: 有限状态，非周期，常返的马尔可夫链有唯一的稳定状态;稳定状态的意思是说，状态的概率不再改变

The` stationary state can be found` by consider $A^n$ where we let n goes to $\infty$.

### Hidden Markov Model

More general than Markov model.
A discrete time Markov Model `with extra features`.

The `emissions` are: probabilistic; state-dependent ; time-independent

$p\left(\mathbf{z}_{1: T}, \mathbf{x}_{1: T}\right)=p\left(\mathbf{z}_{1: T}\right) p\left(\mathbf{x}_{1: T} \mid \mathbf{z}_{1: T}\right)$ $=\left[p\left(z_{1}\right) \prod_{t=2}^{T} p\left(z_{t} \mid z_{t-1}\right)\right]\left[\prod_{t=1}^{T} p\left(\mathbf{x}_{t} \mid z_{t}\right)\right]$

(Page 16)We don’t observe the states of the Markov Chain, but we observe the emissions.
If both E1 and E2 have the same chance to initiate the chain, and we observe sequence “BBB”, what is the most likely state sequence that the chain went through?

Let Q denote the state sequence, and O denote the observed emission.  Our goal is to find: $\underset{Q}{\arg \max } P(Q \mid O)$ where $P(Q \mid O)=\frac{P(O \mid Q) P(Q)}{\sum_{Q} P(O \mid Q) P(Q)}$
 `Parameters:` $\lambda = \{\pi, P, B\}$, where they stand for initial distribution, trasition probabilities and emission probabilities

Common questions:

- How to efficiently calculate emissions: $P(O|\lambda)$
- How to find the most likely hidden state: $\underset{Q}{\arg \max } P(Q \mid O)$
- How to find the most likely parameters:$\underset{\lambda}{\arg \max } P(O \mid \lambda)$

Note: The Forward and Backward Algorithm

#### Very very brief Intro to dynamic programming

Dynamic programming:
Breaking the overall optimization problem into overlapping smaller problems;
Solve each sub-problem once, and reuse the results, thus reducing the computing cost (dramatically); Often working backward.

### Viterbi Algorithm

to maximize the conditional probability, we can simply maximize the joint probability. (By Conditional Proba. Formula)

## Lecture 12

### Estimation in Hidden Markov Model

#### Hidden Markov Model

- Do not observe markov chain, rather other things:
  - $\pi$: initial distribution; $P$:transition probablities; $B$: emission probabilities

- Common Questions:

  - How to efficiently calculate `emissions`: $P(O|\lambda)$
  - How to find the most likely hidden `state`: $\operatorname{argmax}_QP(Q|O)$
  - How to find the most likely `parameters`:$\operatorname{argmax}_{\lambda}P(O|\lambda)$

  where the notations:

  - $Q = \{q^{(1)},\dots, q^{(T)}\}$ denotes the `hidden state sequence`
  - $O = \{O_{1},\dots, O_{T}\}$ denotes the `observe emission sequence`
  - $O^{(t)} = \{O_{1},\dots, O_{t}\}$ denotes `the emission up to time t`

  Note: `emission prob.` a.k.a. `output prob`.

#### Forward and Backward variables

- Forward; Backward:

#### Posterior state prob.

#### The Viterbi Algorithm (shortest path - like)

To find: $\operatorname{argmax}_QP(Q|O)$, is simply to maximize the join probability

...

e.g. 0.15 = max(0.5\*0.5\*0.9\*0.5,    0.5\*0.75\*0.8\*0.5 = 0.15)

Note: Initial: 0.5 v.s. 0.5

#### The Estimation of Parameters

Given the topology of an HMM, to find the initial distribution, the transition probabilities and the emission probabilities, from a set of emitted sequences.

#### The basic idea

There are three pieces: λ, O, and Q. O is observed. We can iterate between λ and Q.
When λ is fixed, we can do one of two things:
	(1) find the best Q given O or (2) integrate out Q
When Q is fixed, we can estimate λ using frequencies.

#### Viterbi training

Steps:

- Start with some initial parameters
- Find the most likely paths using the Viterbi algorithm.$\delta_{t}(i)=\max _{q_{1}, q_{2}, \ldots, q_{t-1}} P\left(q_{1}, q_{2}, \ldots \ldots, q_{t-1}, q_{t}=E i\right.$, and $\left.O^{(t)}\right)$
  $\max _{Q} P(Q, O)=\max _{i} \delta_{T}(i)$
- Re-estimate the parameters. 
  	On next page.
- Iterate `until the most likely paths don’t change`.

Estimating the parameters:

- Transition prob.
- Emission prob.
- Initial prob.

#### Baum-Welch Algo

- The Baum-Welch is a special case of the EM algorithm.

- The Viterbi training is equivalent to a simplified version of the EM algorithm, replacing the expectation with the most likely classification. 

- General flow:

  (1) Start with some initial parameters
  (2) Given these parameters, P(Q|O) is known for every Q. This is done effectively by calculating the forward and the backward variables. 
  (3) Take expectations to obtain new parameter estimates. 
  (4) Iterate (2) and (3) until convergence.

### A few examples

- Market Regimes: “long-term, persistent states that can be utilized for making investments or trading decisions.”



### State Space Model

- use state variable to describe a system
- State variables can be estimated from the data, but are not measured directly

> The Kalman filter;    HMM;    Bayesian structural time series



### Kalman filter

- Only some of these state variables can be measured directly, and these measurements are subject to noise.
- The objective is to infer values for all the state variables from the noisy measurements. 
- The estimated values of the state variables are then used to control the system.
- The parameters can be estimated from the observed time series
  The Kalman filter can be extended to
  	- regression with time-varying coefficients
  	- ARMA process
  	- multivariate time series
  	… …
  	Strength – flexible
  	Weakness – too flexible with too many parameters

## Lecture 13

### Brief summary about models

- Capability to learn highly abstract representations
- Need expert input to select representations
- Good performance on testing data, which is independent from the training data, is most important for a model. (It serves as the basis in model selection.)
- Goals in model building
  - (1) Model selection: 
    	Estimating the performance of different models; choose the best one
  - (2) Model assessment:
    	Estimate the prediction error of the chosen model on new data.

- Ideally, we’d like to have enough data to be divided into three sets:
  	Training set: to fit the models
  	Validation set: to estimate prediction error of models, for the purpose of model selection
  	Test set: to assess the generalization error of the final model
- Bias-variance trade-off

### Tasks for ML in time series data analysis

- `Time series clustering`
  - Need to measure similarities between time series
    Example: financial series
- `Time series classification`
  - Need to measure similarities between time series in relation to the outcome variable
    Example: EEG and other medical measurements	
- `Time series forecasting`
  - Need to model dependency along time
    Example: financial series

### Measuring similarity between two time series

#### Dynamic Time Warping (DTW)

Let t and r be two vectors of lengths m and n, respectively.

Goal: find a monotonically non-decreasing mapping path {(p1,q1),(p2,q2),...,(pk,qk)}, such that the distance on this mapping path ∑|t(pi)−r(qi)| is minimized. 
Boundary conditions: $(p_1, q_1) = (1, 1), (p_k, q_k) = (m, n)$

Solution: dynamic programming
Define: D(i,j) is the DTW distance between t(1:i) and r(1:j), with the mapping path starting from (1,1) to (i,j), D(1,1)=|t(1)−r(1)|.

Iterate from $(1,1)$ :
$$
D(i, j)=|t(i)-r(j)|+\min \left\{\begin{array}{c}
D(i-1, j) \\
D(i-1, j-1) \\
D(i, j-1)
\end{array}\right\}
$$

### Clustering

- Hierarchical; k-means; k-medoids
- These clusters (or selected neighbors) could serve as predictors for forecasting.
- Example: using sub-segment DTW to select historical time frames that are similar to the current data; use them for forecasting 

### Classification

- Use original x: Keep time information ; 1-D convolutional NN	; Ignore time information
  	RF, SVM, xgBoost etc, potentially with variable selection

- Use engineered features: Spectral features; Spatial features depending on the domain knowledge

### Forecasting

- Simple consideration: 
  use previous time points to predict the next time point
  could use multiple variables

- Feature Engineering:
  	If the learner is not trusted to learn the structure from limited data and too many variables
  	Example: 
  		time average, Seasonal lagged time points, seasonal differences
  		Segment-wise average

### More

- KNN Regression
- Support Vector Regression
- Regression Tree
- Tree Ensembles
- 

