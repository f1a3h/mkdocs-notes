---
title: Discrete Random Variables
---
## Basic Concepts

> [!definition]+ Random Variable
> 
> A random variable (RV) is a real-valued function of the experimental outcome.

当一个 RV 能取到的所有值是有限的时, 我们称其是 *discrete*.

每一个 discrete RV (DRV) 都有一个 *probability mass function (PMF)*, 用来表示取某个值的概率. 以 discrete RV 为自变量的 function 定义了另一个 DRV, 这个 DRV 的 PMF 可以由原来的 DRV 的 PMF 求得.

## Probability Mass Function

我们用 $p_{X}(x)$ 来表示 DRV $X$ 的  PMF. 对 event $\{ X=x \}$, 有:

$$p_{X}(x)=\Pr(\{X=x\}).$$

注意 $\sum_{x}p_{X}(x)=1$.

> [!definition]+ The Bernoulli Random Variable
> 
> 设投掷一枚硬币头朝上的概率是 $p$, 我们有 *Bernoulli* RV:
> 
> $$X=\begin{cases} 1, &\text{if a head}, \\ 0, &\text{if a tail.} \end{cases}$$
> 
> 以及它的 PMF:
> 
> $$p_{X}(k)=\begin{cases} p, &\text{if } k=1. \\ 1-p, &\text{if } k=0. \end{cases}$$

通过多个 Bernoulli RV 的结合可以得到许多复杂的 RV, 例如 binomial RV.

> [!definition]+ The Binomial Random Variable
> 如果我们投掷上述硬币 $n$ 次, 设 $X$ 为 heads 的数量, 这个 $X$ 便是以 $n$ 和 $p$ 为 *parameter* 的 *binomial* RV. 不难得到 $X$ 的 PMF:
> 
> $$p_{X}(k)=\Pr(X=k)={n \choose k}p^{k} (1-p)^{n-k}, \quad k=0,1,\dots,n.$$

> [!definition]+ The Geometric Random Variable
> 设 $X$ 表示掷出第一个 head 需要的投掷次数, 于是它的 PMF 为:
> 
> $$p_{X}(k)=(1-p)^k p, \quad k=1,2,\dots,$$
> 
> 这个 $X$ 便是一个 *geometry* RV.

> [!definition]+ The Poisson Random Variable
> PMF 形如
> 
> $$p_{X}(k) = e^{-\lambda}\frac{\lambda^k}{k!}, \quad k=0,1,2,\dots,$$
> 
> 的 RV 是 *Poisson* RV, 其中 $\lambda$ 是其对应的 positive parameter.

以 $\lambda$ 为 parameter 的 Poisson PMF 是对以 $n$ 和 $p$ 为 parameter 的 binomial PMF 一个很好的估计, 其中满足 $\lambda=np$, 且 $n$ 很大, $p$ 很小:

$$
e^{-\lambda} \frac{\lambda^k}{k!} \approx \frac{n!}{k!(n-k)!} p^k (1-p)^{n-k}, \quad k=0, 1, \dots, n.
$$

## Functions of Random Variables

若 $X$ 是一个 RV, 那么 $Y=g(X)$ 也是一个 RV. 可以通过函数 $g$ 和 $p_{X}$ 计算 $p_{Y}$:

$$
p_{Y}(y)= \sum_{x \mid g(x)=y} p_{X}(x).
$$

## Expectation, Mean, and Variance

> [!definition]+ Expectation
> We define the expected value (also called the expectation or the mean) of a random variable $X$, with PMF $p_X$, by
> 
> $$\mathrm{E}[X]=\sum_{x} xp_{X}(x).$$

expectation 在 PMF 上的含义为 the *center of gravity*.

> [!definition]+ Variance
> We define the variance of a RV $X$, sometimes denoted $\sigma^{2}_{X}$ is
> 
> $$\mathrm{var}(X)=\mathrm{E}[(X-\mathrm{E}[X])^2]$$
> 
> And furthermore the *standard deviation* is
> 
> $$\sigma_{X}=\sqrt{ \mathrm{var}(X) }$$

