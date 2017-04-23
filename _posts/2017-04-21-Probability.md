---
layout: post
title: "Probability"
description: "A brief summary on probability theory which includes basic probability inequalities, various types of convergence, law of large numbers, central limit theory, large deviation, martingales. The proof of theorems is omitted, while the real examples and applications are highlighted."
date: 2017-04-21
tags: [math, notes]
comments: true
share: true
---

### Basic Notions

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

$\Lambda$ is called *Radon-Nikodym derivative*, denoted as $\Lambda=\frac{dQ}{dP}$ since

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

In addition, the new measure is defined as

$$Q(A)=\int_A\Lambda dP.$$


### Various Types of Convergence

Definitions

* a.s. convergence

  $$X_n\rightarrow X\quad a.s. \quad \Longleftrightarrow \quad P(\lim_{n\rightarrow \infty}X_n=X)=1.$$
  
* in prob. convergence

  $$ X_n\rightarrow X\quad \textit{in prob.} \quad \Longleftrightarrow \quad \forall \epsilon>0, \quad P(|X_n-X|>\epsilon)\rightarrow 0 \quad as \quad n\rightarrow \infty.$$
  
* $L_p$ convergence

  $$X_n\rightarrow X\quad \textit{in } L_p \quad \Longleftrightarrow \quad E(|X_n|^p)\rightarrow E(|X|^p).$$
  
* in dist. convergence/ weak convergence

  $$X_n\rightarrow X\quad \textit{in dist.}  \quad \Longleftrightarrow \quad \textit{one of the followings}$$

    1. For every continuity point of $F$,

       $$F_n(t)\rightarrow F(t).$$
       
    2. For every continuous bounded function $g(\cdot)$,

       $$E(g(X_n))\rightarrow E(g(X)).$$

#### Relations between convergence modes

##### Example

a) in prob. convergence $\nRightarrow$ a.s. convergence

b) a.s. convergence $\nRightarrow$ $L_p$ convergence

c) $L_p$ convergence $\nRightarrow$ a.s. convergence


### Law of Large Numbers (LLN)

> (Strong law of large numbers (SLLN)) Let $X_1,...,X_n$ be i.i.d. with mean $\mu$. Let $S_n=\sum_{i=1}^nX_i$. Then
> 
> $$\frac{S_n}{n}\rightarrow \mu \quad a.s.$$


### Central Limit Theorem (CLT)

#### CLT for random variable

LNN only captures the deterministic part of $S_n$. It does not tell us the randomness of $S_n$. More precisely, 
    $$S_n=n\mu+o(n).$$
    
CLT answers the following two questions

  * what is the size of $o(n)$?
  * what is the limiting behaviour of $o(n)$?

> (CLT) Let $X_1,...,X_n$ be i.i.d. with mean $\mu$ and variance $\sigma^2$. Let $S_n=\sum_{i=1}^nX_i=n\bar{X}$. Then 
> 
> $$\frac{\sqrt{n}(\bar{X}-\mu)}{\sigma}\sim N(0,1).$$

One of the most important method to prove CLT is Fourier transform method (also called characteristic method). The idea is roughly to replace the bounded continuous function in the definition of weak convergence and plus additional conditions, which follows by Levy's continuity theorem:

> (Levy's continuity theorem) If $\phi_{X_n}(t)\rightarrow \phi(t)$ point-wise for some function $\phi(t)$ *continuous at 0*, then $X_n$ converges weakly to some r.v. $X$ with characteristic function $\phi(t)$.

[Remark] The continuity of $phi(t)$ at 0 guarantees that $\phi(t)$ is indeed a characteristic function of some $X$.

##### Example

Assume $Z\sim N(0,1)$, $X_n=nZ$, then $X_n$ does not converge to a r.v..
Since,
 
$$\phi_{X_n}(t)=E(e^{itX_n})=e^{-\frac{1}{2}n^2t^2}=1_{t=0},$$

is not continuous at $0$.

Another example of CLT which applies to independent r.v.s (identically distributed is not required) is *Lindeberg-Feller CLT*.

> (Lindeberg-Feller CLT) Suppose $X_1,...,X_n$ are independent r.v.s with mean $\mu$ and variance $\sigma_n^2$. Let $s_n^2=\sum_{j=1}^n\sigma_j^2$ denote the variance of partial sum $S_n=X_1+...X_n$. If for every $\epsilon>0$, 
> 
> $$\frac{1}{s_n^2}\sum_{j=1}^nE(X_j^21_{\{|X_j|>\epsilon s_n\}})\rightarrow 0,\quad (\#1)$$
> 
> then
> 
> $$\frac{S_n}{s_n}\rightarrow N(0,1).$$ 
> 
> Conversely, if
> 
> $$\max_{j\leq n}\sigma_j^2/s_n^2 \rightarrow 0\quad and \quad S_n/s_n\rightarrow N(0,1),$$
> 
> then (#1) holds.

#### CLT for quadratic form

> Let $\textbf{X}$=$(X_1,...,X_n)^'$ be a random vector with i.i.d. components. Suppose that $E(X_1)=0$, $E(X_1^2)=1$, $E(X_1^4)=\theta <\infty.$ Let $A=(a_ij)_{n,n}$ be an $n\times n$ Hermitian matrix, with bounded operator norm (largest absolute value of eigenvalue), i.e., $\|A\|\leq K,$ such that the following limits exist
> 
> $$\textit{(diagonal)}\quad \lim_{n\rightarrow \infty}\frac{1}{n}\sum_{i=1}^n a_{ii}=\alpha,\quad \lim_{n\rightarrow \infty}\frac{1}{n}\sum_{i,j=1}^{n}a_{ij}^2=\beta.$$
> 
> Then 
> 
> $$
> \frac{\textbf{X}'A\textbf{X}-tr A}{\sqrt{n}}\rightarrow N(0,S^2),
> $$
> 
> where $S^2=(\theta-3)\alpha+2\beta$.
