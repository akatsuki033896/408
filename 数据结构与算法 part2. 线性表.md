
复习重点：算法设计大题

线性表是具有相同数据类型的 $n$ 个数据元素的**有限**序列，$n$ 为表长。线性表是一种逻辑结构，表示元素之间一对一的相邻关系，顺序表和链表是存储结构。
$$
L=(a_{1},a_{2}\dots a_{n})
$$
逻辑特性：除了表头以外仅有一个直接前驱，除表尾外仅有一个直接后继

|       | 顺序表           | 链表                     | 静态链表        |
| ----- | ------------- | ---------------------- | ----------- |
| 按位置查找 | $O(1)$        | $O(n)$ 遍历              | $O(n)$      |
| 按值查找  | $O(n)$        | $O(n)$ 遍历              | $O(n)$      |
| 插入    | $O(n)$ 大量移动元素 | $O(n)+O(1)$ 遍历找到+修改指针域 | $O(n)+O(1)$ |
| 删除    | $O(n)$ 大量移动元素 | $O(n)+O(1)$ 遍历找到+修改指针域 | $O(n)+O(1)$ |
| 存取方式  | 顺序存取 / 随机存取   | 表头开始顺序存取               | -           |
| 结构    | 逻辑和物理都相邻      | 逻辑相邻，物理不一定相邻           | -           |
## 单链表

- 结构：每个链表结点存有数据和 `next` 指针，指向结点的后继。
- 存储：不要求大片连续空间，改变容量方便，不可随机存取，要耗费空间存放指针。各个结点的存储空间可以不连续，但结点内存储单元地址必须连续(指针)。
- 非随机存取：单链表的元素离散地分布在存储空间，要遍历。**随机存取指的是可以直接用index找到特定的点**。

```tikz
\usetikzlibrary{
  chains,
  shapes.multipart,
  arrows.meta % supersedes the arrows library
}
\begin{document}
\begin{tikzpicture}[
  scale=1.8,
  transform shape,
  list/.style={
     rectangle split,
     rectangle split parts=2,
     draw,
     rectangle split horizontal
  },
  dotarrow/.style={Circle-Stealth},
  start chain
]
   
   \node[list,on chain] (B) {$a_1$};
   \node[list,on chain] (C) {$a_2$};
   %% \node[list,on chain] (D) {\phantom{xx}}; %%
   \node[list,on chain] (D) {$a_3$};
   


   \draw[-{Stealth[length=2mm, width=1mm]}] (-1.5,0) -- node[midway, above left] {{L}} (B);
   \draw (D.north east) -- (D.one split south);
   \draw (D.south east) -- (D.one split north);
   \draw[dotarrow] (B.two |- B.center)  -- (C);
   \draw[dotarrow] (C.two |- C.center)  -- (D);
   
   % the to path below extends the bounding box a lot, this fixes that
   \useasboundingbox ([shift={(-1.3cm,0)}]B.north west) rectangle ([shift={(1cm,-7mm)}]D.south east);
   %% \draw[dotarrow] (D.two |- D.center) to[out=-10,in=190,distance=6cm] (A); %%
   
\end{tikzpicture}
\end{document}
```

```cpp
typedef struct LNode {
	int data;
	struct LNode *next; // next指针
}LNode, *LinkList;

typedef struct LNode LNode;		//重命名
typedef struct LNode *LinkList;	//重命名
```

通常用头指针标识单链表指出起始地址，也就是指向第一个结点。为 `NULL` 时表示为空表。

### 初始化

```cpp
bool InitList(LinkList &L) {
	L = (LNode*)malloc(sizeof(LNode)); // 创建头结点
	L->next = NULL;
	return true;
}

// p为结构体指针时 p->data 等价于 (*p).data

bool InitList(LinkList &L) {
	L = NULL;
	return true;
}

```

### 求表长

