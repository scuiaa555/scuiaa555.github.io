---
layout: post
title: "Probability"
description: "A brief summary on probability theory which includes basic probability inequalities, various types of convergence, law of large numbers, central limit theory, concentration law, martingales. The proof of theorems is omitted, while the real examples and applications are highlighted."
date: 2017-04-21
tags: [math, notes]
comments: true
share: true
redirect_to:
  - http://www.github.com
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

> Let $\textbf{X}$=$(X_1,...,X_n)^'$ be a random vector with i.i.d. components. Suppose that $E(X_1)=0$, $E(X_1^2)=1$, $E(X_1^4)=\theta <\infty.$ Let $A=(a_ij)_{n,n}$ be an $n\times n$ Hermitian matrix, with bounded operator norm (largest absolute value of eigenvalue), i.e., $\|A\|_{op}\leq K,$ such that the following limits exist
> 
> $$\lim_{n\rightarrow \infty}\frac{1}{n}\sum_{i=1}^n a_{ii}=\alpha\quad \textit{(diagonal)},\quad \lim_{n\rightarrow \infty}\frac{1}{n}\sum_{i,j=1}^{n}a_{ij}^2=\beta.$$
> 
> Then 
> 
> $$
> \frac{\textbf{X}'A\textbf{X}-tr A}{\sqrt{n}}\rightarrow N(0,S^2),
> $$
> 
> where $S^2=(\theta-3)\alpha+2\beta$.


### Concentration Law

* LLN: leading deterministic part of partial sum $S_n$;
* CLT: fluctuation of $S_n$;
* LLN+CLT: typical behaviour of $S_n$;
* Concentration law: *atypical* behaviour of $S_n$, which studies

$$P(\frac{S_n}{n}\geq t)\leq Ce^{-cnt^2}.$$

More general form takes as

$$P(|f(X_1,...,X_n)-Ef(X_1,...,X_n)|\geq t)\leq e^{-h(n,t)},$$

where the order of $h$ w.r.t. n is crucial.

If we rewrite CLT as

$$P(\frac{S_n}{n}-\mu\geq \frac{t}{\sqrt{n}})=P(N(0,1)\geq t)+o(1),$$

which is equivalent to

$$P(\frac{S_n}{n}-\mu\geq x)=P(N(0,1)\geq \sqrt{n}x)+o(1).$$

From above, no matter how large we increase $x$, there is always $o(1)$ term on the RHS, so we cannot get some bound that better than $o(1)$ by CLT. For this reason, the concentration law is trying to improve the estimation of $P(\frac{S_n}{n}\geq x)$ when $x$ is large.

##### Example 1

(Cramer's actuarial problem) Suppose $n$ clients have each paid a premium of $c$ dollars for life insurance over a period of time. Assume the claims are i.i.d. nonnegative bounded r.v.s $X_1,...,X_n$. Suppose the total premium of the insurance company is $cn$. Let $EX_1=\mu$. The chance that the insurance company bankcrupt is

$$P(\sum_{i=1}^nX_i\geq cn)\leq e^{-nI( c )}\quad if \quad c>\mu,$$

where $I( c )\sim (c-\mu)^2.$

##### Example 2

(Borel's geometric concentration) Let $\mu^n$ be the uniform measure on the $n$-dimensional cube $\Omega=[-1,1]^n$. Let $H$ be a hyperplane that is orthogonal to a principle diagonal of $[-1,1]^n$, i.e.,

$$H=(1,...,1)^\perp,$$

where $\perp$ stands for orthogonal complement and dimension of $H$ is $n-1$.

Let 

$$H_r=\{x\in [-1,1]^n:\textit{dist}(x,H)\leq r\}.$$

Then

$$\mu^n(H_{\epsilon\sqrt{n}}^c)=1-\mu^n(H_{\epsilon\sqrt{n}})\leq Ce^{-c\epsilon^2n}.$$

[Remark 1] If $n$ is large, the probability that locates in this small region is very high, that is why this kind of inequality is called concentration.

[Remark 2] Let $X_1,...,X_n$ be i.i.d. uniformly distributed on $[-1,1]$. We have

$$\textit{dist}(X,H)=\frac{<X,(1,...,1)>}{\|(1,...,1)\|_2}=\frac{\sum X_i}{\sqrt{n}}.$$

Hence,

$$\mu^n(H_{\epsilon\sqrt{n}}^c)=P(|\frac{S_n}{n}>\epsilon|),$$

which is a concentration law.

#### Concentration law for Rademacher r.v.

> (Hoeffding's inequality) Let {$X_i$} be a sequence of i.i.d. Rademacher r.v., i.e., $P(X_i=1)=P(X_i=-1)=\frac{1}{2}$ (Note that Bernoulli r.v. is $P(X_i=1)=P(X_i=0)=\frac{1}{2}$). Then for any given $a_1,...,a_n\in R$ and $t\geq 0$
> 
> $$P(\sum_{i=1}^nx_ia_i\geq t)\leq \exp(-\frac{t^2}{2\sum_{i=1}^n a_i^2}).$$


#### Concentration law for bounded r.v.

> (Hoeffding-Chernoff's inequality) Let {$X_i$} be independent bounded r.v.s $X_i\in[a_i,b_i]$. Then
> 
> $$
> P(\bar{X}-E\bar{X}\geq t)\leq \exp(-\frac{2n^2t^2}{\sum_{i=1}^n(b_i-a_i)^2}).
> $$

Hoeffding-Chernoff's inequality does not use the variance information of $X_i$, while Bennett's inequality takes that into consideration, which gives better bound in most cases.

> (Bennett's inequality) Let $X_i$ be i.i.d with mean $\mu$ and variance $\sigma^2$ and bounded $|X_i-\mu|\leq M$. Then
> 
> $$P(\frac{S_n}{n}-\mu\geq t)\leq \exp(-\frac{n\sigma^2}{M}\phi(\frac{tM}{\sigma^2}))\quad \forall t\geq 0,$$
> 
> where
> 
> $$\phi(x)=(1+x)\log(1+x)-x.$$


#### Concentration law for r.v. with "bounded-like" condition

> (Azuma's inequality) Let $X_1,..,X_n$ be $n$ independent r.v.s(they are not necessarily of dimension rather than 1 and their dimensions are not necessarily equal). Let $f$ be a function which satisfies
> 
> $$|f(X_1,...,X_i,...,X_n)-f(X_,1...,X_i',...,X_n)| \leq a_i$$
> 
> for some constants $a_1,...,a_n$. Then
> 
> $$P(f-Ef)\geq t)\leq \exp(-\frac{t^2}{2\sum_{i=1}^n a_i}).$$


[Remark] If $f=\sum_{i=1}^nX_i$, then the condition of $f$ reduces to bounded condition and Azuma's inequality reduces to Hoeffding-Chernoff's inequality.


##### Example 1

(Balls and bins) There are $m$ balls and $n$ bins. We randomly throw each ball into a bin. Let $X_i$ be the bin selected by $i$-th ball and $f=f(X_1,...,X_n)$ be the number of empty bins. Then

$$P(|f-Ef|\geq t)\leq 2\exp(-\frac{t^2}{2m}),$$

since each ball cannot change the number of empty bins by 1. In addition, $Ef=n\cdot (\frac{n-1}{n})^m$ which could be proved by method of indicator functions.


##### Example 2

(Chromatic number of random graph) Let $G(n,p)$ be an Erdos-Renyi graph, i.e., there are $n$ vertices $v_1,...,v_n$. Each possible edge $(v_i,v_j)$ is included in the graph with probability $p$, independent from all the other edges. Let $f=\mathcal{X}(G(n,p))$ be the chromatic number, which is the smallest number of colors needed to color the vertices so that NO two adjacent vertices share the same color. Then

$$P(|\mathcal{X}(G(n,p))-E\mathcal{X}(G(n,p))|\geq t)\leq 2\exp(-\frac{t^2}{2n}).$$

Let $X_i$ be the randomness in the set of possible edges between $v_i$ and $v_1,...,v_{i-1}.$ More specifically, $X_i=(X_1^i,...,X_{i-1}^i)$, where $X_k^i$ is $1$ if there is an edge between $v_i$ and $v_k$, $0$ otherwise. Let $f=\mathcal{X}(G(n,p))=f(X_1,...,X_n),$ which satisfies

$$|f(X_1,...,X_i,...,X_n)-f(X_1,...,X_i',...,X_n)|\leq 1,$$

since at most we use an additional color to accommodate the changes corresponding to one vertex. In addition, it is known (non-trivial) that 

$$E\mathcal{X}(G(n,p))=\frac{-\log (1-p)}{2}\cdot \frac{n}{\log n}.$$

[Remark 1] To calculate the chromatic number of a general graph is a NP hard problem.

[Remark 2] Here $X_i$ are random vectors and are not of the same dimension.

##### Example 3

(Max-cut for random graph) Let $G=\{V,E\}$ be a generic graph, where $V=\{1,...,n\}$. A cut of the graph $G$ is a map $\sigma:V\rightarrow \{0,1\}^n.$ Let $MC(\sigma)=\#\{(i,j)\in E:\sigma(i)\neq \sigma(j)\}.$ We want to investigate  the maximum value of $MC(\sigma)$ over all possible cut:

$$X=\max_{\sigma}MC(\sigma).$$

Now, we choose $G$ to be an Erdos-Renyi graph $G(n,\frac{1}{2})$. A concentration law of $X$ will follow by Azuma's.


##### Example 4

(Random interval packing) Let $I_i, i=1,...,n$ i.i.d subintervals of $[0,1]$, where the endpoints of $I_i$ are chosen from $[0,1]$ uniformly. A packing of $[0,1]$ is a subset of $\{I_1,...,I_n\}$, such that all intervals in this subset are pairwise disjoint. Let $X$ be the maximum cardinality over all packings, i.e., the cardinality of the packing with the largest size. A concentration law of $X$ will follow by Azuma's.


#### Gaussian concentration inequality

The above inequalities suffer from their limitations of boundedness requirements that even cannot even be applied to Gaussian r.v.s.

> Let $X=(X_1,...,X_n)$ be a random vector with i.i.d $N(0,\sigma^2)$ components. Let $f:R^n\rightarrow R$ be a function which is Lipschitz with constant $|F|_L$, i.e.,
> 
> $$|f(X)-f(Y)|\leq |F|_L\|X-Y\|_2 \quad \forall X,Y\in R^n.$$
> 
> Then,
> 
> $$P(|f(X)-Ef(X)|\geq t)\leq C\exp(-\frac{ct^2}{\sigma^2|F|_L^2}),\quad t\geq 0.$$

The example functions that satisfies Lipschitz condition are $f(X)=\sum X_i$, $f(X)=\max X_i$, $f(X)=\|X\|_2$, $f(X)=\ln(e^{X_1}+...+e^{X_n})$.

The smaller variance $\sigma$ is and the smaller the Lipschitz constant $|F|_L$ is, the sharper the inequality is.

