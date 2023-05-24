---
title: "Total Returns from Evolution Operators"
date: 2023-05-24
draft: false
categories:
- Quant
tags:
- Math
- Algebra
- Quant
- Finance-Basics
---

Multi-period compounding of returns is often introduced through the analysis of cashflows from the investment of a unit
principal at a fixed rate. Here we take a different approach: using the math of time-evolution operators to examine a 
price timeseries.


## Time Evolution Operators

$$U(t, t + \tau): f(t) \to f(t + \tau)$$

to evolve a function in time

$$f(t_2) = U(t_1, t_2) f(t_1)$$

A natural candidate for satisfying this definition is simply the ratio of the timeseries at the boundary points. 

$$U(t_1, t_2) = \frac{f(t_2)}{f(t_1)}$$

Algebraic relation among propagators gives an important identity, choosing three points in time $t_1, t_2, t_3$ such that $t_1 < t_2 < t_3$

$$U(t_1, t_3) = U(t_1, t_2) U(t_2, t_3)$$
