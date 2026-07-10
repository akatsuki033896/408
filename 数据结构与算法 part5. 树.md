
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

#考点 选择题，记住性质结论

\[2016\] 若⼀棵⾮空k（k≥2）叉树T中的每个⾮叶结点都有k个孩⼦，则称T为正则k叉树。请回答
下列问题并给出推导过程。
1）若T有m个⾮叶结点，则T中的叶结点有多少个？($km+1-m$)
2）若T的⾼度为h（单结点的树h=1），则T的结点数最多为多少个？最少为多少个？
($1+k(h-1) \leq T\leq \frac{k^h-1}{k-1}$) 一刷最小推错了

## 树的存储

#考点 

### 顺序存储

用一组连续的存储单元从上到下，从左到右存储完全二叉树的结点元素。

完全二叉树和满二叉树适合顺序存储，因为结点序号可以唯一地反应结点之间的逻辑关系，节省存储空间，一般二叉树需要添加空结点。

![[二叉树顺序存储.png]]

```cpp
#define MaxSize 100

struct TreeNode{
	ElemType value; //结点中的数据元素
	bool isEmpty;	//结点是否为空
} TreeNode;
```

### 链式存储

存储一般二叉树 / 线索二叉树，寻找指定结点孩子较为方便。

![[链域.png]]

```cpp
typedef struct BiTNode{
	ElemType data;	//数据域
	struct BiTNode *lChild, *rChild;	//左、右孩子指针
}BiTNode, *BiTree;
```

$n$ 个结点的二叉链表有 $2n-(n-1)=n+1$ 个空链域。

![[链式存储表示.png]]


|     | 双亲表示法       | 孩子兄弟表示法                       | 孩子表示法                                    |
| --- | ----------- | ----------------------------- | ---------------------------------------- |
| 特点  | 用连续存储单元存储双亲 | 把树的结点按照“左孩子右兄弟”转换成二叉树，用二叉链表存储 | 每个结点的孩子视为单链表存储的线性表，$n$ 个结点组成另一个基于顺序表的线性表 |
| 优点  | 找双亲方便       | 找孩子方便                         | 找孩子方便                                    |
| 缺点  | 找孩子要遍历      | 找双亲要遍历                        | 找双亲要遍历                                   |

## 二叉树的遍历

### 前序遍历

根-左-右

递归

```cpp
void preOrder(BiTree root){
	if (root == NULL) return;
	visit(root);			//访问根结点
	preOrder(root->lChild);	//递归遍历左子树
	preOrder(root->rChild);	//递归遍历右子树
}
```

迭代, 时间复杂度 $O(n)$, 空间复杂度 $O(\log n)$ ~ $O(n)$

```cpp
void preOrder(TreeNode* root){
	if (root == NULL) return;
	
	stack <TreeNode*> stk;	// 等价于递归栈
	TreeNode* node = root;
	while(!stk.empty() || node!=NULL) {
		while (node != NULL) {	// 找到最左下方的结点
			visit(node);		// 边找边访问根结点
			stk.push(node);
			node = node->left;
		}
		node = stk.top();
		stk.pop();
		node = node->right;
	}
}
```

### 中序遍历

左-根-右

递归, 时间复杂度 $O(n)$, 空间复杂度 $O(\log n)$ ~ $O(n)$

```cpp
void inOrder(BiTree root) {
	if (root == NULL) return;
	
	inOrder(root->lChild);	//递归遍历左子树
	visit(root);			//访问根结点
	inOrder(root->rChild);	//递归遍历右子树
}
```

迭代，时间复杂度 $O(n)$, 空间复杂度 $O(\log n)$ ~ $O(n)$

```cpp
void inOrder(TreeNode* root) { 
	stack<TreeNode*> stk; 
	while (root != NULL || !stk.empty()) { 
		while (root != NULL) {	// 找到最左下方的结点
			stk.push(root); 
			root = root->left; 
		} 
		root = stk.top(); 
		stk.pop(); 
		visit(root);					// 左子树为空，访问根结点
		root = root->right; 
	}
}
```

