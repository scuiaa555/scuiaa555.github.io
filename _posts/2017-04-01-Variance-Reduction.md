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

$$y_i(b)=y_i-b(x_i-E[X]).$$

