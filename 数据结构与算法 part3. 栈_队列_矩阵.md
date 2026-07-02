
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
- 数学性质：$n$ 个不同元素进栈时，出栈元素不同排列的个数为 $\frac{1}{n+1}C_{2n}^n$
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

### 链式存储 - 链栈

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

>[!bug] 假溢出
>顺序队列头指针和尾指针指向同一个地方会发生上溢出，但是 `data[MAX - 1]` 仍然有数据，也就是假溢出 
### 循环队列

 把存储队列的表从逻辑上视为一个环

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
- 初始：`Q.front = Q.rear = 0`;
- 队首指针进1：`Q.front = (Q.front + 1) % MaxSize`
- 队尾指针进1：`Q.rear = (Q.rear + 1) % MaxSize` —— 队尾指针后移，当移到最后一个后，下次移动会到第一个位置

#### 判满 / 判空

1. 牺牲一个存储单元来判断队空和队满，入队时少用一个队列单元
   队满：`(Q.rear + 1) % MaxSize == Q.front`
   队空：`Q.front == Q.rear`
   队列元素个数：`(Q.rear + MaxSize - Q.front) % MaxSize`
2. 不牺牲存储空间，定义一个变量 `size`用于记录队列此时记录了几个数据元素，初始化 `size = 0`，进队成功 `size++`，出队成功`size--`，根据 `size` 的值判断队满与队空
   队满：`size == MaxSize`
   队空：`size == 0`
 
  ```cpp 
define MaxSize 10;	 
typedef struct{
	ElemType data[MaxSize];   
	int front, rear;		
	int size;	//队列当前长度
}SqQueue;

//初始化队列
void InitQueue(SqQueue &Q){
	Q.rear = Q.front = 0;
	size = 0;
}
  ```
   
3. 不牺牲存储空间，定义一个变量 `tag`用来表示最近进行的操作
   每次删除操作成功时，都令`tag = 0`
   每次插入操作成功时，都令`tag = 1`
   队满条件：`Q.front == Q.rear && tag == 1`（只有插入操作，才可能导致队满）
   队空条件：`Q.front == Q.rear && tag == 0`（只有删除操作，才可能导致队空）
   
```cpp
define MaxSize 10;	 
typedef struct{
	ElemType data[MaxSize];   
	int front, rear;		
	int tag;	//最近进行的是删除or插入
}SqQueue;
```

#### 入队

只能从队尾插入

```cpp
bool EnQueue(SqQueue &Q, ElemType x){
	if ((Q.rear+1) % MaxSize == Q.front)	//队满
		return false;
	Q.data[Q.rear] = x;	//将x插入队尾
	Q.rear = (Q.rear + 1) % MaxSize;	//队尾指针加1取模
	return true;
}
```

#### 出队

删除一个队头元素，用 `x` 返回

```cpp
bool DeQueue(SqQueue &Q, ElemType &x){
	if(Q.rear == Q.front)	//队空报错
		return false;  
	x = Q.data[Q.front];
	Q.front = (Q.front + 1) % MaxSize;	//队头指针后移动
	return true;
}
```
#### 获取队头元素

```cpp
bool GetHead(SqQueue &Q, ElemType &x){
	if(Q.rear == Q.front)	//队空报错
		return false;  
	x = Q.data[Q.front];
	return true;
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
<center>带头结点的链式队列</center>

#### 初始化 / 判空

```cpp
typedef struct LinkNode{//链式队列结点
	ElemType data;
	struct LinkNode *next;
} LinkNode;

typedef struct{//链式队列
	LinkNode * front, *rear;//队列的队头和队尾指针
} LinkQueue;

//初始化队列（带头结点）
void InitQueue(LinkQueue &Q){
	//初始化时，front、rear都指向头结点
	Q.front = Q.rear = (LinkNode*)malloc(sizeof(LinkNode));
	Q.front -> next = NULL;
}

