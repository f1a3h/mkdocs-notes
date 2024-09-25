---
title: "Lec 1: Computational Models, and the \rSchwartz-Zippel Randomized Polynomial Identity Test"
---
## Computational Model

> [!definition]+ Randomized Algorithm
> A *randomized algorithm* is an algorithm that can be computed by a Turing machine (or random access machine), which has access to an infinite string of uniformly random bits. Equivalently, it is an algorithm that can be performed by a Turing machine that has a special instruction “flip-a-coin”, which returns the outcome of an independent flip of a fair coin.

注意以下两个 properties:

- 一个 randomized algorithm 的 output & the computation path 都是 random variable
- computation path 可以用 *binary tree* 的形式来表达, 它的分支表示投掷硬币的结果

randomized algorithm 主要可以分为以下两类:

- Las Vegas Algorithms: output 是 deterministic 且永远正确的, 但是 runtime 是一个 random variable, 并且可能是 unbounded.
- Monte Carlo Algorithms: runtime 是 bounded 但是 output 可能会出错.
	- 1-sided error: 正确答案是 true 时永远会输出 "Yes", 是 false 时会以一个大于 $p$ ($p>0$) 的概率输出 "No".
	- 2-siede error: 输出与正确答案相同的概率至少是 $1 / 2 + \epsilon$ for some $\epsilon > 0$. 多项式时间的这一类型的 Monte Carlo Algorithms 也被称为 "BPP" (Bounded Error Probabilistic Polynomial)

上述 Monte Carlo Algorithms 的正确性看起来并不足够优秀, 事实上我们可以通过运行多次 Monte Carlo Algorithm 来提高它的正确率:

- 1-sided error: 运行 $t$ 次算法, 若有一次输出为 "No" 则直接输出 "No", 否则运行完 $t$ 次后输出 "Yes".
	- 此时的错误率为 $(1 - p)^{t}$
	- 依据 $(1-x)\le e^{-x}$, 我们可以得到 $(1-p)^t \le e^{-pt}$, 由此在给定的 $t$ 下对错误率给出一个上界的分析
	- 这个 trick 常用来扔掉一个 tricky "-" 并用一个 nice exponent 替换, 在 $x$ 足够小时这个近似的误差会非常小
- 2-sided error: 运行 $t$ 次算法, 最后输出占比较多的答案.
	- 此时的正确率至少是 $1-e^{-2t \epsilon^2}$

> [!lemma]+
> If an algorithm is correct with probability $1 / 2 + \epsilon$, and we run it $t$ times and output the majority answer, the probability we are correct is at least $1-e^{-2t \epsilon^2}$.

> [!proof]-
> 
> $$ \begin{aligned}\Pr[\text{majority is wrong}] &\leq \sum_{i=0}^{t / 2} {t \choose i} (1 / 2+\epsilon)^{i}(1 / 2 - \epsilon)^{t-i} \\ &\leq \sum_{i=0}^{t / 2} {t \choose i} (1 / 2+\epsilon)^{t / 2}(1 / 2 - \epsilon)^{t / 2} \\ &= \left( \frac{1 - 4\epsilon^2}{4} \right)^{t / 2} \sum_{i=0}^{t / 2} {t \choose i} \\ &\leq \left(  \frac{1 - 4\epsilon^2}{4} \right)^{t / 2} \sum_{i=0}^{t} {t \choose i} \\ &= \left( \frac{1 - 4\epsilon^2}{4} \right)^{t / 2} 2^t \\ &= (1 - 4 \epsilon^2)^{t / 2} \leq e^{-2t \epsilon^2} \end{aligned} $$
> 
> 因此正确率至少为 $1-e^{-2t \epsilon^2}$.

### What Kind of Resource is Randomness?

complexity theory 中最重要的问题之一就是 randomness 是否真的发挥了作用 (*really helps*), 以及上述几种 algorithms 之间的联系是什么.

> [!question] 
> Are Monte Carlo algorithms at least as powerful than Las Vegas algorithms?

> [!question] 
> Are Las Vegas algorithms at least as powerful as Monte Carlo algorithms?

> [!question] 
> Are deterministic algorithms as powerful as randomized algorithms?

以上 questions 仍然是 open questions, 是 complexity theory 中重要的几个议题.

尽管存在许多例子表明引入 randomness 可以得到一个 simpler & faster randomized algorithm, 但是目前并没有严谨的证明来表明对于这些问题来说 randomness 是否是 essential for any efficient algorithm.

一个解决 Q3 的方法是看我们能否通过一系列 deterministic bits 代替 random coin flips 来让它"look kind of random", 比如用 $\pi$ 的二进制位来替代掷硬币的结果, 也可以分析如果我们给这个 algorithm less-random bits 它能否有所察觉. 这些 ideas 都属于 *derandomization* 的范畴.

