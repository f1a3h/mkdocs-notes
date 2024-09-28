---
title: Solutions to selected exercises
---

## Chap. 1

**Exercise 1.1**

这题关键在于注意到 affine function $f(\mathbf{x})$ 实际是由 $f(\mathbf{0})+f^*(\mathbf{x})$ 组成的, 其中 $f^*(\mathbf{x})$ 是 linear function.

要证明题述的 $f$ 是一个 affine function, 我们只需要证明所有满足 concave 和 convex 的 $f$ 都可以表达为 $a_{0}+\sum_{i=1}^n a_{i}x_{i}$ 的形式即可, 显然 $a_{0} \to f(\mathbf{0}), \sum_{i=1}^n a_{i}x_{i} \to f^*(\mathbf{x})$, 而满足 $\mathfrak{R}^n \mapsto \mathfrak{R}$ 的 linear function 只能是 $\sum_{i=1}^n a_{i}x_{i}$ 的形式, 因此我们只需证明 $f^*(\mathbf{x})=f(\mathbf{x})-f(\mathbf{0})$ 是 linear function 即可.

令 $f^*(\mathbf{x})=f(\mathbf{x})-f(\mathbf{0})$, 下面证明 $f^*$ 是 linear function.

首先, $f^*(\mathbf{0})=f(\mathbf{0})-f(\mathbf{0})=0$.

然后, 我们证明对于任意实数 $c$, 都有 $f^*(c \mathbf{x})=cf^*(\mathbf{x})$:

1. $0 \leq c \leq 1$ 时, 有
   <div>
   $$ \begin{aligned}f^{*}(c \mathbf{x}+(1-c) \mathbf{0}) &= cf(\mathbf{x}) + (1-c)f(\mathbf{0}) - f(\mathbf{0}) \\ &= c[f(\mathbf{x}) - f(\mathbf{0})] \\ &= cf^*(\mathbf{x})\end{aligned} $$
   </div>
2. $c>1$ 时, 有
   <div>
   $$ f^*(\mathbf{x}) = f^*\left( \frac{1}{c} \cdot c \mathbf{x} \right)=\frac{1}{c} f^*(c \mathbf{x}) $$
   </div>
   因此满足 $f^*(c \mathbf{x}) = cf^*(\mathbf{x})$
3. $c \le 0$ 时,
   <div>
   $$ \begin{aligned}f(\mathbf{0}) &= f\left( \frac{1}{2} \mathbf{x} + \frac{1}{2}(-\mathbf{x})\right) \\ &= \frac{1}{2}\left[f(\mathbf{x}) + f(-\mathbf{x})\right] \\ &= \frac{1}{2}[f^*(\mathbf{x}) + f^*(-\mathbf{x})] + f(\mathbf{0}) \\ \end{aligned} $$
   </div>
   于是我们有 $f^*(\mathbf{x})+f^*(-\mathbf{x})=0$, 再直接套用 $c > 0$ 的结果即可证明 $c < 0$ 时同样满足 $f^*(c \mathbf{x}) = cf^*(\mathbf{x})$

最后, 我们证明对于任意实数 $a, b$, 有 $f^*(a\mathbf{x} + b\mathbf{y}) = af^*(\mathbf{x}) + bf^*(\mathbf{y})$.

1. $ab = 0$, 此时待证式等价于 $f^*(c \mathbf{x})=cf^*(\mathbf{x})$
2. $ab>0$, 有 $f^*(a \mathbf{x} + b \mathbf{y}) = (a+b)f^*\left( \frac{a \mathbf{x} + b \mathbf{y}}{a+b} \right)$, 令 $\lambda = \frac{a}{a+b}$, 于是
   <div>
   $$
   \begin{aligned}
   f^*(a \mathbf{x} + b \mathbf{y}) &= (a+b)f^*(\lambda \mathbf{x} + (1-\lambda) \mathbf{y}) \\
   &= (a+b)[\lambda f^*(\mathbf{x}) + (1-\lambda) f^*(\mathbf{y})] \\
   &= af^*(\mathbf{x}) + b f^*(\mathbf{y})
   \end{aligned}
   $$
   </div>
3. $ab < 0$, 令 $\mathbf{y}' = -\mathbf{y}$, 然后套用 $ab > 0$ 的式子以及 $f^*(-\mathbf{y}) = -f^*(\mathbf{y})$ 即可得证.

综上, $f^*(\mathbf{x}): \mathfrak{R}^n \mapsto \mathfrak{R}$ 为 linear function, 从而 $f(\mathbf{x}) = f(\mathbf{0}) + f^*(\mathbf{x})$ 为 affine function.

**Exercise 1.12 (Chebychev center)**

我们只需要令 $\mathbf{y}$ 与 $P$ 边界上最近的一点的距离尽可能大即可, 如何表示出 $\mathbf{y}$ 与 $P$ 边界上的点的距离呢? 不妨考虑 $m=1, n=2$ 的情况, 不难发现 $P$ 的边界是与 $\mathbf{a}_{i}$ 垂直的一条直线, 当 $n=3$ 时则是与 $\mathbf{a}_{i}$ 垂直的平面, 而 $\mathbf{y}$ 与最近的点的距离刚好是 $|\mathbf{a}_{i}' \mathbf{y} - b_{i}|$, 推广到高位情况下则是 $\min_{i}|\mathbf{a}_{i}'\mathbf{y}-b_{i}|$, 转化为 minimize 则有 LP (显然只需要 $\mathbf{y}$ 在 $P$ 内部即可):

$$
\begin{aligned}
\text{minimize} \quad & \max_{i}\{-\lvert \mathbf{a}_{i}'\mathbf{y} - b_{i} \rvert \} \\
\text{subject to} \quad & \mathbf{a}_{i}'\mathbf{y} \leq b_{i} ~\forall i. \\
\end{aligned}
$$
