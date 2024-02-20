---
title: Linear Algebra Done Wrong
date: 2024-02-08
draft: false
math: true
description: Linear Algebra Done Wrong
categories:
  - Study Notes
  - Math
tags:
  - Math
  - Algebra
---

!!! info
	「[Linear Algebra Done Wrong](https://sites.google.com/a/brown.edu/sergei-treil-homepage/linear-algebra-done-wrong)」by Sergei Treil

## Ch.2

![image.png](https://fastly.jsdelivr.net/gh/f1a3h/imgs/202402191510320.png)

从信息量的角度来理解：

无论是从 row vectors 形成的空间还是 column vectors 形成的空间来看，他们所有给出的信息都可以用同一个 matrix 来概括，也就是信息量是相等的，那么他们所形成的 space 必然也会是相同的。

也就是说，他们的 space 取决于维度较小者，假如 # row > # col，那么 column vectors 多出来的维度没有传达任何信息，于是这个 matrix 一定会存在冗余信息，此时表现在 row vectors 上就是多余的 vectors。

!!! quote
	这种更感性的理解往往是发现新领域、新方法的突破口，纯粹的数字推演更多的是在发现后证明其思路严谨性的工具。
	<p align="right">——没错，是我说的😎</p>

