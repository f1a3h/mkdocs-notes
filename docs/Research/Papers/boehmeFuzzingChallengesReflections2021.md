---
title: "Fuzzing: Challenges and Reflections"
tags:
  - Papers
  - Review
share: true
---

## 1. Abstract & Info

### 1.1 Abstract

主要是总结了 fuzzing 目前遇到的一些问题以及可能发展的方向。

### 1.2 Info

**Authors**: Marcel Boehme, Cristian Cadar, Abhik Roychoudhury
**DOI**: 10.1109/MS.2020.3016773
**Publication**: Fuzzing: Challenges and Reflections
**Zotero**: [Boehme et al. - 2021 - Fuzzing Challenges and Reflections.pdf](zotero://select/library/items/4XJENJ6Y)
**Date**: 2021-05-01

## 2. Annotation

Annotated Text：[The main benefit of this technique is that by carefully keeping track of path conditions of all inputs seen so far, it always generates an input traversing a new path (new control flow)](zotero://open-pdf/library/items/4XJENJ6Y?page=2&annotation=NBY9IAI6)
Comment：


Annotated Text：[we should investigate strategies to boost fault finding](zotero://open-pdf/library/items/4XJENJ6Y?page=3&annotation=FMIHHANY)
Comment：🔤我们应该研究促进错误发现的策略🔤

Annotated Text：[ranking bugs in terms of their importance can also improve the effectiveness of fuzzing in practice.](zotero://open-pdf/library/items/4XJENJ6Y?page=3&annotation=QIVTKYCC)
Comment：🔤根据缺陷的重要性进行排序，也可以提高模糊测试在实际中的有效性。🔤

Annotated Text：[To make symbolic execution applicable in practice, correctness or completeness are traded for scalability](zotero://open-pdf/library/items/4XJENJ6Y?page=4&annotation=CAARRS8Q)
Comment：🔤为了使符号执行在实际中得到应用，需要以正确性或完备性来换取可扩展性🔤

Annotated Text：[If we model black-box fuzzing as a random sampling from the program’s input space, we can leverage methods from applied statistics to estimate the residual risk.](zotero://open-pdf/library/items/4XJENJ6Y?page=4&annotation=36TNJSFV)
Comment：🔤如果我们将黑盒模糊测试建模为程序输入空间的随机采样，那么我们可以利用应用统计学中的方法来估计残差风险。🔤

Annotated Text：[we should develop statistical and probabilistic frameworks and methodologies for sound estimation with quantifiable accuracy.](zotero://open-pdf/library/items/4XJENJ6Y?page=4&annotation=QQGFIPLU)
Comment：


Annotated Text：[One model, inspired by constraint solving and verification competitions, is to have different competition categories](zotero://open-pdf/library/items/4XJENJ6Y?page=5&annotation=3MZMMEFE)
Comment：🔤其中一个模型受到约束求解和验证竞争的启发，即具有不同的竞争类别🔤

Annotated Text：[A second model is to come up with challenge problems in the form of buggy programs and have tool developers directly apply the fuzzers to find the hidden bugs.](zotero://open-pdf/library/items/4XJENJ6Y?page=5&annotation=3PYGEUC2)
Comment：🔤第二种模型是以Buggy程序的形式提出挑战性问题，并让工具开发人员直接应用模糊测试来发现隐藏的Bug。🔤

Annotated Text：[Another approach is a continuous evaluation, where fuzzers are repeatedly used to fuzz real programs.](zotero://open-pdf/library/items/4XJENJ6Y?page=5&annotation=QFQH8PDZ)
Comment：🔤另一种方法是连续评估，即反复使用模糊测试对真实程序进行模糊化。🔤

Annotated Text：[If the chosen time budget is too small, the faster, yet less effective, fuzzer might appear more effective.](zotero://open-pdf/library/items/4XJENJ6Y?page=6&annotation=SE7KF2ZA)
Comment：🔤如果选择的时间预算太小，速度越快，但效果越差，模糊器可能会显得更有效。🔤

Annotated Text：[In the implementation, the researcher can make engineering decisions that can substantially affect the effectiveness of the fuzzer.](zotero://open-pdf/library/items/4XJENJ6Y?page=6&annotation=C3YIVR8K)
Comment：🔤In the implementation, the researcher can make engineering decisions that can substantially affect the effectiveness of the fuzzer.🔤

## 3. Key Notes

### 3.1 Problems


### 3.2 Contributions


### 3.3 Method


### 3.4 Comments

