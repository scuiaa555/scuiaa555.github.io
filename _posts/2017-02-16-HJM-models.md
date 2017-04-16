---
layout: post
title: "One-factor Heath–Jarrow–Morton(HJM) model in Multiple Curve Setting"
description: "Here, we listed the model set-up, derivation and use of derivatives pricing in the setting of multiple curves. The pricing methodology includes pricing caplet/floorlet, European swaption, Bermudan swaption, CMS."
date: 2017-02-16
tags: [finance, notes]
comments: true
share: true
---

### 1. Heath-Jarrow-Morton Model


#### Arbitrage-free Condition

Assume that the risk-neutral measure exists and all the zero-coupon bond prices are driven by a common one-dimensional Brownian motion (a strong assumption), i.e.,

$$dP(t,T)=P(t,T)(r(t)dt+\nu(t,T)dW_t^Q).$$

According to the relationship between instantaneous forward rate and zero coupon bonds

$$P(t,T)=\exp(-\int_t^T f(t,u)du),\quad \Longleftrightarrow \quad f(t,T)=-\frac{\partial \ln P(t,T)}{\partial T},$$

$$df(t,T)=\nu(t,T)\nu_T(t,T)dt-\nu_T(t,T)dW_t^Q.$$

Denote 

$$\sigma(t,T)\triangleq -\nu_T(t,T),\quad \Longleftrightarrow \quad \nu(t,T)=-\int_t^T\sigma(t,s)ds \quad\textit{with } \nu(t,t)=0,$$

then

$$df(t,T)=(\sigma(t,T)\int_t^T \sigma(t,s)ds)dt+\sigma(t,T)dW_t^Q.$$

The above dynamic shows the arbitrage-free condition between the drift and volatility terms, which is also called *HJM condition*.

---
#### Zero-coupon Bonds

> In a HJM one factor model, the price of the zero coupon bond can be written as

> $$P(t,T)=\frac{P(0,T)}{P(0,t)}\exp(-\frac{1}{2}\int_0^t(\nu^2(u,T)-\nu^2(u,t))du+\int_0^t(\nu(u,T)-\nu(u,t))dW_u^Q.$$

[Proof.] Based on the dynamic of instantaneous forward rate,

$$f(t,T)=f(0,T)+\int_0^t(\sigma(u,T)\int_u^T\sigma(u,s)ds)du+\int_0^t\sigma(u,T)dW_u^Q.$$ 

Therefore, 

$$\begin{align*}
P(t,T)&=\exp(-\int_t^Tf(t,v)dv)\\
&=\exp(-\int_t^T[f(0,v)+\int_0^t(\sigma(u,v)\int_u^v\sigma(u,s)ds)du+\int_0^t\sigma(u,v)dW_u^Q]dv)\\
&=\frac{P(0,T)}{P(0,t)}\exp(-\int_t^T[\int_0^t(-\sigma(u,v)\nu(u,v)dv)du+\int_0^t\sigma(u,v)dW_u^Q]dv)\\
&=\frac{P(0,T)}{P(0,t)}\exp(\int_0^t(\int_t^T\sigma(u,v)\nu(u,v)dv)du-\int_0^t(\int_t^T\sigma(u,v)dv)dW_u^Q),
\end{align*}
$$

where the last equation is by Fubini's theorem.

Then, by integral by parts,

$$
\begin{align*}
\int_t^T\sigma(u,v)\nu(u,v)dv&=-\int_t^T\frac{\partial \nu(u,v)}{\partial v}\nu(u,v)dv,
\end{align*}
$$

where

$$
\begin{align*}
\frac{\partial \nu(u,v)}{\partial v}\nu(u,v)&=\frac{\partial}{\partial v}[\nu^2(u,v)]-\nu(u,v)\frac{\partial \nu(u,v)}{\partial v}\nu(u,v)
\end{align*}
$$

which implies

$$
\begin{align*}
\frac{\partial \nu(u,v)}{\partial v}\nu(u,v)&=\frac{1}{2}\frac{\partial}{\partial v}[\nu^2(u,v)].
\end{align*}
$$

So,

$$
\begin{align*}
\int_t^T\sigma(u,v)\nu(u,v)dv&=-\int_t^T\frac{\partial \nu(u,v)}{\partial v}\nu(u,v)dv\\
&=-\frac{1}{2}(\nu^2(u,T)-\nu^2(u,t)),
\end{align*}
$$

and

$$
\begin{align*}
\int_t^T\sigma(u,v)dv&=-\int_t^T-\frac{\partial \nu(u,v)}{\partial v}dv\\
&=-(\nu(u,T)-\nu(u,t)).
\end{align*}
$$

Combining these two equations gives rise to the result of zero coupon bonds.






