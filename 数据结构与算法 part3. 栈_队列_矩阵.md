
## 栈

只允许在一端进行插入删除操作的线性表，操作特性为**后进先出(LIFO)**

```tikz
\usetikzlibrary{arrows.meta}

\begin{document}
\begin{tikzpicture}[
	scale=1.3,
	transform shape,
	line width=1pt,
    box/.style={
        draw, minimum width=2.5cm, minimum height=0.8cm
    },
    >=Stealth
]

% 栈元素
%% \node[box] (a5) at (0,0)   {}; %%
\node[box] (a4) at (0,-0.8) {$a_4$};
\node[box] (a3) at (0,-1.6) {$a_3$};
\node[box] (a2) at (0,-2.4) {$a_2$};
\node[box] (a1) at (0,-3.2) {$a_1$};

% 栈边界线
\draw (-1.25,0.4) -- (-1.25,-3.6);
\draw (1.25,0.4) -- (1.25,-3.6);

% 栈顶与栈底标签
\node[left] at (-1.4,0.2) {top};
\node[left] at (-1.4,-3) {bottom};

% 栈顶箭头
\draw[->] (-2.2,0) -- (-1.3,0);

% 栈底箭头
\draw[->] (-2.2,-3.2) -- (-1.3,-3.2);

% 出栈箭头（从顶部向左上弯）
\draw[->] (-0.5,-0.4) to[out=135,in=-85] (-1.2, 1) node[left] {out};

% 进栈箭头（从右上弯入顶部）
\draw[->] (0.8,1) to[out=-45,in=45] (0.3,-0.45) node[right, yshift=-3pt] {in};
\end{tikzpicture}
\end{document}
```

- 栈顶：允许插入删除操作的一端
- 栈底：固定，不允许插入删除操作
- 空栈：不含任何元素的空表

### 卡特兰数

#考点

**定死一个入栈序列的时候可以求得的出栈序列的数量**。

$$
C_{n}=\dfrac{1}{n+1}C_{2n}^n
$$

$n$ 个**不同元素**入栈（已确定序列）时，出栈元素不同排列的个数为 $C_{n}$ 
先序序列（已确定）含 $n$ 个不同元素时，能够确定 $C_{n}$ 种二叉树：对于一个栈，入栈对应了先序序列，出栈对应了中序序列

\[2015\] 先序序列为a, b, c, d的不同二叉树个数是 $\dfrac{1}{5}C_{8}^4$
类似的，先序序列不定死仅有4个元素，一共有 $A_{4}^4\times \dfrac{1}{5}C_{8}^4$ 种二叉树，$\dfrac{1}{5}C_{8}^4$ 种树形
### 顺序栈

用一组**地址连续的存储单元**存放自栈底到栈顶的数据元素
栈顶指针 `top` 指示栈顶元素的位置

```cpp
#define MAX 50
typedef struct {
	int data[MAX];
	int top; // 栈顶指针
} SqStack;

int main() {
	SqStack S;
}
```

#### 初始化

栈顶指针 `top` 初始化为 -1

```cpp
void InitStack (SqStack &S) {
	S.top = -1;
}
```

#### 判空

```cpp
bool StackEmpty(SqStack S) {
	if (S.top == -1) {
		return true;
	}
	else {
		return false;
	}
}
```

#### 进栈

栈不满时 `top` 先加 1 再入栈

```cpp
bool Push(SqStack &S, int x) {
	if (S.top == MAX - 1) {
		return false;
	} // 满了
	S.data[++S.top] = x;
	return true;
}
```

#### 出栈

先出栈，`top`再减 1

```cpp
bool Pop(SqStack &S, int &x) {
	if (S.top == -1) {
		return false;
	}
	x = S.data[S.top--];
	return true;
}
```

#### 获取栈顶元素

```cpp
bool GetTop(SqStack &S, int &x) {
	if (S.top == -1) {
		return false;
	}
	x = S.data[S.top];
	return true;
}
```

若栈顶指针初始化为 `S.top = 0` 即指向栈顶元素的下一位置，入栈为 `S.data[S.top++] = x`，出栈为 `x = S.data[--S.top]`，栈空栈满条件也会变化

### 共享栈

利用栈底位置相对不变的特性，让两个顺序栈共享一个一维数组空间，将两个栈的栈底设置在共享空间的两端，栈顶向中间延伸