```cpp
int Length(LinkList L) {
	int len = 0;
	LNode *p = L;
	while (p->next != NULL) {
		p = p->next;
		len++;
	}
	return len;
} // 遍历
```

### 查找

```cpp
// 按index查找
LNode *GetElem(LinkList L, int i) {
	LNode *p = L;
	int j = 0;
	while (p->next != NULL && j < i) {
		p = p->next;
		j++;
	}
	return p; // 找到就返回指针域
} // 遍历

// 按值查找
LNode *LocateElem(LinkList L, int e) {
	LNode *p = L->next;
	while (p->next != NULL && p.data != e) {
		p = p->next;
	}
	return p;
}
```

### 插入

将值为 `x` 的结点插入单链表的第 `i` 个位置，$O(n)$

```cpp
bool ListInsert(LinkList &L, int x, int i) {
	LNode *p = L;
	int j = 0;
	while (p != NULL && j < i - 1) {
		p = p->next;
		j++;
	} // 找到第 i - 1 个结点 O(n)
	if (p == NULL) {
		return false;
	}
	LNode *s = (LNode*)malloc(sizeof(LNode));
	s->data = x;
	s->next = p->next;
	p->next = s;
	// O(1)
	return true;
}
```

### 删除结点

删除第 `i` 个结点，修改 `p` 的 `next` 指针使他指向 被删除结点 `q` 的下个结点，$O(n)$

```cpp
bool ListDelete(LinkList &L, int i, int e) {
	LNode *p = L;
	int j = 0;
	while (p->next != NULL && j < i - 1) {
		p = p->next;
		j++;
	} // 找到第 i - 1 个结点
	if (p->next == NULL || j > i - 1) {
		return false;
	}
	LNode *q = p->next; // q就是第 i 个结点
	e = q->data; // 记下被删除的值
	p->next = q->next;
	free(q);
	return true;
}
```

### 头插法建立单链表

从一个空表开始生成新结点，将读取到的数据存放到新结点的数据域，然后将新结点插入到当前链表的表头，表长为 $n$ 时时间复杂度为 $O(n)$

**读入数据的顺序和链表中元素的顺序相反**，可以实现逆置

```cpp
LinkList List_HeadInsert(LinkList &L) {
	LNode *s;
	int x; // 元素为int
	L = (LNode*)malloc(sizeof(LNode)); // 创建头结点
	L->next = NULL;
	scanf("%d", &x);
	while (x != 9999) {
		s = (LNode*)malloc(sizeof(LNode));
		s->data = x; 
		s->next = L->next; // 插入
		L->next = s;
		scanf("%d", &x);
	} // 假设输入9999表示输入结束
	return L;
}
```

### 尾插法建立单链表

**读入数据的顺序和链表中元素的顺序相同**，表长为 $n$ 时时间复杂度为 $O(n)$

```cpp
LinkList List_TailInsert(LinkList &L) {
	int x; // 元素为int
	L = (LNode*)malloc(sizeof(LNode)); // 创建头结点
	LNode *s, *r = L; // r 为尾指针
	scanf("%d", &x);
	while (x != 9999) {
		s = (LNode*)malloc(sizeof(LNode));
		s->data = x;
		r->next = s;
		r = s;
		scanf("%d", &x);
	}
	r->next = NULL; // 尾指针为空
	return L;
}
```

## 双链表

单链表由于只有 `next` 指针导致只能从前往后遍历，访问前驱要重头开始遍历，复杂度为 $O(n)$ ，为了克服这个缺点，在结点前加 `prior` 指针访问前驱，构建双链表表头的 `prior` 和表尾的 `next` 为 `NULL`。