### 后序遍历

递归, 时间复杂度 $O(n)$, 空间复杂度 $O(\log n)$ ~ $O(n)$

```cpp
void postOrder(BiTree root){
	if (root == NULL) return;
	
	postOrder(root->lChild);	//递归遍历左子树
	postOrder(root->rChild);	//递归遍历右子树
	visit(root);				//访问根结点
}
```

迭代，时间复杂度 $O(n)$, 空间复杂度 $O(\log n)$ ~ $O(n)$

```cpp
//后序遍历中需要记录前一个访问的结点

void postorderTraversal(TreeNode *root) {
	if (root == NULL) return res;
	
	stack<TreeNode *> stk;
	TreeNode *prev = NULL;
	while (root != NULL || !stk.empty()) {
		while (root != NULL) {	// 找到最左下方的结点
			stk.push(root);
			root = root->left;
		}
		root = stk.top();
		stk.pop();
		if (root->right == NULL || root->right == prev) {// 右子树为空或者已经被访问过，访问根结点
			visit(root);
			prev = root;
			root = NULL;
		} else {	// 右子树非空，在右子树中找到最左下方的结点
			stk.push(root);
			root = root->right;
		}
	}
}
```

### 层次遍历

需要借助队列, 时间复杂度 $O(n)$，空间复杂度 $O(n)$

1. 初始化一个辅助队列
2. 根结点入队
3. 若队列非空，则队头结点出队，访问该结点，依次将其左、右孩子插入队尾（如果有的话）
4. 重复以上操作直至队列为空

```cpp
void levelOrder(TreeNode* root) {
	if (root == NULL) return;
	
	queue <TreeNode*> q;	// 辅助队列
	q.push(root);
	
	while (!q.empty()) {
		int currentLevelSize = q.size();	// 当前层元素个数
		// 一次性出队一层结点
		for (int i = 1; i <= currentLevelSize; ++i) {
			TreeNode* node = q.front();
			q.pop();
			visit(node);
			if (node->left) q.push(node->left);		// 左孩子入队
			if (node->right) q.push(node->right);	// 右孩子入队
		}
	}
}
```

### 唯一确定二叉树

#考点 中序遍历+任何一种遍历可以唯一确定二叉树，找到树的根结点，并根据中序序列划分左右子树，再找到左右子树根结点。

## 线索二叉树

$n$ 个结点的二叉树，有 $n+1$ 个空链域用来记录前驱、后继的信息，**按照遍历顺序，每个节点都要连前驱和后继，没有的则引出为 `NULL`**。

- 若无左子树，令`lChild`指向其前驱结点；若无右子树，令`rChild`指向其后继结点
- 遍历序列中的第一个结点的前驱指向 `NULL`，最后一个结点的后继指向 `NULL`
- 先序线索二叉树：线索指向先序前驱、先序后继
- 中序线索二叉树：线索指向中序前驱、中序后继
- 后序线索二叉树：线索指向后序前驱、后序后继

```tikz
\usetikzlibrary{positioning}
\begin{document}
	\begin{tikzpicture}[scale=2, ultra thick,
	every node/.style={draw=black, circle, very thick,font =\huge, minimum size=3em}, 
	level 1/.style={sibling distance=8em/1, level distance=3em},
	level 2/.style={sibling distance=8em/1.5, level distance=3em},
	level 3/.style={sibling distance=8em/4, level distance=3em},
	]
	\node(A) {$A$}
		child {node (B) {$B$}
			child {node (D) {$D$}
				child {node[xshift=2em] (G) {$G$}}
			}
			child {node (E) {$E$}}
		}
        child {node (C) {$C$}
			child {node[xshift=-4em] (F) {$F$}}
		};
	
	\node[font=\small, draw=none] (nullC) [right=of C, yshift=-2em] {NULL};
	\node[font=\small, draw=none] (nullD) [left=of D, yshift=-2em] {NULL};
	
	\draw[-latex, dashed, orange] (D) to [in=0, out=-120](nullD);
	\draw[-latex, dashed, orange] (G) to [in=-90, out=-120](D);
	\draw[-latex, dashed, purple] (G) to [in=-100, out=-60](B);
	\draw[-latex, dashed, orange] (E) to [in=-80, out=-120](B);
	\draw[-latex, dashed, purple] (E) to [in=-100, out=-60](A);
	\draw[-latex, dashed, orange] (F) to [in=-80, out=-120](A);
	\draw[-latex, dashed, purple] (F) to [in=-100, out=-60](C);
	\draw[-latex, dashed, purple] (C) to [in=-180, out=-60](nullC);
	\end{tikzpicture}
\end{document}
```

