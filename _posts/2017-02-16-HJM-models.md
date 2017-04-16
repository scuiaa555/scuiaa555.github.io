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

-
#### Arbitrage-free condition

Assume that the risk-neutral measure exists and all the zero-coupon bond prices are driven by a common one-dimensional Brownian motion, i.e.,

$$dP(t,T)=P(t,T)(r(t)dt+\nu(t,T)dW_t^Q).$$

According to the relationship between instantaneous forward rate and zero coupon bonds

$$P(t,T)=\exp(-\int_t^T f(t,u)du),\quad \Longleftrightarrow \quad f(t,T)=-\frac{\partial \ln P(t,T)}{\partial T},$$

$$df(t,T)=\nu(t,T)\nu_T(t,T)dt-\nu_T(t,T)dW_t^Q.$$

Denote 

$$\sigma(t,T)\triangleq -\nu_T(t,T),\quad \Longleftrightarrow \quad \nu(t,T)=-\int_t^T\sigma(t,s)ds \quad\textit{with } \nu(t,t)=0,$$

then

$$df(t,T)=(\sigma(t,T)\int_t^T \sigma(t,s)ds)dt+\sigma(t,T)dW_t^Q.$$

The above dynamic shows the arbitrage-free condition between the drift and volatility terms, which is also called *HJM condition*.

#### 

