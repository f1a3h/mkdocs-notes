---
title: Research Log
date: 2024-02-15
draft: false
math: true
description: Research Log
categories:
  - Log
tags:
  - Log
---
**Thu Mar 7**

- 读完了 [[boehmeFuzzingChallengesReflections2021]]

**Fri Mar 8**

- 读了 chakrabortyDeepLearningBased2020 the first pass
	- 认识到对于 software testing 这个领域的了解还是太少了，论文的种类也不太清楚，无法回答「What type of paper is this?」的问题
	- 对于 "Context"，这篇文章主要分析了现有的利用 DL 进行 vulnerability detection 存在的问题，并提出了可能的解决方案，theoretical bases 应该是 semantic information 以及 DL 模型建构方面的理论知识吧（？
	- "Contributions" 大概是分析出来的问题，然后提出了一个可行的 road-map 罢
	- "Clarity"：连我都能勉强知道在干什么，应该算是 well-written 罢（？

**Mon Mar 18**

- 终于读完了 [Deep Learning based Vulnerability Detection: Are We There Yet?](http://arxiv.org/abs/2009.07235) 的 Introduction 呜呜
	- 了解了 fuzzing 按照 methods、types、techniques 分类的几种主要类型
	- 对各种 fuzzer 的 pros & cons 有了一些模糊的理解
	- 要是能有一些更具体的了解（比如伪代码等）就好了 QwQ

**Tue Mar 19**

- [Deep Learning based Vulnerability Detection: Are We There Yet?](http://arxiv.org/abs/2009.07235) 后面的部分实在读不下去了，准备先上手像 SAGE 一类比较经典的论文，其次读读代码，活用 GPT 的力量，综述的话还是抽空读一读 [The Art, Science, and Engineering of Fuzzing: A Survey](http://arxiv.org/abs/1812.00140) 这篇经典吧
- 读完了 [SAGE: Whitebox Fuzzing for Security Testing](https://queue.acm.org/detail.cfm?id=2094081) 这篇 ACM Queue 上的文章，通过它对 SAGE 利用一个简单的事例介绍，对 whitebox fuzzing 还有 symbolic execution 的了解清晰了不少

**Wed Mar 20**

- 读完了 Automated Whitebox Fuzz Testing 的 Section 2，感觉迷雾被一点一点拨开了
	- 伪代码很有 [[Computer Science/Theory of Computer Science/Programming Languages/NJU 软件分析/index|NJU 软件分析]] 的感觉，真不戳😋
	- 边读也逐渐明白了前文一些不明白的地方👍
	- 对一些 limitation 的产生原因也会有自己的猜测和可能的解决思路（天马行空），有点样子了

**Sun Mar 24**

- 读完了 Automated Whitebox Fuzz Testing 除了实验的部分，实验部分准备粗略浏览一下，不精读了，现阶段以知识扩张为主👋
	- 之后写一个 summary