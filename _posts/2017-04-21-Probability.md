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
 
 
c.d.f. of $X$

$$
\begin{align*}
F_X(x)\triangleq \mu_X((-\infty,x])&=P(X^{-1}\{(-\infty,x]\})\triangleq P(X\leq x).
\end{align*}
$$ 

Expectation of $X$

$$
\begin{align*}
E^P[X]&\triangleq \int_{\Omega}X(\omega)P(X\in (\omega,\omega+d\omega])\triangleq \int_{\Omega}X(\omega)P^X(d\omega)\\
&=_{(Thm)}\int_Rx\mu_X(dx)=\int_RxdF_X(x).
\end{align*}
$$

#### Change of measure from $(\Omega,\mathcal{F},P)$ to $(\Omega,\mathcal{F},Q)$

> (Absolutely continuous) If $Q$ is absolutely continuous with respect to $P$,  then
> 
> $$P(A)=0 \quad \textit{implies} \quad Q(A)=0 \quad \forall A\in \mathcal{F}.$$

> (Radon-Nikodym) If $Q$ is absolutely continuous with respect to $P$, then there exists a $\mathcal{F}$-measurable random variable $\Lambda$, such that 
> 
> $$\forall A\in \mathcal{F}, \quad Q(A)=E^Q[1_A]=\int_AdQ=\int_A \Lambda dP=E^P[\Lambda 1_A].$$
> 

$\Lambda$ is called *Radon-Nikodym derivative*, since

$$E^Q[X]=\int_{\Omega}XdQ=\int_{\Omega}X\frac{dQ}{dP}dP=\int_{\Omega}X\Lambda dP=E^P[X\Lambda].$$
 
##### Example

On $(\Omega,\mathcal{F},P)$, $X\sim N(0,1)$, find the Radon-Nikodym derivative such that on the new measure, $X\sim N(\mu,\sigma)$.

$$
\begin{align*}
F^Q_X(x)&=\mu_X^Q((-\infty,x])=\int_{-\infty}^x\frac{1}{\sqrt{2\pi}\sigma}e^{-\frac{(x-\mu)^2}{2\sigma^2}}dx\\
&=Q(X\leq x)=\int_{X\leq x}dQ=\int_{X\leq x}\Lambda dP\\
&\triangleq \int_{-\infty}^x\lambda(x)\frac{1}{\sqrt{2\pi}}e^{-\frac{x^2}{2}}dx,
\end{align*}
$$

which gives rise to

$$\lambda(x)=\frac{1}{\sigma}e^{-\frac{(x-\mu)^2}{2\sigma^2}+\frac{x^2}{2}}.$$

Therefore, 

$$\Lambda=\frac{1}{\sigma}e^{-\frac{(X-\mu)^2}{2\sigma^2}+\frac{X^2}{2}},$$

which is a $\mathcal{F}$-measurable random variable.


### Various types of convergence



