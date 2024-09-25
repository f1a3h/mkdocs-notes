---
title: The geometry of linear programming
---
## Polyhedra and convex sets

### Hyperplanes, halfspaces, and polyhedra

> [!definition] Polyhedron
> A *polyhedron* is a set that can be described in the form $\{\mathbf{x}\in \mathfrak{R}^n | \mathbf{Ax \geq b}\}$, where $\mathbf{A}$ is an $m \times n$ matrix and $\mathbf{b}$ is a vector in $\mathfrak{R}^m$ .

根据 LP 的 general form, 任何 LP prob 的 feasible sets 都是 polyhedron. 形如 $\{\mathbf{x} \in \mathfrak{R}^n | \mathbf{Ax=b, x\geq{0}}\}$ 的 set 也会是一个 polyhedron, 我们称之为 *polyhedron in standard form*.

> [!definition]
> A set $S \subset \mathfrak{R}^n$ is *bounded* if there exists a constant $K$ that the absolute value of every component of every element of $S$ is less than or equal to $K$.

hyperplane 与 halfspace 是与 single linear constraint 相关的概念.

> [!definition] Hyperplane/Halfspace
> Let $\mathbf{a}$ be a nonzero vector in $\mathfrak{R}^n$ and let $b$ be a scalar.
> 
> 1. The set $\{\mathbf{x}\in \mathfrak{R}^n | \mathbf{a'x}= b\}$ is called a *hyperplane*.
> 2. The set $\{\mathbf{x} \in \mathfrak{R}^n | \mathbf{a'x}\geq b\}$ is called a *halfspace*.

> [!note]+
> hyperplane 是 halfspace 的边界. 
> 另外, $\mathbf{a}$ 与 hyperplane 是 perpendicular 的关系.
> 一个 polyhedron 是有限个 halfspaces 的 intersection.
> 
> ![image.png](https://picgo-1259588753.cos.ap-beijing.myqcloud.com/202409211307745.png)

### Convex Sets

> [!definition] Convex sets
> A set $S \subset \mathfrak{R}^n$ is *convex* if for any $\mathbf{x, y} \in S$, and any $\lambda \in [0, 1]$, we have $\lambda \mathbf{x} + (1-\lambda)\mathbf{y} \in S$.
> 
> ![image.png](https://picgo-1259588753.cos.ap-beijing.myqcloud.com/202409211318389.png)

> [!definition] Convex combination/hull
> Let $\mathbf{x}^1, \dots, \mathbf{x}^k$ be vectors in $\mathfrak{R}^n$ and let $\lambda_{1}, \dots, \lambda_{k}$ be nonnegative scalars whose sum is unity.
> 
> 1. The vector $\sum_{i=1}^k \lambda_{i}\mathbf{x}^i$ is said to be a *convex combination* of the vectors $\mathbf{x}^1, \dots, \mathbf{x}^k$.
> 2. The *convex hull* of the vectors $\mathbf{x}^1, \dots, \mathbf{x}^k$ is the set of all convex combinations of these vectors.
> 
> ![image.png](https://picgo-1259588753.cos.ap-beijing.myqcloud.com/202409211318468.png)

下面的定理阐述了上面提到的几个概念之间的关系.

> [!theorem]
> 
> 1. The intersection of convex sets is convex.
> 2. Every polyhedron is a convex set.
> 3. A convex combination of a finite number of elements of a convex set also belongs to that set.
> 4. The convex hull of a finite number of vectors is a convex set.

## Extreme points, vertices, and basic feasible solutions

在 Section 1.4 中可以看出, 最优解倾向于出现在 polyhedron 的 “corner” 中. 本节会用 3 种不同方式定义 “corner”, 然后解释这三种定义之间是等价的.

> [!definition] Extreme point
> Let $P$ be a polyhedron. A vector $\mathbf{x} \in P$ is an *extreme point* of $P$ if we cannot find two vectors $\mathbf{y}, \mathbf{z} \in P$, both different from $\mathbf{x}$, and a scalar $\lambda \in [0,1]$, such that $\mathbf{x}=\lambda \mathbf{y} + (1-\lambda) \mathbf{z}$.
> 
> ![image.png](https://picgo-1259588753.cos.ap-beijing.myqcloud.com/202409211357634.png)

extreme point 就是不能被 $P$ 中其他任意两个点的 convex combination 表示的点.

> [!definition] Vertex
> Let $P$ be a polyhedron. A vector $\mathbf{x} \in P$ is a *vertex* of $P$ if there exists some $\mathbf{c}$ such that $\mathbf{c'z} < \mathbf{c'y}$  for all $\mathbf{y}$ satisfying $\mathbf{y} \in P$ and $\mathbf{y} \not= \mathbf{x}$.
> 
> ![image.png](https://picgo-1259588753.cos.ap-beijing.myqcloud.com/202409211433382.png)

vertex 就是我们能找到一个 hyperplane $\{\mathbf{y} | \mathbf{c'y}=\mathbf{c'x}\}$ 使得 $P$ 在这个 hyperplane 的一侧且与这个 hyperplane 有且仅有一个交点 $\mathbf{x}$, 这个 $\mathbf{x}$ 就是 vertex.

下面给出一个用 linear constraints 表示的, 并且能导出为 algebraic test 的 definition.

考虑用以下 linear equality and inequality constraints 定义的 polyhedron $P \subset \mathfrak{R}^n$

$$\begin{aligned}&\mathrm{a}_{i}^{\prime}\mathrm{x~\geq~}b_{i},\quad&i\in M_{1},\\&\mathrm{a}_{i}^{\prime}\mathrm{x~\leq~}b_{i},\quad&i\in M_{2},\\&\mathrm{a}_{i}^{\prime}\mathrm{x~=~}b_{i},\quad&i\in M_{3},\end{aligned}$$

其中 $M_{1}, M_{2}, M_{3}$ 为 finite index sets, $\mathbf{a}_{i}$ 是 $\mathfrak{R}^n$ 中的 vector, $b_{i}$ 是 scalar.

> [!definition] Active/Binding
> If a vector $\mathbf{x}^*$ satisfies $\mathbf{a}_{i}'\mathbf{x}^* = b_{i}$ for some $i$ in $M_{1}, M_{2},$ or $M_{3}$, we say that the corresponding constraint is *active* or *binding* at $\mathbf{x}^*$.

当满足在 $\mathbf{x}^*$ 处 active 的 constraints 有 $n$ 个时, 我们则得到了一个 $n$ linear equations in $n$ unknowns. 当且仅当这 $n$ 个 equations 是 “linear independent” 时这个系统有唯一解.

> [!theorem]
> Let $\mathbf{x}^*$ be an element on $\mathfrak{R}^n$ and let $I = \{i | \mathbf{a}_{i}'\mathbf{x}^* = b_{i}\}$ be the set of indices of constraints that are active at $\mathbf{x}^*$. Then, the following are equivalent:
> 
> 1. There exist $n$ vectors in the set $\{\mathbf{a}_{i} | i \in I\}$, which are linearly independent.
> 2. The span of the vectors $\mathbf{a}_{i}$, $i \in I$, is all of $\mathfrak{R}^n$, that is, every element of $\mathfrak{R}^n$ can be expressed as a linear combination of the vectors $\mathbf{a}_{i}$, $i \in I$.
> 3. The system of equations $\mathbf{a}_{i}' \mathbf{x} = b_{i}$, $i \in I$, has a unique solution.

换句话说, 当我们拥有了 $n$ 个 linearly independent active constraints 时, 就可以得到一个 unique vector $\mathbf{x}^*$. 不过, $\mathbf{x}^*$ 不一定能满足 inactive constraints, 此时若它能满足所有的 equality constraints 我们就称 $\mathbf{x}^*$ 是一个 basic solution.

> [!definition] Basic (feasible) solution
> Consider a polyhedron $P$ defined by linear equality and inequality constraints, and let $\mathbf{x}^*$ be an element of $\mathfrak{R}^n$.
> 
> 1. The vector $\mathbf{x}^*$ is a *basic solution* if:
> 	1. All equality constraints are active;
> 	2. Out of the constraints that are active at $\mathbf{x}^*$, there are $n$ of them that are linearly independent.
> 2. If $\mathbf{x}^*$ is a basic solution that satisfies all of the constraints, we say that it is a *basic feasible solution*.
> 
> ![image.png](https://picgo-1259588753.cos.ap-beijing.myqcloud.com/202409211513717.png)

到目前为止, 我们给出了 3 个针对同一个概念的不同定义, 其中 2 个是 geometric (extreme point, vertex), 另一个是 algebraic (basic feasible solution).

> [!theorem]
> Let $P$ be a nonempty polyhedron and let $\mathbf{x}^* \in P$. Then, the following are equivalent:
> 
> 1. $\mathbf{x}^*$ is a vertex;
> 2. $\mathbf{x}^*$ is an extreme point;
> 3. $\mathbf{x}^*$ is a basic feasible solution.

> [!proof]
> #TODO

> [!corollary]
> Given a finite number of linear inequality constraints, there can only be a finite number of basic or basic feasible solutions.
> 
> > [!proof]-
> > 给一个不太严谨的证明.
> > 
> > 假设给出了 $m$ 个 linear inequality constraints, 从其中取出 $n$ 个满足 linearly independent constraints 的取法是有限的, 而每个取法都唯一对应一个 basic (feasible) solution, 因此 basic (feasible) solution 的数量也一定是有限的.

## Polyhedra in standard form

考虑 standard form 的 LP, 令 $P = \{\mathbf{x} \in \mathfrak{R}^n | \mathbf{Ax} = \mathbf{b}, \mathbf{x} \geq \mathbf{0}\}$, 我们希望找到 basic solution. 不妨设 $\mathbf{A}$ 是一个 $m \times n$ 的矩阵且这 $m$ 个 row vectors 是 linearly independent 的, $\mathbf{x}$ 要有解的话必然有 $m \leq n$. 显然解出 $\mathbf{Ax} = \mathbf{b}$ 只能有 $m$ 个 active constraints, 此时我们只需要将剩下 $n-m$ 个 $x_{i}$ 设为 $0$ 来满足 $\mathbf{x} \geq 0$ 即可得到 $n$ 个 active constraints.

> [!theorem]
> Consider the constraints $\mathbf{Ax} = \mathbf{b}$ and $\mathbf{x} \geq \mathbf{0}$ and as­sume that the $m \times n$ matrix $\mathbf{A}$ has linearly independent rows. A vector $x \in \mathfrak{R}^n$ is a basic solution if and only if we have $\mathbf{Ax} = \mathbf{b}$, and there exist indices $B(1), \dots, B(m)$ such that:
> 
> 1. The columns $\mathbf{A}_{B(1)}, \dots, \mathbf{A}_{B(m)}$ are linearly independent;
> 2. If $i \not= B(1), \dots, B(m)$, then $x_{i}=0$.

通过上述定理, 我们可以根据下面的程序来构造出一个 standard form polyhedron 的所有 basic solutions.

> [!summary] Procedure for constructing basic solutions
> 
> 1. Choose $m$ linearly independent columns $\mathbf{A}_{B(1)}, \dots, \mathbf{A}_{B(m)}$.
> 2. Let $x_{i}=0$ for all $i \not= B(1), \dots, B(m)$.
> 3. Solve the system of $m$ equations $\mathbf{Ax} = \mathbf{b}$ for the unknowns $x_{B(1)}, \dots, x_{B(m)}$.

如果通过这个程序得到的一个 basic solution 满足 nonnegative, 那么它就是一个 basic feasible solution.

若 $\mathbf{x}$ 是一个 basic solution, 那它对应的 $x_{B(1)}, \dots, x_{B(m)}$ 被称为 *basic variables*, 其余的 variables 则称为 *nonbasic*, 同理, 对应的 columns $\mathbf{A}_{B(1)}, \dots, \mathbf{A}_{B(m)}$ 为 *basic columns*, 并且它们 form a *basis* of $\mathfrak{R}^m$. 对于 $\mathbf{A}$ 的 basic columns 有不同组合, 也就会得到 *distinct/different* bases; 我们通常将 *basic indices* 集合 $\{B(1), \dots, B(m)\}$ 不同的 bases 看作是不同的. (翻译的有点绕口 QAQ)

由 $m$ 个 basic columns 组合而成的 matrix $\mathbf{B}$ 称为 *basic matrix*, 类似地我们也可以定义 basic variables 的组合 vector $\mathbf{x}_{B}$.

借由下图可以得到一个 intuitive view of basic solutions.

![image.png](https://picgo-1259588753.cos.ap-beijing.myqcloud.com/202409212017223.png)

### Correspondence of bases and basic solutions

bases 到 basic solution 的映射关系可以看作一个非单射函数, 即不同的 bases 可能得到相同的 basic solution, 这一现象有重要的 algorithmic implications, 与 degeneracy 密切相关.

### Adjacent basic solutions and adjacent bases

当两个不同的 basic solutions 对应的 linearly independent constraints 有 $n-1$ 个是相同的时, 我们就称它们是 *adjacent*. adjacent bases 的定义也是类似的.

### The full row rank assumption on $\mathbf{A}$

> [!theorem]
> Let $P=\{\mathbf{x} | \mathbf{Ax} = \mathbf{b}, \mathbf{x} \geq \mathbf{0}\}$ be a nonempty polyhedron, where $\mathbf{A}$ is a matrix of dimensions $m \times n$, with rows $\mathbf{a}_{1}', \dots, \mathbf{a}_{m}'$.
> 
> Suppose that $\mathrm{rank}(A) = k < m$ and that the rows $\mathbf{a}_{i_{1}}', \dots, \mathbf{a}_{i_{k}}'$ are linearly independent. Consider the polydron
> 
> $$Q = \{\mathbf{x} | \mathbf{a}_{i_{1}}'\mathbf{x} = b_{i}, \dots, \mathbf{a}_{i_{k}}'\mathbf{x} = b_{i_{k}}, \mathbf{x} \geq \mathbf{0}\}.$$
> 
> Then $Q=P$.

根据此定理, 我们可以得知, 只要满足 feasible set 是非空的, 任意 standard form 的 LP prob 都可以 reduce 到一个等价的且所有 equality constraints 都是 linearly independent 的 LP prob.

## Degeneracy

在 general form 下约束形式是 $\geq$, 那么自然 active constraints 的数量是可以大于 $n$ 的, 此时我们就称我们有一个 degenerate basic solution.

> [!definition] Degenerate basic solution
> A basic solution $\mathbf{x} \in \mathfrak{R}^n$ is said to be *degenerate* if more than $n$ of the constraints are active at $\mathbf{x}$.
> 
> ![image.png](https://picgo-1259588753.cos.ap-beijing.myqcloud.com/202409212047165.png)

### Degeneracy in standard form polyhedra

> [!definition]
> Consider the standard form polyhedron $P = \{\mathbf{x} \in \mathfrak{R}^n | \mathbf{Ax} = \mathbf{b}, \mathbf{x} \geq \mathbf{0}\}$ and let $\mathbf{x}$ be a basic solution. Let $m$ be the number of rows of $\mathbf{A}$. The vector $\mathbf{x}$ is a *degenerate* basic solution if more than $n - m$ of the components of $\mathbf{x}$ are zero.

这算是上一个定义的 special case.

> [!note] 
> 注意区分它与 row rank $< m$ 的情况. 这里的产生原因不是 row rank 而是 $\mathbf{A, b}$ 较为特殊.
> 
> 常见的题目中, $\mathbf{A}$ 和 $\mathbf{b}$ 通常具有一个特殊的结构来达到 degeneracy. 不过 degeneracy 往往比这个 argument 看上去更常见.

![image.png](https://picgo-1259588753.cos.ap-beijing.myqcloud.com/202409212102666.png)

在 standard form polyhedra 中对 degeneracy 进行 visualize, 我们不妨令 $n-m=2$ 并考虑 feasible set 在这个 2D 下的投影. 通过下图的例子, 我们可以看出要构造出 degeneracy, 我们首先要找到 $B$ 点这样的特殊点, 然后从它的几个交线中任意选出 2 条作为 nonbasic variables 满足的条件, 其余的 variables 则设为 basic variables, 这样的选择往往有多个.

![image.png](https://picgo-1259588753.cos.ap-beijing.myqcloud.com/202409212119608.png)

### Degeneracy is not a purely geometric property

basic feasible solution 的 degeneracy 并不是一个 geometric property, 而是取决于一个 polyhedron 特定的 representation.

在某个 representation 下是 degenerate 的 basic feasible solution 在另一个 representation 可以是 nondegenerate 的. 如果一个 basic feasible solution 在某个特定的 standard form representation 下是 degenerate 的, 那么可以证明它在同一 polyhedron 的任何 standard form representation 下都是 degenerate 的.

## Existence of extreme points

这一小节我们讨论 polyhedron 具有至少一个 extreme point 的充要条件.

下图启发我们 extreme point 的存在取决于这个 polyhedron 是否包含一个 infinite line.

![image.png](https://picgo-1259588753.cos.ap-beijing.myqcloud.com/202409212224078.png)

> [!definition]
> A polyhedron $P \subset \mathfrak{R}^n$ *contains a line* if there exists a vector $\mathbf{x} \in P$ and a nonzero vector $\mathbf{d} \in \mathfrak{R}^n$ such that $\mathbf{x} + \lambda \mathbf{d} \in P$ for all scalars $\lambda$.

> [!theorem]
> Suppose that the polyhedron $P = \{\mathbf{x} \in \mathfrak{R}^n | \mathbf{a}_{i}' \mathbf{x} \geq b_{i}, i = 1, \dots, m\}$ is nonempty. Then, the following are equivalent:
> 
> 1. The polyhedron $P$ has at least one extreme point.
> 2. The polyhedron $P$ does not contain a line.
> 3. There exist $n$ vectors out of the family $\mathbf{a}_{1}, \dots, \mathbf{a}_{m}$, which are linearly independent.

> [!proof]
> #TODO

> [!note] 
> 一个 bounded polyhedron 必不包含一条 line, 类似地, positive orthant $\{\mathbf{x} | \mathbf{x} \geq \mathbf{0}\}$ 也不包含一条 line, 由于 standard form 的 polyhedron 包含了 positive orthant, 因此它也不会包含一条 line.

> [!corollary]
> Every nonempty bounded polyhedron and every nonempty polyhedron in standard form has at least one basic feasible solution.

## Optimality of extreme points

> [!theorem]
> Consider the linear programming problem of minimizing $\mathbf{c'x}$ over a polyhedron $P$. Suppose that $P$ has at least one extreme point and that there exists an optimal solution. Then, there exists an optimal solution which is an extreme point of $P$.
> 
> > [!proof]-
> > 令 $Q$ 为 optimal solutions 的集合, 我们假设它是非空的. 令 $P=\{\mathbf{x} \in \mathfrak{R}^n | \mathbf{Ax} \geq \mathbf{b}\}$ 为 general form 的约束条件的 polyhedron, 不妨设 $\mathbf{c'x}$ 的 optimal value 为 $v$, 于是 $Q = \{\mathbf{x} \in \mathfrak{R}^n | \mathbf{Ax} \geq \mathbf{b}, \mathbf{c'x} = v\}$ 也是一个 polyhedron, 并且 $Q \subset P$, 因为 $P$ 不存在 line ($\leftarrow$ 存在 extreme point), 因此 $Q$ 也不存在 line, 从而 $Q$ 也存在 extreme point.
> > 
> > ![image.png](https://picgo-1259588753.cos.ap-beijing.myqcloud.com/202409221349180.png)
> > 
> > 设 $\mathbf{x}^*$ 是 $Q$ 的一个 extreme point, 我们证明它同时也是 $P$ 的一个 extreme point. 我们不妨利用反证法, 从它的逆否命题入手, 即若 $x^*$ 不是 $P$ 的 extreme point, 那么它也不会是 $Q$ 的 extreme point, 然后导出矛盾.
> > 
> > 假设 $\mathbf{x}^*$ 不是 $P$ 的 extreme point, 那么存在 $\mathbf{y,z} \in P, \lambda \in [0, 1], \text{s.t. } \mathbf{x}=\lambda\mathbf{y}+(1-\lambda)\mathbf{z}$. 由于 $\mathbf{x} \in Q$, 因此 $\mathbf{c'x}=\mathbf{c'}(\lambda \mathbf{y}+(1-\lambda)\mathbf{z})=v$. 因为 $v$ 是 optimal value, 所以 $\mathbf{c'y} \geq v, \mathbf{c'z} \geq v$. 要满足上面的等式当且仅当 $\mathbf{c'y}=\mathbf{c'y}=v$, 即 $\mathbf{y,z} \in Q$, 这与 $\mathbf{x}$ 是 $Q$ 中的 extreme point 矛盾. 因此 $Q$ 中的 extreme point 一定也是 $P$ 中的 extreme point.
> > 
> > 又 $Q$ 是 optimal solution 的集合, 因此 $Q$ 中的 extreme points 一定属于 optimal solutions, 从而 $P$ 的 extreme points 中必然存在 optimal solution.

上述定理可以 extend to 不包含 line 的 polyhedra in standard form 和 bounded polyhedra.

下面的定理更强, 它不假定 optimal solution 的存在然后证明了当 optimal cost 是 finite 时一定会存在 optimal solution.

> [!theorem]
> Consider the linear programming problem of minimizing $\mathbf{c'x}$ over a polyhedron $P$. Suppose that $P$ has at least one extreme point. Then, either the optimal cost is equal to $-\infty$, or there exists an extreme point which is optimal.
> 
> > [!proof]-
> > 此定理的证明本质和上一定理的证明一样, 不同之处在于这里我们证明当我们朝某个 basic feasible solution 移动的过程, cost 也是非递增的. 我们会使用到下列术语: 若 $P$ 中的元素 $\mathbf{x}$ 能找到的满足 active at $\mathbf{x}$ 同时是 linearly independent 的约束数量最大值为 $k$, 我们就称 $\mathbf{x}$ 的 $\mathrm{rank}$ 为 $k$.
> > 
> > 从下图可以看出这个证明的 intuition.
> > 
> > ![IMG_07D92B1F0673-1.jpeg](https://picgo-1259588753.cos.ap-beijing.myqcloud.com/202409221512291.jpeg)
> > 
> > 首先我们假设 optimal cost 是 finite 的. 令 $P = \{\mathbf{x} \in \mathfrak{R}^n | \mathbf{Ax} \geq \mathbf{b}\}$, 考虑其中 $\mathrm{rank}(x) = k < n$ 的元素 $\mathbf{x}$. 之后我们将证明我们总能找到 $\mathbf{y}$ 满足 $\mathrm{rank}(y)>\mathrm{rank}(x)$ 且 $\mathbf{c'y} \leq \mathbf{c'x}$. 不妨设 $I=\{i | \mathbf{a}_{i}'\mathbf{x} = b_{i}\}$, 则 $\lvert I \rvert=k<n$, 因此我们总能找到 $\mathbf{d} \in \mathfrak{R}^n, \text{s.t. } \mathbf{a}_{i}'\mathbf{d}=0, \forall i \in I$, 并且我们可以保证可以找到同时满足 $\mathbf{c'd} \leq 0$ 的 $\mathbf{d}$.
> > 
> > 先考虑 $\mathbf{c'd} < 0$ 的情况. 设 $\mathbf{y}=\mathbf{x}+\lambda \mathbf{d}$, 为了使 $\mathrm{cost}(\mathbf{y}) < \mathrm{cost}(\mathbf{x})$ 我们令 $\lambda > 0$, 此时所有的 $\mathbf{y}$ 构成一条 half-line. 沿着这条 half-line 走 ($\lambda$ 增大), 由于 $P$ 不包含一条 line ($\leftarrow$ 存在 extreme point), 因此我们必然会到达一个点, 它满足 $\lambda^* > 0, \mathbf{a}'_{i}(\mathbf{x}+\lambda^{*}\mathbf{d})=b_{i}, \forall i \in I$ 并且会有 $\exists j \not\in I, \text{s.t. } \mathbf{a}'_{j}(\mathbf{x} + \lambda^{*}\mathbf{d})=b_{j}$. 我们令 $\mathbf{y}^*=\mathbf{x} + \lambda^{*}\mathbf{d}$, 注意到 $\mathbf{c'y} < \mathbf{c'x}$ 且 $\mathbf{a_{j}}$  必然与 $\mathbf{a}_{i}, \forall i \in I$ 满足 linearly independent, 于是 $\mathrm{rank}(\mathbf{y}^*) \geq k+1$.
> > 
> > 然后考虑 $\mathbf{c'd}=0$. 此时 $\lambda$ 可以取任意实数, 都会满足 $\mathrm{cost}(\mathbf{y}) = \mathrm{cost}(\mathbf{x})$, 并且对应的路径是一条 line, 与上一情况相同, 无论我们往哪头走都一定能找到一个 $\mathbf{y}^*$ 使得 $\mathrm{rank}(\mathbf{y}^*) \geq k+1, \mathrm{cost}(\mathbf{y}^*)=\mathrm{cost}(\mathbf{x})$.
> > 
> > 无论哪种情况, 我们都能找到一个 rank 更大同时 cost 不小于 $\mathbf{x}$ 的点, 不断重复上述过程, 我们总能找到一个点 $\mathbf{w}$ 满足 rank = $n$ ($\to \mathbf{w}$ 是一个 basic feasible solution) 并且 $\mathbf{c'w} \leq \mathbf{c'x}$.
> > 
> > 综上所述, 我们设 $\mathbf{w^1}, \dots, \mathbf{w}^r$ 为 $P$ 的 basic feasible solutions, 令 $\mathbf{w}^*$ 为 cost 最小的 solution. 根据上述讨论, 对于任意的 $\mathbf{x} \in P$, 我们都能找到一个 $\mathbf{w}^i$ 满足 $\mathbf{c'w}^i \leq \mathbf{c'x}$, 于是就有 $\mathbf{c'w}^* \leq \mathbf{c'x}, \forall \mathbf{x} \in P$, 从而 $\mathbf{w}^*$ 是 optimal solution.

注意对于一个 general LP prob, 如果 feasible set 没有 extreme points, 那么上述定理不能被直接应用. 而如果我们能将其转化为一个等价的 standard form LP prob, 就可以直接应用上述定理了.

> [!corollary]
> Consider the linear programming problem of minimizing $\mathbf{c'x}$ over a nonempty polyhedron. Then, either the optimal cost is equal to $-\infty$ or there exists an optimal solution.
> 
> > [!note] 
> > 注意与 nonlinear cost function 的 optimization problems 进行对比. 例如 minimizing $1 / x$ subject to $x \geq 1$ 的问题, optimal cost 并不是 $-\infty$ 而 optimal solution 却不存在.

## Representation of bounded polyhedra

> [!theorem]
> A nonempty and bounded polyhedron is the convex hull of its extreme points.
> 
> > [!proof]
> > #TODO 

## Projections of polyhedra: Fourier-Motzkin elimination

Fourier-Motzkin elimination 是解决 LP prob 的最老的办法, 由于它步骤繁多, 因此并不实用, 但是它给我们带来了许多有趣的 corollaries.

这个方法的关键点在于 *projection*: 如果 $\mathbf{x}=(x_{1}, \dots, x_{n})$ 是 $\mathfrak{R}^n$ 中的一个 vector 并且 $k \leq n$, 则 projection mapping $\pi_{k}: \mathfrak{R}^{n} \to \mathfrak{R}^{k}$ 将 $\mathbf{x}$ 映射为它的前 $k$ 个 coordinates:

$$\pi_{k}(\mathbf{x}) = \pi_{k}(x_{1}, \dots, x_{n}) = (x_{1}, \dots, x_{n}).$$

我们还对集合 $S \subset \mathfrak{R}^n$ 定义 projection

$$\begin{align}
\Pi_{k}(S) &= \{\pi_{k}(\mathbf{x})|\mathbf{x} \in S\} \\
&= \{(x_{1}, \dots, x_{k}) | \text{there exist } x_{k+1}, \dots, x_{n} \text{ s.t. } (x_{1}, \dots, x_{n}) \in S\}
\end{align}$$

![image.png](https://picgo-1259588753.cos.ap-beijing.myqcloud.com/202409221815321.png)

不难发现, $P \subset \mathfrak{R}^n$ 非空 $\leftrightarrow$ $\Pi_{n-1}(P) \subset \mathfrak{R}^{n-1}$ 非空, 从而我们根据 $\Pi_{1}(P)$ 是否非空即可判断 polyhedron $P$ 是否非空.

现在我们考虑 elimination method 是如何构造 projection $\Pi_{n-1}(P)$ 来消除 $x_{n}$ 的.

> [!info] Elimination algorithm
> 
> 1. Rewrite each constraint $\sum_{j=1}^{n} a_{ij}x_{j} \geq b_{i}$ in the form $$a_{in}x_{n} \geq -\sum_{j=1}^{n-1}a_{ij}x_{j}+b_{i}, \quad i=1, \dots, m;$$ if $a_{in}\not=0$, divide both sides by $a_{in}$. By letting $\mathbf{\bar{x}}=(x_{1, \dots, x_{n-1}})$, we obtain an equivalent representation of $P$ involving the following constraints: 
>    
>    $$\begin{aligned} x_{n} & \geq d_{i}+\mathbf{f}_{i}^{\prime} \overline{\mathbf{x}}, & & \text { if } a_{i n}>0, \\ d_{j}+\mathbf{f}_{j}^{\prime} \overline{\mathbf{x}} & \geq x_{n}, & & \text { if } a_{j n}<0, \\ 0 & \geq d_{k}+\mathbf{f}_{k}^{\prime} \overline{\mathbf{x}}, & & \text { if } a_{k n}=0. \end{aligned}$$
>    
>    Here, each $d_{i}, d_{j}, d_{k}$ is a scalar, and each $\mathbf{f}_{i}, \mathbf{f}_{j}, \mathbf{f}_{k}$ is a vector in $\mathfrak{R}^{n-1}$.
> 2. Let $Q$ be the polyhedron in $\mathfrak{R}^{n-1}$ defined by the constraints 
>    
>    $$\begin{aligned} d_{j}+\mathbf{f}_{j}^{\prime} \overline{\mathbf{x}} & \geq d_{i}+\mathbf{f}_{i}^{\prime} \overline{\mathbf{x}}, & & \text { if } a_{i n}>0 \text { and } a_{j n}<0, \\ 0 & \geq d_{k}+\mathbf{f}_{k}^{\prime} \overline{\mathbf{x}}, & & \text { if } a_{k n}=0. \end{aligned}$$

> [!theorem]
> The polyhedron $Q$ constructed by the elimination al­gorithm is equal to the projection $\Pi_{n-1}(P)$ of $P$.

可以看出, 只要我们允许 elimination algorithm $n-1$ 次就能得到 $\Pi_{1}(P)$. 不幸的是, 每跑一次约束条件的数量就会剧烈增长. 尽管最终得到 $\Pi_{1}(P)$ 是一维的, 几乎所有 constraints 都是多余的, 但是我们仍然需要对 constraints 进行 enumerate 才能决定它是否是 redundant 的.

根据算法的步骤结合上述定理, 对于 polyhedron $P$, 任意 $1 \leq k \leq n$ 的 projection $\Pi_{k}(P)$ 仍然是一个 polyhedron.

> [!corollary]
> Let $P \subset \mathfrak{R}^{n+k}$ be a polyhedron. Then the set $$\{\mathbf{x} \in \mathfrak{R}^{n} | \text{there exists } \mathbf{y} \in \mathfrak{R}^{k} \text{ such that } (\mathbf{x}, \mathbf{y}) \in P\}$$ is also a polyhedron.

> [!corollary]
> Let $P \subset \mathfrak{R}^n$ be a polyhedron and let $\mathbf{A}$ be an $m \times n$ matrix. Then, the set $Q=\{\mathbf{Ax} | \mathbf{x} \in P\}$ is also a polyhedron.
> 
> > [!question]-
> > 书上这个部分的证明感觉有点问题啊 QwQ.
> > #TODO 

> [!corollary]
> The convex hull of a finite number of vectors is a polyhedron.
> 
> > [!proof]-
> > 把 convex hull 
> > 
> > $$\left\{\sum_{i=1}^{k}\lambda_{i}\mathbf{x}^{i} \vert \sum_{i=1}^{k}\lambda_{i}=1, \lambda_{i} \geq 0 \right\}$$ 
> > 
> > 看作将 polyhedron 
> > 
> > $$\left\{(\lambda_{1}, \dots, \lambda_{k}) | \sum_{i=1}^{k}\lambda_{i}=1, \lambda_{i} \geq 0 \right\}$$ 
> > 
> > 左乘上矩阵 $\mathbf{X}$ 的线性映射, 然后根据前一个引理即可得证.

最后我们考虑 elimination algorithm 如何应用于 LP prob 中. 假设我们的目标是 minimize $\mathbf{c'x}$ subject to a polyhedron $P$. 我们定义一个新的 variable $x_{0}=\mathbf{c'x}$, 然后我们运行 $n$ 次 elimination algorithm 来消除 $x_{1}, \dots, x_{n}$, 就会剩下集合

$$
Q = \{x_{0} | \text{there exists } \mathbf{x} \in P \text{ such that } x_{0}=\mathbf{c'x}\},
$$

此时 optimal cost 就是 $Q$ 中最小的元素, 而 optimal solution $\mathbf{x}$ 可以通过 backtracking 找到.