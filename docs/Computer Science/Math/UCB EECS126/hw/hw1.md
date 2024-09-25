---
title: Homework 1
---
## Syllabus

略.

## Balls & Bins

将 balls 和 bins 视为 distinguishable.

考虑每个 ball 被扔进哪个 bin 中, 则样本空间 $\Omega = \{1, 2, \dots, n\}^{n}$.

假设第 $k$ 个 bin 是空的, 那么就是将 $n$ 个 ball 扔进 $n-1$ 个 bin 中并且每个 bin 都不为空, 方案数为 $\frac{{n-1 \choose 1}{n \choose 1}(n-1)!}{2!}=\frac{n!(n-1)}{2}$. 然后乘上选出空 bin 的方案数, 总的方案数为 $\frac{n!n(n-1)}{2}$.

假设事件 $A$ 表示恰好一个 bin 是空的, 那么我们有

$$\Pr(A)=\frac{\frac{n!n(n-1)}{2}}{n^n}=\frac{n!(n-1)}{2n^{n-1}}=\frac{n!{n \choose 2}}{n^n}$$

如果我们将 balls 视为 indistinguishable 而 bins 视为 distinguishable, 那么求解过程可以转化为 $x_{1}+x_{2}+\cdots+x_{n}=n$ 的正/非负整数解的方案数了.

## Coin Flipping and Symmetry

脑子瓦特了一直在想反过来的事件 more than $\to$ less than, 然后发现中间缺了个 equal to 不好解决.

这里要反过来的是 head $\to$ tail. 令事件 $A=\{\text{more heads in the first }n+1\text{ tosses than the last }n\text{ tosses}\}$, 事件 $B=\{\text{more tails in the first }n+1\text{ tosses than the last }n\text{ tosses}\}$, 事实上, $B$ 等价于 "more heads in the last $n$ tosses than the first $n+1$ tosses", 然后会发现由于 $n+1 \not= n$ 所以不可能存在中间情况"heads & tails in the first $n+1$ tosses equal to the last $n$ tosses respectively", 也就是说 $\Omega=A\cup B$, 而 $A, B$ 又构成对称, 因此 $\Pr(A)=\Pr(B)=\frac{1}{2}$.

## Superhero Basketball

首先我们应该思考事件 "CA was always strictly ahead of S ($A$)" 的反事件应该是什么, 显然是要么 "S was always strictly ahead of CA" 要么是中间某一个时刻打成了平局, 从结果上看第一种情况是不可能发生的, 那么反事件即 "中间某一时间打成了平局 ($T$)".

不难发现, 如果 Superman 拿下了第 1 分, 这种情况必然属于事件 $T$, 而 CA 拿下第 1 分则同时与事件 $A, T$ 存在交集. 记事件 $S, C$ 分别表示 S 和 CA 拿下第 1 分, 那么事件 $T=S\cup(C\cap T)$.

*考察 $C \cap T$, 在前面平局的得分序列中, 将 CA 的得分与 S 的得分互换, 可以发现所有方案与 $S \cap T$ 的平局得分序列的方案一模一样, 而后面的非平局得分序列与 $C \cap T, S \cap T$ 都无关, 因此 $C \cap T$ 实际和 $S \cap T$ 是对称的, 即 $\Pr(C \cap T)=\Pr(S \cap T)$.*

于是我们有

$$
\begin{align}
\Pr(A) &= 1-\Pr(T) \\
&= 1-(\Pr(S)+\Pr(C\cap T)) \\
&= 1-(\Pr(S)+\Pr(S \cap T)) \\
&= 1-2\Pr(S)
\end{align}
$$

容易求出 $\Pr(S)=\frac{{n+m-1\choose n}}{{n+m \choose n}}=\frac{m}{m+n}$, 因此最后求得 $\Pr(A)=1-\frac{2m}{m+n}=\frac{n-m}{n+m}$.

> [!note]+ 
> This is yet another famous problem known as the *ballot problem*.


## Expanding the NBA

令事件 $B_{i}=\{\text{i-th city is the best}\}, A = \{\text{best city is chosen}\}$.

当 $i=1, \dots, m$ 时, 显然 $\Pr(A \mid B_{i})=0$.

当 $i>m$ 时, 考虑 $\Pr(\bar{A} \mid B_{i})$. 由于最好的城市没有被选上, 这说明必然存在 $j < i$ 使得第 $j$ 的城市优于前 $j-1$ 个城市且这 $j-1$ 个城市中没有类似存在的城市. 这样直接思考比较麻烦, *既然前 $i$ 个城市中最好的那个没被选上, 我们不妨考虑一下第 2 好的那个*.

假设前 $i$ 个城市中第二好的城市位于 $k$.

- $k\leq m$: 此时必然会选择最好的城市
- $m < k < i$: 第二好的城市不一定被选上, 但是最好的城市一定不会被选上

这时情况清楚了, 当且仅当 $m<k<i$ 时最好的城市不会被选上.

于是有 $\Pr(\bar{A} \mid B_{i})=\frac{i-m-1}{i-1}$, 因此 $\Pr(A \mid B_{i})=1-\frac{i-m-1}{i-1}=\frac{m}{i-1}$.

由于 $\Pr(B_{i})=\frac{1}{N}$, 所以有

$$
\begin{align}
\Pr(A) &= \sum_{i=m+1}^{N} \Pr(A \mid B_{i}) \Pr(B_{i}) \\
&= \frac{m}{N} \sum_{i=m+1}^{N} \frac{1}{i-1} \\
&\approx \frac{m}{N} (\ln N - \ln m) \\
&= -\frac{m}{N}\ln \frac{m}{N}.
\end{align}
$$

令 $x=\frac{m}{N}$, 于是 $\Pr(A)\approx -x\ln x$, 它的最大值在 $x=\frac{1}{\epsilon}$ 时取到, 为 $\frac{1}{\epsilon}$.

> [!note]+ 
> This problem is a famous example from optimal stopping theory and is commonly known as the *secretary problem*.
> 
> In fact, one may use a dynamic programming approach to see why the policy outlined here is in fact the optimal policy. If you are interested, the details of such an approach can be found in *Dynamic Programming and the Secretary Problem* by Beckmann.

## Passengers on a Plane

经典错排问题, 这里要求用容斥原理解决.

略.

## Upperclassmen

简单的条件概率题, 略.