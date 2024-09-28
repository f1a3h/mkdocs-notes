---
title: Lec 2
---
## Linearity of Expectation

形式如下:

$$\mathbb{E}\left[ \sum_{i}a_{i}X_{i} \right] = \sum_{i}a_{i}\mathbb{E}[X_{i}]$$

### Peogion-hole Principle

> [!question] 
> There are $n$ pigeons and $n$ pigeon-holes; each pigeon has its own pigeon-hole. However, after a wild night the $n$ pigeons return to a uniformly random pigeon-hole (so it could be that some holes are empty, and some have more than one pigeon). What's the expected number of empty pigeon-holes?

警惕思维定势!

一开始就直接令 $E_{i}$ 表示恰好 $i$ 个 empty pigeon-holes, 然后假定鸽子按序号从前到后进行排列, 前 $n-i$ 个鸽子将 $n-i$ 个非空鸽巢填满的方案数是 $(n-i)!$, 接着后面的 $i$ 个鸽子则随机选择一个鸽巢进驻, 得到 $\Pr[E_{i}] = \frac{(n-i)!(n-i)^{i}}{n^{n}}$, 然后就有

$$
\mathbb{E}[\text{number of empty holes}] = \sum_{i=0}^{n-1} i\cdot\Pr[E_{i}] = \sum_{i=0}^{n-1} \frac{(n-i)!(n-i)^{i}}{n^{n}} \cdot i
$$

得到了一坨不可名状的式子......

好吧, 让我们来看看 solutions 是怎么写的.

$$
\mathbb{E}\sum_{i=1}^{n} \mathbf{1}[\text{hole } i \text{ is empty}] = \sum_{i}\Pr[\text{hole } i \text{ is empty}].
$$

呀嘞呀嘞, 居然**从问题主体产生 0/1 贡献的角度出发**来求解 $\mathbb{E}$, 单个主体**是否**为空的概率简单好写, 显然这里是 $\Pr[\text{hole } i \text{ is empty}] = \left( 1 - \frac{1}{n} \right)^n$, 然后我们就知道 empty holes 的期望数量为

$$
n(1 - 1 / n)^{n}.
$$

之后我们对这个值做一个估计. 当 $n$ 很大的时候, 由 Taylor series $e^{ -1/n }=1-\frac{1}{n}+\frac{1}{2n^{2}}-\frac{1}{6n^{3}}+\cdots$ 有 $1 - 1 / n \approx e^{ -1/n }$, 我们可以得到

$$
n(1-1 / n)^n \approx n \cdot e^{ -n \cdot 1/n } = n / e.
$$

### Coupon Collecting

Let $W$ be a collection of $n$ words. For example, $W = \{\texttt{hello, randomized, algorithms, penguin, spaceship, . . ., mushroom}\}$. There's a button in front of you. Each time you push the button, it will say a word uniformly at random from $W$. If you push the button multiple times, it answers independently each time. (In particular, it's possible to get the same word twice). The question is:

> [!question] 
> What is the expected number of times you push the button before you see all $n$ words?

令 $X_{i}$ 为看到第 $i$ 个新词的时间, 分以下几步来解决这个问题.

> [!question] Q1
> What is $\mathbb{E}X_{1}$?

显然是 $1$.

> [!question] Q2
> What is $\mathbb{E}(X_{2}-X_{1})$? That is, in expectation, how many times do you press the button, after you have seen the first word, before you see a new, second word?

显然这是 $p_{2}=1-\frac{1}{n}$ 的 geometric distribution, 因此 $\mathbb{E}(X_{2}-X_{1}) = \frac{1}{p_{2}} = \frac{n}{n-1}$.

> [!question] Q3
> What is $\mathbb{E}(X_{3}-X_{2})$?

同上, 此时 $p_{3} = 1 - \frac{2}{n}$, 则 $\mathbb{E}(X_{3}-X_{2}) = \frac{n}{n-2}$.

> [!question] Q4
> For any $i=2,3,\dots,n$, what is $\mathbb{E}(X_{i}-X_{i-1})$?

$p_{i} = 1 - \frac{i-1}{n}$, 于是

$$
\mathbb{E}(X_{i}-X_{i-1}) = \frac{n}{n-i+1}
$$

> [!question] Q5
> Use your answers to the above, plus linearity of expectation, to answer our question:
> 
> what is the expected number of times you push the button before you see all $n$ words? It's okay if your answer is a summation, but if you have time try to simplify it to get a big-Theta expression.

要求的显然是 $\mathbb{E}X_{n}$, 根据前面的结论, 有