//判断队列是否为空
bool IsEmpty(LinkQueue Q){
	if(Q.front == Q.rear)	//也可用 Q.front -> next == NULL
		return true;
	else
		return false;
}
```

#### 入队

建立新结点，将新结点插入到链表的尾部，让 `Q.rear` 指向新插入的结点

```cpp
void EnQueue(LinkQueue &Q, ElemType x){
	LinkNode *s = (LinkNode *)malloc(sizeof(LinkNode));	//申请一个新结点
	s->data = x;
	s->next = NULL;		//s作为最后一个结点，指针域指向NULL
	Q.rear->next = s;	//新结点插入到当前的rear之后
	Q.rear = s;			//表尾指针指向新的表尾
}
```
#### 出队

若队列非空，则取出队首，将它从链表删除，让 `Q.front` 指向下个结点

```cpp
bool DeQueue(LinkQueue &Q, ElemType &x){
	if(Q.front == Q.rear)
		return false;	//空队
	
	LinkNode *p = Q.front->next;	//p指针指向即将删除的结点 (头结点所指向的结点)
	x = p->data;
	Q.front->next = p->next;	//修改头结点的next指针
					//如果p是最后一个结点的话，p->next为NULL，所以这个关系也是正确的
	if(Q.rear == p)	//此次是最后一个结点出队
		Q.rear = Q.front;	//修改rear指针，释放后队列变为空队列
	free(p);	//释放结点空间
	
	return true;
}
```
## 双端队列

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

## 栈和队列的应用
### 栈的应用1 - 括号匹配

用栈实现括号匹配：`( ( ( ) ) )` 最后出现的左括号最先被匹配（栈的特性—LIFO）

- 初始时栈为空，从左往右遍历算术表达式中的各个括号
    - 当遍历到左括号时，将其压入栈顶
    - 当遍历到右括号时，将栈顶的一个左括号弹出，并判断出栈的左括号是否与当前遍历到的右括号匹配
        - 如果栈空而没有左括号弹出，则匹配失败
        - 如果匹配，则继续遍历下一个括号
        - 如果不匹配，则匹配失败
    - 当遍历过所有的括号后
        - 若栈空，则匹配成功
        - 若干非空，则匹配失败

```cpp
#define MaxSize 10   

typedef struct{
	char data[MaxSize];
	int top;
} SqStack;

//考试时可直接使用基本接口，但最好简要说明接口

void InitStack(SqStack &S)
bool StackEmpty(SqStack &S)
bool Push(SqStack &S, char x)
bool Pop(SqStack &S, char &x)


bool bracketCheck(char str[], int length){
	SqStack S;
	InitStack(S);
	for(int i=0; i<length; i++){
		if(str[i] == '(' || str[i] == '[' || str[i] == '{'){
			Push(S, str[i]);	//扫描到左括号，入栈
		}
		else{
			if(StackEmpty(S))	//扫描到右括号，且当前栈空
				return false;	//匹配失败
			
			char topElem;		//存储栈顶元素
			Pop(S, topElem);	//栈顶元素出栈
				
			if(str[i] == ')' && topElem != '(' )
				return false;
			
			if(str[i] == ']' && topElem != '[' )
				return false;
			
			if(str[i] == '}' && topElem != '{' )
				return false;	   
		}
	}
	return StackEmpty(S);	//栈空说明匹配成功
}
```

### 栈的应用2- 表达式求值
#### 中缀表达式转后缀表达式

中缀表达式中必须用括号来指示运算次序，后缀表达式没有括号，运算符在两个运算数后面，用一个栈保存暂时还不能确定运算顺序的运算符

- 从左到右处理各个元素，直到末尾
    - 遇到数：直接加入后缀表达式
    - 遇到界限符：遇到 `'('` 直接入栈；遇到 `')'` 则依次弹出栈内运算符并加入后缀表达式，直到弹出 `'('` 为止。注意：`'('` 不加入后缀表达式； ^[提示：优先计算括号内的内容]
    - 遇到运算符：依次弹出栈中优先级高于或等于当前运算符的所有运算符 ^[提示：$×$ ÷ 高于$+$ −]，并加入后缀表达式，若碰到 `'('` 或栈空则停止。之后再把当前运算符入栈。 ^[提示：根据左优先原则，先执行左侧高优先级或同等优先级的运算]
- 按上述方法处理完所有字符后，将栈中剩余运算符依次弹出，并加入后缀表达式

>[!example]
>`A+B*(C-D)-E/F` 转为 `ABCD-*+EF/-`
#### 后缀表达式求值

栈用于存放当前暂时不能确定运算次序的操作数

- 从左往右扫描下一个元素，直到处理完所有元素
    - 若扫描到操作数，则压入栈；
    - 若扫描到运算符，则弹出两个栈顶元素，执行相应的运算，运算结果压回栈顶
- 若表达式合法，则最后栈中只会留下一个元素，就是最终结果

>[!attention]
先出栈的是“右操作数”（先出栈的原本在栈的右边/上面）
### 栈的应用3 - 递归和函数调用的工作原理

递归可以把原始问题转换为属性相同，但规模较小的问题（求阶乘、斐波那契数列）

函数调用时，需要用一个栈存储：
- 调用返回地址：返回到上一层递归的该地址处，继续执行此层递归在该地址之后的代码
- 实参：递归返回值
- 局部变量：递归所需参数
递归调用时，函数调用栈称为 “**递归工作栈**”：
- 每进入一层递归，就将递归调用所需信息压入栈顶
- 每退出一层递归，就从栈顶弹出相应信息
缺点：效率低，太多层递归可能回导致栈溢出；可能包含很多次重复计算
### 队列的应用1 - 树的层次遍历

### 队列的应用2 - 图的BFS

### 队列的应用3 - 操作系统

- CPU资源分配，先来先服务(FCFS) 中采用就绪进程队列
- 打印数据缓冲区，SPOOLing技术中采用缓冲区队列

---
## 数组和特殊矩阵
如何用最小的内存空间来存储同样的数据

### 数组的定义
$n$ 个相同类型元素构成的**有限**序列，是线性表的推广，一旦被定义维数和维界就不再改变，只有初始化，存取，修改元素，销毁的操作

### 数组的存储结构
#### 一维数组

```cpp
ElemType a[100];
```

- 各数组元素大小相同，物理上连续存放
- 起始地址：`LOC`
- 数组下标：默认从0开始
- 数组元素 `a[i]` 的存放地址 = `LOC + i × sizeof(ElemType)`
#### 二维数组

```tikz
\begin{document}
	\begin{tikzpicture}[scale=2, line width=1.5pt,
	every node/.style={draw=black, rectangle, very thick, font =\huge, rectangle, minimum width=6em, minimum height=3em}, 
	]
		\foreach \i in {0,1}{
			\foreach \j in {0,1,2,3}{
				\ifnum\i=1
					\node at(\j*2,-\i) {b[\i][\j]};
				\else
					\node at(\j*2,-\i) {b[\i][\j]};
				\fi
			}
		}
	\end{tikzpicture}
