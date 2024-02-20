---
title: "MIT 18.330: Introduction to Numerical Analysis"
date: 2024-02-05
draft: false
math: true
description: Introduction to Numerical Analysis
categories:
  - Study Notes
  - Math
tags:
  - Math
---


!!! info
	MIT [Introduction to Numerical Analysis, Spring 2020](https://github.com/mitmath/18330) 基于课程的 slides &  notes 的学习笔记。

## Root Finding

假设我们需要求解方程 $g(x)=h(x)$，这相当于求解函数 $f(x):= g(x)-h(x)$ 的零点。由上一节课的知识我们可以将 $f(x)$ 写成 Taylor 多项式的形态然后转化为求多项式的根。

令 $p(x)=a_0+a_1x+\cdots+a_nx^n$，根据 *[fundamental theorem of algebra](https://en.wikipedia.org/wiki/Fundamental_theorem_of_algebra)* ，$p(x)$ 存在 $n$ 个复数根，因此我们可以将 $p(x)$ 写成 $p(x)=(x-x_1)^{m_1}\cdots(x-x_k)^{m_k}$ 的形式。

当 $n=2$ 时，$p(x)$ 的根有一个很简单的通解公式；当 $n=3,4$ 时，有一个很复杂的通解公式；当 $n\ge5$ 时，根据 [Abel-Ruffini theorem](https://en.wikipedia.org/wiki/Abel%E2%80%93Ruffini_theore) ，此时没有通解公式。

我们的目的是使用 numerical methods 找出所有的**近似**解，存在一些 methods (e.g. [Aberth](https://en.wikipedia.org/wiki/Aberth_method)) 可以同时求出所有解，但是接下来的内容主要考虑的是求单个解。

由于 numerical methods 只能逼近解，所以这个 method 必然是 iterative 的（感性理解一下）。假设我们使用算法 $g$，初始值是 $x_0$，那么每次迭代的解之间存在 $x_{n+1}=g(x_n)$ 的关系。

假设 $\{x_n\}$ 收敛于 $x^{\ast}$，那么有 $g(x^*)=x^*$（假设 $g$ 连续），其中 $x^*$ 是 $g$ 的不动点，相当于 $g$ 与 $y=x$ 的交点，于是问题转化为求解 $f(x):=g(x)-x$ 的根。

不动点存在定理：

- If $g$ is continuous and maps $[a,b]$ into itself, then $\exists$ a fixed point
- If $|g'(x)|\le k\ \forall\  x\in [a,b]$ with $k<1$, then unique
- Called a *contraction mapping*

我们将这样一种迭代称为 *discrete-time dynamical system* 。

令 $\delta_n := x_n-x^*$ ，我们有（拉格朗日中值定理）

$$
\begin{aligned}
	\delta_{n+1}&=x_{n+1}-x^*\\
	&= g(x_n)-x^*\\
	&= g(x^*+\delta_n)-x^*\\
	&\simeq \delta g'(x^*)
\end{aligned}
$$

令 $\alpha=g'(x^*)$ ，我们有 $\delta_{n+1}=\alpha\delta_n$ 

- Decays (*stable fixed point*): $|\alpha|<1$ 
- Grows (*unstable fixed point*): $\alpha>1$ 

如果一种迭代满足 $\lim_{n\to \infty} \dfrac{|x_{n+1}-x^*|}{|x_n-x^*|\alpha}=\lambda$（$\alpha, \lambda$ 是常数）的话，我们就称它是 *order $\alpha$* 的。

- $\alpha=1$: *linearly convergent*
- $\alpha=2$: *quadratic convergent*

牛顿迭代求根法：$x_{n+1}=x_n-\dfrac{f(x_n)}{f'(x_n)}$ 是 quadratic convergent。

