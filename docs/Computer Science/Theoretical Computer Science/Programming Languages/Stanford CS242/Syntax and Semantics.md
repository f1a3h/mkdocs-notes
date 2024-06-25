---
title: Syntax and Semantics
date: 2024-03-10
draft: false
math: true
description: 
categories:
  - Study Notes
tags:
  - PL
  - TCS
---
以 arithmetic 的语言为例学习 programming languages 的语言。

- Syntax: arithmetic expression 的结构是什么
- Static semantics: arithmetic expression 的 subset 具有什么样的含义
- Dynamic semantics: 给出的这个 arithmetic expression 的含义是什么

## Syntax

languages 以 *primitives* 或者表示 atomic unit of meaning 的 objects 开头，programming languages 中的 primitives 有 characters、booleans、functions 等。

arithmetic 的结构可以用 context-free grammar (CFG) 表示，例如（[Backus-Naur Form](https://en.wikipedia.org/wiki/Backus–Naur_form)）：

![[Screenshot 2024-03-11 at 19.41.40.png]]

grammar 是一个 meta-linguistic 的概念，用来表示 language 的结构。

### Syntax Trees

CFG 也可以描述树形结构，而由线性的字符组成的文字表达式则很难表示为树形的结构。从这一个角度出发，我们可以定义 syntax 为 establishing the basic structure of a language.

language 的 syntax 又可以分为 abstract syntax 和 concrete syntax，分别指向它的 structure 和 symbols。一个 language 可能有多个 concrete syntax 对应同一个 abstract syntax。

concrete syntax tree 或者说 parse/deriviation tree，是用来表示某些 context-free languages 的 syntactic structure 的树形结构，它相对于 abstract syntax tree (AST) 保留了 symbols，例如下图[^1]中的 `INT`、`ID` 等：

![[Screenshot 2024-03-11 at 20.24.41.png]]

[^1]: Stanford CS143 [lecture05 slides](https://web.stanford.edu/class/cs143/lectures/lecture05.pdf), p7.

而 AST 则进一步抽象，去掉了与 semantics 无关的信息（e.g. grouping parentheses），仅仅保留了 structural details，例如下图[^2]：

![[Screenshot 2024-03-11 at 20.32.00.png]]

[^2]: [Abstract syntax tree - Wikipedia](https://www.wikiwand.com/en/Abstract_syntax_tree).

### Ambiguity and Precedence

CFG 可能是 ambiguity 的，对于某个 CFL 的 string，这个 CFG 可能得到不同的 parse tree，于是这个 CFG 就是 ambiguity 的。

## Semantics

