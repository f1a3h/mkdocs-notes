---
title: "Sample Space and\r Probability"
---
[Bertrand's paradox](https://en.wikipedia.org/wiki/Bertrand_paradox_(probability)): 一个很有意思的悖论, 4 个不同的答案都看上去很合理.

- [Bertrand's Paradox (with 3blue1brown) - Numberphile](https://youtu.be/mZBwsm6B280?si=fWuLxxyPlHMbnhSR)
- [More on Bertrand's Paradox (with 3blue1brown) - Numberphile](https://youtu.be/pJyKM-7IgAU?si=_o9JcZpH54HhKEoS)

[The Monty Hall Problem](https://en.wikipedia.org/wiki/Monty_Hall_problem)

## Total probability theorem and Bayes' rule

> [!theorem] Total Probability Theorem
> Let $A_1 ,\dots , A_n$ be disjoint events that form a partition of the sample space (each possible outcome is included in exactly one of the events $A_1, . . . , A_n$) and assume that $\Pr(A_i) > 0$, for all $i$. Then, for any event $B$, we have
> 
> $$\begin{aligned} \Pr(B) &= \Pr(A_{1} \cap B)+ \cdots + \Pr(A_{n} \cap B) \\ &= \Pr(A_{1})\Pr(B \mid A_{1}) + \cdots + \Pr(A_{n})\Pr(B | A_{n}) \end{aligned}$$

> [!theorem] Bayes' Rule
> 
> Let $A_1 , A_2,\dots , A_n$ be disjoint events that form a partition of the sample space, and assume that $\Pr(A_i) > 0$, for all $i$. Then, for any event $B$ such that $\Pr(B) > 0$, we have
> 
> $$\begin{aligned}\Pr(A_{i} \mid B) &= \frac{\Pr(A_{i})\Pr(B \mid A_{i})}{\Pr(B)}\\ &= \frac{\Pr(A_{i})\Pr(B \mid A_{i})}{\Pr(A_{1})\Pr(B\mid A_{1})+\cdots+ \Pr(A_{n})\Pr(B \mid A_{n})} \end{aligned}$$

