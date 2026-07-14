
## 图的概念与性质

#考点 推断计算

### 顶点之间的关系

1. 路径：顶点 $v_{p}$ 到顶点 $v_{q}$ 之间的一条路径是 $v_{p},v_{i_{1}},v_{i_{2}},\dots,v_{i_{m}},v_{q}$，边的数目为路径长度
2. **简单路径：在路径序列中，顶点不重复出现的路径**
   **简单回路：除第一个顶点和最后一个顶点外，其余顶点不重复出现的回路**
   **回路不是简单路径**
3. 路径长度：路径上边的数目
4. 点 $u$ 到点 $v$ 的距离：最短路径. 若不存在路径则为 $\infty$
5. 连通：无向图中若顶点 $v$ 到 $w$ 有路径存在则是连通的
   连通图：任意两个点都是连通的图
   对于 $n$ 个顶点的无向连通图 $G$ 最少有 $n−1$ 条边
   若 $G$ 是非连通图，则最多可能有 $C_{n-1}^2$ 条边
6. 强联通：有向图中若一对顶点 $v$ 和 $w$，$v\to w$ 和 $w\to v$ 都有路径
   强连通图：任意一对顶点都是强连通的图，若有 $n$ 个顶点则最少有 $n$ 条边

### 图的常见概念

1. 图 $G=(V,E)$ 和 $G'=(V',E')$，如果$V’⊆V$，$E’⊆E$，则称$G$’为$G$的⼦图。
2. 完全图：**每一对相异的顶点都有且只有一条边相连**。
   ⽆向完全图有$\dfrac{n(n-1)}{2}$条边，有向完全图有$n(n-1)$条弧。完全图⼀定是连通/强连通的，并且完全图的边数是给定顶点数量时最多的。
3. 连通分量：⽆向图中的极⼤连通⼦图
   强连通分量：有向图中的极⼤强连通⼦图，极⼤表示边数达到顶点能承受的最⼤数量，即原图中和某顶点相关的边都需要出现。
4. 生成树：⽆向连通图中⼀个包含图中所有顶点的极⼩连通⼦图
	- 极⼩表示保证连通的前提下边数要达到最⼩
	- 极⼩连通⼦图本身不要求必须包含图中的所有顶点
	- 最⼩⽣成树是所有⽣成树中**边的权值之和最⼩**的⽣成树
5. 对于⽆向图来说，最少需要$n-1$条边才能连通，边数⼤于$n-1$时必有环，对于有向图来说，最少需要$n$条弧才能保证强连通（形成⼀个环）
6. ⽆向图中所有顶点的度之和等于边数的$2$倍
   有向图所有顶点的⼊度之和=出度之和=弧数
7. $n$个顶点的⽆向图最少有$1$个连通分量，最多有$n$个连通分量。最少情况下$n$个顶点两两之间都有路径，所以⽆向图本身就是⾃⼰唯⼀的连通分量，最多情况下不存在边，每个顶点独⽴并⾃成⼀个连通分量。
8. 保证联通：⽆论边怎么换位置，图都是连通的。
   可以实现连通：边在某些位置中可以构造出连通图，但如果换位置可能就⽆法确保依旧连通。
   让$n$个顶点的无向图实现连通至少要$n-1$条边，保证连通至少需要$\dfrac{(n-1)(n-2)}{2}+1$条边。

若$G$为含有$10$条边的⾮连通⽆向图，那么图$G$⾄少有多少个顶点？
考虑完全图的情况，$10=\dfrac{4\times5}{2}$即有$5$个顶点。如果要非连通⽆向图再加一个孤立顶点即可，至少$6$个顶点。

具有35个顶点和10条边的⽆向图的连通分量最多是？
考虑35个顶点的⽆向图最多有35个连通分量，此时每个点都孤立。但是给出条件有10条边，令10条边组成完全无向图即耗费最少的顶点，组成的这个图$10=\dfrac{4\times5}{2}$即有$5$个顶点，剩下的30个孤立顶点组成30个连通分量，加上完全无向图一共31个连通分量。

## 邻接矩阵和邻接表

#考点 

|           | 邻接表                                                                    | 邻接矩阵                |
| --------- | ---------------------------------------------------------------------- | ------------------- |
| 空间复杂度     | 无向图：$O(\vert V\vert+2\vert E\vert)$，有向图：$O(\vert V\vert+\vert E\vert)$ | $O(\vert V\vert^2)$ |
| 适用场景      | 存储稀疏图                                                                  | 存储稠密图               |
| 表示方式      | 不唯一                                                                    | 唯一                  |
| 计算度、入度、出度 | 计算有向图的度、入度不方便，其余很方便                                                    | 遍历对应行或列             |
| 找相邻的边     | 找有向图的入边不方便，其余很方便                                                       | 遍历对应行或列             |