$$
\begin{align}
\mathbb{E}X_{n} &= \mathbb{E}(X_{n}-X_{n-1}+X_{n-1}-X_{n-2}+\cdots+X_{2}-X_{1} + X_{1}) \\
&= \frac{n}{1} + \frac{n}{2} + \cdots + \frac{n}{n-1} + 1 \\
&= n\sum_{i=1}^{n} \frac{1}{i} \\
&= \Theta(n \log n).
\end{align}
$$

> [!note]+ 
> 对于求解期望时间的问题, 我们设 $X_{i}$ 为第 $i$ 个主体出现的时间是很常见的设法.
> 
> 在此题中, 直接求解 $\mathbb{E}X_{n}$ 是很困难的, 此时我们不妨考虑拆分 $\mathbb{E}X_{n}$ 为方便求解的形式, 例如 Karger's algorithm 的定理证明中的条件概率形式
> 
> $$\begin{align} \Pr[\text{output }C] &= \Pr[E_{1} \land E_{2} \land \dots \land E_{n-2}] \\ &= \Pr[E_{1}] \cdot \Pr[E_{2} | E_{1}] \cdot \Pr[E_{3}| E_{2}, E_{1}] \cdot \dots \cdot \Pr[E_{n-2} | E_{1}, E_{2}, \dots, E_{n-3}].\end{align}$$
> 
> 亦或是本题中拆分成的差项求和的形式, 据此我们也可以将它拆分成除式乘积的形式或是其它. 不过, 求期望时间的问题中差项求和的形式应该会更好用.

## Karger's Min-Cut Algorithm

### High-Level Intuition

Karger's min-cut algorithm 通过一系列随机的 *edge contraction* 操作进行缩点, 最后剩下两个点就得到了一个“最小”割.

它的 intuition 在于最小割的割边数量小, 每次会被选中的概率也就相对小.

### The Algorithm

> [!definition] Edge contraction
> Given a graph $G=(V, E)$ with $n$ vertices $V=\{v_{1}, \dots, v_{n}\}$, and an edge $e \in E$ that connects vertices $v_{i}, v_{j}$, the graph resulting from *contracting edge* $e$ will have $n-1$ vertices, namely $V / \{v_{i}\}$, and edge set defined as follows: for every edge $e' \in E$ that does not have $v_{i}$ as an endpoint, $e'$ is an edge of the new graph. For every edge $e' \in E$ that connects $v_{i}, v_{k}$ for $k \neq j$, we add the edge $(v_{j}, v_{k})$ to the edge.

