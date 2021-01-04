---
title: Notes of Gaussian Processes for Machine Learning
date: 2020-02-08 17:13:14
tags: [GP, ML]
categories: Notes
mathjax: true

---

# Ch6 Relationships between GPs and Other Models

## 6.1 Reproducing Kernel Hilbert Spaces

##### Preliminary

This part covers reproducing kernel Hilbert spaces(RKHSs), which define a Hilbert space of sufficiently-smooth functions corresponding to a given positive semidefinite kernel $k$.

>  **Kernel**: A general name for a function $k$ of two arguments mapping a pair of inputs $\mathbf{x} \in \mathcal{X}, \mathbf{x}^{\prime} \in \mathcal{X}$ into $\mathbb{R}$ is a kernel.
>  (This term arises in the theory of integral operators, where the operator $T_{k}$ is defined as $\left(T_{k} f\right)(\mathbf{x})=\int_{\mathcal{X}} k\left(\mathbf{x}, \mathbf{x}^{\prime}\right) f\left(\mathbf{x}^{\prime}\right) d \mu\left(\mathbf{x}^{\prime}\right)$, where $\mu$ denotes a measure.)
>
>  **Positive semidefinite**: A kernel is said to be positive semidefinite if 
>  $$
>  \int k\left(\mathbf{x}, \mathbf{x}^{\prime}\right) f(\mathbf{x}) f\left(\mathbf{x}^{\prime}\right) d \mu(\mathbf{x}) d \mu\left(\mathbf{x}^{\prime}\right) \geq 0
>  $$
>   for all $f \in L_{2}(\mathcal{X}, \mu)$

<!-- more -->

##### Formal definition of RKHS.

> ***Def 6.1*** (Reproducing kernel Hilbert space). Let $\mathcal{H}$ be a Hilbert space of real functions $f$ defined on an index set $\mathcal{X}$. Then $\mathcal{H}$ is called a reproducing kernel Hilbert space endowed with an inner product $\langle\cdot, \cdot\rangle_{\mathcal{H}}$ (and norm $\|f\|_{\mathcal{H}} = \sqrt{\langle f, f\rangle_{\mathcal{H}}})$) if there exists a function $k: \mathcal{X} \times \mathcal{X} \rightarrow \mathbb{R}$ with the following properties:
>
> 1. for every $\mathbf{x}, k\left(\mathbf{x}, \mathbf{x}^{\prime}\right)$ as a function of $\mathbf{x^{\prime}}$ belongs to $\mathcal{H},$ and
>
> 2. $k$ has the reproducing property $\langle f(\cdot), k(\cdot, \mathbf{x})\rangle_{\mathcal{H}}=f(\mathbf{x})$.
>
> > Note: As $k(\mathbf{x}, \cdot)$ and $k\left(\mathbf{x}^{\prime}, \cdot\right)$ are in $\mathcal{H}$, we have that $\left\langle k(\mathbf{x}, \cdot), k\left(\mathbf{x}^{\prime}, \cdot\right)\right\rangle_{\mathcal{H}}=k\left(\mathbf{x}, \mathbf{x}^{\prime}\right)$.

***

The RKHS uniquely determines $k$, and vice versa, as stated in the following theorem:

> **Theorem 6.1** (Moore-Aronszajn theorem). Let $\mathcal{X}$ be an index set. Then for every positive definite function $k(\cdot,\cdot)$ on $\mathcal{X} \times \mathcal{X}$ there exists a unique RKHS, and vice versa.



***

##### From Mercer's theorem point of view 

###### Recall some basic things here

> **Eigenvalue&Eigenfunction**: A function $\phi(\cdot)$ that obeys the integral equation 
> $$
> \int k\left(\mathbf{x}, \mathbf{x}^{\prime}\right) \phi(\mathbf{x}) d \mu(\mathbf{x})=\lambda \phi\left(\mathbf{x}^{\prime}\right)
> $$
> is called an eigenfunction of kernel $k$ with eigenvalue $\lambda$ with respect to measure $\mu$.

