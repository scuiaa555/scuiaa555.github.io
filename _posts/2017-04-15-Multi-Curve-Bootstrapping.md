---
layout: post
title: "Multiple Curve Bootstrapping"
description: "The main methodology, bootstrapping instruments, multiple bootstrapping curve and interpolation scheme are listed here. The examples implemented by QuantLib C++ are also shown at the end."
date: 2017-04-15
tags: [finance, notes]
comments: true
share: true
---

### Bootstrapping Instruments (Pillars)

Denote the following dates: <br> 
$t_0$ = today, <br>
$T^S$ = start date/settlement date of the contract (usually equals to $Spot(t_0)$, say 2 business days for EUR market, except Deposit ON, TN, SN), <br>
$T_i^F$ = fixing date of the ith Ibor (the lag between fixing date and start date of period is also called spot lag, 2 business days for EUR and USD, 0 day for GBP, ref. p.2 of ***Henrard, Marc. "The irony in derivatives discounting part II: The crisis." Wilmott Journal 2.6 (2010): 301-316.***) and <br>
[$T_i,T_{i+1}$] = period of interest accrued. The time interval denoted as $j$ is typically 1d (overnight), 1m, 3m, 6m and 12m, so $\large{T_1=T_0+j}$.

Main references:

* ***Ametrano, Ferdinando M., and Marco Bianchetti. "Everything you always wanted to know about multiple interest rate curve bootstrapping but were afraid to ask." (2013).***

##### Deposits

The market quotes at time $t_0$ = today a standard strip of Deposits based on Ibor rates, with fixing date $T_0^F=t_0$, start date $T_0$ = $Spot(t_0)$ and maturity date $T_1=T_0+j$, where $j$ goes from 1d up to 1y.

The price at time t, from the point of view of the Lender, such that $T_0^F\leq t \leq T_1$, is given by

$$
\begin{align*}
Depo(t;T_0^F, T_0,T_1)&=P^j(t,T_1)E_t^{Q_{T_1}}[Depo(T_1;T_0^F, T_0,T_1)]\\
&=P^j(t,T_1)[1+\tau(T_0,T_1)R^j_{Depo}(T_0^F, T_0,T_1)].
\end{align*}
$$
where $R^j_{Depo}(t_0;T_0^F, T_0,T_1)$ is today's market quote.

Note that the Deposit is not a collateralised contract. Hence we used a discount factor $P^j$ (not $P^D$) based on a rate tenor $j$ consistent with the Deposit rate tenor.

At the settlement date $T^S$, the value of Deposit should equal to notional (=1 in this case), i.e.,

$$Depo(T^S;T_0^F, T_0,T_1)=P^j(T^S,T_1)[1+\tau(T_0,T_1)R^j_{Depo}(T_0^F, T_0,T_1)]=1,$$

which gives rise to

$$P^j(T^S,T_1)=\frac{1}{1+\tau(T_0,T_1)R^j_{Depo}(t_0;T_0^F, T_0,T_1)}.$$

This equation above is consistent with the definition of $P^j$, where 

$$
\begin{align*}
Depo(t;T_0^F, T_0,T_1)&=P^j(t,T_1)E_t^{Q_{T_1}}[Depo(T_1;T_0^F, T_0,T_1)]\\
&=P^j(t,T_1)(1+\tau(T_0,T_1)E_t^{Q_{T_1}}[Ibor(T_0^F;T_0^F, T_0,T_1)]\\
&=P^j(t,T_1)(1+\tau(T_0,T_1)\frac{1}{\tau(T_0,T_1)}(\frac{P^j(t,T_0)}{P^j(t,T_1)}-1))\\
&=P^j(t,T_0).
\end{align*}
$$

When $t=T^S$, $Depo(T^S;T_0^F, T_0,T_1)