```tikz
\usetikzlibrary{
  chains,
  shapes.multipart,
  arrows.meta % supersedes the arrows library
}
\begin{document}
\begin{tikzpicture}[
  scale=1.8,
  transform shape,
  list/.style={
     rectangle split,
     rectangle split parts=3,
     draw,
     rectangle split horizontal
  },
  dotarrow/.style={Circle-Stealth},
  start chain
]
   
   \node[list,on chain] (A) {};
   \node[list,on chain] (B) {\nodepart{second} $a_1$};
   \node[list,on chain] (C) {\nodepart{second} $a_2$};
   \node[list,on chain] (D) {\nodepart{second} $a_3$};
   

   \draw (D.north east) -- (D.two split south);
   \draw (D.south east) -- (D.two split north);
   \draw (A.north west) -- (A.one split south);
   \draw (A.south west) -- (A.one split north);
   
   \draw[-{Stealth[length=2mm, width=1mm]}] (-1.5,0) -- node[midway, above left] {{L}} (A);
   \draw[dotarrow] (A.third |- A.center) -- (B);
   \draw[dotarrow] (B.third |- B.center)  -- (C);
   \draw[dotarrow] (C.third |- C.center)  -- (D);
   \draw[dotarrow] (B.one |- B.center) -- ++(0,-0.5) -- ++(-1.725,0) -- (A.south);
  \draw[dotarrow] (C.one |- C.center) -- ++(0,-0.5) -- ++(-1.825,0) -- (B.south);
  \draw[dotarrow] (D.one |- D.center) -- ++(0,-0.5) -- ++(-1.825,0) -- (C.south);
   
  
   % the to path below extends the bounding box a lot, this fixes that
   \useasboundingbox ([shift={(-1.3cm,0)}]A.north west) rectangle ([shift={(1cm,-7mm)}]D.south east);
   
   
\end{tikzpicture}
\end{document}
```

```cpp
typedef struct DNode {
	int data;
	struct DNode *prior, *next;
}DNode, *DLinkList;
```

### 插入

`prior` 便于找到前驱，$O(1)$ 先建立 `p` 的原后继结点和 新结点`s` 的连接，再建立 `p` 和 `s` 的连接

```cpp
s->next = p->next; // 1 将*s插入到*p后
p->next->prior = s;
s->prior = p;
p->next = s; // 4

// 1 必须在 4 之前否则*p的后继结点指针会丢掉
```

### 删除

`prior` 便于找到前驱，$O(1)$

```cpp
p->next = q->next;
q->next->prior = p;
free(q);
```

## 循环链表

对结构不熟，画不出图来

- 最后一个结点的指针指向头结点，链表形成环。
- 判空条件：最后一个结点尾指针等于头指针 `L` 则不为空
- 插入删除：操作若在表尾进行则和单链表不同
- **可以从任意结点开始遍历链表**。

```tikz
\usetikzlibrary{
  chains,
  shapes.multipart,
  arrows.meta % supersedes the arrows library
}
\begin{document}
\begin{tikzpicture}[
  scale=1.8,
  transform shape,
  list/.style={
     rectangle split,
     rectangle split parts=2,
     draw,
     rectangle split horizontal
  },
  dotarrow/.style={Circle-Stealth},
  start chain
]
   
   \node[list,on chain] (A) {head};
   \node[list,on chain] (B) {$a_1$};
   \node[list,on chain] (C) {$a_2$};
   %% \node[list,on chain] (D) {\phantom{xx}}; %%
   \node[list,on chain] (D) {$a_3$};

   \draw[-{Stealth[length=2mm, width=1mm]}] (-1.5,0) -- node[midway, above left] {{L}} (A);
   \draw[dotarrow] (A.two |- A.center) -- (B);
   \draw[dotarrow] (B.two |- B.center)  -- (C);
   \draw[dotarrow] (C.two |- C.center)  -- (D);
   \draw[dotarrow] (D.two |- D.center) -- ++(0,-0.5) -- ++(-6.4,0) -- (A.south);
   
   % the to path below extends the bounding box a lot, this fixes that
   \useasboundingbox ([shift={(-1.3cm,0)}]A.north west) rectangle ([shift={(1cm,-7mm)}]D.south east);
   
   
\end{tikzpicture}
\end{document}
```

