
## 树

- 结点的层次 / 深度：从上往下数，默认从1开始
- 树的高度 / 深度：树中结点的最大层数
- 结点的高度：从下往上数,以该结点为根的子树的高度.
- 结点的度：一个结点的孩子（分支）个数
- 树的度：树中所有结点度数的最大值
- 叶结点：度为0的结点

### 树的性质

1. 总结点数：$n_{0}+n_{1}+\dots+n_{m}$，其中 $n_{i}$ 是度为 $i$ 的结点数
2. 总结点数 $n=总度数+1$
3. 总度数 / 总分支数：$n_{1}+2n_{2}+\dots+mn_{m}$

### 度为 $m$ 的树和$m$ 叉树的区别

| 度为 m 的树                                            | $m$ 叉树                                   |
| -------------------------------------------------- | ---------------------------------------- |
| 树的度：$m$ 为各结点的度的最大值                                 | $m$ 叉树：每个结点最多只能有 $m$ 个孩子的树               |
| 任意结点的度 $⩽m$  <br>至少有一个结点度 $=m$                     | 任意结点的度 $⩽m$ <br>允许所有结点的度 $<m$            |
| 一定是非空树，至少有 $m+1$ 个结点                               | 可以是空树                                    |
| 第 $i$ 层至多有 $m^{i-1}$ 个结点                           | 第 $i$ 层至多有 $m^{i-1}$ 个结点                 |
| 至多有 $\dfrac{m^h-1}{m-1}$ 个结点，至少有 $h+m−1$ 个结点       | 至多有 $\dfrac{m^h-1}{m-1}$ 个结点，至少有 $h$ 个结点 |
| 最小高度为 $⌈log_{m}⁡[(n(m−1)+1)]⌉$，  <br>最大高度为 $n−m+1$ | 最小高度为 $⌈log_{m}⁡[(n(m−1)+1)]⌉$           |

## 二叉树

### 性质

1. 非空二叉树上的叶结点数等于度为 $2$ 的结点数加 $1$ 即 $n_{0}=n_{2}+1$，$n_{0}+n_{2}$ 一定是奇数
2. 非空二叉树的第 $k$ 层最多有$2^{k-1}$ 个结点
3. 高度为 $h$ 的二叉树至少有 $2^h-1$ 个结点
4. 含有 $n$ 个结点的二叉树最多有 $\dfrac{C_{2n}^{n}}{n+1}=\dfrac{(2n)!}{n!(n+1)!}$

### 特殊二叉树

#### 满二叉树

- 高度为 $h$ ，且含有 $2^h−1$ 个结点
- 从层序为 $1$开始编号，结点 $i$ 的左孩子为 $2i$，右孩子为 $2i+1$，结点 $i$ 的父结点为 $⌊i/2⌋$

```tikz
\begin{document}
	\begin{tikzpicture}[scale=2, ultra thick,
	every node/.style={draw=black, circle, very thick,font =\huge, minimum size=3em}, 
	level 1/.style={sibling distance=8em/1, level distance=3em},%横向距离与纵向距离
	level 2/.style={sibling distance=8em/2, level distance=3em},
	level 3/.style={sibling distance=8em/4, level distance=3em},
	]
	\node(1) {1}
		child {node (2) {2}
			child {node (4) {4}
				child {node[fill=red!50] (8) {8}}
				child {node[fill=red!50] (9) {9}}
			}
			child {node (5) {5}
				child {node[fill=red!50] (10) {10}}
				child {node[fill=red!50] (11) {11}}
			}
		}
        child {node (3) {3}
			child {node (6) {6}
				child {node[fill=red!50] (12) {12}}
				child {node[fill=red!50] (13) {13}}
			}
			child {node (7) {7}
				child {node[fill=red!50] (14) {14}}
				child {node[fill=red!50] (15) {15}}
			}
		};
	\end{tikzpicture}
\end{document}
```

#### 完全二叉树

在满二叉树的基础上，去掉了末端的几个结点的二叉树。满二叉树是完全二叉树的一种特殊情况。

- 只有最后两层可能有叶结点
- **度为$1$的结点只可能1个或0个**

从层序为 $1$ 开始编号到 $n$：

- 结点 $i$ 的左孩子为 $2i$ 或不存在，右孩子为 $2i+1$ 或不存在
- 结点 $i$ 的父结点为 $⌊i/2⌋$（如果有的话）
- $i⩽⌊n/2⌋$为分支结点，$i>\left \lfloor n/2 \right \rfloor$为叶子结点
- $n$ 为奇则每个分支节点都有左右孩子，$n$ 为偶则最后一个分支节点 $i=\frac{n}{2}$ 只有左孩子

```tikz
\begin{document}
	\begin{tikzpicture}[scale=2, line width=1.5pt,
	every node/.style={draw=black, circle, very thick,font =\huge, minimum size=3em}, 
	level 1/.style={sibling distance=8em/1, level distance=3em},%横向距离与纵向距离
	level 2/.style={sibling distance=8em/2, level distance=3em},
	level 3/.style={sibling distance=8em/4, level distance=3em},
	]
	\node(1) {1}
		child {node (2) {2}
			child {node (4) {4}
				child {node[fill=red!50] (8) {8}}
				child {node[fill=red!50] (9) {9}}
			}
			child {node (5) {5}
				child {node[fill=red!50] (10) {10}}
				child {node[fill=red!50] (11) {11}}
			}
		}
        child {node (3) {3}
			child {node (6) {6}
				child {node[fill=red!50, xshift=-2em] (12) {12}}
			}
			child {node[fill=red!50] (7) {7}}
		};
	\end{tikzpicture}
\end{document}
```

## 树形推断

#考点 选择题

【2016】若⼀棵⾮空k（k≥2）叉树T中的每个⾮叶结点都有k个孩⼦，则称T为正则k叉树。请回答
下列问题并给出推导过程。
1）若T有m个⾮叶结点，则T中的叶结点有多少个？
2）若T的⾼度为h（单结点的树h=1），则T的结点数最多为多少个？最少为多少个？

