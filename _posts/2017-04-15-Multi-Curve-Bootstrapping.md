---
layout: post
title: "Multiple Curve Bootstrapping"
description: "The main methodology, bootstrapping instruments, multiple bootstrapping curve and interpolation scheme are listed here. The examples implemented by QuantLib C++ are also shown at the end."
date: 2017-04-15
tags: [finance, notes]
comments: true
share: true
---

Main references:

* [1]***Ametrano, Ferdinando M., and Marco Bianchetti. "Everything you always wanted to know about multiple interest rate curve bootstrapping but were afraid to ask." (2013).***
* [2]***Henrard, Marc. "The irony in derivatives discounting part II: The crisis." Wilmott Journal 2.6 (2010): 301-316.***

---
### Multiple Curves

We adopt the definition according to p.3 of [2], i.e.,

&emsp;&emsp;**DEFINITION** 

> The present value of the accrued interest indexed to $Ibor(T^F;T,T+j)$ is
> 
> $$
> \tau(T,T+j)P^D(t,T+j)E_t^{Q_{T+j}}[Ibor(T^F;T,T+j)]\triangleq P^D(t,T+j)(\frac{P^j(t,T)}{P^j(t,T+j)}-1).
> $$
> 
> And the only direct link between $P^j$ and $Ibor$ is valid at fixing date $T^F$, i.e.,
> 
> $$
> Ibor(T^F;T,T+j)=\frac{1}{\tau(T,T+j)}(\frac{P^j(T^F,T)}{P^j(T^F,T+j)}-1).
> $$

For j=1m,3m,6m,12m, the fixing date $T^F$ usually does not equal start date $T$. However, the fixing date of overnight rate is equal to $T$ by definition of overnight rate, which leads to

$$P^D(t_0,T)=E^Q_{t_0}[e^{\int_{t_0}^T-r_{on}(s)ds}].$$

---
### Bootstrapping Instruments (Pillars)

Denote the following dates: <br> 
$t_0$ = today, <br>
$T^S$ = start date/settlement date of the contract (usually equals to $Spot(t_0)$, say 2 business days for EUR market, except Deposit ON, TN, SN), <br>
$T_i^F$ = fixing date of the ith Ibor (the lag between fixing date and start date of period is also called spot lag, 2 business days for EUR and USD, 0 day for GBP, ref. p.2 of [2]) and <br>
[$T_i,T_{i+1}$] = period of interest accrued. The time interval denoted as $j$ is typically 1d (overnight), 1m, 3m, 6m and 12m, so $T_1=T_0+j$. $\large{T_{n}}$


#### 1. Deposits