存储结构

```cpp
//线索二叉树结点
typedef struct ThreadNode{
	ElemType data;
	struct ThreadNode *lChild, *rChild;	// 左、右孩子指针
	int ltag, rtag;						// 左、右线索标志
}ThreadNode, *ThreadTree;
```

为了区分 `lChild` 和 `rChild` 指向的是左右孩子还是前驱或后继结点，需要增加 `tag` 为作为标识

- `ltag = 0`：`lChild` 指向结点的左孩子
- `ltag = 1`：`lChild` 指向结点的前驱
- `rtag = 0`：`rchild` 指向结点的右孩子
- `rtag = 1`：`rchild` 指示结点的后继

### 二叉树的线索化

#考点 将二叉链表中的空指针改为指向前驱或后继的线索. 实质是遍历一次二叉树（迭代遍历）

中序线索化：若某节点有右孩子，则后继结点是它右子树中最左下的结点，若有左孩子，则前驱结点是左子树中最右下的结点。

## 树和森林

### 二叉树和树 / 森林转换

#考点 数量推断运算不熟悉，森林与二叉树遍历对应关系

树转为二叉树：左孩子右兄弟

二叉树转森林：二叉树的根、根的右孩子、根的有孩子的右孩子以此类推都为森林的树的根结点，其余按照左孩子右兄弟转换。

森林转二叉树：森林中所有树的根结点组成二叉树的根，根的右孩子，根的右孩子的右孩子以此类推。其余按照左孩子右兄弟转换。

![[树和森林转换.png]]

![[树和森林转换2.png]]

![[树和森林转换3.png]]

![[树和森林转换4.png]]

设森林F对应的⼆叉树为BT，结点总数是m，若BT的右⼦树共有n个结点，则森林F中第⼀棵树
的结点总数是(m-n，右子树n个结点说明森林除了第一棵树剩下所有树的结点一共有n个)

森林F中有4棵树，结点个数分别为M1，M2，M3，M4，当F转换成⼆叉树之后，该⼆叉树的根
节点的右⼦树的结点总数是(M2+M3+M4)

### 二叉树和树 / 森林遍历对应关系

#考点 背下来

| 树    | 二叉树  | 森林     |
| ---- | ---- | ------ |
| 先根遍历 | 先序遍历 | 先序遍历   |
| 后根遍历 | 中序遍历 | 中/后序遍历 |

## 哈夫曼树

#考点 10/16逆天考频，不熟悉 $m$ 叉树构造

### 二叉哈夫曼树

含有 $n$ 个带权叶结点的二叉树中，带权路径长度(WPL)最小的二叉树为哈夫曼树 / 最优二叉树。不局限于二叉树，也存在于多叉树中

- 结点的带权路径长度：树的根到第 $i$ 个叶结点的路径长度 $l_{i}$ 和该结点的权值 $w_{i}$ 的乘积。
- 树的带权路径长度：所有叶结点的WPL的总和。

$$
WPL=\sum_{i=1}^nw_{i}l_{i}
$$

![[WPL.png]]

\[2023\] 在由6个字符组成的字符集S中，各字符出现的频次分别为3，4，5，6，8，10，为S构
造的哈夫曼编码的**加权平均⻓度**为 $\dfrac{(8+10)\times2+(3+4+5+6)\times 3}{3+4+5+6+8+10}=2.5$

### 构造

给定 $n$ 个权值分别为$w_{1}, w_{2}, \cdots, w_{n}$的结点，构造哈夫曼树的算法描述如下：

