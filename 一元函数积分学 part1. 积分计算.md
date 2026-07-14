
## 一元函数积分

1. 原函数：对任意 $x\in I$ 有 $F'(x)=f(x)$ 则 $F(x)$ 为 $f(x)$ 在 $I$ 上的原函数(我导完等于你)
2. 定义：若 $F(x)$ 为 $f(x)$ 在 $I$ 上原函数则 $\displaystyle\int f(x)dx=F(x)+C$

### 性质

1. $\displaystyle \int(k_{1}f_{1}+k_{2}f_{2})dx=k_{1}\int f_{1}dx+k_{2}\int f_{2}dx$
2. $\displaystyle\int df(x)=\int f'(x)dx=f(x)+C$
3. $\displaystyle\left[ \int f(x)dx \right]'=f(x),\;d\left[ \int f(x)dx \right]=f(x)dx$

### 原函数存在定理

1. 连续函数存在原函数
2. 含有第一类间断点和无穷间断点的函数不存在原函数
3. 含有震荡间断点的函数可能存在原函数

## 基本积分公式

| 类型      | 积分公式                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| ------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 幂函数     | $\displaystyle \int x^adx=\frac{1}{a+1}x^{a+1}+C(a\neq-1)$<br>$\displaystyle\int \frac{dx}{x}=\ln\vert x\vert+C$                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| 指数函数    | $\displaystyle \int a^xdx=\frac{a^x}{\ln a}+C$<br>$\displaystyle\int e^xdx=e^x+C$                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| 对数函数    | $\displaystyle\int \ln xdx=x\ln x-x+C$                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| 三角函数    | $\displaystyle\int \sin x dx=-\cos x+C$<br>$\displaystyle\int \cos xdx=\sin x+C$<br>$\displaystyle\int \tan xdx=\int \frac{\sin x}{\cos x}dx=-\int \frac{1}{\cos x}d(\cos x)=-\ln\vert \cos x\vert+C$<br>$\displaystyle\int \cot xdx=\int \frac{\cos x}{\sin x}dx=\int \frac{1}{\sin x}d(\sin x)=\ln\vert \sin x\vert+C$<br>$\displaystyle\int \sec xdx= \frac{\sec x(\sec x+\tan x)}{\sec x+\tan x}dx=\int \frac{d(\sec x+\tan x)}{\sec x+\tan x}=\ln\vert \sec x+\tan x\vert +C$<br>$\displaystyle\int \csc xdx=\int \frac{\csc x(\csc x+\cot x)}{\csc x+\cot x}dx=-\int \frac{d(\csc x+\cot x)}{\csc x+\cot x}=-\ln\vert \csc x+\cot x\vert +C$ |
| 平方和差    | $\displaystyle \int \frac{1}{1+x^2}dx=\arctan x+C$<br>推广：$\displaystyle\int \frac{1}{a^2+x^2}dx=\frac{1}{a}\arctan \frac{x}{a}+C$<br>$\displaystyle\int \frac{1}{1-x^2}dx=\frac{1}{2}\int\left( \frac{1}{1+x} +\frac{1}{1-x}\right)dx=\frac{1}{2}\ln \vert \frac{1+x}{1-x}\vert+C$<br>推广：$\displaystyle\int \frac{1}{a^2-x^2}dx=\frac{1}{2a}\ln\vert \frac{a+x}{a-x}\vert+C$                                                                                                                                                                                                                                                                       |
| 根号下平方和差 | $\displaystyle\int \frac{1}{\sqrt{ 1-x^2 }}dx=\arcsin x+C$<br>推广：若 $a>0$ 则 $\displaystyle \int \frac{1}{\sqrt{ a^2-x^2 }}dx=\int \frac{1}{\sqrt{ 1-\left( \frac{x}{a} \right)^2 }}d\left( \frac{x}{a} \right)=\arcsin \frac{x}{a}+C$<br>$\displaystyle\int \frac{1}{\sqrt{ a^2+x^2 }}dx=\ln(x+\sqrt{ a^2+x^2 })+C$<br>$\displaystyle \int \frac{1}{\sqrt{ x^2-a^2 }}dx=\ln\vert x+\sqrt{ x^2-a^2 }\vert+C$                                                                                                                                                                                                                                         |

## 积分方法

### 凑微分

本质是复合求导公式

$\displaystyle\int f(u(x))u'(x)dx=\int f(u(x))d(u(x))=F(u(x))+C$

### 分部积分法

反对幂指三，靠后的往后积，本质是乘法求导公式