![png](https://scuiaa555.github.io/assets/images/2017-04-15-deposit.png){:height="125px" width="500px"}

The market quotes at time $t_0$ = today a standard strip of Deposits based on Ibor rates, with fixing date $T_0^F=t_0$, start date $T_0$ = $Spot(t_0)$ and maturity date $T_1=T_0+j$, where $j$ goes from 1d up to 1y.

The price at time t, from the point of view of the Lender, such that $T_0^F\leq t \leq T_1$, is given by

$$
\begin{align*}
Depo(t;T_0^F, T_0,T_1)&=P^j(t,T_1)E_t^{Q_{T_1}}[Depo(T_1;T_0^F, T_0,T_1)]\\
&=P^j(t,T_1)[1+\tau(T_0,T_1)R^j_{Depo}(T_0^F, T_0,T_1)].
\end{align*}
$$

where $R^j_{Depo}(t_0;T_0^F, T_0,T_1)$ is today's market quote and $\tau(T_0,T_1)$ should follow the *Business Day Convention, End of Month Convention and Day Count Convention* of the corresponding contract.

Note that the Deposit is not a collateralised contract. Hence we used a discount factor $P^j$ (not $P^D$) based on a rate tenor $j$ consistent with the Deposit rate tenor.

> At the settlement date $T^S=T_0$, the value of Deposit should equal to notional (=1 in this case), i.e.,
> 
> $$Depo(T^S;T_0^F, T_0,T_1)=P^j(T^S,T_1)[1+\tau(T_0,T_1)R^j_{Depo}(T_0^F, T_0,T_1)]=1,$$
> 
> which gives rise to
> 
> $$P^j(T^S,T_1)=\frac{1}{1+\tau(T_0,T_1)R^j_{Depo}(t_0;T_0^F, T_0,T_1)}.$$

This equation above is consistent with the definition of $P^j$, where 

$$
\begin{align*}
Depo(t;T_0^F, T_0,T_1)&=P^j(t,T_1)E_t^{Q_{T_1}}[Depo(T_1;T_0^F, T_0,T_1)]\\
&=P^j(t,T_1)(1+\tau(T_0,T_1)E_t^{Q_{T_1}}[Ibor(T_0^F;T_0^F, T_0,T_1)]\\
&=P^j(t,T_1)(1+\tau(T_0,T_1)\frac{1}{\tau(T_0,T_1)}(\frac{P^j(t,T_0)}{P^j(t,T_1)}-1))\\
&=P^j(t,T_0).
\end{align*}
$$

When $t=T^S=T_0$, $Depo(T^S;T_0^F, T_0,T_1)=1$ and when $t=T_0^F$, $Depo(T_0^F;T_0^F, T_0,T_1)=P^j(T_0^F,T_0)\leq 1$.

###### Three Overnight Deposits

![png](https://scuiaa555.github.io/assets/images/2017-04-15-deposit2.png)

The first Deposit, denoted with ON (Overnight Deposit), starts today and matures tomorrow; the second Deposit, denoted with TN(Tomorrow-Next Deposit), starts tomorrow and matures 1 day after. The next Deposit, denoted with SN (Spot-Next Deposit) starts at spot date and matures 1 day after.


---
#### 2. FRA

![png](https://scuiaa555.github.io/assets/images/2017-04-15-fra.png)

The standard (textbook) FRA (collateralised) exchanges the floating interest with fixed at time $T_1$, which gives the present value at time $t$, such that $t\leq T_1$,

$$
\begin{align*}
FRA_{Std}(t;T_0^F,T_0,T_1)&=P^D(t,T_1)E_t^{Q_{T_1}}[\tau(T_0,T_1)(Ibor(T_0^F;T_0^F,T_0,T_1)-K)]\\
&=P^D(t,T_1)[\tau(T_0,T_1)\frac{1}{\tau(T_0,T_1)}(\frac{P^j(t,T_0)}{P^j(t,T_1)}-1)-\tau(T_0,T_1)K]\\
&=P^D(t,T_1)(\frac{P^j(t,T_0)}{P^j(t,T_1)}-1-\tau(T_0,T_1)K).
\end{align*}
$$

At the settlement date $T^S$, the value of FRA is zero, which leads to

$$R_{FRA,Std}^j(t_0;T_0^F,T_0,T_1)=\frac{1}{\tau(T_0,T_1)}(\frac{P^j(T^S,T_0)}{P^j(T^S,T_1)}-1),$$

where $R_{FRA,Std}^j(t_0;T_0^F,T_0,T_1)$ refers to today's market quotes of FRAs.

The market traded FRA, however, pays the floating leg at time $T_0$, so the payoff at time $T_0$ is

$$FRA_{Mkt}(T_0;\textbf{T})=\frac{\tau(T_0,T_1)(Ibor(T^F_0;T_0^F,T_0,T_1)-K)}{1+\tau(T_0,T_1)Ibor(T^F_0;T_0^F,T_0,T_1)}.$$

Since FRA are traded OTC between collateralised counterparties, the present value can be expressed by convexity adjustment,

$$FRA_{Mkt}(t;\text{T})=P^D(t,T_0)[1-\frac{1+K\tau(T_0,T_1)}{1+E_t^{Q_{T_1}}[Ibor(T_0^F;T_0^F,T_0,T_1)]\tau(T_0,T_1)}e^{C_{FRA}^j(t;T_0)}].$$

At the settlement date $T^S$, the value of market traded FRA is zero, which leads to

$$R_{FRA,Mkt}^j(t_0;T_0^F,T_0,T_1)=\frac{1}{\tau(T_0,T_1)}\{[1+E_t^{Q_{T_1}}[Ibor(T_0^F;T_0^F,T_0,T_1)]\tau(T_0,T_1)]e^{C_{FRA}^j(t;T_0)}-1\}.$$

> In ***Mercurio, Fabio. "LIBOR market models with stochastic basis." (2010),*** it is proved that, for 
> typical post credit market situations, the actual size of the convexity adjustment results to be below 1
>  bp, even for long maturities. Hence, in practical situation, we can discard the convexity adjustment and
>  use the classical pricing expressions
>
> $$R_{FRA,Mkt}^j(t_0;T_0^F,T_0,T_1)=\frac{1}{\tau(T_0,T_1)}(\frac{P^j(T^S,T_0)}{P^j(T^S,T_1)}-1).$$

---
#### 3. Futures

![png](https://scuiaa555.github.io/assets/images/2017-04-15-futures.png)

The payoff at time $T_0$ (same as market traded FRA) is given as

$$Futures(T_0;\textbf{T})=N[1-Ibor(T_0^F;T_0^F,T_0,T_1)].$$

The present value of Futures is calculated by taking expectation under risk-neutral measure (due to daily settlement mechanism) only,

$$
\begin{align*}
Futures(t;\textbf{T})&=E^Q_t(N[1-Ibor(T_0^F;T_0^F,T_0,T_1)])\\
&=N[1-R_{Fut}^j(t;\textbf{T})],
\end{align*}
$$

where 

$$
R_{Fut}^j(t;\textbf{T})=E_t^{Q_{T_1}}[Ibor(T_0^F;T_0^F,T_0,T_1)]+C_{Fut}^j(t;T_0).
$$

The convexity adjustment depends on the particular model, and arises because of the daily marking to market and margination mechanism of Futures, which is not negligible. The standard expiry months $T_1$ are March, June, September and December.

Therefore,

> $$R_{Fut}^j(t_0;T_0^F,T_0,T_1)-C_{Fut}^j(t_0;T_0)=\frac{1}{\tau(T_0,T_1)}(\frac{P^j(T^S,T_0)}{P^j(T^S,T_1)}-1).$$
> 
> Note that there is a subtle point that the convexity adjustment is from today to $T_1$, not from $T^S$.

---
#### 4. Interest Rate Swaps (IRS)

![png](https://scuiaa555.github.io/assets/images/2017-04-15-irs.png)
![png](https://scuiaa555.github.io/assets/images/2017-04-15-irs2.png)

The fair rate of IRS is

$$
\begin{align*}
R_{IRS}^j(t;\textbf{T},\textbf{S})&=\frac{\sum_{i=1}^nP^D(t;T_i)E_t^{Q_{T_i}}[Ibor(T_{i-1}^F;T_{i-1}^F,T_{i-1},T_{i})]\tau(T_{i-1},T_i)}{A^D(t;\textbf{S})}\\
&=\frac{\sum_{i=1}^nP^D(t;T_i)(\frac{1}{\tau(T_{i-1},T_i)}(\frac{P^j(t,T_{i-1})}{P^j(t,T_i)}-1))\tau(T_{i-1},T_i)}{A^D(t;\textbf{S})}\\
&=\frac{\sum_{i=1}^nP^D(t;T_i)(\frac{P^j(t,T_{i-1})}{P^j(t,T_i)}-1)}{A^D(t;\textbf{S})}
\end{align*}
$$

where $A^D(t;\textbf{S})$ is the risk-free annuity given by

$$A^D(t;\textbf{S})=\sum_{j=1}^mP^D(t,S_j)\tau(S_{j-1},S_j).$$

> So at time $T^S$,
> 
> $$
> \begin{align*}
> R_{IRS}^j(T^S;\textbf{T},\textbf{S})=\frac{\sum_{i=1}^nP^D(T^S;T_i)(\frac{P^j(T^S,T_{i-1})}{P^j(T^S,T_i)}-1)}{A^D(T^S;\textbf{S})},
> \end{align*}
> $$
> 
> where
> 
> $$A^D(T^S;\textbf{S})=\sum_{j=1}^mP^D(T^S,S_j)\tau(S_{j-1},S_j).$$

---
#### 5. Overnight Indexed Swaps (OIS)


> At time $T^S$,
> 
> $$
> \begin{align*}
> R_{OIS}^j(T^S;\textbf{T},\textbf{S})=\frac{P^D(T^S,T_0)-P^D(T^S,T_n)}{A^D(T^S;\textbf{S})}.
> \end{align*}
> $$

---
### Bootstrapping OIS curve