如何获取真正的 random bits 以及如何利用它们也是值得探讨的问题. 举例来说, 如果没有 unbiased 或者 independent 的 coins, 我们能否从中提取 (*distilling*) 或分解 (*extracting*) randomness? 如果给定的是一大堆看似 random 但实际可能在某些未知的情况下 correlate 的 coins, 我们能否从中得到一小部分的 perfectly random coins?

当然, 这些问题并不是重点, 对这门课来说重点是如何设计和分析 randomized algorithms.

## Randomized Polynomial Identity Testing

第一个学习的 randomized algorithm 是用来决定一个多元多项式是否恒为 0 的. 给定一个 $n$ 元 degree 为的 $d$ 的多项式, 它的项数可以远超过 ${n \choose d} \approx n^d$ 数量级, 一项项拆开然后抵消的算法在最坏情况下需要 exponential time.

在考虑这个问题的 randomized algorithm之前, 我们先来看看它有哪些重要应用:

- graph analysis: 众所周知, 我们可以用邻接矩阵来表示一个 graph, 而这个矩阵的行列式可以包含很多关于这个 graph 的信息 (尤其是行列式是否为 0), 而行列式实质上就是多项式!
- primality checking: $P(z)\coloneqq (1+z)^n-1-z^n \pmod{n}$ 当且仅当 $n$ 为质数时为 0.

### Schwartz-Zippel Polynomial Identity Test

![image.png](https://picgo-1259588753.cos.ap-beijing.myqcloud.com/202408131512295.png)

> [!theorem]+
> If $P = 0$, then the algorithm will always output “P = 0”. If $P \not= 0$, then the algorithm will be correct with probability at least $1 − \frac{degree(P)}{\lvert S \rvert}$ where $degree(P)$ is the maximum sum of the degrees of terms in any monomial (e.g. $degree(x^{3}_{1}x_{2}^{2}+x_{1}^2x_{3}^2)=5$.)

> [!proof]-
> 设 $degree(P)=d$. 我们先考虑 $n=1$ 的情况, 显然当 $P \equiv 0$ 时一定会输出 "P = 0", 当 $P \not\equiv 0$ 时, $P$ 至多有 $d$ 个零解, 那么 $\Pr[P(r_{1})=0]\leq \frac{d}{\lvert S \rvert}$, 直觉上来说, 当 $n>1$ 时这个概率应该会更小, 因此我们不妨考虑数学归纳法.
> 
> $n=1$ 时, 根据上述分析, 显然成立.
> 
> 假设结论在 $\leq n-1$ 时成立, 现在考虑 $= n$ 的情况.
> 
> 我们不妨将 $P$ 改写为 $P(x_{1}, x_{2}, \dots, x_{n})=x_{n}^k Q(x_{1}, x_{2}, \dots, x_{n-1})+T(x_{1}, x_{2}, \dots, x_{n})$, 其中 $x_{n}$ 在 $T$ 中的 degree $< k$.
> 
> 固定 $r_{1}, r_{2}, \dots, r_{n-1}$, 若 $Q(r_{1}, r_{2}, \dots, r_{n-1})=0$, 此时 $P$ 退化为度小于 $k$ 的一元多项式, 这种情况 (A) 下 $\Pr[P=0|A]\leq\Pr[Q=0]\cdot \frac{k}{\lvert S \rvert}\leq \frac{d}{\lvert S \rvert} \cdot \frac{k}{\lvert S \rvert}\leq \frac{d}{\lvert S \rvert}$. 若 $Q(r_{1}, r_{2}, \dots, r_{n-1})\not= 0$, 此时 $P$ 退化为度为 $k$ 的一元多项式, 于是 $\Pr[P\not=0|B]\geq\Pr[Q\not=0]\left( 1-\frac{k}{\lvert S \rvert} \right)\geq \left( 1-\frac{d}{\lvert S \rvert} \right)\left( 1-\frac{k}{\lvert S \rvert} \right)=1-\frac{d}{\lvert S \rvert}+\frac{(d-k)k}{\lvert S \rvert^2}\geq 1-\frac{d}{\lvert S \rvert}$. 综合两种情况, 总的 $\Pr[P\not=0]\geq 1-\frac{d}{\lvert S \rvert}$. 得证.

### Discussion

目前我们并没有找到一个解决 polynomial identity test 的 sub-exponential time 的 deterministic algorithm, 不过针对某些特定 polynomials, determinis algorithms 是已知的. 事实上, 如果找到一个 deterministic polynomial time polynomial identity testing algorithm, 这会得到一些 extremely strong complexity-theoretic results. 我们相信这两者是存在且正确的, 但是这远远超出了我们目前所有的方法/工具能解决的上限,