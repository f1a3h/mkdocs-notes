---
title: 量子波动速通
date: 2024-02-24
draft: false
math: true
description: 
categories: 
tags:
  - writeup
---
## 数学

### 位运算

- [[LuoguP5539|【XR-3】Unknown Mother-Goose]]

### 整除相关

#### 最大公约数

- [「Luogu P2152」 \[SDOI2009\]SuperGCD](https://www.luogu.com.cn/problem/P2152)
	- 复杂度显然是一个 $\log$ ，$\gcd$ 的复杂度就是 $\log{n}$ ，就是一个高精度取模而已

#### 欧拉函数

- [「Luogu P2158」 \[SDOI2008\]仪仗队](https://www.luogu.com.cn/problem/P2158)
	- 首先，左上角和右下角的三角形是对称的，因此我们只计算右下角的三角形的答案。
	- 以左下角的点为原点画坐标轴，先看最后一列 $x=n, y=1\sim n$ 的点到原点的直线，发现 $y = \dfrac{y}{n}x$ ，只有 $y$ 是 $n$ 的约数的点不能被看见，因此最后一列的答案是 $\varphi(n)$ 
	- 同理，第 $k$ 列的答案是 $\varphi(k)$ 
- [「Luogu P2568」GCD](https://www.luogu.com.cn/problem/P2568)
	- $n \le 10^7$ 可以筛出所有素数，假设当前考虑 $p$，$x$ 可能是 $p$ 的 $1\sim \lfloor \dfrac{n}{p} \rfloor$ 倍，不妨设 $x=kp,y=rp$，仅考虑 $y<x$ 的情况，那么 $r<k\land r\bot k$ ，这样的 $r$ 共有 $\varphi(k)$ 个，那么素数 $p$ 的答案就是 $2\Sigma_{i=1}^{\lfloor\frac{n}{p}\rfloor} \varphi(i) - 1$ 
- [「Luogu P2398」 GCD SUM](https://www.luogu.com.cn/problem/P2398)
	- 直接求解的话光是枚举的复杂度就是一个 $n^2$ 承受不起，这时候就应该考虑 $\gcd(x, y)=p$ 的贡献
	- 之后就和 Luogu P2568 的思路是一样的了

### 同余方程

#### 中国剩余定理

- [「Luogu P3868」[TJOI2009] 猜数字](https://www.luogu.com.cn/problem/P3868)
	- 裸的 CRT