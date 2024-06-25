---
title: "DPV Chapter 1: Algorithms with numbers"
---
## Basic arithmetic

Addition: 最简单也是最优的方法就是按 bit 相加，复杂度是 $O(n)$

Multiplication: 

1. The grade-school algorithm: 就是平时手算使用的算法，复杂度 $O(n^2)$
2. Al Khwarizmi’s second algorithm: 使用归并的思想，按照 digits 分为高位的 digits 和低位的 digits
	* $x\cdot y = (x_{h}\cdot 10^{n/2}+x_{l})\cdot(y_{h}\cdot 10^{n/2}+y_{l})=x_h\cdot y_h\cdot 10^n + (x_h\cdot y_l + x_l\cdot y_h)\cdot 10^{n/2}+x_l \cdot y_l$
	* $$ T(n)=\begin{cases}4T(\frac{n}{2})+Cn, n>1\\ Cn, n=1 \end{cases} (C \text{ is a constant})$$
	* 总复杂度仍然是 $O(n^2)$