1. 将这 $n$ 个结点分别作为 $n$ 棵仅含一个结点的二叉树，构成森林 $F$
2. 构造一个新结点，从 $F$ 中选区两棵根结点权值最小的树作为新结点的左右子树，并且将新结点的权值置为左右子树上根结点的权值之和
3. 从 $F$ 中删除刚才选出的两棵树，同时将新得到的树加入到 $F$ 中去
4. 重复操作，直至 $F$ 中只剩下一棵树为止

### 性质

1. 有$n$个结点，则在哈夫曼树的构造过程中新建了$n-1$个结点，最终⼀共$2n-1$个结点
2. 哈夫曼树只有度为$0$和$2$的结点，$n=n_{0}+n_{2}$
3. ⾮空⼆叉树满⾜$n_{0}=n_{2}+1$（$n=n_{0}+n_{1}+n_{2},n=n_{1}+2n_{2}+1$推出），故哈夫曼树$n=2n_{0}-1=2n_{2}+1$（不熟）
4. 权值（频度）越⼩的结点⼀般离根结点越远，利⽤这条性质保证出现最多的字符拥有最短的编码
5. 同⼀组权值构造出的哈夫曼树可能不唯⼀

\[2019\] 对$n$个互不相同的符号进⾏哈夫曼编码。若⽣成的哈夫曼树共有$115$个结点，则$n$的值是 $2n-1=115, n=58$（所有符号都是叶结点）

### $m$ 叉哈夫曼树

1. 定义：只有度为$0$和$m$的结点的哈夫曼树
2. 构造过程：初始结点有$n_{0}$个，⾸先计算$k=(n_{0}-1)\text{mod}(m-1)$，如果$k\neq0$，则需要补充$m-1-k$个权为$0$的结点
3. 仿照二叉哈夫曼树的构造过程，每次结合$m$个权值最⼩的结点，重复该过程

### 哈夫曼编码

$n$个字符出现的频率作为权构造⼀棵哈夫曼树，由哈夫曼树求得的编码就是哈夫曼编码

![[哈夫曼编码.png]]

定⻓哈夫曼编码：所有字符都位于哈夫曼树的同⼀层，哈夫曼编码⻓度相同
变⻓哈夫曼编码：符都位于哈夫曼树的不同层，哈夫曼编码⻓度不同

1. 哈夫曼树的构造过程决定了字符⼀定是叶⼦结点
2. 哈夫曼编码是前缀编码（不允许某个字符的编码是另⼀个的前缀）所以两个字符不能出现在树中从根到某个叶⼦结点的同⼀路径上

\[2020\] 若任意⼀个字符的编码都不是其他字符编码的前缀，则称这种编码具有前缀特性。现有
某字符集（字符个数≥2）的不等⻓编码，每个字符的编码均为⼆进制的0、1序列，最⻓为L位，且
具有前缀特性。请回答以下问题：
1）哪种数据结构适宜保存上述具有前缀特性的不等⻓编码？
2）基于你所设计的数据结构，简述从0/1串到字符串的译码过程。
3）简述判定某字符集的不等⻓编码是否具有前缀特性的过程。

## 并查集

#考点 从未考过务必重视，可能会考并查集的应用

1. 管理多个不相交集合
2. 核心操作先查再并

存储结构：双亲表示法，每个子集合用一棵树表示，所有这种树构成的森林就是整个集合，根节点的父亲以 `-1` 表示。

