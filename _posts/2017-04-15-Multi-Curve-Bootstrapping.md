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

Assume $t_0$=today, $T^S$=start date/settlement date of the contract (usually equals to $Spot(t_0)$, say 2 business days for EUR market, except Deposit ON, TN, SN), $T_i^F$=fixing date of the ith Ibor (the lag between fixing date and start date of period is also called spot lag, 2 business days for EUR and USD, 0 day for GBP, ref. p.2 of ***Henrard, Marc. "The irony in derivatives discounting part II: The crisis." Wilmott Journal 2.6 (2010): 301-316.***) and [$T_i,T_{i+1}$]=period of interest accrued. The time interval denoted as $j$ is typically 1d (overnight), 1m, 3m, 6m and 12m. $\large{T_1=T_0+j}$

Main references:

* ***Ametrano, Ferdinando M., and Marco Bianchetti. "Everything you always wanted to know about multiple interest rate curve bootstrapping but were afraid to ask." (2013).***

#### Deposits