```tikz
\usetikzlibrary{arrows.meta}

\begin{document}
\begin{tikzpicture}[
	scale=1.3,
	transform shape,
	line width=1pt,
    box/.style={
        draw, minimum width=4cm, minimum height=0.8cm
    },
    >=Stealth
]

\node[box] (A) at (0,0)   {};
\node[box] (B) at (3,0) {};

\draw[->] (-2,-1.5) -- (-2,-0.5);
\draw[->] (1,-1.5) -- (1,-0.5);
\draw[->] (2,-1.5) -- (2,-0.5);
\draw[->] (5,-1.5) -- (5,-0.5);

\draw[->] (-0.5,0.125) -- (1,0.125);
\draw[->] (3,0.125) -- (2,0.125);

\node[left] at (-1.5,-1.75) {Stack 0 bottom};
\node[left] at (1,-1.75) {Stack 0 top};
\node[left] at (3.5,-1.75) {Stack 1 top};
\node[left] at (7,-1.75) {Stack 1 bottom};
\node[left] at (-1.75,0.8) {0};
\node[left] at (6,0.8) {MAX - 1};
\end{tikzpicture}
\end{document}
```
<center>共享存储空间</center>

- 两个栈的栈顶指针都指向栈顶元素，`top0 == -1` 时栈 0 为空， `top1 == MAX - 1` 时栈 1 为空
- 栈顶指针相邻 `top1 - top0 == 1` 时栈满
- 栈 0 进栈时 `top0` 先加 1 再赋值，栈 1 进栈时 `top1` 先减 1 再赋值，出栈则相反

共享栈可以更有效的**利用存储空间**，整个存储空间被占满才发生上溢，存取数据复杂度为 $O(1)$

### 链栈

便于将多个栈共享存储空间和提高效率，不存在栈满上溢的情况
采用没有头结点的单链表实现，所有操作在单链表的表头进行

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
   
   % label
   \node[left] at (0.35,0.5) {top};
   \node[left] at (5,0.5) {bottom};

   \draw[-{Stealth[length=2mm, width=1mm]}] (-1.5,0) -- node[midway, above left] {{Lhead}} (B);
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
typedef struct Linknode {
	int data;
	struct Linknode *next;
} LiStack;
```

操作与链表相似，带头结点和不带头结点的链栈实现不同

## 队列

只允许在队头(*front*) 进行插入，队尾(*rear*)进行删除的线性表，**先进先出(FIFO)**
空队列是不含任何元素的空表

```tikz
\usetikzlibrary{arrows.meta}

\begin{document}
\begin{tikzpicture}[
	scale=1.3,
	transform shape,
    box/.style={
        draw, minimum width=2.5cm, minimum height=0.8cm
    },
    line width=1pt,
    >=Stealth
]

\draw (-3,0.4) -- (2.5,0.4);
\draw (-3,-0.4) -- (2.5,-0.4);

\node[left] at (1, 0) {$a_1, a_2, a_3, a_4, a_5$};

\draw[->] (-3,-1.2) -- (-3,-0.5);
\draw[->] (2,-1.2) -- (2,-0.5);
\node[left] at (-3, -1) {front};
\node[left] at (2, -1) {rear};

\draw[->] (-2.5, 0) -- (-4, 0);
\draw[->] (4.5, 0) -- (2, 0);
\node[left] at (-3, 0.4) {pop};
\node[left] at (3.8, 0.4) {push};

\end{tikzpicture}
\end{document}