![image.png](https://picgo-1259588753.cos.ap-beijing.myqcloud.com/202409231046911.png)

不难发现, 由于缩点删边操作的存在, 上述算法跑一次的复杂度是 $O(\lvert E \rvert)$ 的. 下面我们考虑这个算法能返回 minimum cut 的概率.

> [!theorem]
> The probability that the above algorithm return a minimum cut is at least $\frac{2}{n(n-1)} \geq 2 / n^{2}$.

这个定理的证明依赖下面的 lemma:

> [!lemma]
> Let $S_{1}, S_{2} \subset V$ be a minimum cut. Suppose we contract an edge $(u, v)$ that does not cross the cut. Then $S_{1}, S_{2}$ will be a minimum cut of the new graph resulting from the contraction.
> > [!proof]-
> > 假设 $S_{1}, S_{2}$ 在原图中的割边数目为 $s$, 由于我们 contract 的边不属于割边, 因此 $S_{1}, S_{2}$ 这个分割在新图中仍然存在, 且割边数目不变. 假设 $S_{1}, S_{2}$ 不属于新图的最小割, 那么新图的最小割的割边数目必然小于 $s$. 这个新的最小割产生源头必然是 $(u, v)$ 的 contraction. 不妨设 $(u, v)$ 经过 contraction 后得到的新结点为 $v^*$, 那么新的割边必然存在与 $v^*$ 相连的边, 由于 contraction 不改变 $u, v$ 结点连边以外的边, 因此任何以某条 $v^*$ 相连的边为割边的方案都在原图中存在一个对应的割边不变的方案, 不妨设为 $S_{1}', S_{2}'$, 由于 $S_{1}, S_{2}$ 在原图中为最小割, 那么 $S_{1}', S_{2}'$ 对应的割边数目必然大于等于 $s$, 从而它在新图中对应的方案的割边数目也必然大于等于 $s$, 从而 $S_{1}, S_{2}$ 在新图中仍然是最小割.

有了上述 lemma 保证 contraction 不改变原最小割之后, 我们可以证明 Karger's algorithm 的正确率定理了.

> [!proof]+
> 由于可能存在多个最小割, 因此我们证明 Karger's algorithm 返回任意一个最小割的概率至少为 $\frac{2}{n(n-1)}$.
> 
> 考察其中一个最小割 $C$. Karger's algorithm 能成功返回 $C$ 等价于 $n-2$ 次 contract 的都是不越过 $C$ 的边. 记第 $i$ 次 contract 的边没有越过 $C$ 为事件 $E_{i}$, 我们有:
> 
> $$\begin{align} \Pr[\text{output }C] &= \Pr[E_{1} \land E_{2} \land \dots \land E_{n-2}] \\ &= \Pr[E_{1}] \cdot \Pr[E_{2} | E_{1}] \cdot \Pr[E_{3}| E_{2}, E_{1}] \cdot \dots \cdot \Pr[E_{n-2} | E_{1}, E_{2}, \dots, E_{n-3}]\end{align}$$
> 
> 设 $C$ 的割边数目为 $k$, 则显然有 $\Pr[E_{1}] = 1 - \frac{k}{\text{total number of edges}}$, 对于进行过 contraction 后的概率, 我们需要估计此次 contraction 后的边数, **由于最小割的割边数目为 $k$, 这说明每时每刻结点的最小 degree 必然大于等于 $k$**, 根据 degree 可以估计边数 (**毕竟 degree 是联系点数与边数的桥梁**). 对于 $E_{1}$, 边数 $\geq nk / 2$, 对于 $E_{i} | E_{1}, \dots, E_{i-1}$, 有边数 $\geq k(n-i+1) / 2$, 于是:
> 
> $$\Pr[E_{i} | E_{1}, E_{2}, \dots, E_{i-1}] \geq 1 - \frac{k}{k(n-i+1) / 2} = \frac{n-i-1}{n-i+1}.$$
> 
> 结合上面的结果, 我们对 output $C$ 的概率得到一个估计值:
> 
> $$\Pr[\text{output }C] \geq \frac{n-2}{n} \cdot \frac{n-3}{n-1} \cdot \dots \cdot \frac{2}{4} \cdot \frac{1}{3} = \frac{2}{n(n-1)}.$$
> 
> 于是定理得证.

当然, $\approx 2 / n^{2}$ 的成功率看上去似乎很小, 但别忘了这是概率算法, 只要我们运行 $t=cn^{2} / 2$ 次算法并取这 $t$ 次结果里的最小割, 那么这个算法的失败率至多只有 $\left(  1-\frac{2}{n^{2}}  \right)^{cn^{2} / 2} \leq e^{-\frac{cn^{2}}{2} \cdot \frac{2}{n^{2}} } = e^{-c}$ (trick: $1-x \leq e^{-x}$), 也就是说, 想要 0.9 的正确率只需要运行 $O(n^{2})$ 次算法.

### Modified-Karger

有没有办法做的更好? 一种提高效率的办法基于这个 intuition: 每经过一次「成功的」 contraction, 非割边减少而割边不变, 下一次 contraction 的失败率就会提高, 而刚开始的几次 contraction 实际失败率很低, 与其从头再跑一遍 Karger's algorithm, 不如从某一次 contraction 开始重新跑.

于是我们得到了 Modified-Karger's algorithm:

![image.png](https://picgo-1259588753.cos.ap-beijing.myqcloud.com/202409261818302.png)

> [!question] Q1
> Give a bound on the probability, in terms of $n$ and $m$ and $k$, that Modified-Karger is successful. (You may not be able to find the probability exactly, but give a decent lower bound, like we did for the original Karger's algorithm; it's okay if your expression is a bit complicated).
> 
> > [!hint]- 
> > You can break up the failure probability into two parts: the probability that we choose an edge crossing the mincut when reducing $G$ to $G'$, and the probability that we choose an edge crossing the cut in all of the $k$ runs of Karger of $G'$.

由于运行 $k$ 次 Karger's algorithm 只能对失败率的下界进行估计, 因此这里分析 Modified-Karger 也对失败率的下界进行估计.

首先, 我们有

$$
\Pr[\text{fail on }G] = \Pr[G \to G' \text{ fails}] + (\Pr[\text{fail on }G'])^k
$$

> [!note]- 为什么是加?
> 如果想要 succeed on $G$, 很容易想到用条件概率, 然而实际上
> 
> $$\Pr[\text{succeed on }G] = \Pr[G \to G'\text{ succeeds}] \cdot \Pr[(k \text{ runs on } G') \text{succeeds}]$$
> 
> 实际上是因为 $G'$ 是否 succeed 与 $G \to G'$ 无关, 若 $G \to G'$ 失败了 $G'$ 也一样能成功, 只是这时 $G'$ 的最小割并不是 $G$ 的最小割了, 因而 $G \to G'$ succeeds 和 ($k$ runs on) $G'$ 是独立的, 于是我们可以知道 succeed 的概率表达式是乘积的形式了. fail on $G$ 相当于对 succeed 取反, 相当于两个子事件取 $\lnot$ 后取 $\lor$, 所以 fail on $G$ 的概率表达式是加法的形式.

