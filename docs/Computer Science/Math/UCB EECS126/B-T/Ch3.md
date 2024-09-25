---
title: General Random Variables
---
## Continuous Random Variables and PDFs

> [!definition]+ Continuous RV
> A random variable $X$ is called *continuous* if there is a nonnegative function $f_{X}$, called the *probability density function of $X$*, or PDF for short, such that
> 
> $$\Pr(X \in B)=\int_{B} f_{X}(x) \mathrm{d}x$$
> 
> for every subset $B$ of the real line.

> [!definition]+ Expectation
> The *expected value* or *expectation* or *mean* of a continuous RV $X$ is defined by
> 
> $$\mathrm{E}[X]=\int_{-\infty}^{\infty} xf_{X}(x) \mathrm{d}x.$$

> [!definition]+ Variance
> The *variance*, denoted by $\mathrm{var}(X)$, is defined as the expected value of the RV
> 
> $$\mathrm{var}(X)=\mathrm{E}[(X-\mathrm{E}[X])^2].$$

> [!definition]+ Exponential RV
> An *exponential* RV has a PDF of the form
> 
> $$f_{X}(x)=\begin{cases}\lambda e^{-\lambda x}, &\text{if } x\geq 0, \\ 0, &\text{otherwise,}\end{cases}$$
> 
> where $\lambda$ is a positive parameter characterizing the PDF.

ERV 的 mean 和 variance 分别为

$$\mathrm{E}[X]=\frac{1}{\lambda}, \quad \mathrm{var}(X)=\frac{1}{\lambda^2}$$

## Cumulative Distribution Functions

> [!definition]+ CDF
> The *cumulative distribution function (CDF)* of a RV $X$ is denoted by $F_{X}$ and provides the probability $\Pr(X \leq x)$. In particular, for every $x$ we have
> 
> $$F_{X}(x) = \Pr(X \leq x) = \begin{cases}\sum_{k \leq x} p_{X}(k), \quad &\text{if } X \text{ is discrete},\\ \int_{-\infty}^x f_{X}(t) \mathrm{d}t, &\text{if } X \text{ is continuous}.\end{cases}$$

对于 geometric CDF, 我们有

$$
F_{\text{geo}}(n)=\sum_{k=1}^n p(1-p)^{k-1} = 1-(1-p)^{n}, \quad \text{for } n=1, 2, \dots
$$

对于 exponential CDF, 我们有

$$
F_{\text{exp}}(x) = \begin{cases}
\Pr(X \leq x) = 0, &\text{for } x \leq 0, \\
\int_{0}^x \lambda e^{-\lambda t} \mathrm{d}t = 1 - e^{-\lambda t}, &\text{for } x > 0.
\end{cases}
$$

## Normal Random Variables

> [!definition]+ Normal RV
> A continuous RV $X$ is said to bew *normal* or *Gaussian* if it has a PDF of the form
> 
> $$f_{X}(x)=\frac{1}{\sqrt{ 2\pi } \sigma} e^{-(x-\mu)^2 / 2\sigma^2},$$
> 
> where $\mu$ and $\sigma$ are two scalar parameters characterizing the PDF, with $\sigma$ assumed positive.

它的 mean 和 variance 分别为

$$
\mathrm{E}[X]=\mu, \quad \mathrm{var}(X)=\sigma^{2}.
$$

> [!definition]+ Standard Normal
> A NRV $Y$ with zero mean and unit variance is said to be a *standard normal*. Its CDF is denoted by $\Phi$:
> 
> $$\Phi(y) = \Pr(Y \leq y) = \Pr(Y < y) = \frac{1}{\sqrt{ 2\pi }}\int_{-\infty}^y e^{-t^2 / 2} \mathrm{d}t.$$

我们可以借助 *standard normal table* 来求得 $\arg \Phi(y)$.

令 $X$ 为一个 NRV, 其中 mean 为 $\mu$, variance 为 $\sigma^2$, 我们可以 "standardize" $X$ 得到一个新的 SNV $Y$:

$$
Y=\frac{X-\mu}{\sigma}.
$$

## Joint PDFs of Multiple Random Variables

> [!definition]+ Joint PDF
> We say that two continuous RV associated with the same experiment are *jointly continuous* and can be described in terms of a *joint PDF* $f_{X, Y}$  if $f_{X, Y}$ is a nonnegative function that satisfies
> 
> $$\Pr((X, Y) \in B) = \iint_{(x, y) \in B} f_{X, Y}(x, y) \mathrm{d}x\mathrm{d}y,$$
> 
> for every subset $B$ of the two-dimensional plane.

也可以根据 joint PDF 求出 marginal PDFs.

类似的, 我们也有 joint CDFs:

$$
F_{X, Y}(x, y) = \Pr(X \leq x, Y \leq y).
$$

CDF 求导可得对应的 PDF:

$$
f_{X, Y}(x, y) = \frac{\partial^2 F_{X, Y}}{\partial x \partial y}(x, y). 
$$
> [!summary]+ 
> 
> - continuous uniform over $[a, b]$:
> 	- $$f_{X}(x)=\begin{cases} \frac{1}{b-a}, &\text{if }a\leq x \leq b, \\ 0, &\text{otherwise,} \end{cases}$$
> 	- $$\mathrm{E}[X]=\frac{a+b}{2}, \quad \mathrm{var}(X) = \frac{(b-a)^2}{12}.$$
> - exponential with parameter $\lambda$:
> 	- $$f_{X}(x) = \begin{cases}\lambda e^{ -\lambda x }, &\text{if } x \geq 0, \\ 0, &\text{otherwise},\end{cases} \quad F_{X}(x) = \begin{cases}1 - e^{ -\lambda x }, &\text{if } x \geq 0, \\ 0, &\text{otherwise,}\end{cases}$$
> 	- $$\mathrm{E}[X]=\frac{1}{\lambda}, \quad \mathrm{var}(X)=\frac{1}{\lambda^{2}}$$
> - normal with parameters $\mu$ and $\sigma^{2}>0$:
> 	- $$f_{X}(x) = \frac{1}{\sqrt{ 2\pi }\sigma}e^{ -(x-\mu)^{2} / 2\sigma^{2} },$$
> 	- $$\mathrm{E}[X]=\mu, \quad \mathrm{var}(X)=\sigma^{2}.$$

## Conditioning & Independence

与上一章的内容是类似的, 这里不再赘述.

## The Continuous Bayes' Rule

> [!summary] 
> 令 $Y$ 为一个 continuous RV.
> 
> - 若 $X$ 是一个 continuous RV, 我们有
>   
>   $$f_{Y}(y)f_{X | Y}(x | y) = f_{X}(x) f_{Y|X}(y|x),$$ 于是 $$f_{X|Y}(x|y)=\frac{f_{X}(x)f_{Y|X}(y|x)}{f_{Y}(y)}=\frac{f_{X}(x)f_{Y|X}(y|x)}{\int_{-\infty}^\infty f_{X}(t)f_{Y|X}(y|t)\mathrm{d}t}$$
> - 若 $N$ 是一个 discrete RV, 我们有 
>   
>   $$f_{Y}(y)\Pr(N=n|Y=y)=p_{N}(n)f_{Y|N}(y|n),$$ 于是 $$\Pr(N=n|Y=y)=\frac{p_{N}(n)f_{Y|N}(y|n)}{f_{Y}(y)}=\frac{p_{N}(n)f_{Y|N}(y|n)}{\sum_{i}p_{N}(i)f_{Y|N}(y|i)},$$ 以及 $$f_{Y|N}(y|n)=\frac{f_{Y}(y)\Pr(N=n|Y=y)}{p_{N}(n)}=\frac{f_{Y}(y)\Pr(N=n|Y=y)}{\int_{-\infty}^\infty f_{Y}(y)\Pr(N=n|Y=y)\mathrm{d}t}$$
> - 对于 $\Pr(A|Y=y)$ 和 $f_{Y|A}(y)$ 也有类似的式子.