```

### 顺序队列

分配一块连续的存储单元存放队列中的元素，设置头指针 `front` 和尾指针 `rear`

```cpp
#define MAX 50
typedef struct {
	int data[MAX];
	int front, rear;
} SqQueue;
```

#### 初始化

队头、队尾指针指向0，队尾指针是最后一个元素的下标+1

```cpp
void InitQueue(SqQueue &Q){
	Q.rear = Q.front = 0;
}
```

#### 入队

将 `x` 插入队尾，尾指针+1

```cpp
bool EnQueue(SqQueue $Q, ElemType x){
	if(队列已满) {
		return false;
	}
	Q.data[Q.rear] = x;		//将x插入队尾
	Q.rear = Q.rear + 1;	//队尾指针后移
	return true;
}
```
#### 判空

```cpp
bool QueueEmpty(SqQueue 0){
	if(Q.rear == Q.front)
		return true;
	else 
		return false;
}
```

### 链式队列

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
   
   \node[list,on chain] (B) {};
   
   % label
   \node[left] at (0.5,0.5) {rear};
   \draw[->] (-0.7,0.7) -- (-0.2,0.25);

   \draw[-{Stealth[length=2mm, width=1mm]}] (-1.5,0) -- node[midway, above left] {{front}} (B);
   \draw (B.north east) -- (B.one split south);
   \draw (B.south east) -- (B.one split north);

   % the to path below extends the bounding box a lot, this fixes that
   \useasboundingbox ([shift={(-1.3cm,0)}]B.north west) rectangle ([shift={(1cm,-7mm)}]B.south east);

   
\end{tikzpicture}
\end{document}
```

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
  line width=1pt,
  start chain
]
   
   \node[list,on chain] (B) {};
   \node[list,on chain] (C) {$a_1$};
   \node[list,on chain] (D) {$a_2$};
   
   % label
   \node[left] at (4,1) {rear};
   \draw[->] (3.2,0.7) -- (3.7,0.25);

   \draw[-{Stealth[length=2mm, width=1mm]}] (-1.5,0) -- node[midway, above left] {{front}} (B);
   % \draw (D.north east) -- (D.one split south);
   % \draw (D.south east) -- (D.one split north);
   \draw[dotarrow] (B.two |- B.center)  -- (C);
   \draw[dotarrow] (C.two |- C.center)  -- (D);

   % the to path below extends the bounding box a lot, this fixes that
   \useasboundingbox ([shift={(-1.3cm,0)}]B.north west) rectangle ([shift={(1cm,-7mm)}]D.south east);
   %% \draw[dotarrow] (D.two |- D.center) to[out=-10,in=190,distance=6cm] (A); %%
   
\end{tikzpicture}
\end{document}
```

### 双端队列

两端都可以进行插入和删除操作的线性表

```tikz
\usetikzlibrary{arrows.meta}

\begin{document}
\begin{tikzpicture}[
	scale=1.3,
	transform shape,
    box/.style={
        draw, minimum width=2.5cm, minimum height=0.8cm
    },
    >=Stealth
]

\draw (-3,0.4) -- (2.5,0.4);
\draw (-3,-0.4) -- (2.5,-0.4);

\node[left] at (1, 0) {$a_1, a_2, a_3, a_4, a_5$};

\draw[->] (-3,-1.2) -- (-3,-0.5);
\draw[->] (2,-1.2) -- (2,-0.5);
\node[left] at (-3, -1) {front};
\node[left] at (2, -1) {rear};

\draw[->] (-2, 0) -- (-4, 0);
\draw[->] (-4, -0.2) -- (-2, -0.2);


\draw[->] (2, 0) -- (4.5, 0);
\draw[->] (4.5, -0.2) -- (2, -0.2);
\node[left] at (-3, 0.4) {pop};
\node[left] at (3.5, 0.4) {pop};
\node[left] at (-3, -0.4) {push};
\node[left] at (3.5, -0.4) {push};

\end{tikzpicture}
\end{document}

```

 入队时前端进的元素排列在后端进的元素的前面，出队时，无论前端还是后端出队，先出的元素排列在后出的元素的前面
 
- **输入受限**的双端队列：允许一端插入，两端删除的线性表
- **输出受限**的双端队列：允许两端插入，一端删除的线性表

### 假溢出和循环队列

#考点 主要围绕指针的设置，判断队空队满等

**假溢出**：顺序队列头指针和尾指针指向同一个地方会发生上溢出，但是 `data[MAX - 1]` 仍然有数据，也就是假溢出，为了解决假溢出的问题引入循环队列。

循环队列把存储队列的表从逻辑上视为一个环。

```tikz
\begin{document}
	\begin{tikzpicture}[scale=1.8,line width=1pt]
	\tikzstyle{every node}=[font=\huge,rectangle,minimum width=1cm, minimum height=1cm]
		\node[draw=black] at (1,0) (1){$q_{5}$};
		\node[draw=black] at (2,0) (2){$q_{6}$};
		\node[draw=black,dashed] at (3,0) (3){};
		\node[draw=black] at (4,0) (4){$q_{1}$};
		\node[draw=black] at (5,0) (5){$q_{2}$};
		\node[draw=black] at (6,0) (6){$q_{3}$};
		\node[draw=black] at (7,0) (7){$q_{4}$};
		
		\draw[-,line width=1pt] (0.5,0.6) -- (7.5,0.6);
		\draw[-,line width=1pt] (0.5,-0.6) -- (7.5,-0.6);
		
		\draw[-latex] (1) -- (0,0) -- (0,1) -- (8,1) -- (8,0) -- (7);
		\node at(4,-1) (front){front};
		\node at(3,-1) (rear){rear};
		\draw[->] (front) -- (4);
		\draw[->] (rear) -- (3);
	\end{tikzpicture}
