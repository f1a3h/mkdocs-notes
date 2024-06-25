---
title: Lexical Analysis
date: 2024-03-23
draft: false
math: true
description: 
categories:
  - Study Notes
tags:
  - Compilers
  - Lexical-Analysis
---
## Informal Sketch

lexical analysis 的目标就是将 string 拆分（识别）成 tokens，而 token 又会根据用途被进一步分类，例如分为 identifier、integer、keyword、whitespace 等，最终 lexical analysis 会生成 a stream of tokens.

设计一个 lexical analyzer 可以分为以下两个步骤：

1. Define a finite set of tokens
	- Tokens describes all items of interest
	- Choice of tokens depends on language & design of parser
2. Describe which strings belong to each token

最终 lexer 会返回 token-lexeme pairs，并丢弃对 parser 无用的 pairs 例如 whitespace.

> [!summary]+ 
> - The goal of lexical analysis is to 
> 	- Partition the input string into lexemes
> 	- Identify the tokens of each lexeme
> - Left-to-right scan => lookahead sometimes needed

## Regular Languages

识别 lexemes 的所有 formalisms 中最流行的是 [[Automata and Language Theory#Regular Languages|regular languages]].

> [!note]+ Syntax v.s. Semantics
> ![[Screenshot 2024-03-23 at 17.09.23.png]]
> ![[Screenshot 2024-03-23 at 17.09.51.png]]
> 
> （感性理解一下）

