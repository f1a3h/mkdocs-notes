---
title: AC自动机
date: 2024-01-26
draft: false
math: true
description: AC自动机学习笔记
categories:
  - Study Notes
tags:
  - String
  - DFA
  - Trie
---
众所周知，给定一个文本串 $t$ 和一个模式串 $s$，要求 $s$ 在 $t$ 中的所有出现可以用 [KMP 算法](https://oi-wiki.org/string/kmp/)在 $\mathcal{O}(|s+t|)$ 的时间内实现。但是，如果有多个模式串呢？给每个模式串都跑一遍 KMP 显然时间复杂度会是 $|s+t|$ 乘上 $s$ 的数量的级别的，当 $s$ 很多的时候就寄了。

考虑一下，很多个模式串的话，我们能不能从他们的公共前缀入手减少冗余的搜索量？这样的话，可以用模式串建一棵 [Trie 树](https://oi-wiki.org/string/trie/) 然后在 Trie 树上跳 fail 数组与 $t$ 的子串进行匹配，如果匹配成功且当前结点存在对应模式串的话就可以给这个模式串统计的答案+1.

于是这样就产生了 [AC 自动机](https://oi-wiki.org/string/ac-automaton)！

???+ note
	Trie 树就是一种[有限状态自动机 (Deterministic Finite Automaton, DFA)](https://www.wikiwand.com/en/Deterministic_finite_automaton)，而 KMP 在文本串中求不断匹配求前缀函数的过程也可以看作 DFA 运转的过程，也就是所谓的 KMP 自动机。
	
	这样来看，其实 AC 自动机就是 Trie 树上但是状态转移函数不同（和 KMP 的差不多）的自动机。

## Build

首先当然是用模式串构造 Trie 树了，具体步骤就不说了。

```cpp
void insert(string& s) {
	int u = 0;
	for (int i = 0; i < s.length(); ++i) {
		int p = s[i] - 'a';
		if (!tr[u][p]) tr[u][p] = ++cnt;
		u = tr[u][p];
	}
	tot[u]++;
}
```

之后利用 KMP 的思想构造 fail 数组。

与 KMP 在文本串上构造 next (fail) 数组不同，AC 自动机是在 Trie 树上，也就是对模式串构造 fail 数组的。

???+ tip "Deep Dark Fantasy♂"
	构造 fail 数组都是对自身的前后缀进行匹配的过程，KMP 对 $s$+#+$t$ 的新串构造 fail 数组其实是利用了只对 $t$ 的前缀和 $s$ 进行匹配是很方便的（$\mathcal{O}(|s|)$）的，而 AC 自动机由于有多个模式串因此这一个优势失效了。
	
	也就是说 AC 自动机在 Trie 树上构造 fail 数组并不是利用上面一点，而是为了匹配失败时能更快地寻找到下一个可能匹配上的模式串，这也是我们为什么要借助公共前缀构造 Trie 树的原因。

类似地，我们按照顺序 BFS 一遍 Trie 树，假设当前结点为 $u$ ，要处理的边上对应字符为 $\mathtt{c}$ ，并且所有深度小于等于 $u$ 的结点（包括它自己）的 fail 数组都已构造好了，那么我们不断跳 `fail[u]` 直到跳到一个 `tr[fail[u]][c]` 存在的结点即可，然后再令 `fail[tr[u][c]]` 指向这个结点。

不过，跳到不存在的结点 `tr[fail[u]][c]` 显然浪费了时间，如果我们可以在处理结点 $u$ 时将不存在的结点 `tr[u][c]` 也处理出来，令其指向 `tr[fail[u]][c]` 即可进行路径压缩，保证了跳 `fail[u]` 时只需要跳一次，具体步骤如下：

如果 `tr[u][c]` 存在，那么 `fail[tr[u][c]] = tr[fail[u]][c]`；如果不存在，则令 `tr[u][c] = tr[fail[u]][c]` ，由于 `fail[u]` 的深度必然小于 $u$ ，按照前面的处理方式，`tr[fail[u]][c]` 必然已经有了对应好的结点，并且这个结点深度同样是小于 $u$ 的，所有它的 fail 数组也是已经处理好了的，就不需要我们再手动指定一个 `fail[tr[u][c]]` 了，这也是为什么前一种情况下不需要管 `tr[fail[u]][c]` 是否存在的原因。

```cpp
void build() {
	std::queue <int> q;
	for (int i = 0; i < 26; ++i){
		if (tr[0][i]) q.push(tr[0][i]);
	}

	while (!q.empty()) {
		int u = q.front();
		q.pop();

		for (int i = 0; i < 26; ++i) {
			if (tr[u][i]) {
				fail[tr[u][i]] = tr[fail[u]][i];
				q.push(tr[u][i]);
			} else {
				tr[u][i] = tr[fail[u]][i];
			}
		}
	}
}
```

## Matching

匹配过程就是直接在 Trie 树上对 $t$ 进行匹配，每走到一个结点就不断跳 fail 数组，看跳到的结点是否有一个对应的模式串，如果有的话就说明这个模式串在当前 $t$ 的子串的后缀中出现了，于是就给它的答案加一即可。

```cpp
int query(string& t) {
	int u = 0, ret = 0;
	for (int i = 0; i < t.length(); ++i) {
		int p = t[i] - 'a';
		u = tr[u][p];
		for (int j = u; j > 0 && tot[j] != -1; j = fail[j]) {
			ret += tot[j];
			tot[j] = -1;
		}
	}
	return ret;
}
```

## Optimization

从前面构造 fail 数组的过程中可以发现，所有结点有且仅有一条 fail 边，并且一定指向深度小于它的结点，这不就是一棵树吗？那么我们匹配时不断跳 fail 边不就相当于从当前结点一步一步地跳到根节点吗？既然 fail 树是已知的，那么我们完全没有必要每次匹配都往上跳，完全可以给当前结点打一个 flag，最后直接在 fail 树上统计答案嘛。

当然，统计答案又可以分成好几种方法，比如在建图时记录 fail 边的入度，最后拓扑排序时统计子树的答案，也可以直接 DFS 一遍 fail 树求出子树和。

## Code

[Luogu P3808 AC自动机（简单版）](https://www.luogu.com.cn/problem/P3808)

对应无 optimization 版本的代码。

```cpp
#include <bits/stdc++.h>

using std::cin;		using std::cout;
using std::string;

const int N = 1e6 + 5;

class AC_Automaton {
private:
	int cnt = 0;
	int tr[N][26], fail[N], tot[N];

public:
	void insert(string& s) {
		int u = 0;
		for (int i = 0; i < s.length(); ++i) {
			int p = s[i] - 'a';
			if (!tr[u][p]) tr[u][p] = ++cnt;
			u = tr[u][p];
		}
		tot[u]++;
	}

	void build() {
		std::queue <int> q;
		for (int i = 0; i < 26; ++i){
			if (tr[0][i]) q.push(tr[0][i]);
		}

		while (!q.empty()) {
			int u = q.front();
			q.pop();

			for (int i = 0; i < 26; ++i) {
				if (tr[u][i]) {
					fail[tr[u][i]] = tr[fail[u]][i];
					q.push(tr[u][i]);
				} else {
					tr[u][i] = tr[fail[u]][i];
				}
			}
		}
	}

	int query(string& t) {
		int u = 0, ret = 0;
		for (int i = 0; i < t.length(); ++i) {
			int p = t[i] - 'a';
			u = tr[u][p];
			for (int j = u; j > 0 && tot[j] != -1; j = fail[j]) {
				ret += tot[j];
				tot[j] = -1;
			}
		}
		return ret;
	}
} AC;

int main() {
	std::ios::sync_with_stdio(false);
	cin.tie(0), cout.tie(0);

	int n;
	cin >> n;

	string s;
	for (int i = 1; i <= n; ++i) {
		cin >> s;
		AC.insert(s);
	}
	AC.build();
	cin >> s;
	cout << AC.query(s);
	return 0;
}
```

带 optimization 的代码的修改也很简单，就不重新写了。