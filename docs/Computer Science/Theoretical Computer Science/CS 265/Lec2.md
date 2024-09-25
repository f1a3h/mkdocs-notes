---
title: "Lec 2: Linearity of Expectation, Karger’s Min-Cut Algorithm, and Quicksort with Random Pivot"
---
## Linearity of Expectation

形式如下:

$$\mathrm{E}\left[ \sum_{i}a_{i}X_{i} \right] = \sum_{i}a_{i}\mathrm{E}[X_{i}]$$

## Karger's Min-Cut Algorithm

### High-Level Intuition

Karger's min-cut algorithm 通过一系列随机的 *edge contraction* 操作进行缩点, 最后剩下两个点就得到了一个“最小”割.

它的 intuition 在于最小割的割边数量小, 每次会被选中的概率也就相对小.

### The Algorithm

> [!definition] Edge contraction
> Given a graph $G=(V, E)$ with $n$ vertices $V=\{v_{1}, \dots, v_{n}\}$, and an edge $e \in E$ that connects vertices $v_{i}, v_{j}$, the graph resulting from *contracting edge* $e$ will have $n-1$ vertices, namely $V / \{v_{i}\}$, and edge set defined as follows: for every edge $e' \in E$ that does not have $v_{i}$ as an endpoint, $e'$ is an edge of the new graph. For every edge $e' \in E$ that connects $v_{i}, v_{k}$ for $k \neq j$, we add the edge $(v_{j}, v_{k})$ to the edge.

![image.png](https://picgo-1259588753.cos.ap-beijing.myqcloud.com/202409231046911.png)

上述算法单次运行的正确率下界为 $\frac{2}{n(n-1)} \geq \frac{2}{n^{2}}$, 