```tikz
\begin{document}
	\begin{tikzpicture}[scale=2, ultra thick,
	every node/.style={draw=black, circle, very thick, font =\huge, minimum size=3em}, 
	level 1/.style={sibling distance=4em/2, level distance=3em},%横向距离与纵向距离
	level 2/.style={sibling distance=4em/2, level distance=3em},
	level 3/.style={sibling distance=4em/2, level distance=3em},
	]
	\node[fill=green!30] (A) at(0,0) {$A$}
		child {node[fill=green!30] (B) {$B$}
			child {node[fill=green!30] (E) {$E$}
				child {node[fill=green!30] (K) {$K$}}
				child {node[fill=green!30] (L) {$L$}}
			}
			child {node[fill=green!30] (F) {$F$}}
		};
	\node[fill=purple!30] (C) at(1.5,0) {$C$}
		child {node[fill=purple!30] (G) {$G$}};
	\node[fill=orange!30] (D) at(3,0) {$D$}
		child {node[fill=orange!30] (H) {$H$}}
		child {node[fill=orange!30] (I) {$I$}}
		child {node[fill=orange!30] (J) {$J$}};
	\end{tikzpicture}
\end{document}
```

| 编号       | 0   | 1   | 2   | 3   | 4   | 5   | 6   | 7   | 8   | 9   | 10  | 11  |
| -------- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| `data`   | A   | B   | C   | D   | E   | F   | G   | H   | I   | J   | K   | L   |
| `parent` | -1  | 0   | -1  | -1  | 1   | 1   | 2   | 3   | 3   | 3   | 4   | 4   |

### 基本操作

```cpp
#define MAX 10
void Initial(int S[]) {
	for (int i = 0; i < MAX; i++) {
		s[i] = -1;
	}
} // 初始化

int Find(int S[], int x){
	while (S[x] >= 0)	
		x = S[x];		//不断找父结点
	return x;	//根的S[]小于0，返回根结点
} // 查找x, 返回x所属根节点，时间复杂度为O(d), d为树的高度

void Union(int S[], int root1, int root 2){
	if(root1 == root2)
		return;
	S[root1] += s[root2];
	S[root2] = root1;	//将根Root2连接到另一根Root1下面
}

```

![[并查集合并.png]]

### 优化的查找合并

优化后查找+合并复杂度为 $O(\alpha(n))$

```cpp
int findWithPathCompression(int S[], int x) {
	int root = x;
	while (S[root] >= 0)
		root = S[root]; // 找集合的根
	while (x != root) {
		int t = S[x];
		S[x] = root; // 把x挂到root下面
		x = t; // 接下来往上处理x的父结点，全部挂到root下面
	}
	return root;
}
```

![[并查集路径压缩.png]]

```cpp
void UnionBySize(int S[], int root1, int root 2){
	if(root1 == root2)
		return;
	if (S[root1] < S[root2]) {
		S[root1] += S[root2];
		S[root2] = root1;	//将根Root2连接到另一根Root1下面
	}
	else {
		S[root2] += S[root1];
		S[root1] = root2;
	}
} // 优化合并后树的高度在O(log_n)内
```

![[并查集优化合并.png]]

### 并查集的应用

#### 判断是否有环

1. 初始化并查集
2. 遍历所有边，对于边$(u,v)$：Find u所在集合的Root u， Find v所在集合的Root v
3. 对两个顶点判断是否属于同⼀集合：如果Root u = Root v，加⼊$(u,v)$会形成环，如果Root u $≠$ Root v，使⽤Union合并两个集合。

#### Kruskal

1. 初始化并查集
2. 将途中所有边按权重从⼩到⼤排序
3. 遍历所有边，对于边$(u,v)$：Find u所在集合的Root u， Find v所在集合的Root v
4. 对两个顶点判断是否属于同⼀集合：如果Root u = Root v，加⼊$(u,v)$会形成环，不满足MST定义，就跳过。如果Root u $≠$ Root v，把边添加到MST，使⽤Union合并两个集合。
5. 如果MST的边数达到 $\vert v\vert-1$ 则停止，如果边不够但没有更多的边可选说明图不联通，生成不了MST。

#### 判断无向图的连通性

1. 初始化并查集
2. 遍历所有边，对于边$(u,v)$：Find u所在集合的Root u， Find v所在集合的Root v
3. 对两个顶点判断是否属于同⼀集合：如果Root u = Root v什么都不做，如果Root u $≠$ Root v，使⽤Union合并两个集合，u和v已经联通
4. 遍历图中所有顶点，使⽤Find找到其根，如果所有顶点所在集合的根相同，则图连通。