根据前面对 Karger's algorithm 的分析, 很容易得到

$$\Pr[G \to G'\text{ fails}] \leq 1 - \frac{m(m-1)}{n(n-1)}$$

以及

$$(\Pr[\text{fail on }G'])^k \leq \left( 1 - \frac{2}{m(m-1)} \right)^k$$

相加便得到

$$\Pr[\text{fail on }G] \leq 1 - \frac{m(m-1)}{n(n-1)} + \left( 1 - \frac{2}{m(m-1)} \right)^k$$

> [!question] Q2
> Choose $m = \sqrt{ n }$ and $k = n\log n$. Use part 1 to show that the success probability of Modified-Karger is $\Omega(1 / n)$.
> 
> > [!hint]- 
> > The fact that we're looking for a $\Omega(\cdot)$ answer means that it's okay to ignore pesky constants in your analysis.
> 
> > [!hint]- 
> > You might want to use the useful fact that $1-x \leq e^{-x}$ for all $x$.

代入 $m,k$ 的值可以得到

$$
\Pr[\text{succeed on }G] \geq \frac{1}{n+\sqrt{ n }} - \left( \frac{1}{n} \right)^{2/(1 - \frac{1}{\sqrt{ n }})}
$$

记右边的式子为 $f(n)$, 令 $g(n) = \frac{1}{n}$, 我们有

$$
\lim_{ n \to \infty } \frac{f(n)}{g(n)} = \frac{1}{1 + \frac{1}{\sqrt{ n }}} - \left( \frac{1}{n} \right)^{\frac{\sqrt{ n }+1}{\sqrt{ n }-1}}
$$

显然, $\lim_{ n \to \infty } \frac{1}{1 + \frac{1}{n}}=1$, 又有 $\left( \frac{1}{n} \right)^{1} < \left( \frac{1}{n} \right)^{\frac{\sqrt{ n }+1}{\sqrt{ n }-1}} < \left( \frac{1}{n} \right)^2$, 由夹逼定理可得 $\lim_{ n \to \infty } \left( \frac{1}{n} \right)^{\frac{\sqrt{ n }+1}{\sqrt{ n }-1}} = 0$, 从而

$$
\lim_{ n \to \infty } \frac{f(n)}{g(n)} = 1
$$

因为 $\Pr[\text{succeed on }G] \geq f(n)$, 所以

$$
\Pr[\text{succeed on }G] = \Omega(1 / n).
$$

> [!question] Q3
> Show that if you repeat Modified-Karger, with the parameters above, $\Theta(n)$ times, you can obtain a success probability of $0.99$.

略.

> [!question] Q4
> How does this compare to the original Karger's algorithm? More precisely:
> 
> We saw in the mini-lecture that repeating Karger's algorithm $O(n^{2})$ times leads to a success probability of $0.99$. This would involve $O(n^{3})$ edge contractions ($n$ edge contractions per run of Karger's algorithm).
> 
> You've just shown that "repeating Modified-Karger $\Theta(n)$ times" also has a success probability of $0.99$. How many edge contractions does this involve?

$G \to G'$ 需要的 contraction 次数为 $O(n-m)=O(n)$, 在 $G'$ 跑单次 Karger's algorithm 的 contraction 次数为 $O(m)$, $k$ 次则为 $O(km)=n^{\frac{3}{2}}\log n$, 于是运行一次 Modified-Karger 需要的 contraction 次数为 $O(n+n^{\frac{3}{2}}\log n)=O(n^{\frac{3}{2}}\log n)$. 最后我们运行 $\Theta(n)$ 次 Modified-Karger, 总的 contraction 数量为 $O(n^{\frac{5}{2}}\log n)$, 由于 $\log n = o(\sqrt{ n })$, 因此 $O(n^{\frac{5}{2}}\log n)$ 要优于 $O(n^{3})$.

> [!question] Q5
> If you're done with the above and we haven't finished group work yet, you can think about the following two challenge problems:
> 
> > [!question] 
> > Above, we saw how to improve Karger's algorithm by breaking one step down into two steps. What if we were to iterate on this idea? (That is, instead of just running Karger on $G′$, recursively run Modified-Karger on $G′$). How would you pick the parameters here? How few edge contractions can you get, if you want success probability $0.999$?
> 
> > [!question] 
> > Can you derandomize Karger's algorithm?

对于 Prob.1, Karger 还给出了两个 randomized algorithm, check out [Karger, Stein 1996](https://dl.acm.org/doi/10.1145/234533.234534) and [Karger 1998](https://arxiv.org/abs/cs/9812007).

对于 Prob.2, [Kawarabayashi, Thorup, 2015](https://arxiv.org/abs/1411.5123) 给出了一个 near-linear time deterministic algorithm.