用于对反 / 对 / 幂 / 三角 / 指 或(指 / 三) 两类函数乘积，顺序靠后的凑微分看成 $v$，其余看作 $u$，$\displaystyle\int udv=uv-\int vdu$

### 列表法求分部积分结果

上面放多项式，下面放指数或三角函数，**上导下积，对角相乘，正负相间**。

### 换元法

设 $x=\varphi(t)$ 可导，且 $\varphi'(t)\neq0$，若 $\displaystyle \int f(\varphi(t))\varphi'(t)dt=F(t)+C$ 则 $\displaystyle\int f(x)dx=\int f(\varphi(t))\varphi'(t)dt=F(t)+C=F(\varphi^{-1}(x))+C$


|           | 方法                                                                                                                                                                     | 适用情况               |
| --------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------ |
| 三角代换      | 1. $\sqrt{ a^2-x^2 }令x=a\sin t$<br>2. $\sqrt{ a^2+x^2 }令x=a\tan t$<br>3. $\sqrt{ x^2-a^2 }令x=a\sec t$                                                                  | 根号下平方和差，换回来的时候画三角形 |
| 根式代换      | 1. $^n\sqrt{ ax+b }=t$<br>2. $\displaystyle ^n\sqrt{ \frac{ax+b}{cx+d} }=t$<br>3. $^n \sqrt{ ax+b }和^m \sqrt{ ax+b }$ 令 $^l\sqrt{ ax+b }=t$ 其中 $l$ 为 $m,n$ 的最小公倍数（不常用） | 根号下一次方             |
| 倒代换       | $\displaystyle x=\frac{1}{t}$                                                                                                                                          | 分母次数较高             |
| 整体代换      | 令复杂函数 $=t$                                                                                                                                                             | 一堆东西               |
| 万能代换（不常用） | 令 $\displaystyle \tan \frac{x}{2}=t$ 则 $\displaystyle x=2\tan t,\;\sin x=\frac{2t}{1+t^2},\cos x= \frac{1-t^2}{1+t^2}$                                                 | 三角有理式              |

## 化简方法

|       | 方法                                                                                                                                                                                                         | 适用情况     |
| ----- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------- |
| 拆项/裂项 | 分母有什么分子拆什么，拆完约分，分别对其积分                                                                                                                                                                                     | 分母可以因式分解 |
| 提项    | 1. 提高次 eg. $\displaystyle \frac{1}{x(x^7+2)}=\frac{1}{x^8(1+2x^{-7})}$<br>2. 提根号内一项<br>3. 提 $e^x$<br>4. 分子比分母高几次就提几次，然后拆项                                                                                  |          |
| 同乘    | 1. 多项式差几次乘几次<br>2. 平方差公式<br>3. 同乘 $e^x$                                                                                                                                                                    |          |
| 同除    | 1. 除最高的次数<br>2. 除 $e^x$<br>3. 二次四次除二次 eg. $\displaystyle \frac{1+x^2}{1+x^4}=\frac{\frac{1}{x^2}+1}{\frac{1}{x^2}+x^2}$                                                                                    |          |
| 配方    | 1. 二次多项式配方 eg.$\displaystyle \frac{1}{\sqrt{ x(4-x) }}=\frac{1}{\sqrt{ 4-(x-2)^2 }}$<br>2. 质因式分母配方 eg. $\displaystyle \frac{1}{x^2-x+1}=\frac{1}{\left( x-\frac{1}{2} \right)^2+(\frac{\sqrt{ 3 }}{2})^2}$ |          |

## 函数性质

1. 奇偶性：**对称区间内偶倍奇零**。
   $f(x)$ 在 $[-a,a]$ 上连续则 $\displaystyle \int_{-a}^af(x)dx=0(f(x)为奇函数),=2\int_{0}^af(x)dx\;(f(x)为偶函数)$，$f(x)=\dfrac{1}{2}[(f(x)+f(-x))+(f(x)-f(-x)])$ 偶+奇
2. 周期性：常用于三角函数，**从任何点开始积满一个周期都等于从0积满一个周期**。
   $f(x)$ 是周期为 $T$ 的连续函数则对任意 $a$ 有 $\displaystyle \int_{a}^{a+T}f(x)dx=\int_{0}^Tf(x)dx$

## 常见题型

### 有理函数

分母可因式分解：简单的可以直接因式分解了然后拆，复杂的待定系数
分母不可因式分解：先看能不能凑分母，分子凑出一个分母导数然后凑进去

### 带根号

待补充

### 三角函数