>**Theorem 4.2** (**Mercer's theorem**). Let $(\mathcal{X}, \mu)$ be a finite measure space and $k \in L_{\infty}\left(\mathcal{X}^{2}, \mu^{2}\right)$ be a kernel such that $T_{k}: L_{2}(\mathcal{X}, \mu) \rightarrow L_{2}(\mathcal{X}, \mu)$ is positive definite. Let$\phi_{i} \in L_{2}(\mathcal{X}, \mu)$ be the normalized eigenfunctions of $T_{k}$ associated with the eigenvalues $\lambda_{i}>0$. Then:
>
> 1. the eigenvalues $\left\{\lambda_{i}\right\}_{i=1}^{\infty}$ are absolutely summable
>
> 2. $$
>    k\left(\mathbf{x}, \mathbf{x}^{\prime}\right)=\sum_{i=1}^{\infty} \lambda_{i} \phi_{i}(\mathbf{x}) \phi_{i}^{*}\left(\mathbf{x}^{\prime}\right)​
>    $$
>
>     holds $\mu^2$ almost everywhere, where the series converges absolutely and uniformly $\mu^2$ almost everywhere.
>
>Note: the sum may terminate at some value $N \in \mathbb{N}$ (degenerate kernel case), or the sum may be infinite(nondegenerate kernel case). 
>
>Q: The eigenfunctions are orthonormal $w.r.t.$ $\mu$, i.e. we have $\int \phi_{i}(\mathbf{x}) \phi_{j}(\mathbf{x}) d \mu(\mathbf{x})=\delta_{i j}$.  

###### An example of RKHS

> a real positive semidefinite kernel $k\left(\mathbf{x}, \mathbf{x}^{\prime}\right)$ with an eigenfunction expansion $k\left(\mathbf{x}, \mathbf{x}^{\prime}\right)=\sum_{i=1}^{N} \lambda_{i} \phi_{i}(\mathbf{x}) \phi_{i}\left(\mathbf{x}^{\prime}\right)$ relative to measure $\mu$, and
> a Hilbert space comprised of linear combinations of the the eigenfunctions, i.e. $f(\mathbf{x})=\sum_{i=1}^{N} f_{i} \phi_{i}(\mathbf{x})$ with $\sum_{i=1}^{N} f_{i}^{2} / \lambda_{i}<\infty$.
> Then the inner product $\langle f, g\rangle_{\mathcal{H}}$ in this Hilbert space between $f(\mathbf{x})$ and $g(\mathbf{x})=\sum_{i=1}^{N} g_{i} \phi_{i}(\mathbf{x})$ is defined as 
> $$
> \langle f, g\rangle_{\mathcal{H}}=\sum_{i=1}^{N} \frac{f_{i} g_{i}}{\lambda_{i}}
> $$
> Thus this Hilbert is equipped with a norm $\|f\|_{\mathcal{H}}$ where $\|f\|_{\mathcal{H}}^{2}=\langle f, f\rangle_{\mathcal{H}}=\sum_{i=1}^{N} f_{i}^{2} / \lambda_{i}$.
> Note that for $\|f\|_{\mathcal{H}}$ to be finite the sequence of coefficients $\{f_i\}$ must decay quickly; 
> effectively this imposes a smoothness condition on the space.

Next to show that this Hilbert space is the RKHS corresponding to the kernel $k$, i.e. that it has the reproducing property. 
This is achieved as 
$$
\langle f(\cdot), k(\cdot, \mathbf{x})\rangle_{\mathcal{H}}=\sum_{i=1}^{N} \frac{f_{i} \lambda_{i} \phi_{i}(\mathbf{x})}{\lambda_{i}}=f(\mathbf{x})
$$
Similarly
$$
\left\langle k(\mathbf{x}, \cdot), k\left(\mathbf{x}^{\prime}, \cdot\right)\right\rangle_{\mathcal{H}}=\sum_{i=1}^{N} \frac{\lambda_{i} \phi_{i}(\mathbf{x}) \lambda_{i} \phi_{i}\left(\mathbf{x}^{\prime}\right)}{\lambda_{i}}=k\left(\mathbf{x}, \mathbf{x}^{\prime}\right)
$$
Notice also that $k(\mathbf{x}, \cdot)$ is in the RKHS as it has norm $\sum_{i=1}^{N}\left(\lambda_{i} \phi_{i}(\mathbf{x})\right)^{2} / \lambda_{i}=k(\mathbf{x}, \mathbf{x})<\infty$. 
We have now demonstrated that this Hilbert space fulfils the two conditions given in **Def 6.1**.
As there is a unique RKHS associated with $k(\cdot,\cdot)$, this Hilbert space must be that RKHS.

###### Advantage of  the abstract formulation of the RKHS

The eigenbasis will change as we use different measures $\mu$ in Mercer's theorem.
However, the RKHS norm is in fact solely a property of the kernel and is invariant under this change of measure. 

###### Sample function

If we sample the coefficients $f_i$ in the eigenexpansion $f(\mathbf{x})=\sum_{i=1}^{N} f_{i} \phi_{i}(\mathbf{x})$ from $\mathcal{N}\left(0, \lambda_{i}\right)$ then 
$$
\mathbb{E}\left[\|f\|_{\mathcal{H}}^{2}\right]=\sum_{i=1}^{N} \frac{\mathbb{E}\left[f_{i}^{2}\right]}{\lambda_{i}}=\sum_{i=1}^{N} 1
$$
Thus if N is infinite, the sample functions are not in $\mathcal{H}$, as the expected value of the RKHS norm is infinite.
However, the posterior mean after observing some data will lie in the RKHS, due to the smoothing properties of averaging.

##### From reproducing kernel map point of view

Consider the space of functions $f$ defined as 
$$
\left\{f(\mathbf{x})=\sum_{i=1}^{n} \alpha_{i} k\left(\mathbf{x}, \mathbf{x}_{i}\right): n \in \mathbb{N}, \mathbf{x}_{i} \in \mathcal{X}, \alpha_{i} \in \mathbb{R}\right\}
$$
Let $g(\mathbf{x})=\sum_{j=1}^{n^{\prime}}\alpha_j^{\prime}k(\mathbf{x},\mathbf{x}_j^{\prime})$. Then we define the inner product
$$
\langle f, g\rangle_{\mathcal{H}}=\sum_{i=1}^{n} \sum_{j=1}^{n^{\prime}} \alpha_{i} \alpha_{j}^{\prime} k\left(\mathbf{x}_{i}, \mathbf{x}_{j}^{\prime}\right)
$$
Q: condition 1 of **Def 6.1** is fulfilled under the reproducing kernel map construction.

Next show the reproducing property as :
$$
\langle k(\cdot, \mathbf{x}), f(\cdot)\rangle_{\mathcal{H}}=\sum_{i=1}^{n} \alpha_{i} k\left(\mathbf{x}, \mathbf{x}_{i}\right)=f(\mathbf{x}).
$$

## 6.2 Regularization

##### Preliminary

###### regularizer

###### kernel ridge regression

###### representer theorem

##### RKHSs defined in terms of differential operators

###### null space

###### Green's function 

###### Two examples

##### Obtaining the Regularized Solution

###### regularization network

##### The Relationship of the Regularization View to Gaussian Process Prediction

## 6.3 Spline Models



## 6.4 Support Vector Machines



## 6.5 Least-squares Classification

## 6.6 Relevance Vector Machines