\end{document}
```

```cpp
#define M 4
#define N 2
ElemType b[M][N];
```

- 起始地址：`LOC`
- $M$ 行 $N$ 列的二维数组 `b[M][N]` 中，`b[i][j]`的存储地址：
    - 行优先存储：`LOC + (i * N + j) * sizeof(ElemType)`
    - 列优先存储：`LOC + (j * M + i) * sizeof(ElemType)`
- 行优先 / 列优先存储优点：实现随机存储

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
<center>行优先存储</center>

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

<center>列优先存储</center>

>[!attention] 
>`A[n][n]` 等价于 `A[0...n-1][0...n-1]`

### 特殊矩阵的压缩存储
压缩存储：为多个值相同的元素只分配一个存储空间，对零元素不分配空间
#### 对称矩阵

$n$ 阶对称矩阵 $\mathbf{A}$ 中对任意元素都有 $a_{ij}=a_{ji}$

```tikz
\begin{document}
\def\myMatrix{{
  {0,5,3,3,4},
  {5,0,2,6,7},
  {3,2,0,8,9},
  {3,6,8,0,6},
  {4,7,9,6,0}
}}
\begin{tikzpicture}[scale=1, transform shape, ultra thick,
  every node/.style={font=\huge, draw=black, very thick, rectangle, minimum width=1cm, minimum height=1cm},
  ]
  \def\rowfirst{0};
  \foreach \row in {0,1,2,3,4} {
  	\foreach \col in {0,1,2,3,4} {
  		\pgfmathtruncatemacro{\value}{\myMatrix[\row][\col]};
  			\ifnum\row=\col
  				\node[fill=blue!50] at (\col, -\row) {\value};
  				\node[fill=blue!50] at (\rowfirst+8, -1) {\value};
  				\xdef\rowfirst{\rowfirst+1};
  			\else
  				\ifnum\row>\col
  					\node[fill=red!50] at (\col, -\row) {\value};
  					\node[fill=red!50] at (\rowfirst+8, -1) {\value};
  					\xdef\rowfirst{\rowfirst+1};
  				\else
  					\node at (\col, -\row) {\value};
  				\fi
  			\fi
  	}
  }
  \def\colfirst{0};
  \foreach \col in {0,1,2,3,4} {
  	\foreach \row in {0,1,2,3,4} {
  		\pgfmathtruncatemacro{\value}{\myMatrix[\row][\col]};
  			\ifnum\row=\col
  				\node[fill=blue!50] at (\colfirst+8, -3) {\value};
  				\xdef\colfirst{\colfirst+1};
  			\else
  				\ifnum\row>\col
  					\node[fill=red!50] at (\colfirst+8, -3) {\value};
  					\xdef\colfirst{\colfirst+1};
  				\else
  				\fi
  			\fi
  	}
  }
  % 序号
  \foreach \i in{1,2,3,4,5}{
  	\node[draw=none, font=\Large] at (\i-1, 1) {\i};
  	\node[draw=none, font=\Large] at (-1, -\i+1) {\i};
  }
  \foreach \i in{0,1,...,14}{
  	\node[draw=none, font=\Large] at(\i+8, -2){\i};
  	\node[draw=none, font=\Large] at(\i+8, -4){\i};
  }
