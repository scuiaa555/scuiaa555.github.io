---
layout: post
title: "Probability"
description: "A brief summary on probability theory which includes basic probability inequalities, various types of convergence, law of large numbers, central limit theory, large deviation, martingales. The proof of theorems is omitted, while the real examples and applications are highlighted."
date: 2017-04-21
tags: [math, notes]
comments: true
share: true
---

### Basic notions

Probability space

$$(\Omega,\mathcal{F},P)$$

Random variable of $(\Omega,\mathcal{F})$

$$
\begin{align*}
X: \Omega \rightarrow R \quad s.t.\\
X^{-1}(B)\in \mathcal{F}\quad \forall B\in \mathcal{B}.
\end{align*}
$$

Law of $X$: $\mu_X$

The law of random variable $X$ is a probability measure on $(R,\mathcal{B})$ which is induced by $X$ by

$$\mu_X(B)\triangleq P(X^{-1}(B)).$$

> $(\Omega,\mathcal{F},P)$ is kind of objective (to my point of view), which means that once $P$ is fixed, the distribution that is associated on this probability space is determined. Conversely, if you assume a random follows certain distribution, you already made the assumption that there exists such a probability space (with $P$) that holds this random variable.
 
 
 