### 邻接矩阵

$$
\boldsymbol{A}[i][j]=\left\{\begin{array}{ll}
1 \;\; \text{i和j之间连通}\\
0 \;\; \text{i和j之间不连通}\\
\end{array}\right.
$$

```tikz
\usetikzlibrary{positioning}
\usepackage{array}
\begin{document}
	\begin{tikzpicture}[scale=1, ultra thick,
	every node/.style={draw=black, circle, very thick,font =\huge, minimum size=3em}, 
	arrow/.style={-},
	]
	\node [below] at(0,-1) (A) {A};
	\node [below=of A] (C) {C};
	\node [left=of C] (B) {B};
	\node [right=of C] (D) {D};
	\node [below left=of C, xshift=2em, yshift=-1em] (E) {E};
	\node [below right=of C, xshift=-2em, yshift=-1em] (F) {F};
	
	\draw[arrow] (A)--(B);
	\draw[arrow] (C)--(A);
	\draw[arrow] (D)--(A);
	\draw[arrow] (E)--(B);
	\draw[arrow] (E)--(C);
	\draw[arrow] (F)--(B);
	\draw[arrow] (F)--(D);
	
	\matrix[ampersand replacement=\&][below, rectangle, draw=none] at(10,0) {
		\& \node(species2) [rectangle, draw=none] {
		\newcolumntype{M}[1]{>{\centering\arraybackslash}m{#1}}
		\setlength{\extrarowheight}{1em}
		\begin{tabular}{M{1em}M{1em}M{1em}M{1em}M{1em}M{1em}} 
			A & B & C & D & E & F \\ 
		\end{tabular}
		};\\
		\node(species3) [shape=rectangle, draw=none] {
			\begin{tabular}{c}
			A \\
			B \\
			C \\
			D \\
			E \\
			F \\ 
			\end{tabular}
		}; \& 
		\node(species4) [shape=rectangle, draw=none] {
		\newcolumntype{M}[1]{>{\centering\arraybackslash}m{#1}} 
		\begin{tabular}{|M{1em}|M{1em}|M{1em}|M{1em}|M{1em}|M{1em}|} 
			\hline
			0 & 1 & 1 & 1 & 0 & 0 \\ \hline
			1 & 0 & 0 & 0 & 1 & 1 \\ \hline
			1 & 0 & 0 & 0 & 1 & 0 \\ \hline
			1 & 0 & 0 & 0 & 0 & 1 \\ \hline
			0 & 1 & 1 & 0 & 0 & 0 \\ \hline
			0 & 1 & 0 & 1 & 0 & 0 \\ \hline
		\end{tabular}
		};\\
	};
	\end{tikzpicture}
\end{document}
```

---

$$
\boldsymbol{A}[i][j]=\left\{\begin{array}{ll}
1 \;\; \text{边(i,j)为该顶点的出度}\\
0 \;\; \text{i和j之间不连通}\\
\end{array}\right.
$$

```tikz
\usetikzlibrary{positioning}
\usepackage{array}
\begin{document}
	\begin{tikzpicture}[scale=1, ultra thick,
	every node/.style={draw=black, circle, very thick,font =\huge, minimum size=3em}, 
	arrow/.style={-latex},
	]
	\node [below] at(0,-1) (A) {A};
	\node [below=of A] (C) {C};
	\node [left=of C] (B) {B};
	\node [right=of C] (D) {D};
	\node [below left=of C, xshift=2em, yshift=-1em] (E) {E};
	\node [below right=of C, xshift=-2em, yshift=-1em] (F) {F};
	
	\draw[arrow] (A)--(B);
	\draw[arrow] (C)--(A);
	\draw[arrow] (D)--(A);
	\draw[arrow] (E)--(B);
	\draw[arrow] (E)--(C);
	\draw[arrow] (F)--(B);
	\draw[arrow] (F)--(D);
	
	\matrix[ampersand replacement=\&][below, rectangle, draw=none] at(10,0) {
		\& \node(species2) [rectangle, draw=none] {
		\newcolumntype{M}[1]{>{\centering\arraybackslash}m{#1}}
		\setlength{\extrarowheight}{1em}
		\begin{tabular}{M{1em}M{1em}M{1em}M{1em}M{1em}M{1em}} 
			A & B & C & D & E & F \\ 
		\end{tabular}
		};\\
		\node(species3) [shape=rectangle, draw=none] {
			\begin{tabular}{c}
			A \\
			B \\
			C \\
			D \\
			E \\
			F \\ 
			\end{tabular}
		}; \& 
		\node(species4) [shape=rectangle, draw=none] {
		\newcolumntype{M}[1]{>{\centering\arraybackslash}m{#1}} 
		\begin{tabular}{|M{1em}|M{1em}|M{1em}|M{1em}|M{1em}|M{1em}|} 
			\hline
			0 & 1 & 0 & 0 & 0 & 0 \\ \hline
			0 & 0 & 0 & 0 & 0 & 0 \\ \hline
			1 & 0 & 0 & 0 & 0 & 0 \\ \hline
			1 & 0 & 0 & 0 & 0 & 0 \\ \hline
			0 & 1 & 1 & 0 & 0 & 0 \\ \hline
			0 & 1 & 0 & 1 & 0 & 0 \\ \hline
		\end{tabular}
		};\\
	};
	\end{tikzpicture}
\end{document}
```

---

$$
\boldsymbol{A}[i][j]=\left\{\begin{array}{ll}
w_{ij} \;\; \text{i和j之间连通，等于无向带权图中边的权值}\\
\infty \;\; \text{i和j之间不连通}\\
\end{array}\right.
$$

```tikz
\usetikzlibrary{positioning}
\usepackage{array}
\begin{document}
	\begin{tikzpicture}[scale=1, ultra thick,
	every node/.style={draw=black, circle, very thick,font =\huge, minimum size=3em}, 
	arrow/.style={-},
	]
	\node [below] at(0,-1) (A) {A};
	\node [below=of A] (C) {C};
	\node [left=of C] (B) {B};
	\node [right=of C] (D) {D};
	\node [below left=of C, xshift=2em, yshift=-1em] (E) {E};
	\node [below right=of C, xshift=-2em, yshift=-1em] (F) {F};
	
	\draw[arrow] (A)--(B)node[midway, above, font=\large, draw=none]{5};
	\draw[arrow] (C)--(A)node[midway, right, font=\large, draw=none]{1};
	\draw[arrow] (D)--(A)node[midway, above, font=\large, draw=none]{6};
	\draw[arrow] (E)--(B)node[midway, below, font=\large, draw=none]{3};
	\draw[arrow] (E)--(C)node[midway, above, font=\large, draw=none]{3};
	\draw[arrow] (F)--(B)node[midway, below, font=\large, draw=none]{4};
	\draw[arrow] (F)--(D)node[midway, above, font=\large, draw=none]{2};
	
	\matrix[ampersand replacement=\&][below, rectangle, draw=none] at(10,0) {
		\& \node(species2) [rectangle, draw=none] {
		\newcolumntype{M}[1]{>{\centering\arraybackslash}m{#1}}
		\setlength{\extrarowheight}{1em}
		\begin{tabular}{M{1em}M{1em}M{1em}M{1em}M{1em}M{1em}} 
			A & B & C & D & E & F \\ 
		\end{tabular}
		};\\
		\node(species3) [shape=rectangle, draw=none] {
			\begin{tabular}{c}
			A \\
			B \\
			C \\
			D \\
			E \\
			F \\ 
			\end{tabular}
		}; \& 
		\node(species4) [shape=rectangle, draw=none] {
		\newcolumntype{M}[1]{>{\centering\arraybackslash}m{#1}} 
		\begin{tabular}{|M{1em}|M{1em}|M{1em}|M{1em}|M{1em}|M{1em}|} 
			\hline
			0 & 5 & 1 & 6 & $\infty$ & $\infty$ \\ \hline
			5 & 0 & $\infty$ & $\infty$ & 3 & 4 \\ \hline
			1 & $\infty$ & 0 & $\infty$ & 3 & $\infty$ \\ \hline
			6 & $\infty$ & $\infty$ & 0 & $\infty$ & 2 \\ \hline
			$\infty$ & 3 & 3 & $\infty$ & 0 & $\infty$ \\ \hline
			$\infty$ & 4 & $\infty$ & 2 & $\infty$ & 0 \\ \hline
		\end{tabular}
		};\\
	};
	\end{tikzpicture}
\end{document}
```

---

$$
\boldsymbol{A}[i][j]=\left\{\begin{array}{ll}
w_{ij} \;\; \text{i和j之间连通，等于有向带权图中为出度的边的权值}\\
\infty \;\; \text{i和j之间不连通}\\
\end{array}\right.
$$

```tikz
\usetikzlibrary{positioning}
\usepackage{array}
\begin{document}
	\begin{tikzpicture}[scale=1, ultra thick,
	every node/.style={draw=black, circle, very thick,font =\huge, minimum size=3em}, 
	arrow/.style={-latex},
	]
	\node [below] at(0,-1) (A) {A};
	\node [below=of A] (C) {C};
	\node [left=of C] (B) {B};
	\node [right=of C] (D) {D};
	\node [below left=of C, xshift=2em, yshift=-1em] (E) {E};
	\node [below right=of C, xshift=-2em, yshift=-1em] (F) {F};
	
	\draw[arrow] (A)--(B)node[midway, above, font=\large, draw=none]{5};
	\draw[arrow] (C)--(A)node[midway, right, font=\large, draw=none]{1};
	\draw[arrow] (D)--(A)node[midway, above, font=\large, draw=none]{6};
	\draw[arrow] (E)--(B)node[midway, below, font=\large, draw=none]{3};
	\draw[arrow] (E)--(C)node[midway, above, font=\large, draw=none]{3};
	\draw[arrow] (F)--(B)node[midway, below, font=\large, draw=none]{4};
	\draw[arrow] (F)--(D)node[midway, above, font=\large, draw=none]{2};
	
	\matrix[ampersand replacement=\&][below, rectangle, draw=none] at(10,0) {
		\& \node(species2) [rectangle, draw=none] {
		\newcolumntype{M}[1]{>{\centering\arraybackslash}m{#1}}
		\setlength{\extrarowheight}{1em}
		\begin{tabular}{M{1em}M{1em}M{1em}M{1em}M{1em}M{1em}} 
			A & B & C & D & E & F \\ 
		\end{tabular}
		};\\
		\node(species3) [shape=rectangle, draw=none] {
			\begin{tabular}{c}
			A \\
			B \\
			C \\
			D \\
			E \\
			F \\ 
			\end{tabular}
		}; \& 
		\node(species4) [shape=rectangle, draw=none] {
		\newcolumntype{M}[1]{>{\centering\arraybackslash}m{#1}} 
		\begin{tabular}{|M{1em}|M{1em}|M{1em}|M{1em}|M{1em}|M{1em}|} 
			\hline
			0 & 5 & $\infty$ & $\infty$ & $\infty$ & $\infty$ \\ \hline
			$\infty$ & 0 & $\infty$ & $\infty$ & $\infty$ & $\infty$ \\ \hline
			1 & $\infty$ & 0 & $\infty$ & $\infty$ & $\infty$ \\ \hline
			6 & $\infty$ & $\infty$ & 0 & $\infty$ & $\infty$ \\ \hline
			$\infty$ & 3 & 3 & $\infty$ & 0 & $\infty$ \\ \hline
			$\infty$ & 4 & $\infty$ & 2 & $\infty$ & 0 \\ \hline
		\end{tabular}
		};\\
	};
	\end{tikzpicture}
\end{document}
```

---

### 邻接表

```tikz
\usetikzlibrary{positioning}
\newcommand{\drawEdges}[3]{
	\pgfmathsetmacro{\idx}{#1}
	\def\edges{#2}% 边
	\def\values{#3}% 边权值
	\foreach \edge [count=\i] in \edges {
		\ifnum\edge=-1
			\node[draw=none, font=\LARGE] at(\i*4, -\idx) (temp){NULL};
		\else
			\node[fill=gray!20, minimum width=1cm, minimum height=2em] at(\i*4, -\idx) (temp){\edge};
			\node[minimum width=1cm, minimum height=2em] at(\i*4+2, -\idx) (\idx-\i){};
		\fi
		\pgfmathtruncatemacro{\pre}{\i-1}
		\draw[-latex] (\idx-\pre.center) -- (temp);
	}
	\foreach \value[count=\i] in \values{
		\node[minimum width=1cm, minimum height=2em] at(\i*4+1, -\idx) {\value};
	}
}

\begin{document}
	\begin{tikzpicture}[scale=1, transform shape,ultra thick,
	every node/.style={draw=black, circle, very thick,font =\huge, minimum size=3em}, 
	arrow/.style={-},
	]
	\node [below] at(0,-1) (A) {A};
	\node [below=of A] (C) {C};
	\node [left=of C] (B) {B};
	\node [right=of C] (D) {D};
	\node [below left=of C, xshift=2em, yshift=-1em] (E) {E};
	\node [below right=of C, xshift=-2em, yshift=-1em] (F) {F};
	
	\draw[arrow] (A)--(B)node[midway, above, font=\large, draw=none]{5};
	\draw[arrow] (C)--(A)node[midway, right, font=\large, draw=none]{1};
	\draw[arrow] (D)--(A)node[midway, above, font=\large, draw=none]{6};
	\draw[arrow] (E)--(B)node[midway, below, font=\large, draw=none]{3};
	\draw[arrow] (E)--(C)node[midway, above, font=\large, draw=none]{3};
	\draw[arrow] (F)--(B)node[midway, below, font=\large, draw=none]{4};
	\draw[arrow] (F)--(D)node[midway, above, font=\large, draw=none]{2};
	\end{tikzpicture}
	\begin{tikzpicture}[scale=0.7, transform shape, ultra thick]
	\tikzstyle{every node}=[font=\huge, draw=black, very thick, rectangle, minimum width=1cm, minimum height=1cm]
		\node[draw=none, font=\large] at(0,1){data};
		\node[draw=none, font=\large] at(1,1){first};
		
		\def\myChar{A,B,C,D,E,F} 
		\foreach \char [count=\i] in \myChar {
			\pgfmathtruncatemacro{\idx}{\i-1};%整数结果
			\node at (0,-\idx) (node-\idx){\char};
			\node[font=\Large, draw=none, left of = node-\idx] {\idx};
			\node at (1,-\idx) [minimum width=1cm] (\idx-0){};
		}
		
		\drawEdges{0}{1,2,3,-1}{5,1,6};
		\drawEdges{1}{0,4,5,-1}{5,3,4};
		\drawEdges{2}{0,4,-1}{1,3};
		\drawEdges{3}{0,5,-1}{6,2};
		\drawEdges{4}{1,2,-1}{3,3};
		\drawEdges{5}{1,3,-1}{4,2};
	\end{tikzpicture}
\end{document}
```

```tikz
\usetikzlibrary{positioning}
\newcommand{\drawEdges}[3]{
	\pgfmathsetmacro{\idx}{#1}
	\def\edges{#2}% 边
	\def\values{#3}% 边权值
	\foreach \edge [count=\i] in \edges {
		\ifnum\edge=-1
			\node[draw=none, font=\LARGE] at(\i*4, -\idx) (temp){NULL};
		\else
			\node[fill=gray!20, minimum width=1cm, minimum height=2em] at(\i*4, -\idx) (temp){\edge};
			\node[minimum width=1cm, minimum height=2em] at(\i*4+2, -\idx) (\idx-\i){};
		\fi
		\pgfmathtruncatemacro{\pre}{\i-1}
		\draw[-latex] (\idx-\pre.center) -- (temp);
	}
	\foreach \value[count=\i] in \values{
		\node[minimum width=1cm, minimum height=2em] at(\i*4+1, -\idx) {\value};
	}
}

\begin{document}
	\begin{tikzpicture}[scale=1, transform shape, ultra thick,
	every node/.style={draw=black, circle, very thick,font =\huge, minimum size=3em}, 
	arrow/.style={-latex},
	]
	\node [below] at(0,-1) (A) {A};
	\node [below=of A] (C) {C};
	\node [left=of C] (B) {B};
	\node [right=of C] (D) {D};
	\node [below left=of C, xshift=2em, yshift=-1em] (E) {E};
	\node [below right=of C, xshift=-2em, yshift=-1em] (F) {F};
	
	\draw[arrow] (A)--(B)node[midway, above, font=\large, draw=none]{5};
	\draw[arrow] (C)--(A)node[midway, right, font=\large, draw=none]{1};
	\draw[arrow] (D)--(A)node[midway, above, font=\large, draw=none]{6};
	\draw[arrow] (E)--(B)node[midway, below, font=\large, draw=none]{3};
	\draw[arrow] (E)--(C)node[midway, above, font=\large, draw=none]{3};
	\draw[arrow] (F)--(B)node[midway, below, font=\large, draw=none]{4};
	\draw[arrow] (F)--(D)node[midway, above, font=\large, draw=none]{2};
	\end{tikzpicture}
	\begin{tikzpicture}[scale=0.7, transform shape, ultra thick]
	\tikzstyle{every node}=[font=\huge, draw=black, very thick, rectangle, minimum width=1cm, minimum height=1cm]
		\node[draw=none, font=\large] at(0,1){data};
		\node[draw=none, font=\large] at(1,1){first};
		
		\def\myChar{A,B,C,D,E,F} 
		\foreach \char [count=\i] in \myChar {
			\pgfmathtruncatemacro{\idx}{\i-1};%整数结果
			\node at (0,-\idx) (node-\idx){\char};
			\node[font=\Large, draw=none, left of = node-\idx] {\idx};
			\node at (1,-\idx) [minimum width=1cm] (\idx-0){};
		}
		
		\drawEdges{0}{1,-1}{5};
		\drawEdges{1}{-1}{};
		\drawEdges{2}{0,-1}{1};
		\drawEdges{3}{0,-1}{6};
		\drawEdges{4}{1,2,-1}{3,3};
		\drawEdges{5}{1,3,-1}{4,2};
	\end{tikzpicture}
\end{document}
```

### 边的计算

|     | 邻接矩阵                                             | 邻接表                                                                               |
| --- | ------------------------------------------------ | --------------------------------------------------------------------------------- |
| 无向图 | 顶点$v_{i}$的度=第$i$行元素之和=第$i$列元素之和                  | 顶点$v_{i}$的度=第$i$个边表中结点个数                                                          |
| 有向图 | 顶点$v_{i}$的出度=第$i$行元素之和<br>顶点$v_{i}$的入度=第$i$列元素之和 | 顶点$v_{i}$的出度=第$i$个边表中的结点个数<br>顶点$v_{i}$的⼊度需要遍历各顶点的边表才能得出，即顶点$v_{i}$的⼊度就是在边表中出现的次数 |

|     | 邻接矩阵                                     | 邻接表                               |
| --- | ---------------------------------------- | --------------------------------- |
| 无向图 | 对称，可以压缩存储上下三角即需要$\dfrac{n(n-1)}{2}$单元存储边 | $n$个顶点表结点 + $2e$个边表结点（边表结点⼀定是偶数个） |
| 有向图 | $n$个顶点的图需要$n^2$单元来存储边                    | $n$个顶点表结点 + $e$个边表结点              |

邻接矩阵中 $A^k[i][j]$ 表示从顶点$i$到顶点$j$之间⻓度为$k$的路径条数。

## 十字链表 / 邻接多重表

#考点 结构本身就是一个考点

## DFS / BFS

#考点 掌握思想和代码，代码要会写，应用是在原有的代码基础上改过来的

### 深度优先搜索

1. 从⼀个顶点开始，先标记访问并输出它，再去检查它的邻接点
2. 如果邻接点未被访问过，就对邻接点再次调⽤DFS
3. 重复上述操作，直到所有顶点都被标记访问

`DFSTraverse()` 中调⽤ `DFS()` 的次数 = 连通分量的个数（回顾连通分量的定义）

```cpp
bool visited[MVNum];
void DFSTraverse(Graph G) {
	for (v = 0; v < G.vexnum; ++v)
		visited[v] = false;
	for (v = 0; v < G.vexnum; ++v)
		if(!visited[v]) DFS(G,v);
}

void DFS(Graph G, int v) {
	cout << v;
	visited[v] = true;
	for (w = FirstAdjVex(G,v); w >= 0; w = NextAdjVex(G,v,w)) {
		if(!visited[w])
			DFS(G,w);
	}
}
```

```cpp
// 核心代码
for (w = FirstAdjVex(G,v); w >= 0; w = NextAdjVex(G,v,w)){
	if(!visited[w])
		DFS(G,w);
}
```

|       | 邻接矩阵                                                                                                                                                            | 邻接表                                                                                                                                         |
| ----- | --------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------- |
| 过程    | 1. 从给定的参数顶点 v 开始，⾸先遍历邻接矩阵中第 v ⾏的所有元素，如果第 w 列的值不为 0，说明顶点 v 与顶点 w 之间存在边<br><br>2. 若此时顶点 w 尚未被访问，则将 w 作为参数递归调⽤ DFS<br><br>3. 继续以同样⽅式访问 w 的所有邻接点，直到所有可达顶点都被访问为⽌<br> | 1. 从给定的参数顶点 v 开始先访问 v 所对应的邻接表，获取其第⼀个邻接点指针 p<br><br>2. 沿着邻接表的边表不断向后遍历，每次取出当前边所指向的邻接点 w，如果 w 尚未被访问，就将 w 作为参数递归调⽤ DFS<br><br>3. 直到所有可达顶点都被访问为⽌ |
| 时间复杂度 | $O(n^2)$                                                                                                                                                        | $O(n+e)$                                                                                                                                    |

### 广度优先搜索

`BFSTraverse()` 中调⽤ `BFS()` 的次数 = 连通分量的个数

1. 从⼀个顶点开始先标记访问并输出它，再将它加⼊队列
2. 然后不断从队列中取出队头顶点，检查该顶点的所有邻接点，如果某个邻接点未被访问过就标记访问、输出，并加⼊队列
3. 重复上述过程，直到队列为空为⽌

```cpp
bool visited[MVNum];
void BFSTraverse(Graph G) {
	for(v = 0; v < G.vexnum; ++v)
		visited[v] = false;
	for(v = 0; v < G.vexnum; ++v)
		if(!visited[v]) BFS(G,v);
}

void BFS(Graph G, int v) {
	cout << v;
	visited[v] = true;
	InitQueue(Q);
	EnQueue(Q,v);
	while(!QueueEmpty(Q)) {
		DeQueue(Q,u);
		for(w = FirstAdjVex(G,u); w >= 0; w = NextAdjVex(G,u,w))
		if(!visited[w]) {
			cout << w;
			visited[w] = true;
			EnQueue(Q,w);
		}
	}
}
```

|       | 邻接矩阵                                                                                                                                                                                                                                | 邻接表                                                                                                                                                                                                                                 |
| ----- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 过程    | 从给定的参数顶点 v 开始，⾸先输出顶点 v，并将其标记为已访问，将顶点 v ⼊队，当队列不为空时循环执⾏以下操作：<br><br>1. 从队列中取出队头顶点 u，遍历邻接矩阵第 u ⾏的所有元素，依次检查每个顶点 w<br><br>2. 如果 `G.arcs[u][w]` 不为 0，说明 u 与 w 之间存在边<br><br>3. 如果顶点 w 尚未被访问，则输出 w、标记访问，并将 w ⼊队，<br>继续检查 u ⾏中剩下的所有列，直到扫描完整⾏ | 从给定的参数顶点 v 开始，⾸先输出顶点 v，并将其标记为已访问，将顶点 v ⼊队<br>当队列不为空时循环执⾏以下操作：<br><br>1. 从队列中取出队头顶点 u，访问顶点 u 所对应的邻接表，获取第⼀个邻接点指针 p<br><br>2. 沿着邻接表不断向后遍历，每次取出当前边指向的邻接点 w<br><br>3. 如果 w 尚未被访问，则输出 w 并标记访问，并将 w ⼊队<br><br>4. 继续遍历邻接表的下⼀项，直到 p 为 `NULL` |
| 时间复杂度 | $O(n^2)$                                                                                                                                                                                                                            | $O(n+e)$                                                                                                                                                                                                                            |

### 应用

DFS：判断环路、拓扑排序
BFS：（**权值相同**的有权图/无权图）单源最短路径

DFS和BFS都可判断图是否连通。通过对代码进⾏修改，BFS实际上也可以⽤于判断环路或实现拓扑排序。

#### DFS判断有向图是否存在环

为了准确判断，通常需要为每个顶点维护三种状态：未访问、正在访问、已访问。

1. 从某个顶点出发进⾏DFS
2. 在遍历过程中，如果遇到某个邻接点处于正在访问的状态，说明该有向图存在环路

```cpp
bool hasCycle = false;
void DetectCycleDFS(Graph G, int v) {
	status[v] = Visiting; // 正在访问
	for(w = FirstAdjVex(G, v); w >= 0; w = NextAdjVex(G, v, w)){
		if(status[w] == Visiting) // 再次遇到正在访问的点 → 环路
			hasCycle = true;
		if(status[w] == Unvisited)
			DetectCycleDFS(G, w);
	}
	status[v] = Visited; // 已访问
}

void CheckGraphCycle(Graph G){
	for(v = 0; v < G.vexnum; v++) {
		if(status[v] == Unvisited)
			DetectCycleDFS(G, v);
	}
	if(hasCycle)
		cout << "图中存在回路";
	else
		cout << "图中⽆回路";
}
```

#### DFS判断无向图是否存在环

1. 从某个顶点出发进⾏DFS
2. 在遍历过程中，如果遇到某个邻接点处于正在访问的状态，并且这个正在访问的节点不是上⼀步访问的节点，说明该⽆向图存在环路
3. 为了准确判断，通常需要为每个顶点维护三种状态：未访问、正在访问、已访问

```cpp
bool hasCycle = false;
void DetectCycleDFS(Graph G, int v, int parent) {
	status[v] = Visiting;
	for(w = FirstAdjVex(G, v); w >= 0; w = NextAdjVex(G, v, w)) {
		if(status[w] == Visiting && w != parent)
			hasCycle = true;
		if(status[w] == Unvisited)
			DetectCycleDFS(G, w, v); // 传递当前节点作为w的⽗节点
	}
	status[v] = Visited;
}

void CheckGraphCycle(Graph G) {
	for(v = 0; v < G.vexnum; v++) {
		if(status[v] == Unvisited)
		DetectCycleDFS(G, v, -1);
		// 初始结点没有⽗节点
	}
	if(hasCycle)
		cout << "图中存在回路";
	else
		cout << "图中⽆回路";
}
```

#### DFS实现拓扑排序

1. 每个顶点v在DFS退出递归前都会记录当前时间戳`time`，并赋值给
`finishTime[v]`
2. 因为后继节点的DFS会先于v完成，因此它们的finishTime⼀定⽐v⼩
3. 最后按照finishTime从⼤到⼩排序所有顶点，就得到了⼀个合法的拓扑序列

#### BFS计算单源最短路

1. 从起点v出发，将其距离初始化为0，其余所有顶点初始化为∞，并将v⼊队作为搜索起点
2. 每次从队列中取出顶点u，访问它的所有未访问邻接点w，并将`d[w]`设为`d[u]+1`
3. 因为BFS⼀层层推进的，第⼀个访问到的路径⼀定最短，最终`d[]`数组记录的即为起点到每个顶点的最短路径⻓度

```cpp
void BFS_DISTANCE(Graph G, int u) {
	for(i = 0; i < G.vexnum; i++)
		d[i] = ∞;
	visited[u] = TRUE;
	d[u] = 0;
	EnQueue(Q, u);
	while(!isEmpty(Q)) {
		DeQueue(Q, u);
		for (w = FirstNeighbor(G, u); w >= 0; w = NextNeighbor(G, u, w)) {
			if(!visited[w]) {
				visited[w] = TRUE;
				d[w] = d[u] + 1;
				EnQueue(Q, w);
			}
		}
	}
}
```

## 拓扑排序

#考点 比较热门

### 有向无环图

描述$((a+b)(b(c+d))+(c+d)e)((c+d)*e)$

```tikz
\begin{document}
	\begin{tikzpicture}[scale=1.5, line width=1.5pt,
	every node/.style={draw=black, circle, very thick,font =\huge, minimum size=3em}, 
	level 1/.style={sibling distance=16em/1, level distance=3em},%横向距离与轴向距离
	level 2/.style={sibling distance=16em/2, level distance=3em},
	level 3/.style={sibling distance=16em/4, level distance=3em},
	level 4/.style={sibling distance=16em/8, level distance=3em},
	level 5/.style={sibling distance=16em/8, level distance=3em},
	]
	\node at(0,0) {$*$}
		child {node {$+$}
			child {node {$*$}
				child {node {$+$}
					child {node {$a$}}
					child {node {$b$}}
				}
				child {node {$*$}
					child {node {$b$}}
					child {node {$+$}
						child {node {$c$}}
						child {node {$d$}}
					}
				}
			}
	        child {node {$*$}
				child {node {$+$}
					child {node {$c$}}
					child {node {$d$}}
				}
				child {node {$e$}}
			}
		}
        child {node {$*$}
			child {node {$+$}
				child {node {$c$}}
				child {node {$d$}}
			}
			child {node {$e$}}
		};
	\end{tikzpicture}
\end{document}
```

```tikz
\begin{document}
\begin{tikzpicture}[scale=1.5, line width=1.5pt,
	every node/.style={draw=black, circle, very thick,font =\huge, minimum size=3em}, 
	level 1/.style={sibling distance=5em/1, level distance=3em},%横向距离与轴向距离
	level 2/.style={sibling distance=5em/2, level distance=3em},
	level 3/.style={sibling distance=5em/2, level distance=3em},
	level 4/.style={sibling distance=5em/2, level distance=3em},
	]
	\node at(8,0) (1) {$*$}
		child {node (2) {$+$}
			child {node (3) {$*$}
				child {node (5) {$+$}
					child {node (9) {$a$}}
					child {node (10) {$b$}}
				}
				child {node (6) {$*$}}
			}
		}
        child[level distance=6em] {node (4) {$*$}
			child[level distance=3em] {node (7) {$+$}
				child {node (11) {$c$}}
				child {node (12) {$d$}}
			}
			child[level distance=3em] {node (8) {$e$}}
		};
		\draw[-](2)--(4);
		\draw[-](6)--(7);
		\draw[-](6)--(10);
	\end{tikzpicture}
\end{document}
```

合并后顶点不存在重复的操作数

1. 把各个操作数不重复地排成一排
2. 标出各个运算符的生效顺序（先后顺序有点出入无所谓，主要为了不遗漏）
3. 按顺序加入运算符，注意“分层”
4. 从底向上逐层检查**同层的运算符**是否可以合并

### 拓扑排序

**拓扑排序**是对有向无环图的顶点的一种排序，它使得若存在一条从顶点A到顶点B的路径，则在排序中B出现在A的后面。

1. 在有向图中选择一个入度为0的顶点并输出它
2. 从图中删除该顶点和它的所有出度

重复这2步操作，直到不存在入度为0的顶点，若最终输出的顶点数小于有向图中的顶点数，则图中**存在环**。否则输出的顶点序列即为该图的一个拓扑序列。

1. 拓扑排序可能不唯一
2. 若有向图中有环，则该图不存在拓扑排序
3. 若有向图的拓扑有序序列唯一，则图中入度为0和出度为0的顶点都只有1个
4. 顶点数大于1的强连通图不能进行拓扑排序
5. 如果拓扑序列中A在B之前，原图中不一定有<A,B>
6. 有向无环图的拓扑序列唯一，并不能唯一确定该图

## 最小生成树

#考点 

生成树：无向连通图中一个包含图中所有顶点的极小连通子图
最小生成树：所有生成树中边的权值之和最小的生成树