\end{tikzpicture}
\end{document}
```


只存储主对角线和下（上）三角区的数据
使用一维数组 `B[n(n+1)/2]` 存储各个元素

行优先存储主对角线和下三角区数据时

$$
\begin{equation} 
k_{ij}=\left\{ \begin{array}{**lr**} [1+2+⋯+(i−1)]+j−1=\dfrac{i(i-1)}{2}+j-1 & i\geq j\\  
k_{ji}& i<j\end{array} \right. 
\end{equation}
$$

列优先存储主对角线和下三角区数据时

$$
\begin{equation} 
k_{ij}=\left\{ \begin{array}{**lr**} [n+(n−1)+⋯+(n−j+2)]+i−1=\dfrac{(2n−j+2)(j−1)}{2}+i−1 & i\geq j\\  
k_{ji}& i<j\end{array} \right. 
\end{equation}
$$

#### 三角矩阵

上(下)三角区的所有元素都为同一常量

```tikz
\begin{document}
\def\myMatrix{{
  {1,0,0,0,0},
  {5,1,0,0,0},
  {3,2,1,0,0},
  {3,6,8,1,0},
  {4,7,9,6,1}
}}
\begin{tikzpicture}[scale=1, ultra thick,
  every node/.style={font=\huge, draw=black, very thick, rectangle, minimum width=1cm, minimum height=1cm},
  ]
  \def\rowfirst{0};
  \foreach \row in {0,1,2,3,4} {
  	\foreach \col in {0,1,2,3,4} {
  		\pgfmathtruncatemacro{\value}{\myMatrix[\row][\col]};
  			\ifnum\row=\col
  				\node[fill=blue!50] at (\col, -\row) {\value};
  				\node[fill=blue!50] at (\rowfirst+8, -1) {\value};
  				\xdef\rowfirst{\rowfirst+1};
  			\else
  				\ifnum\row>\col
  					\node[fill=red!50] at (\col, -\row) {\value};
  					\node[fill=red!50] at (\rowfirst+8, -1) {\value};
  					\xdef\rowfirst{\rowfirst+1};
  				\else
  					\node at (\col, -\row) {\value};
  				\fi
  			\fi
  	}
  }
  \def\colfirst{0};
  \foreach \col in {0,1,2,3,4} {
  	\foreach \row in {0,1,2,3,4} {
  		\pgfmathtruncatemacro{\value}{\myMatrix[\row][\col]};
  			\ifnum\row=\col
  				\node[fill=blue!50] at (\colfirst+8, -3) {\value};
  				\xdef\colfirst{\colfirst+1};
  			\else
  				\ifnum\row>\col
  					\node[fill=red!50] at (\colfirst+8, -3) {\value};
  					\xdef\colfirst{\colfirst+1};
  				\else
  				\fi
  			\fi
  	}
  }
  % 存储常量c
  \node at(23,-1) {0};
  \node at(23,-3) {0};
  % 序号
  \foreach \i in{1,2,3,4,5}{
  	\node[draw=none, font=\Large] at (\i-1, 1) {\i};
  	\node[draw=none, font=\Large] at (-1, -\i+1) {\i};
  }
  \foreach \i in{0,1,...,15}{
  	\node[draw=none, font=\Large] at(\i+8, -2){\i};
  	\node[draw=none, font=\Large] at(\i+8, -4){\i};
  }
\end{tikzpicture}
\end{document}
```

只存储主对角线，元素一次，常量一次

行优先存储主对角线和上三角区数据时

$$
\begin{equation} 
k_{ij}=\left\{ \begin{array}{**lr**} n+(n-1)+\dots+(n-i+2)+(j-i+1)-1=\dfrac{(i-1)(2n-i+2)}{2}+(j-i) & i\geq j\\  
\dfrac{n(n+1)}{2}& i<j\end{array} \right. 
\end{equation}
$$
#### 三对角矩阵

所有非零元素都集中在以主对角线为中心的三条对角线，其他都为零
当 $|i−j|>1$ 时，有 $a_{ij}=0$，即只需存储主对角线以及上下左右相邻元素第一行和最后一行两个元素，其余三个元素，共 $3n−2$ 个元素
#### 稀疏矩阵

非零元素的个数相对于矩阵元素的个数来说非常少

- 顺序存储——三元组`<行，列，值>`
- 链式存储——十字链表法
    - 向下域 `down`：指针指向第 j 列的各个元素
    - 向右域 `right`：指针指向第 i 行的各个元素

---

## 考点分类

### 栈和队列的操作

### 卡特兰数

==完全不会==