> [!theorem]+ Expectations of Functions of RVs
> Let $X$ be a random variable with PMF $p_X$, and let $g(X)$ be a function of $X$. Then, the expected value of the random variable $g( X)$ is given by
> 
> $$\mathrm{E}[g(X)]=\sum_{x}g(x)p_{X}(x).$$

> [!theorem]+ Linearity of Expectation
> We have
> 
> $$\mathrm{E}[X+Y]=\mathrm{E}[X]+\mathrm{E}[Y]$$
> 
> for arbitrary $X$ and $Y$ that are defined on the same probability space.

> [!theorem]+ Variance in Terms of Moments Expression
> 
> $$\mathrm{var}(X)=\mathrm{E}[X^2]-(\mathrm{E}[X])^2.$$

- Bernoulli RV:
	- $\mathrm{E}[X]=p$
	- $\mathrm{var}(X)=p(1-p)$
- discrete uniform RV:
	- $\mathrm{E}[X]=\frac{a+b}{2}$
	- $\mathrm{var}(X)=\frac{(b-a)(b-a+2)}{12}$
- binomial RV:
	- $\mathrm{E}[X]=np$
	- $\mathrm{var}(X)=np(1-p)$
- geometry RV:
	- $\mathrm{E}[X]=\frac{1}{p}$
	- $\mathrm{var}(X)=\frac{1-p}{p^2}$
- Poisson RV:
	- $\mathrm{E}[X]=\lambda$
	- $\mathrm{var}(X)=\lambda$

## Joint PMFs of Multiple Random Variables

通过 joint PMF 计算单个 RV 的 PMF:

$$
p_{X}(x)=\sum_{y} p_{X,Y}(x, y), \quad p_{Y}(y)=\sum_{x} p_{X, Y}(x, y).
$$

为了与 joint PMF 区分, 我们有时也将 $p_{X}, p_{Y}$ 称为 *marginal* PMFs. 我们可以用 *tabular method* 来从 joint PMF 计算出 marginal PMFs.

若 $Z=g(X, Y)$, 我们有

$$
p_{Z}(z)=\sum_{\{(x, y) \mid g(x, y)=z\}} p_{X, Y}(x, y).
$$

计算 expectation:

$$
\mathrm{E}[g(X, Y)]=\sum_{x}\sum_{y}g(x, y)p_{X, Y}(x, y)
$$

类似的, 上述式子也能拓展到更多的 RVs 上.

## Conditioning

> [!definition]+ Conditional PMF on an Event
> The conditional PMF of a random variable $X$, conditioned on a particular event $A$ with $\Pr(A) > 0$, is defined by
> 
> $$p_{X\mid A}(x)=\Pr(X=x\mid A)=\frac{\Pr(\{X=x\}\cap A)}{\Pr(A)}.$$

> [!definition]+ Conditional PMF on Another RV
> 
> $$p_{X\mid Y}(x \mid y)=\frac{p_{X, Y}(x, y)}{p_{Y}(y)}.$$

> [!theorem]+ Total Expectation Theorem
> 
> - $\mathrm{E}[X]=\sum_{i=1}^{n}\Pr(A_{i})\mathrm{E}[X \mid A_{i}]$
> - $\mathrm{E}[X]=\sum_{y}p_{Y}(y)\mathrm{E}[X \mid Y=y]$

## Independence

> [!definition]+ Independence from an Event
> We say that the random variable $X$ is independent of the event $A$ if
> 
> $$\Pr(X=x \text{ and } A)=\Pr(X=x)\Pr(A)=p_{X}(x)\Pr(A), \quad \forall x.$$

> [!definition]+ Independence of RVs
> We say that two random variables $X$ and $Y$ are independent if
> 
> $$p_{X, Y}(x, y)=p_{X}(x)p_{Y}(y), \quad \forall x, y$$

如果 RV $X$ 和 $Y$ 是 independent 的, 那么有

$$
\begin{eqnarray*}
\mathrm{E}[XY]&=&\mathrm{E}[X]\mathrm{E}[Y]\\
\mathrm{E}[g(X)h(Y)]&=&\mathrm{E}[g(X)]\mathrm{E}[h(Y)]\\
\mathrm{var}(X+Y)&=&\mathrm{var}(X)+\mathrm{var}(Y)
\end{eqnarray*}
$$

