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

