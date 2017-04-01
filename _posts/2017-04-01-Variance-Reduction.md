---
layout: post
title: "Variance Reduction of Monte Carlo"
description: "A study note on variance reduction techniques that are introduced in the book of Glasserman's."
date: 2017-04-01
tags: [finance, notes]
comments: true
share: true
---

Background: By using Monte Carlo simulation to price derivatives, we always face the problem that simulate $N$ paths of stocks or other state variables or function of them $f(\cdot)$ as one-dimension $Y$ or d-dimension ($Y_1,Y_2,...,Y_d$) to estimate $E[Y]=\mu$. The $n$ paths are denoted as $y_1,...,y_n$, where each $y_i$ is a d-dimensional realisation (replication)of random vectors.

The usual estimator is $\hat{\mu}=\bar{Y}=\frac{y_1+y_2+...+y_n}{n}$.

### 1. Control Variates

Control variate exploits information about the errors in estimates of known quantities to reduce the error in an estimate of an unknown quantity.

On each replication we calculate another output $x_i$ along with $y_i$. Suppose that the pairs $(x_i,y_i)$,$i=1,2,...,n$ are i.i.d. and that the expectation $E[X]$ of the $x_i$ is known. Then, for any fixed $b$ we can calculate

$$y_i(b)=y_i-b(x_i-E[X]),$$

and the new estimator $\hat{\mu}$ is

$$\hat{\mu}=\bar{Y}(b)=\frac{1}{n}\sum_{i=1}^n(y_i-b(x_i-E(X))).$$

The variance $\hat{\mu}$

$$Var(\hat{\mu})=\frac{Var(Y)+b^2Var(X)-2bCov(X,Y)}{n}\leq\frac{Var(Y)}{n}$$

if and only if

$$b^2Var(X)\leq 2bCov(X,Y).$$

The optimal $b^*$ is given by

$$b^*=\frac{Cov(X,Y)}{Var(X)},$$

and estimated by

$$\hat{b}=\frac{\sum_{i=1}^n(x_i-\bar{X})(y_i-\bar{Y})}{\sum_{i=1}^n(x_i-\bar{X})^2}.$$

An example is to use the underlying asset

$$\frac{1}{n}\sum_{i=1}^n(y_i-b[S_i(T)-e^{rT}S(0)]).$$

In multi-dimensional case,

$$\bar{Y}(b)=\bar{Y}-b^T(\bar{X}-E[X]).$$

Non-linear controls, for example

$$\bar{Y}\frac{E[X]}{\bar{X}}$$

adjusts $\bar{Y}$ upward if $0<\bar{X}<E[X]$, downward if $0<E[X]<\bar{X},$ and thus may be attractive if $x_i$ and $y_i$ are positively correlated. Other estimators of this type include

$$\bar{Y}\frac{\bar{X}}{E[X]} \quad and \quad \bar{Y}\exp(\bar{X}-E[X])\quad and \quad \bar{Y}^{(\bar{X}/E[X])}.$$

To analyse the large-sample behaviour is based on delta method, i.e.,

$$\sqrt{n}[h(\bar{X})-h(\mu)]\rightarrow N(0, \nabla h(\mu)\Sigma \nabla h(\mu)^T),$$

where covariance matrix of $\vec{X}$ and $Y$.


### 2. Antithetic Variates

The method of antithetic variates attempts to reduce variance by introducing negative dependence between pairs of replications.

In one-dimensional case, $F^{-1}(U)$ and $F^{-1}(1-U)$ both have distribution $F$.

In multi-dimensional case, we need the additional assumption that the random variables in the random vectors are independent, then $(F_1^{-1}(U_1),...,F_d^{-1}(U_d))$ and ($F_1^{-1}(1-U_1),...,F_d^{-1}(1-U_d))$ both have the same distribution.

Antithetic sampling produces a sequence of pairs of observations $(y_1,\tilde{y}_1),(y_2,\tilde{y}_2),...(y_n,\tilde{y}_n)$ whose features are

- the pairs $(y_1,\tilde{y}_1),(y_2,\tilde{y}_2),...(y_n,\tilde{y}_n)$ are i.i.d;
- for each $i$, $y_i$ and $\tilde{y}_i$ have the same distribution, though ordinarily they are not independent.

The antithetic variates estimator is

$$\hat{\mu}=\frac{1}{n}\sum_{i=1}^n(\frac{y_i+\tilde{y}_i}{2}).$$

We assume that the computational effort required to generate a pair $(y_i,\tilde{y}_i)$ is approximately twice the effort required to generate $y_i$. Then

$$Var(\hat{\mu})\leq Var [\frac{1}{2n}\sum_{i=1}^{2n}y_i]$$

if and only if

$$Cov(y_i,\tilde{y}_i)\leq 0.$$

To generate a Brownian path, assume that the discretise $(0,T)$ into $d$ intervals. Because of the property of independent increments of Brownian motion, random variables inside $(W_1,...,W_d)$ are independent. So the antithetic sampling can be implemented as

path 1: $W_1^1,...,W_d^1$ $\rightarrow y_1$<br> 
        $\quad \quad \  -W_1^1,...,-W_d^1$ $\rightarrow \tilde{y}_1$<br>
        ...<br>
path n: $W_1^n,...,W_d^n$ $\rightarrow y_n$<br> 
        $\quad \quad \  -W_1^n,...,-W_d^n$ $\rightarrow \tilde{y}_n$<br>
        
        
### 3. Stratified Sampling

Stratified sampling refers broadly to any sampling mechanism that constrains the fraction of observations drawn from specific subsets (or strata) of the sample space, according to

$$
\begin{align*}
E[Y]=E_X{E(Y|X)}&=\sum_{i=1}^KE(Y|X\in A_i)P(X\in A_i)\\
&=\sum_{i=1}^Kp_iE(Y|X\in A_i).
\end{align*}
$$

The stratified sampling estimator

$$\hat{\mu}=\sum_{i=1}^Kp_i\cdot\frac{1}{n_i}\sum_{j=1}^{n_i}y_{ij},$$

where $y_{ij}$ is the j-th sample from i-th strata.

There are two main issues that need to be considered

- choosing the stratification variable $X$ (could be $Y$ itself), the strata $A_1,...,A_K$, and the allocation $n_1,...,n_K$;
- generating samples from the distribution of $(X,Y)$ conditional on $X\in A_i$.
