---
title: Introduction
---
## Variants of the linear programming problem

*general* linear programming problem 的形式如下:

$$\begin{aligned}
\text{minimize} \quad & \mathbf{c}^{\prime}\mathbf{x} \\
\text{subject to} \quad & \mathbf{a}_i^{\prime}\mathbf{x} \geq b_i,\quad i\in M_1, \\
&\mathbf{a}_i^{\prime}\mathbf{x} \leq b_i,\quad i\in M_2,&  \\
&\mathbf{a}_i^{\prime}\mathbf{x} = b_i,\quad i\in M_3, \\
&x_j \geq 0,\quad j\in N_1, \\
&x_j \leq 0,\quad j\in N_2.
\end{aligned}$$

其中 $M_{1 \sim 3}$ 为 finite index sets, $N_{1\sim 2}$ 为 $\{1, \dots, n\}$ 的子集.

- *decision variables*: $x_{1}, \dots, x_{n}$
- *feasible solution/vector*: 满足所有约束条件的 vector $\mathbf{x}$
- *feasible set/region*: 所有 feasible solution 的集合
- *free/unrestricted* variable: 如果 $j$ 既不在 $N_1$ 也不在 $N_2$, 我们就将 $x_{j}$ 称为 free/unrestricted variable
- *objective/cost* function: $\mathbf{c'x}$
- *optimal (feasible) solution*: 使 objective function 取到最小值的 feasible solution $\mathbf{x^*}$
- *optimal cost*: $\mathbf{c'x^*}$
- *unbounded (below)*: optimal cost 为 $-\infty$

由于 $\mathbf{a}_{i}'\mathbf{x} \geq b_{i} \leftrightarrow (-\mathbf{a}_{i})'\mathbf{x} \geq -b_{i}$ 以及 $\mathbf{a}_{i}'\mathbf{x} = b_{i} \leftrightarrow \mathbf{a}_{i}'\mathbf{x} \geq b_{i} \land \mathbf{a}_{i}'\mathbf{x}\leq b_{i}$, 所以我们可以将约束不等式统一为 $\mathbf{a}_{i}'\mathbf{x}\geq b_{i}$ 的形式, 再令

$$\mathbf{A}=\left[\begin{array}{ccc}-&\mathbf{a}_1^{\prime}&-\\&\vdots&\\-&\mathbf{a}_m^{\prime}&-\end{array}\right], \quad \mathbf{b}=(b_{1}, \dots, b_{m}).$$

于是 LP problem 的 general form 成为:

$$\begin{aligned}
\text{minimize} \quad & \mathbf{c}^{\prime}\mathbf{x} \\
\text{subject to} \quad & \mathbf{Ax} \geq \mathbf{b}. \\
\end{aligned}$$

*standard form* 的 LP problem 形式如下:

$$\begin{aligned}
\text{minimize} \quad \mathbf{c}^{\prime}&\mathbf{x} \\
\text{subject to} \quad \mathbf{A}&\mathbf{x} = \mathbf{b} \\
& \mathbf{x} \geq \mathbf{0}.
\end{aligned}$$
通过以下方式我们可以将 general form 转化为 standard form:

1. Elimination of free variables: 将 free variable $x_{j}$ 改写为 $x_{j}^+ - x_{j}^-$, 然后令 variables $x_{j}^+, x_{j}^-\geq 0$ <==> 任何实数可以被写成两个非负数之差;
2. Elimination of inequality constraints: 对于 $\sum_{j=1}^n a_{ij}x_{j} \leq b_{i}$, 我们再添加一个满足 $x_{n+1} \geq 0$ 的 variable, 将不等式变为等式 $\sum_{j=1}^n a_{ij}x_{j} + x_{n+1} = b_{i}$. 我们将 $x_{n+1}$ 这样的 variable 称为 *slack* variable.

## Piecewise linear convex objective functions

> [!definition] Convex function
> A function $f:\mathfrak{R}^n \mapsto \mathfrak{R}$ is called *convex* if for every $\mathbf{x, y} \in \mathfrak{R}^n$, and every $\lambda \in [0,1]$, we have
> 
> $$f(\lambda \mathbf{x} + (1-\lambda)\mathbf{y})\leq\lambda f(\mathbf{x}) + (1-\lambda)f(\mathbf{y}).$$

> [!definition] Concave function
> A function $f:\mathfrak{R}^n \mapsto \mathfrak{R}$ is called *concave* if for every $\mathbf{x, y} \in \mathfrak{R}^n$, and every $\lambda \in [0,1]$, we have
> 
> $$f(\lambda \mathbf{x} + (1-\lambda)\mathbf{y})\geq\lambda f(\mathbf{x}) + (1-\lambda)f(\mathbf{y}).$$

对于 convex function, 任意连接图像上的两个点, 两点之间的曲线总是会在它们这条线段的下方, 俗称 "下凸".

形如 $f(\mathbf{x}) = a_{0} + \sum_{i=1}^n a_{i} x_{i}$ 的 function 被称为 *affine* function, 其中 $a_{0}, \dots, a_{n}$ 为 scalars. affine functions 是唯一既是 convex 又是 concave 的 functions.

convex function 一个重要的 property 就是 local minima <==> global minima, 这对于设计 efficient optimization algorithms 很有用.

另一个 property 是一组 convex functions 取 max 得到的仍然是 convex function, concave function 则是反过来:

> [!theorem]
> Let $f_{1}, \dots, f_{m}: \mathfrak{R}^n \mapsto \mathfrak{R}$ be convex functions. Then, the function $f$ defined by $f(\mathbf{x})=\max_{i=1, \dots, m} f_{i}(\mathbf{x})$ is also convex.
> 
> > [!proof]-
> > 令 $\mathbf{x, y} \in \mathfrak{R}^n, \lambda \in [0, 1]$, 我们有
> > 
> > $$\begin{align}f(\lambda \mathbf{x}+(1-\lambda)\mathbf{y}) &= \max_{i=1, \dots, m} f_{i}(\lambda \mathbf{x}+(1-\lambda)\mathbf{y}) \\ &\leq \max_{i=1, \dots, m}\lambda f_{i}(\mathbf{x})+(1-\lambda)f_{i}(\mathbf{y}) \\ &= \lambda\max_{i=1, \dots, m} f_{i}(\mathbf{x})+(1-\lambda)\max_{i=1, \dots, m} f_{i}(\mathbf{y}) \\ &= \lambda f(\mathbf{x}) + (1-\lambda)f(\mathbf{y})\end{align}.$$

形如 $\max_{i=1, \dots, m}(\mathbf{c}_{i}'\mathbf{x}+d_{i})$ 的 function 被称为 *piecewise linear function*. 它比 linear functions 更 general, 不一定能保持 linearity, 但是仍然是 convex 的 (根据上面的 theorem), 并且我们可以用它来 approximate 一个 general convex function.

不妨来考虑将 piecewise linear function 作为 objective function:

$$\begin{aligned}\mathrm{minimize}&\quad\max_{i=1,\dots,m}(\mathbf{c}_i^{\prime}\mathbf{x}+d_i)\\\mathrm{subject~to}&\quad\mathbf{Ax}\geq\mathbf{b}.\end{aligned}$$

令 $z=\max_{i=1, \dots ,m}(\mathbf{c}_{i}' \mathbf{x}+d_{i})$, 根据 $z$ 的表达式我们可以写出一系列对 $\mathbf{x}$ 的 constraints, 此时我们需要最小化的对象为 $z$, 问题转化为如下形式 ($z, \mathbf{x}$ 为 decision variables):

$$\begin{array}{rcl}\mathrm{minimize}&z\\\mathrm{subject~to}&z&\geq&\mathbf{c}_i^{\prime}\mathbf{x}+d_i,\\&\mathbf{A}\mathbf{x}&\geq&\mathbf{b},\end{array}\quad i=1,\ldots,m,$$

> [!summary]+ 
> 可以看出来经过一个简单的转化, 我们可以将 objective function 为 piecewise linear function (或能用它近似的 convex function) 的非 LP prob 转化为一个 LP prob.
> 
> 或者是 $f$ 为 piecewise linear function 的形如 $f(\mathbf{x})\leq h$ 的 constraint 也能改写为
> 
> $$\mathbf{f}_{i}'\mathbf{x}+g_{i}\leq h, \quad i=1, \dots, m,$$
> 
> 然后接着应用 LP.

显然绝对值函数是 piecewise linear function, 那么包含绝对值的 problems 都可以尝试用上述技巧进行转化. 具体而言, 形式如下的 prob:

$$\begin{aligned}\mathrm{minimize}&&\sum_{i=1}^nc_i|x_i|\\\mathrm{subject~to}&&\mathbf{Ax}\geq\mathbf{b},\end{aligned}$$
> [!warning] 
> 系数 $c_{i}$ 必须为非负, 否则 objective function 就不是 convex function.

我们可以将其改写为:

$$\begin{array}{rl}\mathrm{minimize}&\sum_{i=1}^nc_iz_i\\\mathrm{subject~to}&\mathbf{Ax}\geq\mathbf{b}\\&x_i\leq z_i,\quad&i=1,\ldots,n,\\&-x_i\leq z_i,\quad&i=1,\ldots,n.\end{array}$$

或者利用 "任意非负数都可以写成两个非负数之和", 引入两个新的 variants 改为如下形式:

$$\begin{aligned}\mathrm{minimize}&\quad\sum_{i=1}^{n}c_{i}(x_{i}^{+}+x_{i}^{-})\\\mathrm{subject~to}&\quad\mathbf{A}\mathbf{x}^{+}-\mathbf{A}\mathbf{x}^{-}\geq\mathbf{b}\\&\quad\mathbf{x}^{+},\mathbf{x}^{-}\geq\mathbf{0}.\end{aligned}$$
## Algorithms and operation counts

optimization problems 一般使用 *algorithms* 解决, 算法的运行时间可以通过 count the number of arithmetic operations 来衡量, 但是对于复杂的问题来说具体的 count 不现实, 因此我们引入 magnitude notation.

> [!definition] Magnitude notation
> Let $f$ and $g$ be functions that map positive numbers to positive numbers.
> 
> 1. We write $f(n)=O(g(n))$ if there exist positive numbers $n_{0}$ and $c$ such that $f(n)\leq cg(n)$ for all $n \geq n_{0}$.
> 2. We write $f(n)=\Omega(g(n))$ if there exist positive numbers $n_{0}$ and $c$ such that $f(n) \geq cg(n)$ for all $n \geq n_{0}$.
> 3. We write $f(n) = \Theta(g(n))$ if both $f(n) = O(g(n))$ and $f(n)=\Omega(g(n))$ hold.