## 循环双链表

判空条件：头结点的 `prior` 和 `next` 都等于 `L`

```tikz
\usetikzlibrary{
  chains,
  shapes.multipart,
  arrows.meta % supersedes the arrows library
}
\begin{document}
\begin{tikzpicture}[
  scale=1.8,
  transform shape,
  list/.style={
     rectangle split,
     rectangle split parts=3,
     draw,
     rectangle split horizontal
  },
  dotarrow/.style={Circle-Stealth},
  start chain
]
   
   \node[list,on chain] (A) {};
   \node[list,on chain] (B) {\nodepart{second} $a_1$};
   \node[list,on chain] (C) {\nodepart{second} $a_2$};
   \node[list,on chain] (D) {\nodepart{second} $a_3$};
   
   \draw[-{Stealth[length=2mm, width=1mm]}] (-1.5,0) -- node[midway, above left] {{L}} (A);
   \draw[dotarrow] (A.third |- A.center) -- (B);
   \draw[dotarrow] (B.third |- B.center)  -- (C);
   \draw[dotarrow] (C.third |- C.center)  -- (D);
   \draw[dotarrow] (B.one |- B.center) -- ++(0,-0.5) -- ++(-1.725,0) -- (A.south);
  \draw[dotarrow] (C.one |- C.center) -- ++(0,-0.5) -- ++(-1.825,0) -- (B.south);
  \draw[dotarrow] (D.one |- D.center) -- ++(0,-0.5) -- ++(-1.825,0) -- (C.south);

   \draw[dotarrow] (D.third |- D.center) -- ++(0,0.5) -- ++(-7.5,0) -- (A.north);
   \draw[dotarrow] (A.one |- A.center) -- ++(0,-0.75) -- ++(7.55,0) -- (D.south);
   % the to path below extends the bounding box a lot, this fixes that
   \useasboundingbox ([shift={(-1.3cm,0)}]A.north west) rectangle ([shift={(1cm,-7mm)}]D.south east);
   
\end{tikzpicture}
\end{document}
```

## 静态链表

```tikz
\usetikzlibrary{
  chains,
  shapes.multipart,
  arrows.meta % supersedes the arrows library
}
\begin{document}
\begin{tikzpicture}[
  scale=1.8,
  transform shape,
  list/.style={
     rectangle split,
     rectangle split parts=2,
     draw,
     rectangle split horizontal
  },
  dotarrow/.style={Circle-Stealth},
  start chain
]
   
   \node[list,on chain] (B) {$a_1$};
   \node[list,on chain] (C) {$a_2$};
   %% \node[list,on chain] (D) {\phantom{xx}}; %%
   \node[list,on chain] (D) {$a_3$};
   
   \draw (D.north east) -- (D.one split south);
   \draw (D.south east) -- (D.one split north);
   \draw[dotarrow] (B.two |- B.center)  -- (C);
   \draw[dotarrow] (C.two |- C.center)  -- (D);
   
   % the to path below extends the bounding box a lot, this fixes that
   \useasboundingbox ([shift={(-1.3cm,0)}]B.north west) rectangle ([shift={(1cm,-7mm)}]D.south east);
   %% \draw[dotarrow] (D.two |- D.center) to[out=-10,in=190,distance=6cm] (A); %%
   
\end{tikzpicture}
\end{document}
```
用数组来描述线性表的链式存储结构，也有数据域 `data` 和指针域 `next`，要预先分配一块连续内存空间，这里的指针是**结点在数组中的相对地址（数组下标）**。插入删除和动态链表相同。

```cpp
#define MAX 50
typedef struct {
	int data;
	int next; // next == -1 为结束
} SLinkList[MAX];
```

#考点 线性表基本操作 / 算法设计

\[2021\] 循环链表删除元素：对结构不熟，画不出图来