\end{document}
```

```tikz
\begin{document}
	\begin{tikzpicture}[scale=1.8,line width=1pt]
	\tikzstyle{every node}=[font=\huge,rectangle,minimum width=1cm, minimum height=1cm]
		\node[draw=black] at (1,0) (1){$q_{5}$};
		\node[draw=black] at (2,0) (2){$q_{6}$};
		\node[draw=black,dashed] at (3,0) (3){};
		\node[draw=black] at (4,0) (4){$q_{1}$};
		\node[draw=black] at (5,0) (5){$q_{2}$};
		\node[draw=black] at (6,0) (6){$q_{3}$};
		\node[draw=black] at (7,0) (7){$q_{4}$};
		
		\draw[-,line width=1pt] (0.5,0.6) -- (7.5,0.6);
		\draw[-,line width=1pt] (0.5,-0.6) -- (7.5,-0.6);
		
		\draw[-latex] (1) -- (0,0) -- (0,1) -- (8,1) -- (8,0) -- (7);
		\node at(4,-1) (front){front};
		\node at(2,-1) (rear){rear};
		\draw[->] (front) -- (4);
		\draw[->] (rear) -- (2);
	\end{tikzpicture}
\end{document}
```

队空和队满的条件一样

|     | 尾指针指向队尾元素的下个位置                                                                        | 尾指针指向队尾元素                                                                                             |
| --- | ------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------- |
| 队空  | `rear == front`                                                                       | `(rear + 1) % MAXSIZE == front`                                                                       |
| 队满  | `rear == front`                                                                       | `(rear + 1) % MAXSIZE == front`                                                                       |
| 入队  | `Q.data[rear] = x;`<br>`rear = (rear + 1) % MAXSIZE;`                                 | `rear = (rear + 1) % MAXSIZE;`<br>`Q.data[rear] = x;`                                                 |
| 出队  | `x = Q.data[front];`<br>`front = (front + 1) % MAXSIZE;`                              | `x = Q.data[front];`<br>`front = (front + 1) % MAXSIZE;`                                              |
| 长度  | 左边：`(rear - 0)` <br>右边：`(MAXSIZE - front)`<br>长度：`(rear + MAXSIZE - front) % MAXSIZE` | 左边：`(rear - 0 + 1)` <br>右边：`(MAXSIZE - 1 - front + 1)`<br>长度：`(rear + 1 + MAXSIZE - front) % MAXSIZE` |

#### 区别队空队满

1. 牺牲一个单元不放元素
	- 队满：`(Q.rear + 1) % MaxSize == Q.front`
	- 队空：`Q.front == Q.rear`
2. 设置 `tag`，删除成功后 `tag = 0`，插入成功后 `tag = 1`
	- 队满：`Q.front == Q.rear && tag == 1`（只有插入操作，才可能导致队满）
	- 队空：`Q.front == Q.rear && tag == 0`（只有删除操作，才可能导致队空）
3. 设置 `size` 表示队列中元素个数
	- 队满：`size == MaxSize`
	- 队空：`size == 0`

\[2019\]请设计⼀个队列，要求满⾜：1. 初始时队列为空；2. ⼊队时，允许增加队列占⽤空间；3.出队后，出队元素所占⽤的空间可重复使⽤，即整个队列所占⽤的空间只增不减；4. ⼊队操作和出队操作的时间复杂度始终保持为O(1)。请回答：

1）该队列是应选择链式存储结构，还是应选择顺序存储结构？
2）画出队列的初始状态，并给出判断队空和队满的条件。
3）画出第⼀个元素⼊队后的队列状态。
4）给出⼊队操作和出队操作的基本过程。

## 栈的应用

#考点 选择题

### 括号匹配

每出现一种左括号，就把他压入栈，等待它对应的右括号。如果此时出现了新的左括号，则视为最高优先级，压入栈并等待新的出现的右括号。遇到了当前等待的右括号就出栈，成对的消除。**最后出现的左括号最先被匹配对应栈后进先出的特性。**

### 表达式求值

主要掌握手算的方法：高优先级，从左往右

表达式扫描结束但操作符栈非空时依次弹出操作符栈中的符号并计算

![Image of Screenshot 2026-07-04 161940](https://beeimg.com/images/t26247608114.png)

### 中缀表达式转后缀表达式

常考，例如 `A+B*(C-D)-E/F` 转为 `ABCD-*+EF/-`

- 扫描顺序：从左到右
- 遇到操作数：直接写
- 遇到运算符：依次弹出栈中大于等于当前运算符优先级的运算符，碰到 `(` 或栈空就停止，入栈当前扫描到的运算符
- 遇到 `()`：遇到 `(` 入栈，遇到 `)` 弹出栈中 `(` 后入栈的所有运算符 

### 递归

函数调用栈 / 递归工作栈：函数调用时存储调用信息，每进入一层递归，就将递归调用所需信息压入栈顶，每退出一层递归，就从栈顶弹出相应信息。太多层递归可能回导致栈溢出。

- 调用返回地址：返回到上一层递归的该地址处，继续执行此层递归在该地址之后的代码
- 实参：递归返回值
- 局部变量：递归所需参数

## 队列的应用

#考点 

利用队列辅助实现算法：二叉树层次遍历、BFS、BFS求单源最短路

队列的常⽤场景：

1. 要求公平调度：就绪队列、阻塞队列、各种请求队列等
2. 需要排队处理数据的缓冲区
3. 多级反馈队列调度算法

## 数组存储

### 行优先存储

```tikz
\begin{document}
	\begin{tikzpicture}[scale=2, line width=1.5pt,
	every node/.style={draw=black, rectangle, very thick, font =\huge, rectangle, minimum width=6em, minimum height=3em}, 
	]
		\foreach \i in {0,1}{
			\foreach \j in {0,1,2,3}{
			\ifnum\i=1
				\node at(\i*8+\j*2,0){b[\i][\j]};
			\else
				\node at(\i*8+\j*2,0){b[\i][\j]};
			\fi
			}
		}
	\end{tikzpicture}
\end{document}
```

### 列优先存储

```tikz
\begin{document}
	\begin{tikzpicture}[scale=2, line width=1.5pt,
	every node/.style={draw=black, rectangle, very thick, font =\huge, rectangle, minimum width=6em, minimum height=3em}, 
	]
		%\idx=1
		\foreach \i in {0,1}{
			\foreach \j in {0,1,2,3}{
				\ifnum\i=1
					\node at(\i*2+\j*4,0) {b[\i][\j]};
				\else
					\node at(\i*2+\j*4,0) {b[\i][\j]};
				\fi
			}
		}
	\end{tikzpicture}
\end{document}
```


## 特殊矩阵存储

#考点 考频较高，涉及有难度的计算比如求一个元素压缩存储到一维数组后的下标，傻逼王道给的什么公式我背不下来。

- 下标从0开始：`A[i][j]` **前**有几个元素，对应数组下标就是几
- 下标从1开始：`A[i][j]` **是**第几个元素，对应数组下标就是几

`A[i][j]` 行列号可以从0开始也可以从1开始。

### 稀疏矩阵

非零元素的个数相对于矩阵元素的个数来说非常少，以三元组（顺序存储）和十字链表（链式存储）存储。

![Image of Screenshot 2026-07-04 183149](https://beeimg.com/images/q49303136033.png)

### 对称矩阵

存储时按行/列优先只存储上三角+对角线或下三角+对角线即 `A[i][j] = A[j][i]`

![Image of Screenshot 2026-07-04 175346](https://beeimg.com/images/w53190547623.png)

### 三角矩阵

上/下三角矩阵：只存储三角区域元素以及一个常数

![Image of Screenshot 2026-07-04 175253](https://beeimg.com/images/w80182601233.png)


三对角矩阵：按照行/列优先存储三条对角线上的元素

![Image of Screenshot 2026-07-04 175617](https://beeimg.com/images/k95656191074.png)

\[2020\] 将⼀个$10\times10$阶对称矩阵$M$的上三⻆部分的元素$m_{i,j}(1≤i≤j≤10)$按列优先存⼊C语⾔的⼀维数组$N$中，求元素$m_{7,2}$在$N$中的下标($22$)

\[2021\] ⼆维数组$A$按⾏优先⽅式存储，每个元素占⽤1个存储单元。若元素`A[0][0]`的存储地址
是100，`A[3][3]`的存储地址是220，则元素`A[5][5]`的存储地址是($39\times5+6+100-1=300$)

