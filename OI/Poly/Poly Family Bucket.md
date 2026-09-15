---
前置知识: "[[Determinant]]"
前置知识 2: "[[Mean Value Theorem]]"
前置知识 3: "[[Combinatorical Formulas]]"
---
# PART 1 —— 多项式基础知识
## Prework- $\text I$ -代数式
这部分或许不严谨。  
初中的定义是：是由数和表示数的字母经有限次加、减、乘、除、乘方和开方等代数运算所得的式子，或含有字母的数学表达式称为代数式。  
这可以理解为，不含对数，（反）三角函数的初等函数。  
某人曾经说过：万物皆映射。  
所以你可以简单地理解为代数式就是一种特殊的 $\mathbb{R}^n$ 到 $\mathbb{R}$ 的映射。  
其值域应当为某个 $\mathbb A$，即如果你将选出的那些“元”所构成的集合并上 $\mathbb Z$ 的结果当做一个新 $\mathbb Z$ 的话，那么在这个新的 $\mathbb Z$ 上的代数数的集合。  

## Prework- $\text {II}$ -单项式
形如 $ax^n$ 的式子，$x$ 是一个符号。  
我必须要在这里陈述一些看似无用的信息：  
$ax^n\times bx^m=abx^{n+m}$ 以及  
$(x^n)^m=x^{nm}$。  
虽然这是一个稀松平常的公式，但是当巨大的信息量不断进入我们的脑海后，推式子时推出 $x^{nm}=x^n+x^m$ 仍是一个常见的错误。   
## Prework- $\text {III}$ -多项式
### PIII-1 群  
这一部分虽然似乎与主题无关。但是在此不得不多说一点。  
通常表示为 $(R,\cdot)$。（$R$ 是任意某个集合，不一定是实数集）  
此处 $\cdot$ 是一个由 $R^2\to R$ 的映射 $f(x,y)(x,y\in R)$。    
+ 封闭性，即 $\forall x,y\in R,f(x,y)\in R$。  
+ 满足结合律，即 $f(x,f(y,z))=f(f(x,y),z)$。    
+ 存在单位元 $\varepsilon\in R$，使得 $\forall a\in R,f(a,\varepsilon)=f(\varepsilon,a)=a$。  
+ 存在逆元，即 $\forall a\in R,\exists a^{-1}\in R,s.t. f(a,a^{-1})=f(a^{-1},a)=\varepsilon$，$s.t.$ 是“使得”的意思（such that）。  

（注意，虽然 $f$ **不需要有**交换律，但是对于单位元、逆元是例外，这一点感谢 nalemy 学长曾经的提醒，之前一直忽略这一点）  
而**半群**是没有单位元和逆元的群，**幺半群**是有单位元但没有逆元的群。  
### PIII-2 阿贝尔群   
具有交换律的群，也称作**交换群**，即 $\forall x,y\in R,f(x,y)=f(y,x)$。  
这里想到某人的一句话（大抵是这样的）：  

> “如果你问一个中国小学生 $1234+5678$ 是多少，他会立刻回答 $6912$，但是如果你问一个法国小学生 $2+3$ 是多少，他会说：‘我不知道答案是几，但是我知道 $2+3=3+2$，因为加法在整数上构成了一个阿贝尔群。’ ”    

当然这**仅仅是一个笑话**。  
### PIII-3 环 
通常用 $<R,+,\times>$ 表示 （不一定是加和乘）。    
我们设 $a+b:=f(a,b),a\times b:=g(a,b)$。（符号 “$:=$“ 的意思是“定义为”）  
其中：  
+ $(R,+)$ 是一个阿贝尔群。  
+ $(R,\times)$ 是一个半群。  
+ $g(f(x,y),z)=f(g(x,z),g(y,z)),g(x,f(y,z))=f(g(x,y),g(x,z))$，也就是 $\times$ 对 $+$ 有分配律（不要搞反了，而且必须同时满足左分配律和右分配律，因为 $\times$ 不一定交换）。  

如果 $g(a,b)=g(b,a)$，即 $\times$ 有交换律，则称为**交换环**。    
如果 $(R,\times)$ 是幺半群，则称为**幺环**。  
如果 $a,b\neq 0,ab\neq 0$，其中 $0$ 为 $+$ 的单位元，则称为**无零因子环**。  
如果 $R$ 中除了 $+$ 的单位元（零元）外所有元素都有 $\times$ 的逆元，则称为**除环**。  
**整环**既是交换环，又是幺环，同时是无零因子环。
### PIII-4 域
域（field）是**交换除环**。  
**数域**是 $\mathbb{C}$ 的满足域的条件的子集。  
当然，field 在物理学中亦有“场”的意思。  
### PIII-5 定义
$K$ 是一个数域，$x$ 是一个符号，那么形如：
$$
a_nx^n+a_{n-1}x^{n-1}+\cdots+a_1x+a_0
$$
也可记作：
$$
\sum_{i=0}^na_ix^i
$$
的表达式中，$n\in\mathbb{N},a\subseteq{K}$，即**系数**，并且有“两个这种形式的表达式相等当且仅当它们含有完全相同的项（除去系数为 $0$ 的项外，系数为 $0$ 的项允许任意删去和添加）”这一性质时，则称这种表达式为 $K$ 上的**一元多项式环**。  
此时 $+$ 就是一般意义上的加法，指数也是普通的乘法重复叠加。  
对于环这一性质，是可以显然得出的，其中 $+$ 的单位元即为系数全为零的**零多项式**，记作 $0$。  
对于 $K$ 上的一元多项式 $f(x)=a_nx^n+a_{n-1}x^{n-1}+\cdots+a_1x+a_0$， $a_0$ 记作“常数项”，而 $n$ 记作 $f(x)$ 的**次数**，简称 $\deg f(x)$。  

当 $\deg f(x)\to \infty$ 时，我们可称 $f(x)$ 为**形式幂级数**，它可以构成**形式幂级数环**。  
对于某个域 $\mathbb{F}$ 上的形式幂级数环记作 $\mathbb{F}[[x]]$。  
一个形式幂级数即为其系数序列的 OGF（普通生成函数）。  
本文中可能对生成函数部分说明较少，如果详细讲解之可能需要高于本文全文一倍的篇幅，我们的重点仍然是形式幂级数（多项式）的具体应用和计算。  

这里需要注意的是，$x$ 只是一个形式记号，它没有实际意义，也没有范围的限定，它的作用只是维护多项式的结构，而多项式的本质实际上就是它的系数序列，那些定义在多项式上的运算和函数也不要真的将其中的 $x$ 当作未知数，它只是一个结构符号。  
如果将 $x$ 当作自变量，那么得到的结果就是**多项式函数**，而不是多项式。  
关于这一点会在后文集合幂级数部分进一步描述。

零多项式的次数记作 $-\infty$。
## Prework- $\text {IV}$ -多项式基本运算
数域 $K$ 上的所有一元多项式记作 $K[x]$。    
我们可以非常容易地得到 $K[x]$ 的基本运算。  
设 $f(x)=\displaystyle\sum_{i=0}^na_ix^i,g(x)=\displaystyle\sum_{i=0}^mb_ix^i$。

### PIV-1 加法
$f(x)+g(x)=\displaystyle\sum_{i=0}^{\max(n,m)}(a_i+b_i)x^i$。  
这是基础的合并同类项，不需要多说。  
### PIV-2 减法
减法就是加上对于 $+$ 的逆元。
### PIV-3 卷积
我们第一个接触到的卷积或许是数论中的狄利克雷卷积。   
对于两个数论函数 $f(n),g(n)$，那么它们卷积后的函数 $h(n)=\displaystyle\sum_{d|n}f(d)g(\dfrac{n}{d})$。  
它具有大量有趣的性质，不过如果想要深究它就不得不提到 DGF 以及黎曼函数 $\zeta$，而这不是现在所讨论的主要内容，所以仅仅做为一个引子。  
普通的函数 $f(x),g(x)$ 的卷积是：$$\displaystyle\int_{-\infty}^{\infty}f(\tau)g(x-\tau)\mathrm{d}\tau$$
数列的**卷积**是这样定义的。  
对于两个数列 $\{a_n\},\{b_n\}$，设 $c=a*b$，则有：
$$
c_n=\sum_{i=0}^n a_ib_{n-i}
$$
也可以更加直观地表示为：
$$
c_n=\sum_{i+j=n} a_ib_j
$$
此时需要说明 $i,j\in \mathbb{N}$。  
所以狄利克雷卷积亦可表示成：
$$
h(n)=\sum_{ij=n} f(i)g(j)
$$
你还可以扩展出位运算卷积等：
$$
c_n=\sum_{i\oplus j=n} a_ib_j
$$
卷积是定义在数列上的运算，不过它对于多项式有一些辅助意义。      

### PIV-4 乘法
$$
\begin{aligned}
f(x)\times g(x)&=(\sum_{i=0}^na_ix^i)(\sum_{i=0}^nb_ix^i)\\
&=\sum_{i=0}^n(a_ix^i\sum_{j=0}^nb_jx^j)\\
&=\sum_{i=0}^n\sum_{j=0}^na_ix^ib_jx^j\\
&=\sum_{i=0}^n\sum_{j=0}^ma_ib_jx^{i+j}\\
\end{aligned}
$$
这样就是一般的表示形式。   
你也可以用卷积来表示，你需要变换枚举顺序，枚举 $s=i+j$：  
$$
\begin{aligned}
&=\sum_{s=0}^{n+m}(\sum_{i+j=s}a_ib_j)x^s\\
&=\sum_{s=0}^{n+m}c_sx^s  
\end{aligned}
$$
也就是，以 $a*b$ 为系数的 $n+m$ 次多项式。  

至于除法，多项式经过除法之后可能就不是多项式了，我们将在后文讨论。    
### PIV-5 复合  
定义 $f(x)\circ g(x)$ 为 $f(g(x))$。  
根据此进行拓展，可以定义二元多项式（二元形式幂级数）：
$$
F(x,y)=\sum_{i=0}^{n}\sum_{j=0}^m f_{ij}x^iy^j
$$
二元形式幂级数环即为 $\mathbb{F}[[x,y]]$。
## Prework- $\text {V}$ -因式分解
小学课本上说，数的除法是数的乘法的逆运算。    
中学课本上说，因式分解是多项式乘法的逆运算。    

这就产生了歧义，因为你不能说，因数分解是数的乘法的逆运算。  
对于某个函数的逆运算，实际上是对逆元执行该运算，而不是将这个运算“分解”开来。    
正因如此，因为没有对于幂运算的单位元（也就代表着没有逆元），同时还不满足交换律，所以它没有一个严格的逆运算，因此产生出开方与对数。  
驳斥的说法是，开方是幂运算的逆运算，对数是指数运算的逆运算，这么说也不无道理，只是将底数或指数看作“主元”罢了。   

关于因式分解，具体方法有很多，对于因式分解本身有如下性质：  
$\mathbb{R}$ 上的多项式 $P(x)$ 能分解为一次因式和**无实根**二次因式的乘积，即 
$$
P(x)=A\prod_{i=1}^{s}(x-x_i)^{m_i}\prod_{i=1}^t(x^2+a_ix+b_i)^{n_i}
$$
在允许 $a_i,b_i$ 是虚数的情况下，就可以完全分解为：
$$
P(x)=A\prod_{i=1}^{\deg P(x)}(x-x_i)^{m_i}
$$
而 $x_i$ 即为 $P(x)=0$ 的复数根。  

> [!abstract]- 证明
>> [!note] 引理
>> 若复数 $z$ 是方程 $P(x)=a_0x^n+a_1x_{n-1}+\cdots+a_{n-1}x+a_n=0$ 的根，则其共轭复数 $\overline{z}$ 也是该方程的根。  
>> 证明：  
>> 首先因为 $\overline{z}^n=\overline{z^n}$。  
>> 所以 $P(\overline{x})=\overline{P(x)}=\overline{0}=0$。
> 
> 有了引理，可以用数学归纳法证明该结论。    
> 对于次数为 $1,2$ 的情况，结论显然成立。
> 对于次数更高的情况，假设更小次数的已经被证明，根据代数基本定理，方程 $P(x)=0$ 必有一复数根 $c$。  
> 若 $c$ 为实数，则 $P(x)=(x-c)Q(x),Q(x)$ 亦为实系数多项式，次数更低，得证。  
> 若 $c$ 为虚数，则 $P(x)=(x-c)(x-\overline{c})Q(x),Q(x)$ 为实系数多项式，而 $x^2-cx-\overline{c}x+c\overline{c}$ 因为 $\Delta<0$ 显然为无实根二次式，$Q(x)$ 次数更低，也得证。  
> $\square$

（正方形是 Q.E.D. 即“证毕”的意思）  
这个定理的典型应用是部分分式法求不定积分。    
我们还需要一些扩展：  
设真分式 $R(x)=\frac {P(x)}{Q(x)}$, 其中 $Q(x)=\prod_{i=1}^{s}(x-x_i)^{m_i}\prod_{i=1}^t(x^2+a_ix+b_i)^{n_i}$，那么存在一系列常数 $A_{ij},B_{ij},C_{ij}$，使得：
$$
R(x)=\sum_{i=1}^s\sum_{j=1}^{m_i}\frac{A_{ij}}{(x-x_i)^j}+\sum_{i=1}^t\sum_{j=1}^{n_i}\frac{B_{ij}x+C_{ij}}{(x^2+a_ix+b_i)^j}
$$
证明方法类似。  
于是根据积分的线性性，把一次，二次的简单分式积分后相加即可。  

## Prework- $\text {VI}$ -多项式同余
与数论中的同余相差不大。  

先定义多项式中的“整除”：  
若 $\exists$ 实系数多项式 $h(x)$，使得 $g(x)h(x)=f(x)$，则记 $g(x)|f(x)$。  

对于 $\mathbb R$ 上的多项式 $f(x),g(x),m(x)$，（不妨设 $\deg f(x)\ge\deg g(x)$）若 $m(x)|(f(x)-g(x))$，则记 $f(x)\equiv g(x) \pmod{m(x)}$。
## Prework- $\text {VII}$ -杂项
### PVII-1 点值表示
多项式最常用的表示方法就是：$\displaystyle\sum_{i=0}^na_ix^i$。  
但是还有一种表示方法是：$\{(x_1,f(x_1)),(x_2,f(x_2)),\cdots,(x_n,f(x_n))\}$。  
$n+1$ 个形如此类的点对就可以唯一确定一个 $n$ 次多项式。      
原因是，你可以列出一个 $n+1$ 元齐次线性方程组（不能忽略常数项），共有 $n+1$ 个方程，因此可以证明这样的方程有唯一解。  
我们可记多项式 $f(x)$ 对于集合 $S$ 作为点集的点值表达式为 $\mathscr{F}_{S}(f)$。  
同样，记点值表达式 $f=\mathscr{F}_{S}^{-1}(T)$，其中 $T=\set{f(x_1),f(x_2),\cdots,f(x_{n+1})},x_i\in S$。

<div STYLE="page-break-after: always;"></div>

# PART 2 —— 多项式基础算法
## Basic- $\text {I}$ -离散傅里叶变换
### BI-1 朴素乘法
根据多项式乘法的公式：  
$$
h(x)=f(x)g(x)=\sum_{i=0}^n\sum_{j=0}^ma_ib_jx^{i+j}
$$
你需要 $nm$ 次累加，才能算出 $h(x)$。    
这样的效率是很慢的，我们考虑优化。  
### BI-2 离散傅里叶变换 （DFT）

> [!warning] 注意 
> 离散傅里叶变换本质上是作用在多项式函数上的变换，由于不涉及极限与微分，姑且不做区分。

根据前文所述，有一种技巧性的方法：    
首先，确定 $n+m+1$ 个不同的值 $x_1,x_2,\cdots,x_{n+m+1}$，然后根据这 $n+m+1$ 个值将 $f(x),g(x)$ 转换成点值表示，即 $\mathscr{F}_{\set{x_i}}(f(x)),\mathscr{F}_{\set{x_i}}(g(x))$。   
由于 $\forall x\in K,f(x)g(x)=(fg)(x)$，故可构造 $(fg)(x)=\mathscr{F}_{\set{x_i}}^{-1}(T),T=\{f(x_i)g(x_i)|1\le i\le n+m+1\}$。    
所以问题转化成如何快速将系数表达式与点值表达式相互转换。  
考虑傅里叶变换的表达式：  
$$
F(\omega)=\mathbb{F}[f(t)]=\int_{-\infty}^{\infty}f(t)\mathrm e^{-\mathrm i\omega t}\mathrm dt
$$
我们将其转化为离散情况，即将积分改成求和，此时设 $x$ 为一个数列：  
$$
\hat x_k=\sum_{i=0}^{n-1} x_i\mathrm{e}^{\mathrm{-i}\frac{2\pi ik}{n}}
$$

此时我们令 $x_i$ 为 $a_i$，即系数序列，那么就有 $\hat x_k=f(\mathrm{e}^{-\mathrm{i}\frac{2\pi k}{n}})$，根据 $\mathrm e^{\mathrm i\pi}=-1$，故而 $\mathrm e^{2\mathrm i\pi}=\mathrm e^{-2\mathrm i\pi}=1$。  
那么其实那些求的值就是单位根。   
所以离散傅里叶其实就是将多项式转成点值表示法，点集为 $n+1$ 次单位根。  
那么，就对与每一个单位根，代入原多项式中进行求值即可。  
但是这样仍然需要 $O(n^2)$ 次计算。  

> [!note] 关于大 $O$ 记号
> 对于连续函数 $f(x),g(x)$，若称 $f(x)=O(g(x))$，则有 $\displaystyle\lim_{x\to \infty}\dfrac{f(x)}{g(x)}$ 为常数 $c$，其中 $c$ 可以为 $0$。   
> 如果某一算法对于 $n$ 的输入规模要计算 $O(f(n))$ 次，则称该算法的**时间复杂度**为 $O(f(n))$。

## Basic- $\text{II}$ - 快速傅里叶变换（FFT）
不妨记 $\omega_n^i$ 表示第 $i$ 个 $n$ 次单位根。  
对于一个多项式
$$
f(x)=\sum_{i=0}^{n-1}a_ix^i
$$
我们根据奇偶项进行拆分（分治思想）：
$$
f(x)=\sum_{i=0,2|i}^{n-1}a_ix^i+\sum_{i=0,2\nmid i}^{n-1}a_ix^i=g(x^2)+xh(x^2)  
$$
其中 $g(x)=\displaystyle\sum_{i=0,2|i}^{n-1}a_ix^{\frac i 2},h(x)=\displaystyle\sum_{i=0,2\nmid i}^{n-1}a_ix^{\frac{i-1}{2}}$。  
所以 $f(\omega_n^i)=g(\omega_n^{2i})+\omega_n^ih(\omega_n^{2i})$。  
故而 $f(\omega_n^i)=g(\omega_{\frac{n}{2}}^{i})+\omega_n^ih(\omega_{\frac{n}{2}}^{i})$。  
同理可得：$f(\omega_n^{i+\frac{n}{2}})=g(\omega_{\frac{n}{2}}^{i})-\omega_n^ih(\omega_{\frac{n}{2}}^{i})$。  
也就是说你只需要计算出   
$$
\begin{gather}
g(\omega_{\frac{n}{2}}^1),g(\omega_{\frac{n}{2}}^2),\cdots,g(\omega_{\frac{n}{2}}^{\frac{n}{2}});\\
h(\omega_{\frac{n}{2}}^1),h(\omega_{\frac{n}{2}}^2),\cdots,h(\omega_{\frac{n}{2}}^{\frac{n}{2}})
\end{gather}
$$
，你就能 $O(n)$ 计算出 $f(\omega_n^{1}),f(\omega_n^{2}),\cdots,f(\omega_n^{n})$。  
设计算**项数**为 $n$ 的多项式的复杂度为 $T(n)$，则有 $T(n)=2T(\dfrac{n}{2})+O(n)$。  
根据主定理，可以得到 $T(n)=O(n\log n)$。  

> [!question] 一些常见问题：
> 1. 为什么 $\log$ 没有底数？
> 根据对数换底公式有推论 $\dfrac{\log_un}{\log_vn}=\log_vu$，因此底数的改变只会使原式乘以一个常数，不会发生阶层面的改变。
> ---
> 2. 什么是**主定理**？  
> 主定理（Master Theorem）的描述如下：  
> 对于递归算法的复杂度：
> $$
> T(n)=aT(\dfrac{n}{b})+f(n)
> $$
> 则有：
> $$T(n)=
> \begin{cases}
> \Theta(n^{\log_ba}),&f(n)=O(n^{\log_ba-\varepsilon})(\varepsilon>0)\\
> \Theta(f(n)),&f(n)=\Omega(n^{\log_ba+\varepsilon})(\varepsilon\ge0)\\
> \Theta(n^{\log_ba}\log^{k+1}n),&f(n)=\Theta(n^{\log_ba}\log^kn)(k\ge0)\\
> \end{cases}
> $$
> ---
> 3. 如果 $\dfrac{n}{2}$ 不是整数怎么办？  
> 我们使 $n$ 是 $2$ 的次幂，如果项数不够就在高项补系数为 $0$ 的项即可。
## Basic- $\text {III}$ -迭代快速傅里叶变换
可以发现，在第 $i$ 次向下递归时，这个系数序列就会反转二进制下的第 $i$ 位。  
例如：
$$
\begin{gather}
a_0,a_1,a_2,a_3,a_4,a_5,a_6,a_7\\
a_0,a_2,a_4,a_6|a_1,a_3,a_5,a_7\\
a_0,a_4|a_2,a_6|a_1,a_5|a_3,a_7\\
a_0|a_4|a_2|a_6|a_1|a_5|a_3|a_7\\
\end{gather}
$$
然后，可以注意到，$(0)_2=000,(1)_2=001,(2)_2=010,(3)_2=011,(4)_2=100,(5)_2=101,(6)_2=110,(7)_2=111$。  
按二进制位翻转后就可以得到最后的结果。  
那么就可以自下而上，逐个合并，也可以得到最终的点值表达式。  
这样的速度更快一些。
## Basic- $\text {IV}$ -快速傅里叶逆变换
快速傅里叶变换有一种矩阵的表达形式：
$$
\begin{bmatrix}\hat x_0\\\hat x_1\\\ \vdots\\\hat x_{n-1}\\\end{bmatrix}
=
\begin{bmatrix}
1&1&1&\cdots&1\\
1&\omega_n^1&\omega_n^2&\cdots&\omega_n^{n-1}\\
1&\omega_n^2&\omega_n^4&\cdots&\omega_n^{2n-2}\\
\vdots&\vdots&\vdots&&\vdots&\\
1&\omega_n^{n-1}&\omega_n^{2n-2}&\cdots&\omega_n^{(n-1)(n-1)}\\
\end{bmatrix}
\begin{bmatrix}a_0\\a_1\\\ \vdots\\a_{n-1}\\\end{bmatrix}
$$
证明显然，直接将将矩阵乘法展开即可。  
那么我们有
$$
\begin{bmatrix}a_0\\a_1\\\ \vdots\\a_{n-1}\\\end{bmatrix}
=
\begin{bmatrix}
1&1&1&\cdots&1\\
1&\omega_n^1&\omega_n^2&\cdots&\omega_n^{n-1}\\
1&\omega_n^2&\omega_n^4&\cdots&\omega_n^{2n-2}\\
\vdots&\vdots&\vdots&&\vdots&\\
1&\omega_n^{n-1}&\omega_n^{2n-2}&\cdots&\omega_n^{(n-1)(n-1)}\\
\end{bmatrix}^{-1}
\begin{bmatrix}\hat x_0\\\hat x_1\\\ \vdots\\\hat x_{n-1}\\\end{bmatrix}
$$
其逆矩阵为：  
$$
\begin{bmatrix}
\frac 1 n&\frac 1 n&\frac 1 n&\cdots&\frac 1 n\\
\frac 1 n&\frac {\omega_n^{-1}} {n}&\frac{\omega_n^{-2}}{n}&\cdots&\frac{\omega_n^{1-n}}{n}\\
\frac 1 n&\frac{\omega_n^{-2}}{n}&\frac{\omega_n^{-4}}{n}&\cdots&\frac{\omega_n^{2-2n}}{n}\\
\vdots&\vdots&\vdots&&\vdots&\\
\frac 1 n&\frac{\omega_n^{1-n}}{n}&\frac{\omega_n^{2-2n}}{n}&\cdots&\frac{\omega_n^{(1-n)(n-1)}}{n}\\
\end{bmatrix}
$$
证明同样直接利用矩阵乘法公式立得。    
同理变换即可。   
所以当你求出 $f(x),g(x)$ 的点值表示后，分别每一项乘在一起，然后再逆变换回去，就可以得到 $f(x)g(x)$，总复杂度 $f(x)g(x)$。
## Basic- $\text {V}$ -快速数论变换（NTT）
在计算中，很多答案的数值是一个天文数字，计算机难以用快捷的方式存储，所以我们通常只需计算出该答案对某个数取模的结果。  
这里需要一些前置知识：
### BV-1 本原单位根
引理：如果 $\gcd(a,n)=1$，则 $a,2a,3a,\cdots,na$ 模 $n$ 的值互不相同。  
反证则该命题立得。  
根据这个引理，则如果 $\gcd(a,n)=1$，那么 $\omega_n^a$ 的若干次方就可以生成出全部 $n$ 次单位根，则 $\omega_n^a$ 记作本原 $n$ 次单位根。
### BV-2 快速数论变换
假设我们要对一个奇素数 $p$ 取模，那么不妨将 FFT 中的单位根变成 $p$ 的原根 $g$ 的 $\dfrac{p-1}{n}$ 次方。  
不妨设 $g^{\frac{p-1}{n}}=\gamma_n$。  
此时不难发现，原根满足单位根的全部性质：
1. 消去引理：$\omega_{nd}^{md}=\omega_{n}^m$  
2. 折半引理：$\omega_n^{m+\frac{n}{2}}=-\omega_n^{m}$  

同样的，我们有 $\gamma_{nd}^{md}\equiv\gamma_{n}^m\pmod p$，  
以及 $\gamma_{n}^{m+\frac{n}{2}}\equiv p-\gamma_{n}^{m}\pmod p$。  
直接代入公式即可证明。  
这样构造的原因是，首先原根的性质与单位根相似，又 $g^1,g^2,\cdots,g^{\varphi(n)}$ 互不相同，而 $\varphi(p)=p-1$，所以只要 $n|(p-1)$，就存在模 $p$ 意义下的本元 $n$ 次方根。  
因为我们需要同 FFT 一样进行优化，所以仍需要保证 $n=2^k(k\in \mathbb{N})$。  
所以 $(p-1)$ 也需要是一个 $2^k$ 的倍数，且这个 $2^k$ 至少要大于需要计算的全部 $n$。   
所以最常用的模数 $p$ 是：  
$$
\begin{aligned}
998244353=7\times17\times2^{23}+1,&&g=3\\
\end{aligned}
$$
还有其余的如下表所示。  
![aaa|300](https://cdn.luogu.com.cn/upload/image_hosting/8pifjwv3.png)  

我们发现 NTT 对于 $p$ 的限制非常强，所以通常来说仅仅在竞赛中较为常用。  
## Basic- $\text {VI}$ -位运算卷积
### BVI-1 位运算
#### BVI-1-1 按位与
按位与，又称为按位且，一般记为 $\&,\wedge$ 或 $\operatorname{and}$。  
假设我们有两个正整数 $A,B$，我们先将它转成二进制 $(A)_2,(B)_2$，然后，对于二进制数的每一位，如果 $(A)_2,(B)_2$ 的这一位**都是** $1$，则 $(A\&B)_2$ 的这一位是 $1$，否则这一位是 $0$。  
例：
$$
100101011100\ \&\ 111000100111 = 100000000100
$$
#### BVI-1-2 按位或
按位或，一般记为 $|,\vee$ 或 $\operatorname{or}$。  
假设我们有两个正整数 $A,B$，我们先将它转成二进制 $(A)_2,(B)_2$，然后，对于二进制数的每一位，如果 $(A)_2,(B)_2$ 的这一位**都是** $0$，则 $(A|B)_2$ 的这一位是 $0$，否则这一位是 $1$。  
例：
$$
100101011100\ |\ 111000100111 = 111101111111
$$
#### BVI-1-2 按位异或
按位异或一般记为 $\oplus,^\wedge$ 或 $\operatorname{xor}$。  
假设我们有两个正整数 $A,B$，我们先将它转成二进制 $(A)_2,(B)_2$，然后，对于二进制数的每一位，如果 $(A)_2,(B)_2$ 的这一位相同，则 $(A\oplus B)_2$ 的这一位是 $0$，否则这一位是 $1$。  
例：
$$
100101011100\ \oplus\ 111000100111 = 011101111011
$$
### BVI-2 位运算卷积 
#### BVI-2-1 定义
我们现在有两个序列 $\{a_n\},\{b_n\}$ （**这里假定序列下标从 $0$ 开始，也就是说最后一项是 $a_{n-1}$**）。
我们定义     
$$
\begin{aligned}
\phi_x=\sum_{i\&j=x}a_ib_j\\
\psi_x=\sum_{i|j=x}a_ib_j\\
\xi_x=\sum_{i\oplus j=x}a_ib_j\\ 
\end{aligned}
$$
此时我们考虑如何快速计算这三个序列的值。  
#### BVI-2-2 快速沃尔什变换
##### 按位与卷积
仿照快速傅里叶变换的思路，定义 $\Phi\{a\}$ 是序列 $\{a_n\}$ 经过按位与意义下快速沃尔什变换后的结果。  
此时我们希望有 $\Phi\{\phi\}=\Phi\{a\}\times\Phi\{b\}$。  
此处 “$\times$” 就是每一项对应相乘。  
这一步仅需在 $O(n)$ 的复杂度下从 $0$ 到 $(n-1)$ 依次遍历即可。  
所以问题转化成如何构造 $\Phi$，以及如何快速计算 $\Phi\{a\}$。  
令 $\Phi\{a\}_x=\displaystyle\sum_{x\&i=x(0\le i<n)}a_i$。  
验证：
$$
\begin{aligned}
&\Phi\{a\}_x\times\Phi\{b\}_x\\
=&\left(\sum_{x\&i=x(0\le i<n)}a_i\right)\left(\sum_{x\&j=x(0\le j<n)}b_j\right)\\
=&\sum_{x\&i=x(0\le i<n)}\left(a_i\sum_{x\&j=x(0\le j<n)}b_j\right)\\
=&\sum_{x\&i=x(0\le i<n)}\sum_{x\&j=x(0\le j<n)}a_ib_j\\
=&\sum_{x\&(i\&j)=x(0\le i,j<n)}a_ib_j\\
=&\Phi\{\phi\}_x
\end{aligned}
$$
满足条件。
> [!question] 上述步骤中的细节
>  1. 两个和式乘起来的括号是可以分开的，原因是乘法对加法有分配律，故直接拆开式子即可。  
>  2.
> 	$$
> 	\begin{cases}x\&i=x\\ x\&j=x\end{cases}\iff x\&(i\&j)
> 	$$
> 	原因是，对于每一个二进制位，如果 $x$ 的这一位是 $1$，那么 $i,j$ 的这一位也必然是 $1$，所以 $i\&j$ 的这一位也一定是 $1$。  
> 	这两个条件是等价的，所以用“$\iff$”。

快速计算 $\Phi\{a\}$ 的方法依然使用分治思想进行考虑。  
依然假定 $n=2^k(k\in\mathbb{N})$。  
对于序列的前半段 $\{a_{[0,\frac{n}{2})}\}$，它们的下标的二进制首位是 $0$，对于后半段 $\{a_{[\frac{n}{2},n)}\}$，它们的下标的二进制首位是 $1$。
设 $\{a_{[0,\frac{n}{2}]}\}=\{u\},\{a_{[\frac{n}{2}+1,n)}\}=\{v\}$。
首先我们可以得到 $\Phi\{a\}_i=\Phi\{v\}_i(\frac n 2\le i<n)$，因为对于 $i\&j=i$ 的 $j$ 必定也在 $[\frac{n}{2},n)$ 之间。  
但是对于 $0\le i<\frac n 2$ 的 $i$，对应的 $j$ 的范围是没有限制的，所以此时 $\Phi\{a\}_i=\Phi\{u\}_i+\Phi\{v\}_i$。 
所以我们有 $\Phi\{a\}=\operatorname{merge}(\Phi\{u\}+\Phi\{v\},\Phi\{v\})$ 。  
其中 $\operatorname{merge}$ 为将两个序列拼接。    
那么可以递归求解，复杂度 $T(n)=2T(\frac n 2)+O(n)=O(n\log n)$。  
##### 按位或卷积
同样的，定义 $\Psi\{a\}$ 是序列 $\{a_n\}$ 经过按位或意义下快速沃尔什变换后的结果。  
此时我们希望有 $\Psi\{\psi\}=\Psi\{a\}\times\Psi\{b\}$。   
令 $\Phi\{a\}_x=\displaystyle\sum_{x|i=x(0\le i<n)}a_i$。  
验证：
$$
\begin{aligned}
&\Psi\{a\}_x\times\Psi\{b\}_x\\
=&\left(\sum_{x|i=x(0\le i<n)}a_i\right)\left(\sum_{x|j=x(0\le j<n)}b_j\right)\\
=&\sum_{x|i=x(0\le i<n)}\left(a_i\sum_{x|j=x(0\le j<n)}b_j\right)\\
=&\sum_{x|i=x(0\le i<n)}\sum_{x|j=x(0\le j<n)}a_ib_j\\
=&\sum_{x|(i|j)=x(0\le i,j<n)}a_ib_j\\
=&\Psi\{\psi\}_x
\end{aligned}
$$
满足条件。  
仿照前文，我们有 $\Psi\{a\}=\operatorname{merge}(\Psi\{u\},\Psi\{u\}+\Psi\{v\})$ 。    
递归求解，复杂度 $T(n)=2T(\frac n 2)+O(n)=O(n\log n)$。
##### 按位异或卷积
同样的，定义 $\Xi\{a\}$ 是序列 $\{a_n\}$ 经过按位与意义下快速沃尔什变换后的结果。  
此时我们希望有 $\Xi\{\xi\}=\Xi\{a\}\times\Xi\{b\}$。   
令 $\Xi\{a\}_x=\displaystyle\sum_{x \circ i=0(0\le i<n)}a_i-\displaystyle\sum_{x\circ j=1(0\le j<n)}a_j$。  
其中 $x\circ y=x|y$ 的二进制中 $1$ 的数量的奇偶性。  
由于有 $(x\circ y)\oplus(x\circ z)=x\circ(y\oplus z)$，所以上式如同前文一样可以验证。  
仿照前文，并根据 $\circ$ 的定义，我们有 $\Xi\{a\}=\operatorname{merge}(\Xi\{u\}+\Xi\{v\},\Xi\{u\}-\Xi\{v\})$ 。  
其中 $\operatorname{merge}$ 为将两个序列拼接。    
递归求解，复杂度 $T(n)=2T(\frac n 2)+O(n)=O(n\log n)$。  

#### BVI-2-3 快速沃尔什逆变换
我们希望有 $\Phi^{-1}\{\Phi(a)\}=a,\Psi^{-1}\{\Psi(a)\}=a,\Xi^{-1}\{\Xi(a)\}=a$。  
那么，根据前文所推到的式子, 我们很容易有：  
1. $\Phi^{-1}\{a\}=\operatorname{merge}(\Phi\{u\}-\Phi\{v\},\Phi\{u\})$
2. $\Psi^{-1}\{a\}=\operatorname{merge}(\Psi\{u\},\Psi\{u\}-\Psi\{v\})$ 
3. $\Xi^{-1}\{a\}=\operatorname{merge}(\frac{\Xi\{u\}-\Xi\{v\}}{2},\frac{\Xi\{u\}+\Xi\{v\}}{2})$

## Basic- $\text {VII}$ -多项式求逆
### BVII-1 逆元与费马小定理
对于整数 $a,b,p$，若 $ab\equiv1\pmod p$，则称 $a,b$ 互为模 $p$ 意义下的数论逆元。  
通常记 $b$ 为 $a^{-1}$，在不严格的情况下亦可记作 $\frac{1}{a}$。  
对于数论逆元的计算，通常使用费马小定理：  

> 
> 若 $p$ 为素数，$\gcd(a,p)=1$，即 $a,p$ 互质（可记做 $a\perp p$），那么 $a^{p}$ 和 $a$ 在模 $p$ 意义下同余。
> 

> [!abstract]- 证明
> 可以用数学归纳法证明该结论。    
> 对于 $a$ 为 $1$ 的情况，结论显然成立。
> 如果 $a^p\equiv a \pmod p$ 成立，  
> 则 $(a+1)^p=\displaystyle\sum_{i=0}^p \binom{p}{i}a^i$。  
> 根据组合数的定义，$\dbinom{p}{i}=\dfrac{p!}{i!(p-i)!}$。（本文将始终以 $\binom{n}{m}$ 表示组合数而非 $C_n^m$）    
> 由于 $p$ 是素数，那么 $(i!(p-i)!)$ 中一定不含有带 $p$ 的因子，所以分子上的 $p$ 不会被约掉。  
> 因此 $\dbinom{p}{i}|p$，因此 $(a+1)^p\equiv a+1 \pmod p$。  
>  $\square$

既然有了这个结论，我们就有推论 $a^{p-1}\equiv a^{p-2}\times a\equiv 1\pmod p$。  
所以 $a^{p-2}$ 是 $a$ 的逆元。  
通过快速幂算法可以在 $O(\log P)$ 的复杂度内解决。

### BVII-2 线性求逆元
如果你要求 $x^{-1} \bmod p$，那么让 $p$ 对 $x$ 作带余除法。  
设 $p=qx+r$，所以 $qx+r\equiv 0 \pmod p$。  
两边同乘 $q^{-1}r^{-1}$ 得 $r^{-1}x+q^{-1}\equiv 0 \pmod p$。  
于是有 $r^{-1}x\equiv p-q^{-1} \pmod p$。  
所以 $x^{-1}\equiv r^{-1}(p-q^{-1})^{-1}$。  
有了这个式子，就可以从 $1^{-1}$ 开始，$O(n)$ 递推，得到从 $1$ 到 $n$ 所有整数的逆元。

### BVII-3 多项式求逆
给定一个 $n$ 次多项式 $f(x)$，试求出 $g(x)$，使得 $f(x)g(x)\equiv 1 \pmod {x^n}$。  
首先我们可以求出 $h(x)$ ，满足 $f(x)h(x)\equiv 1\pmod{x^{\lceil\frac{n}{2}\rceil}}$  
很明显因为 $f(x)g(x)\equiv 1 \pmod {x^n}$，所以 $\forall m\le n,f(x)g(x)\equiv 1 \pmod {x^m}$ 。  
所以 
$$
f(x)g(x)\equiv 1\pmod{x^{\lceil\frac{n}{2}\rceil}}
$$
做差得到 
$$
f(x)(g(x)-h(x))\equiv 0\pmod{x^{\lceil\frac{n}{2}\rceil}}
$$
两边同除去一个 $f(x)$ 得 
$$
g(x)-h(x)\equiv 0\pmod{x^{\lceil\frac{n}{2}\rceil}}
$$
两边平方得  
$$
g(x)^2-2g(x)h(x)+h(x)^2\equiv 0\pmod{x^{n}}
$$
移项得   $$g(x)^2\equiv 2g(x)h(x)-h(x)^2\pmod{x^{n}}$$合并同类项   
$$
g(x)^2\equiv h(x)(2g(x)-h(x))\pmod{x^{n}}
$$
两边同除一个 $g(x)$ （模意义下相当于同乘一个 $f(x)$）  
$$
g(x)\equiv h(x)(2-h(x)f(x))\pmod{x^{n}}
$$
$h(x)$ 递归继续算即可，$h(x)f(x)$ 可以用 NTT $O(n\log n)$ 解决。     
最终递归到 $\bmod x$ 时，$g(x)=[x^0]f(x)^{-1}$。  
注意，此时 $[x^0]f(x)$ 不可为 $0$，否则要引入形式 Laurent 级数，后文将会进行详细介绍。  
复杂度 $T(n)=T(\frac n 2)+O(n\log n)=O(n\log n)$。  

## Basic- $\text {VIII}$ -多项式除法/取模

如果要对多项式进行带余除法（注意是**带余除法**而不是乘逆元），我们可以进行如下操作：  
求出 $f(x)$ 除以 $g(x)$ 的商 $Q(x)$ 和余式 $r(x)$。  
设 $f$ 的次数为 $n$，$g$ 的次数为 $m$，则自然有 $Q$ 的次数为 $n-m$，$r$ 的次数不超过 $m-1$。  
那么我们可以认定 $r$ 的次数就是 $m-1$，高次项没有就使系数为 $0$ 即可。  
定义：  
$$
\tilde{F}(x)=x^{c}F(\frac{1}{x})
$$

此处 $c$ 就是 $F$ 的次数。  
容易发现上式其实就是 $F$ 的系数翻转的结果。  
那么根据式子：$f(x)=Q(x)g(x)+r(x)$，替换 $x$ 为 $\frac 1 x$ 得：  
$$
f(\frac 1 x)=g(\frac 1 x)Q(\frac 1 x)+r(\frac 1 x)
$$
等式两边同乘一个 $x^n$ 得：    
$$
\begin{aligned}
x^nf(\frac 1 x)&=x^ng(\frac 1 x)Q(\frac 1 x)+x^nr(\frac 1 x)\\
&=x^mg(\frac 1 x)x^{n-m}Q(\frac 1 x)+x^{n-m+1}x^{m-1}r(\frac 1 x)\\
&=\tilde{g}(x)\tilde{Q}(x)+x^{n-m+1}\tilde{r}(x)\\
&=\tilde{f}(x)
\end{aligned}
$$
也就是  
$$
\tilde{f}(x)=\tilde{g}(x)\tilde{Q}(x)+x^{n-m+1}\tilde{r}(x)
$$
发现放到模 $x^{n-m+1}$ 意义下可以把 $\tilde{r}(x)$ 项消去。  
$$
\tilde{f}(x)\equiv\tilde{g}(x)\tilde{Q}(x)\pmod{x^{n-m+1}}
$$
那 $\tilde{Q}(x)$ 即为 $\tilde{f}(x)\tilde{g}^{-1}(x)$。  
做多项式求逆即可得出 $\tilde{Q}(x)$，翻转系数得 $Q(x)$，回代到原式得 $r(x)$。  
复杂度依然是 $O(n\log n)$。

## Basic- $\text {IX}$ -形式导数
这里引用 OI-wiki 的一句话：

> 
> 尽管一般环甚至未必存在极限，我们依然可以定义形式幂级数的**形式导数**。
> 

如果将多项式仍看作与多项式函数等价，那么可能会有不连续或不可导的情况，甚至 $\mathbb{R}$ 上的很多性质都无法满足，但是对于一个普通的多项式，也即一个形式幂级数，我们无需考虑**函数值**的连续性、可导性甚至是敛散性，直接做形式上的导数即可。  
相当于下式本质上是一个恰好与多项式导数计算法则一致的人为定义：  
$$
\frac{\mathrm d}{\mathrm dx}\sum_{i=0}^{n}a_ix^i=\sum_{i=1}^{n}ia_ix^{i-1}
$$
## Basic- $\text {IX}$ -形式不定积分
此处我们忽略积分结果中的常数项 $C$。
则亦可定义：
$$
\int\sum_{i=0}^{n}a_ix^i\mathrm dx=\sum_{i=1}^{n}\frac{a_i}{i}x^i
$$
## Basic- $\text {XI}$ -指数运算
### BXI-1 多项式自然对数
定义 $\ln(f(x))=\sum_{i\ge0}\frac{f(x)^i}{i}$
如果我们要求 $\ln(f(x))\bmod x^n$ ，可以用如下方式处理。
根据公式：
$$
\frac{\mathrm d}{\mathrm dx}\ln(x)=\frac{1}{x}
$$
那么由链式求导法则可以得到：
$$
\frac{\mathrm d}{\mathrm dx}\ln(f(x))=\frac{f'(x)}{f(x)}
$$
即为：
$$
\int\frac{f'(x)}{f(x)}\mathrm dx=\ln(f(x))
$$

那么做一遍求逆，再做一遍求导和积分就可以得到。  
其实这就是求解分式的不定积分的一种处理方式。 
### BXI-2 多项式 $\exp$
定义 $\exp(x):=\mathrm{e}^x$，我们要求出其模 $x^n$ 得到的多项式。  
首先说明，若 $f(x)$ 的常数项不是 $0$，那么 $\exp(f(x))$ 不存在。  
原因是对它做泰勒展开，得到：$\mathrm{e}^{f(x)}=\displaystyle\sum_{i\ge 0}\dfrac{f(x)^i}{i!}$，  
你会得到 $[x^0]\mathrm{e}^{f(x)}=\displaystyle\sum_{i\ge 0}\dfrac{[x_0]f(x)^i}{i!}$。  
而这在模意义下是发散的。  
注意，虽然形式幂级数是一个形式符号，不考虑其敛散性，但其常数项依然是一个数域中的元素。  
所以 $f(x)$ 的常数项需要为 $0$。  
此时取的 $x^n$ 中 $n$ 越大，最终实现的效果越精细，但不会达到最终答案，因为最终答案 $\mathrm{e}^{f(x)}=\displaystyle\sum_{i\ge 0}\dfrac{f(x)^i}{i!}$ 是一个无穷项的形式幂级数。  
关于这方面的拓展知识涉及到一些有趣的问题，感兴趣的不妨阅读 Elegia 的 [《无理数取模怎么做?》](https://www.luogu.me/article/mpgzgle2)  。
此处先给出关于多项式 $\exp$ 的常用的计算公式：  
设 $E(x)=\exp(f(x))\bmod x^n,E'(x)=\exp(f(x))\bmod \lceil\dfrac{x^n}{2}\rceil$ ，那么有：
$$
E(x)\equiv E'(x)(1-\ln E'(x)+f(x))\pmod {x^n}
$$
这个公式的证明参见后文。
## Basic- $\text {XII}$ -多项式三角函数
根据欧拉公式，我们有 $\mathrm{e}^{\mathrm{i}\theta}=\cos\theta+\mathrm{i}\sin\theta$。  
所以进一步我们可以得出 $\mathrm{e}^{-\mathrm{i}\theta}=\cos\theta-\mathrm{i}\sin\theta$。   
所以有   
$$
\cos\theta=\frac{\mathrm{e}^{\mathrm{i}\theta}+\mathrm{e}^{-\mathrm{i}\theta}}{2}
$$
$$
\sin\theta=\frac{\mathrm{e}^{\mathrm{i}\theta}-\mathrm{e}^{-\mathrm{i}\theta}}{2\mathrm{i}}
$$
直接根据 $\exp$ 计算即可。  
还有另外一个要点，通常我们需要得到答案模 $p$ 的结果，否则输出结果可能太大。  
为了得到 $\mathrm{i}\bmod p$ 的值，我们可以做 $-1$，也就是 $p-1$ 的二次剩余来得出。  
通常，对于 $p=998244353$ 来说，$\mathrm{i}\equiv 86583718$。  

对于 $\tan,\cot,\sec,\csc$ 来说，都可以通过 $\cos,\sin$ 互相计算得出。

## Basic- $\text {XIII}$ -多项式开根
注意，显然开根操作会得到两个和为 $0$ 的解，因此在实际应用中，我们通常取常数项较小的解。  
（此处感谢 LRC 与 Nueryim\_ 学长的提醒）
### BXIII-1 $\exp$ 法
在解题过程中，我们常用到这一变换：$a^b=\mathrm{e}^{b\ln(a)}$。  
根据此式，则有 $\sqrt{a}=\mathrm{e}^{\frac{\ln(a)}{2}}$。  
直接计算即可。
### BXIII-2 倍增法
仿照多项式求逆的思路，我们假定 $g(x)^2\equiv f(x)\pmod{x^n},h(x)^2\equiv f(x)\pmod{x^{\lceil\frac{n}{2}\rceil}}$。    
$$
\begin{aligned}
f(x)-h(x)^2&\equiv0\pmod{x^{\lceil\frac{n}{2}\rceil}}\\
(f(x)-h(x)^2)^2&\equiv0\pmod{x^{n}}\\
(f(x)+h(x)^2)^2&\equiv4f(x)h(x)^2\pmod{x^{n}}\\
\left(\frac{f(x)+h(x)^2}{2h(x)}\right)^2&\equiv f(x)\pmod{x^n}\\
\frac{f(x)+h(x)^2}{2h(x)}&\equiv g(x)\pmod{x^n}\\
\end{aligned}
$$
递归向下计算，复杂度 $T(n)=T(\frac{n}{2})+O(n\log n)=O(n\log n)$。  
那么，对于最终递归到 $\bmod x$ 时，$g(x)$ 为 $[x_0]f(x)$ 模 $P$ 意义下的二次剩余。      
对于二次剩余，常见求法的为 Cipolla 算法，与本文其他部分关系不大，不展开叙述。  
## Basic- $\text {XIV}$ -多项式反三角函数
对反三角函数进行求导，可以得到：  
$$
\frac{\mathrm{d}}{\mathrm{d}x}\arcsin(x)=\frac{1}{\sqrt{1-x^2}}
$$
故有  
$$
\int\frac{1}{\sqrt{1-x^2}}\mathrm{d}x=\arcsin(x)
$$
同理，有：  
$$
-\int\frac{1}{\sqrt{1-x^2}}\mathrm{d}x=\arccos(x)
$$
$$
\int\frac{1}{1+x^2}\mathrm{d}x=\arctan(x)
$$
将 $f(x)$ 代入，仍然使用链式求导法则：  
$$
\begin{aligned}
\int\frac{f'(x)}{\sqrt{1-f(x)^2}}\mathrm{d}x=\arcsin(f(x))\\
\int\frac{-f'(x)}{\sqrt{1-f(x)^2}}\mathrm{d}x=\arccos(f(x))\\
\int\frac{f'(x)}{1+f(x)^2}\mathrm{d}x=\arctan(f(x))\\
\end{aligned}
$$


这里都属于初步的反函数求导法则，不需要其他分析学技巧。  

---
至此，前文所述都属于形式幂级数的最基本内容。

<div STYLE="page-break-after: always;"></div>

# PART 3 —— 多项式相关算法
## Algorithm- $\text {I}$ -牛顿迭代
### AI-1 泰勒公式
$$
f(x)=f (x_0)+f' (x_0)(x-x_0)+\frac{f'' (x_0)}{2!}(x-x_0)^2+\dots+\frac{f^{(n)}(x_0)}{n!}(x-x_0)^n+o ((x-x_0)^n)
$$
其中当 $f(x_0)$ 无穷次可导时，则为泰勒级数：
$$
f(x)=\sum_{i\ge0}\frac{f^{(i)}(x_0)(x-x_0)^i}{i!}
$$
$x_0$ 取 $0$ 时即为麦克劳林级数：  
$$
f(x)=\sum_{i\ge0}\frac{f^{(i)}(0)}{i!}x^i
$$
扩展到二元的情况，有：
$$
f(x+\Delta x,y+\Delta y)=\sum_{i\ge 0}\frac{1}{i!}\left(\Delta x\frac{\partial}{\partial x }\Delta y\frac{\partial}{\partial y }\right)^if(x,y)
$$
### AI-2 多项式牛顿迭代
给你一个二元多项式 $G(x,y)$，求出 $G(x,f(x))\equiv0\pmod{x^n}$ 时的 $f(x)$ 的表达式，$f$ 亦为多项式。  
其中 $G$ 满足：  
+ $\exists \xi,\text{s.t.} G(0,\xi)=0\land\dfrac{\partial G}{\partial y}(0,\xi)\neq0$。  
其实后面那个偏导式的意思即为：$G(x,y)$ 在 $x=0$ 时不恒为 $0$。  
依然考虑倍增法：  
假设 $G(x,g(x))\equiv0\pmod{x^{\lceil\frac{n}{2}\rceil}}$。  
将 $G(x,f(x))$ 对在 $f(x)=g(x)$ 处进行泰勒展开，有：  
$$
G(x,f(x))=\sum_{i\ge0}\frac{\frac{\partial^iG}{\partial y^i}(x,g(x))}{i!}(f(x)-g(x))^i\equiv0\pmod{x^n}
$$
因为有 $f(x)\equiv g(x)\pmod{x^{\lceil\frac{n}{2}\rceil}}$，所以 $(f(x)-g(x))$ 的最低非零次项至少是 $x^{\lceil\frac{n}{2}\rceil}$。   
所以对 $\forall i>1$，都有 $(f(x)-g(x))^i\equiv 0\pmod{x^n}$。
所以原式等价于：
$$
G(x,g(x))+(f(x)-g(x))\frac{\partial G}{\partial y}(x,g(x))\equiv0\pmod{x^n}
$$
移项之后得到：
$$
f(x)\equiv g(x)-\frac{G(x,g(x))}{\frac{\partial G}{\partial y}(x,g(x))}\pmod{x^n}
$$
倍增计算即可。    
其实这个式子就是牛顿迭代的式子。  
初始条件是，$n=1$ 时，$\xi$ 即为解。
### AI-3 具体应用
这里举几个例子。  
#### AI-3-1 多项式求逆  
设我们要求出 $F(x)^{-1}$，那么构造 $G(x,y)=y^{-1}-F(x)$。   
所以：  
$$
f(x)\equiv g(x)-\frac{g(x)^{-1}-F(x)}{-g(x)^{-2}}=2g(x)-F(x)g(x)^{2}=g(x)(2-F(x)g(x))\pmod{x^n}
$$
与之前的推导结果一致。  
#### AI-3-2 多项式 $\exp$
构造 $G(x,y)=\ln(y)-F(x)$。  
$$
f(x)\equiv g(x)-\frac{\ln(g(x))-F(x)}{g(x)^{-1}}=g(x)(1-\ln(g(x))-F(x))\pmod{x^n}
$$
证明了上一章未证明的公式。  
## Algorithm- $\text {II}$ -Chirp-Z 变换
考虑 NTT 或 DFT 的过程，都是将原多项式 $f(x)$ 转化为点值表达式，其中点值取单位根或原根的若干次幂。   
不妨对该过程进行扩展：考虑任意一个非零数 $c$，求出 $f(c^0),f(c^1),\cdots,f(c^{m-1})$。  
首先，有一个显然的引理：$ij=\dbinom{i+j}{2}-\dbinom{i}{2}-\dbinom{j}{2}$。   
然后得到式子：  
$$
\begin{aligned}
f(c^j)&=\sum_{i=0}^{n-1}a_ic^{ij}\\
&=\sum_{i=0}^{n-1}a_ic^{\binom{i+j}{2}-\binom{i}{2}-\binom{j}{2}}\\
&=c^{-\binom{j}{2}}\sum_{i=0}^{n-1}a_ic^{\binom{i+j}{2}-\binom{i}{2}}\\
&=c^{-\binom{j}{2}}\sum_{i=0}^{n-1}\frac{a_i}{\binom{i}{2}}c^{\binom{i+j}{2}}\\
\end{aligned}
$$
设 $u_i=\frac{a_i}{\binom{i}{2}},v_i=c^{\binom{n+m-2-i-j}{2}}$。  
则有：  
$$
\begin{aligned}
&=c^{-\binom{j}{2}}\sum_{i=0}^{n-1}u_iv_{n+m-2-i-j}\\
&=c^{-\binom{j}{2}}\sum_{x+y=n+m-2-j}u_xv_y\\
&=c^{-\binom{j}{2}}(u*v)_{n+m-2-j}\\
\end{aligned}
$$
如果下标超过了定义域那直接赋 $0$，对于 $(u*v)$ 的计算使用 NTT。  
## Algorithm- $\text {III}$ -上升幂与下降幂
定义：  
$$
x^{\overline{n}}=\displaystyle\prod_{i=0}^{n-1}(x+i)=\frac{(x+n-1)!}{(x-1)!}
$$
$$
x^{\underline{n}}=\displaystyle\prod_{i=0}^{n-1}(x-i)=\frac{x!}{(x-n)!}
$$
此后我们重新规定 $\dbinom{n}{m}=\dfrac{n^{\underline{m}}}{m!}$，不难发现，在 $n\in \mathbb{N}$ 时与原先的定义是等价的，但是该式子可以将组合数直接扩展至 $n\in\mathbb{R}$ 而无需引入 $\Gamma$ 函数。  
这样便可以扩展二项式定理至指数为实数的情况，例如 $(a+b)^{\frac{1}{2}}$。  
由此进行扩展，不难得出，  
负的下降幂即为：  
$$
x^{\underline{-n}}=\frac{1}{(x+1)^{\overline{m}}}
$$
负上升幂同理。  
下降幂有一个极好的性质，即其差分的结果与普通幂求导的结果极为相似，求和与积分亦然。这个性质在很多和式变换中可以极大地方便计算。
## Algorithm- $\text {V}$ -分治乘法
对于高次多项式乘低次多项式的情况下，FFT 和 NTT 或许反而不优。  
例如，计算 $n$ 次式 $f(x)$ 乘 $1$ 次式 $(ax+b)$ 来说，做 NTT 需要 $O(n\log n)$ 次运算，但是直接计算 $axf(x)+bf(x)$ 只包含数乘以及多项式加法的开销，复杂度被降成 $O(n)$。  
此方法只有在另一低次式的项数小于 $\log n$ 时才有效。  
不妨进行一些扩展。  
若给定 $n$ 个低次多项式 $f_1(x),f_2(x),\cdots,f_n(x)$，求出 $\displaystyle\prod_{i=1}^nf_i(x)$。  
朴素的方法是对这些多项式扫一遍，依次用 NTT 进行相乘，复杂度为 $O(n^2\log n)$，无法接受。  
根据前文所述方法，直接相乘，可将复杂度降为 $O(n^2)$。  
我们假设 $\displaystyle\prod_{i=1}^{\frac{n}{2}}f_i(x),\displaystyle\prod_{i=\frac{n}{2}+1}^nf_i(x)$ 已经计算好了，那么将这两个多项式进行乘法的复杂度是 $O(n\log n)$。  
由主定理，该算法的复杂度 $T(n)$ 相当于每次递归拆成两部分，合并复杂度 $O(n\log n)$，总复杂度为 $T(n)=2T(\frac{n}{2})+O(n\log n)=O(n\log^2n)$。  
可以接受。  
算法的结构如下图所示。  
![bbb|350](https://cdn.luogu.com.cn/upload/image_hosting/8bfpxdew.png)  
其结构与具体流程类似数据结构线段树。  

## Algorithm- $\text {IV}$ -多项式平移
其实本质是形式幂级数复合的“弱化版“。  
给定一个多项式 $f(x)=\displaystyle\sum_{i=0}^{n-1}a_ix^i$，求出多项式 $f(x+c)$ 。  
这里只介绍最快速的方法：二项式定理法。  
$$
\begin{aligned}
f(x+c)&=\sum_{i=0}^{n-1}a_i(x+c)^i\\
&=\sum_{i=0}^{n-1}a_i\sum_{j=0}^i\binom{i}{j}x^jc^{i-j}\\
&=\sum_{j=0}^{n-1}x^j\sum_{i=j}^{n-1}\binom{i}{j}a_ic^{i-j}\\
&=\sum_{j=0}^{n-1}x^j\sum_{i=j}^{n-1}\frac{i^{\underline{j}}}{j!}a_ic^{i-j}\\
&=\sum_{j=0}^{n-1}x^j\sum_{i=j}^{n-1}\frac{i!}{j!(i-j)!}a_ic^{i-j}\\
&=\sum_{j=0}^{n-1}\frac{x^j}{j!}\sum_{i=j}^{n-1}i!a_i\frac{c^{i-j}}{(i-j)!}
\end{aligned}
$$
设 $u_i=(n-1-i)!a_{n-1-i},v_i=\dfrac{c^i}{i!}$。  
则原式为 $\displaystyle\sum_{j=0}^{n-1}\frac{x^j}{j!}\sum_{i=j}^{n-1}u_{n-1-i}v_{i-j}$。    
再化简：  
$$
\sum_{j=0}^{n-1}\frac{x^j}{j!}\sum_{x+y=n-1-j}u_{x}v_{y}
$$
于是我们用 NTT 计算 $(u*v)$，翻转后每一位乘上 $(j!)^{-1}$ 即为 $f(x+c)$ 的系数。  
## Algorithm- $\text {V}$ -插值  
在高中数学的数列一章中，我们经常需要根据某个数列的前若干项来计算其通项公式。  
一般来说，这些数列的规律都易于观察，例如：${a_n}=\{1,3,7,13,21,31,\cdots\}$，容易观察出其通项公式为 $a_n=x^2-x+1$。    
那么，机械化的计算机该如何解决此类问题？  
或者是，当数列的前若干难以得到简单规律时，该如何得出通项公式？  
进一步的，若给出连续函数的某一些点值，如何求出该函数的某一个可能值？  
我们需要引入新的公式。  
插值，即给定 $n$ 个点值 $x_1,x_2,\cdots,x_n$ 以及 $n$ 个函数值 $y_1,y_2,\cdots,y_n$，求出一个连续函数 $f(x)$，使得：$\forall i\in[1,n]\cap\mathbb{Z},f(x_i)=y_i$。   
主要用于将离散的数据连续化以进行估算，在实际生活中应用相对较广。  
不难发现，IDFT、INTT 其实就是 $x_i$ 为某些特定数时的插值。
### AV-1 线性插值
朴素的构造方法是，对于这 $n$ 个点 $(x_1,y_1),(x_2,y_2),\cdots,(x_n,y_n)$，按横坐标排序后，横坐标相邻的两个点连一条线段，左右端点之外的部分随便取。  
例如下图：  
![ccc|300](https://cdn.luogu.com.cn/upload/image_hosting/9aeowrg7.png)
### AV-2 多项式插值
如果需要使得 $f(x)$ 是一个多项式函数，如下图：  
![ddd|300](https://cdn.luogu.com.cn/upload/image_hosting/k8whha1c.png)

直接的想法是构造方程组：
$$
\begin{cases}
a_0+a_1x_1+a_2x_1^2+\cdots+a_{n-1}x_1^{n}=y_1\\
a_0+a_1x_2+a_2x_2^2+\cdots+a_{n-1}x_2^{n}=y_2\\
\quad\quad\quad\quad\quad\quad\quad\quad\quad\vdots\\
a_0+a_1x_n+a_2x_n^2+\cdots+a_{n-1}x_n^{n}=y_n\\
\end{cases}
$$
直接运用高斯消元解决，复杂度 $O(n^3)$ ，过于昂贵，无法接受。  
于是我们有拉格朗日插值公式：
$$
f(x)=\sum_{i=1}^{n}y_i\prod_{j\neq i}\frac{x-x_j}{x_i-x_j}
$$
验证：
$$
f(x_c)=\sum_{i=1}^{n}y_i\prod_{j\neq i}\frac{x_c-x_j}{x_i-x_j}
$$
当 $i\neq c$ 时，必有一项的分子是 $x_c-x_c=0$，故后面的连乘式一定为 $0$。  
所以只有 $i=c$ 是，后面的连乘式计算结果为 $1$，故答案为 $y_i$，符合要求。       
计算的复杂度为 $O(n^2)$。  
还有另一种方法称为牛顿插值，支持 $O(n)$ 插入新点，限于篇幅，此处不过多描述。   
除多项式插值外，还有三角插值，利用傅里叶级数进行操作，感兴趣的可以自行查阅相关资料。     
插值在在动态规划优化方面也有相应应用。
## Algorithm- $\text {VI}$ -连续点值平移
给你 $f(0),f(1),f(2),\cdots,f(n-1)$ 的值，求出 $f(\delta),f(\delta+1),f(\delta+2),\cdots,f(\delta+n-1)$ 的值。  
这里仅讨论 $\delta\ge n$ 的情况。  
显然的做法是，先进行拉格朗日插值将 $f$ 的表达式求出来，然后对 $c,c+1,\cdots,c+n-1$ 做多点求值。  
复杂度瓶颈是拉格朗日插值的 $O(n^2)$，可以用快速插值做到 $O(n\log^2n)$。  
这里有一种更快的 $O(n\log n)$ 做法。  
首先，仍然根据拉格朗日插值公式有：  
$$
f(x)=\sum_{i=0}^{n-1}f(i)\prod_{j\neq i}\frac{x-j}{i-j}
$$
所以平移之后得到：
$$
\begin{aligned}
f(x+\delta)&=\sum_{i=0}^{n-1}f(i)\prod_{j\neq i}\frac{x+\delta-j}{i-j}\\
&=\sum_{i=0}^{n-1}f(i)\frac{\prod_{j\neq i}(x+\delta-j)}{\prod_{j\neq i}(i-j)}\\
&=\sum_{i=0}^{n-1}f(i)\frac{\prod_{j\neq i}(x+\delta-j)}{(-1)^{n-1-i}i!(n-1-i)!}\\
&=\sum_{i=0}^{n-1}f(i)\frac{\prod_{j\neq i}(x+\delta-j)}{(-1)^{n-1-i}i!(n-1-i)!}\\
&=\sum_{i=0}^{n-1}\frac{f(i)(x+\delta)!}{(-1)^{n-1-i}i!(n-1-i)!(x+\delta-n)!(x+\delta-i)}\\
&=\frac{(x+\delta)!}{(x+\delta-n)!}\sum_{i=0}^{n-1}\frac{f(i)}{(-1)^{n-1-i}i!(n-1-i)!(x+\delta-i)}\\
&=\frac{(x+\delta)!}{(x+\delta-n)!}\sum_{i=0}^{n-1}\frac{f(i)}{(-1)^{n-1-i}i!(n-1-i)!}\frac{1}{x+\delta-i}\\
\end{aligned}
$$
设 $u_i=\dfrac{f(i)}{(-1)^{n-1-i}i!(n-1-i)!},v_i=\dfrac{1}{\delta-n+1+i}$，超出定义域设为 $0$。  
原式则为：
$$
\begin{aligned}
&=\frac{(x+\delta)!}{(x+\delta-n)!}\sum_{i=0}^{n-1}u_iv_{n-1+x-i}\\
&=\frac{(x+\delta)!}{(x+\delta-n)!}\sum_{i=0}^{n-1+x}u_iv_{n-1+x-i}\\
&=\frac{(x+\delta)!}{(x+\delta-n)!}(u*v)_{n-1+x}\\
\end{aligned}
$$
利用 NTT 做出 $(u*v)$，预处理阶乘并计算即可，复杂度瓶颈在 NTT 为 $O(n\log n)$。  

> [!note] 预处理
> 预处理，即为在计算之前就进行的提前处理，如：在 NTT 之前先将从 $1\sim n$ 所有整数的阶乘根据 $n!=n(n-1)!$ 递推全部得到，这样就可以以 $O(n)$ 的时间复杂度计算 $u_i$ 和 $v_i$。
<div STYLE="page-break-after: always;"></div>

## Algorithm- $\text {VII}$ -多项式多点求值

给定 $n$ 次多项式 $f(x)$ 和 $n$ 个数 $a_1,a_2,\cdots,a_n$。  
求出 $f(a_1),f(a_2),\cdots,f(a_n)$ 的值。
### AVII-1 多项式取模方法  
将这 $n$ 个数分成两部分：  
$$
\begin{aligned}
&A_0=\set{a_1,a_2,\cdots,a_{\lfloor\frac{n}{2}\rfloor}}\\
&A_1=\set{a_{\lfloor\frac{n}{2}\rfloor+1},\cdots,a_{n-1},a_n}\\
\end{aligned}
$$
构造辅助多项式 $t_0(x)=\displaystyle\prod_{a_i\in A_0}(x-a_i),t_1(x)=\displaystyle\prod_{a_i\in A_1}(x-a_i)$。  
很显然，当 $x\in A_0$ 时，$t_0(x)=0$。  
对于 $f(x),t_0(x)$ 进行带余除法，得到 $f(x)=t_0(x)Q_0(x)+r_0(x)$。   
当 $t_0(x)\neq0$ 时就是正常带余除法，$t_0(x)=0$ 时则有 $f(x)=r_0(x)$。  

同理可得当 $x\in A_1$ 时 $f(x)=r_1(x)$。  
所以我们将这个问题分治成了两个子问题，即对于 $A_0,A_1$ 中的所有点，分别求出对于 $r_0(x),r_1(x)$ 的值。
也就是说，假设对于次数与点的个数为 $n$ 时，设 $T(n)$ 为多项式多点求值的复杂度，则 
$$
T(n)=2T(\frac n 2)+O(n\log n)
$$
在分治的过程中，每次计算的规模减半并执行两次，每次向下递归的复杂度为 $O(n\log n)$ ——这是多项式取模所带来的复杂度。  
根据主定理，$T(n)=O(n\log^2n)$。  
辅助多项式 $t$ 分治乘法直接得到。  
### AVII-2 常数优化
多项式取模的时间复杂度是 $O(n\log n)$，但实际应用中需要进行求逆，翻转等一系列操作，常数开销大，因此可以使用更快捷的方法。    
对于数 $a$，我们有 $F(a)=[x^0](F(x)\bmod (x-a))$。    
考虑快速计算 $F(x)$ 对 $(x-a_1),(x-a_2),\cdots,(x-a_n)$ 取模的结果。    
设 $D_a(x)=x-a$，做带余除法得到 $F(x)=D_a(x)Q_a(x)+r$。    
注意到 $r$ 仅与 $[x_0]F(x),[x_0]D_a(x),[x_0]Q_a(x)$ 有关，而 $[x_0]F(x),[x_0]D_a(x)$ 是已知的，因此问题被规约为快速计算 $[x_0]Q_a(x)$。    
设 $D_{[l,r]}=\displaystyle\prod_{i=l}^r(x-a_i)$，$Q_{[l,r]}$ 为 $F(x)$ 对 $D_{[l,r]}$ 做带余除法得到的商式。    
根据多项式除法的公式，在本问题中的描述即为：
$$
\begin{aligned}
\tilde{F}(x)\equiv\tilde{D}_{[l,r]}(x)\tilde{Q}_{[l,r]}(x)\pmod{x^{n-r+l}}\\
\tilde{F}(x)\tilde{D}^{-1}_{[l,r]}(x)\equiv\tilde{Q}_{[l,r]}(x)\pmod{x^{n-r+l}}\\
\end{aligned}
$$
根据分治乘法，定义 $m=\lfloor\frac{l+r}{2}\rfloor$。  
由于有 $D_{[l,r]}(x)=D_{[l,m]}(x)D_{[m+1,r]}(x)$，故而 $\tilde{D}_{[l,r]}(x)=\tilde{D}_{[l,m]}(x)\tilde{D}_{[m+1,r]}(x)$，所以推论有 $\tilde{D}^{-1}_{[l,m]}(x)=\tilde{D}^{-1}_{[l,r]}(x)\tilde{D}_{[m+1,r]}(x),\ \tilde{D}^{-1}_{[m+1,r]}(x)=\tilde{D}^{-1}_{[l,r]}(x)\tilde{D}_{[l,m]}(x)$。  
因此有：     
$$
\begin{aligned}
\tilde{Q}_{[l,m]}(x)&\equiv\tilde{F}(x)\tilde{D}^{-1}_{[l,m]}(x)\pmod{x^{n-m+l}}\\
&\equiv\tilde{F}(x)\tilde{D}^{-1}_{[l,r]}(x)\tilde{D}_{[m+1,r]}(x)\pmod{x^{n-m+l}}\\
&\equiv\tilde{Q}_{[l,r]}(x)\tilde{D}_{[m+1,r]}(x)\pmod{x^{n-m+l}}\\
\end{aligned}
$$
同理亦有：  
$$
\begin{aligned}
\tilde{Q}_{[m+1,r]}(x)&\equiv\tilde{Q}_{[l,r]}(x)\tilde{D}_{[l,m]}(x)\pmod{x^{n-r+m-1}}\\
\end{aligned}
$$
$\tilde{{D}}^{-1}$ 可以分治乘法预处理。  
根据递归式，一直计算下去直到得出 $\tilde{Q}_{a}(x)$ 的常数项即可（或者直接是其本身，因为它是一个零次多项式）。  
复杂度没有变化，仍然是 $O(n\log^2 n)$，但每次向下递归从取模变了乘法，常数减小很多。  
由于只有 $[x^0]Q_{[l,r]}(x)$，即 $[x^{r-l}]\tilde{Q}_{[l,r]}(x)$ 有用，又因为 $\tilde{Q}_{[l,r]}(x)$ 之后只会乘以次数小于等于 $(r−l)$ 的多项式，也就是说，只需要保留 $\tilde{Q}_{[l,r]}(x)$ ​最高的 $(r-l)$ 位即可。  
注意，由于 $\tilde{Q}$ 是 $Q$ 取反的结果，所以保留最高的 $(r-l)$ 位后不可在低次项补 $0$，而是直接截断后 $(r-l)$ 位然后形成一个新多项式。  
与该方法等价的是应用转置原理（即 Tellegen 定理），根据 $a_1,a_2,\cdots,a_n$ 构成的范德蒙德矩阵进行递归求解，这里不做过多介绍。        
## Algorithm- $\text {VIII}$ -快速插值  
根据前文所述，朴素的拉格朗日插值的时间复杂度为 $O(n^2)$。  
这里介绍一种更加快速的处理插值的算法，多项式快速插值。  
回顾拉格朗日插值公式：  
$$
f(x)=\sum_{i=1}^{n}y_i\prod_{j\neq i}\frac{x-x_j}{x_i-x_j}
$$
设 $t(x)=\displaystyle\prod_{i=1}^n(x-x_i)$。  
于是 $\dfrac{t(x)}{x-x_i}=\displaystyle\prod_{j\neq i}(x-x_j)$。  
我们发现，当 $x\to x_i$ 时，原式的极限则为 $\displaystyle\prod_{j\neq i}(x_i-x_j)$。  
根据洛必达法则，$\displaystyle\prod_{j\neq i}(x_i-x_j)=\displaystyle\lim_{x\to x_i}\dfrac{t(x)}{x-x_i}=\displaystyle\lim_{x\to x_i}\dfrac{t'(x)}{1}=t'(x_i)$。  
所以以 $O(n)$ 的时间复杂度对 $t(x)$ 求导即可得出 $\displaystyle\prod_{j\neq i}(x_i-x_j)$。  
另一个证明方法是，根据数学归纳法容易得出扩展的求导乘法公式：
$$
\frac{\mathrm d}{\mathrm dx}\prod_{i=1}^nf_i(x)=\sum_{i=1}^nf_i'(x)\prod_{j\neq i}f(j)
$$
根据此公式直接可证明结论。  
于是我们有
$$
f(x)=\sum_{i=1}^{n}\frac{y_i}{t'(x_i)}\prod_{j\neq i}(x-x_j)
$$
但是这仍然是 $O(n^2)$，继续优化，考虑拆分：  
$f_l(x)=\displaystyle\sum_{i=1}^{\lfloor\frac{n}{2}\rfloor}\frac{y_i}{t'(x_i)}\prod_{j\neq i,j\le \lfloor\frac{n}{2}\rfloor}(x-x_j),f_r(x)=\displaystyle\sum_{i=\lfloor\frac{n}{2}\rfloor+1}^{n}\frac{y_i}{t'(x_i)}\prod_{j\neq i,j> \lfloor\frac{n}{2}\rfloor}(x-x_j)$。  
以及 $t_l(x)=\displaystyle\prod_{i=1}^{\lfloor\frac{n}{2}\rfloor}(x-x_i),t_r(x)=\displaystyle\prod_{i=\lfloor\frac{n}{2}\rfloor+1}^{n}(x-x_i)$ 。   
递归到底层时，我们需要计算 $\frac{y_i}{t'(x_i)}$ 的值，对于分母 $t'(x_i)$，多点求值进行预处理即可，复杂度 $O(n\log^2n)$。  
则有 $f(x)=f_l(x)t_r(x)+f_r(x)t_l(x)$，分治计算，复杂度 $T(n)=2T(\frac{n}{2})+O(n\log n)=O(n\log^2n)$。  
<div STYLE="page-break-after: always;"></div>

# PART 4 —— 高阶多项式技巧

## Senior- $\text {I}$ -拉格朗日反演  
### SI-1 形式 Laurent 级数
回顾形式幂级数求逆的过程，只有当 $[x^0]f(x)$ 非零时，$f^{-1}(x)$ 才存在。  
如果我们想要计算 $(x^4+x^3)$ 的逆，可以将其因式分解成：$x^3(x+1)$，再进行求逆，得到 $x^{-3}(x+1)^{-1}$。  
$(x+1)^{-1}$ 是可求的，然而我们无法以形式幂级数的形式处理 $x^{-3}$。  
因此，不妨直接对形式幂级数进行扩展。  
先考察数域 $\mathbb{F}$ 上的形式幂级数的定义：集合 $P=\set{\displaystyle\sum_{i\ge 0}a_ix^i|a_i\in\mathbb{F}}$ 中的元素。  
$\mathbb{F}[[x]]$ 即为 $(P,+,\times)$。  

定义数域 $\mathbb{F}$ 上的形式 Laurent 级数为集合 $L=\set{\displaystyle\sum_{i\ge \beta}a_ix^i|a_i\in\mathbb{F},\beta\in\mathbb{Z}}$。    
其中文译名为洛朗级数。  
对于一个形式 Laurent 级数 $f$，定义 $\operatorname{ord} f$ 为 $f$ 的最低非零次项。  
注意，$\operatorname{ord} f$ 不一定与 $\beta$ 相等，因为 $a_i$ 有可能为 $0$。  
通俗地讲，形式幂级数即为形式 Laurent 级数在最低次大于等于 $0$ 的子集。    
形式 Laurent 级数环记作 $\mathbb{F}((x))$。   
根据定义，$\mathbb{F}[[x]],\mathbb{F}((x))$ 都是整环。  
#### SII-1-1 加法
形式 Laurent 级数的加（减）法亦为同项相加，即：
$$
h=f+g=\sum_{i\ge\max(\operatorname{ord }f,\operatorname{ord }g)}(a_i+b_i)x^i
$$
#### SII-1-2 乘法
若 $h=fg$，则 $\operatorname{ord }h=\operatorname{ord }f+\operatorname{ord }g$。  
$$
h=\left(\sum_{i\ge\operatorname{ord}f}a_ix^i\right)\left(\sum_{i\ge\operatorname{ord}g}b_ix^i\right)
$$
仿照形式幂级数的乘法，化简，最终得到：
$$
h=\sum_{s>\operatorname{ord}f+\operatorname{ord}g}\sum_{i+j=s}a_ib_jx^{s}
$$
其实算是一种广义上的卷积。    

形式 Laurent 级数的形式导数与复合与形式幂级数相差不大。  
### SI-2 形式留数  
定义：对于一个形式 Laurent 级数 $f$，$\operatorname{res} f:=[x^{-1}]f$。  
即 $f$ 的 $-1$ 次项。  
需要提前明确的是，**形式留数具有线性性**。  
同时，形式留数还有一些常用性质。  

---
例如，对 $\forall f\in\mathbb{F}((x)),\operatorname{res} f'=0$。  
这个性质是平凡的。

---
其根据形式导数进行引申的的性质很丰富。  
下式是关于导数乘积法则的性质。
$$
\forall f,g\in\mathbb{F}((x)),\operatorname{res}(fg')=-res(f'g)
$$
证明较为容易： $\operatorname{res}((fg)')=0=\operatorname{res}(fg'+f'g)=\operatorname{res}(fg')+\operatorname{res}(f'g)$。  
最后一步根据形式留数的线性性进行推导。    

---
其他性质如：
$$
\operatorname{res}\frac{f'}{f}=\operatorname{ord}f
$$
证明考虑直接将分子和分母展开，约分即可。

---
最后是一条重要的公式：
$$
\forall f\in\mathbb{F}((x)),g\in\mathbb{F}[[x]],\operatorname{res}((f\circ g)\times g')=\operatorname{res} f\operatorname{ord}g
$$
注意，形式幂级数的 $\operatorname{ord}$ 也是其最低非零次项。  
证明：  
因为形式留数具有线性性，所以仅需考虑 $f=x^n$ 的情况。  
分类讨论，首先考虑 $n\neq -1$ 的情况。
$$
\begin{aligned}
&\operatorname{res}(g^{n}g')\\
=&\operatorname{res}(\frac{1}{n+1}(g^{n+1})')\\
=&\frac{1}{n+1}\operatorname{res}((g^{n+1})')\\
=&0
\end{aligned}
$$
再考虑 $n=-1$ 的情况。  
$$
\begin{aligned}
&\operatorname{res}(\frac{g'}{g})\\
=&\operatorname{ord}g\\
=&\operatorname{res}f\operatorname{ord}g
\end{aligned}
$$
于是原结论证毕。
### SI-3 拉格朗日反演
这是一个处理复合逆的利器.
首先给出一个引理：  
 对于一个形式 Laurent 级数 $F$，若 $[x^0]F=0,[x^1]F\neq 0$，则必定存在形式 Laurent 级数 $G$，使得 $F\circ G=x$，此时 $G$ 则为 $F$ 的**复合逆**，记作 $F^{\langle-1\rangle}$。    

 > [!note] 关于复合逆 
 > 注意到复合逆的定义是 $F\circ G=x$ 而非 $F\circ G=1$，首先这是因为如果条件是 $F\circ G=1$，不难证明只有 $[x^0]F=1$ 时具有唯一平凡解，即 $G=0$，显然没有讨论价值。  
 > 同时，对于复合运算来说，其构成的幺半群 $(\mathbb{F}((x)),\circ)$ 的单位元为恒等映射 $x$，此时可称为 $\mathrm{id}$，而非 “$1$ “。

根据以上引理，可以得出常见的 $4$ 个版本的拉格朗日反演，这里假定 $G=F^{\langle -1 \rangle}\in \mathbb{F}((x)),\Lambda\in\mathbb{F}((x))$：  
$$
\begin{aligned}
&n[x^n]F^k=k[x^{-k}]G^{-n}\\
&n[x^n](\Lambda\circ F)=[x^{n-1}]\frac{\mathrm{d}\Lambda}{\mathrm{d}x}(\frac{x}{G})^n\\
&—————————————\\
&[x^n]F^k=[x^{-k-1}]\frac{\mathrm{d}G}{\mathrm{d}x}G^{-n-1}\\
&[x^n](\Lambda\circ F)=[x^{n}]\Lambda\frac{\mathrm{d}G}{\mathrm{d}x}(\frac{x}{G})^{n+1}\\
\end{aligned}
$$
此处仅证明公式 $2$，其为公式 $1$ 的推广，算是很详细的证明了：  
$$
\begin{aligned}
\Rightarrow&\Lambda\circ (F\circ G)=\Lambda\\
\Rightarrow&(\Lambda\circ F)\circ G=\Lambda\\
\Rightarrow&(\frac{\mathrm{d}(\Lambda\circ F)}{\mathrm{d}x}\circ G)\frac{\mathrm{d}G}{\mathrm{d}x}=\frac{\mathrm{d}\Lambda}{\mathrm{d}x}\\
\Rightarrow&\frac{\mathrm{d}G}{\mathrm{d}x}\sum_{i=0}^{\infty}[x^i]\frac{\mathrm{d}(\Lambda\circ F)}{\mathrm{d}x}G^i=\frac{\mathrm{d}\Lambda}{\mathrm{d}x}\\
\Rightarrow&\frac{\mathrm{d}G}{\mathrm{d}x}\sum_{i=0}^{\infty}[x^i]\frac{\mathrm{d}(\Lambda\circ F)}{\mathrm{d}x}G^{i-n}=G^{-n}\frac{\mathrm{d}\Lambda}{\mathrm{d}x}\\
\Rightarrow&\sum_{i=0}^{\infty}[x^i]\frac{\mathrm{d}(\Lambda\circ F)}{\mathrm{d}x}G^{i-n}\frac{\mathrm{d}G}{\mathrm{d}x}=G^{-n}\frac{\mathrm{d}\Lambda}{\mathrm{d}x}\\
\Rightarrow&\operatorname{res}\left(\sum_{i=0}^{\infty}[x^i]\frac{\mathrm{d}(\Lambda\circ F)}{\mathrm{d}x}G^{i-n}\frac{\mathrm{d}G}{\mathrm{d}x}\right)=\operatorname{res}\left(G^{-n}\frac{\mathrm{d}\Lambda}{\mathrm{d}x}\right)\\
\Rightarrow&\sum_{i=0}^{\infty}[x^i]\frac{\mathrm{d}(\Lambda\circ F)}{\mathrm{d}x}\operatorname{res}\left(G^{i-n}\frac{\mathrm{d}G}{\mathrm{d}x}\right)=\operatorname{res}\left(G^{-n}\frac{\mathrm{d}\Lambda}{\mathrm{d}x}\right)\\
\Rightarrow&\sum_{i=0}^{\infty}[x^i]\frac{\mathrm{d}(\Lambda\circ F)}{\mathrm{d}x}[i-n=-1]=\operatorname{res}\left(G^{-n}\frac{\mathrm{d}\Lambda}{\mathrm{d}x}\right)\\
\Rightarrow&[x^{n-1}]\frac{\mathrm{d}(\Lambda\circ F)}{\mathrm{d}x}=[x^{-1}]\left(G^{-n}\frac{\mathrm{d}\Lambda}{\mathrm{d}x}\right)\\
\Rightarrow&[x^{n-1}]\frac{\mathrm{d}(\Lambda\circ F)}{\mathrm{d}x}=[x^{n-1}]\frac{\mathrm{d}\Lambda}{\mathrm{d}x}(\frac{x}{G})^n\\
\Rightarrow&[x^{n-1}]\frac{\mathrm{d}(\Lambda\circ F)}{\mathrm{d}x}=[x^{n-1}]\frac{\mathrm{d}\Lambda}{\mathrm{d}x}(\frac{x}{G})^n\\
\Rightarrow&n[x^{n}](\Lambda\circ F)=[x^{n-1}]\frac{\mathrm{d}\Lambda}{\mathrm{d}x}(\frac{x}{G})^n\\
\end{aligned}
$$
最后一步基于形式 Laurent 级数复合形式 Laurent 级数仍然是形式 Laurent 级数以及其形式导数的定义而得出。      
但是，上面的式子在 $n=0,k<0$ 时无法给出常数项的值，因此拓展到公式 $3,4$。   
其证明过程与公式 $2$ 证明过程类似。  
## Senior- $\text {II}$ -常系数齐次线性递推
### SII-1 斐波那契数列快速递推
注意，与一般的定义不同，我们认为数列 $\{a_n\}$ 的首项为 $a_0$。  
故此处假定斐波那契数列斐波那契数列的前两项为 $f_0=0,f_1=1$，递推公式为：$f_i=f_{i-1}+f_{i-2}$。  
如果我们要计算 $f_n$，直接的方法是通过递推式逐一向后递推，复杂度为 $O(n)$。  
根据矩阵乘法公式，若矩阵 $C=A\times B$，则：  
$$
C_{ij}=\sum_{k=1}^nA_{ik}B_{kj}
$$
因此可以得出：  
$$
\begin{bmatrix}
0 & 1\\
1 & 1\\
\end{bmatrix}
\begin{bmatrix}
f_{i-1}\\ f_{i}
\end{bmatrix}
=
\begin{bmatrix}
f_{i}\\f_{i+1}
\end{bmatrix}
$$
所以：
$$
\begin{bmatrix}
f_{n}\\f_{n+1}
\end{bmatrix}=
\begin{bmatrix}
0 & 1\\
1 & 1\\
\end{bmatrix}^{n}
\begin{bmatrix}
0 \\ 1\\
\end{bmatrix}


$$
第二个问题是，考虑如何快速计算 $\begin{bmatrix}0 & 1\\1 & 1\\\end{bmatrix}^{n}$。  
直接相乘的复杂度是 $O(n)$。  
考虑优化，对于任意底数 $a$（不要求是数域中的元素）， $a^{n}=(a^{\frac{n}{2}})^2\times a^{n\bmod 2}$，根据该式递归求解，由于平方可以 $O(1)$ 求解，因此每次递归问题规模就被缩小一半，一共最多递归 $O(\log n)$ 次，故时间复杂度为 $O(\log n)$。  
由于 $k$ 阶矩阵乘法的复杂度是 $k^3$，因此计算 $f_n$ 的复杂度即为 $O(k^3\log n),k=2$。  
这是 $k=2$ 的情况。  
考虑一般情况，若一个数列 $f$ 满足 $f_i=\displaystyle\sum_{j=1}^dc_jf_{i-j}$，给定 $f_0,f_1,f_2,\cdots,f_{d-1}$，如何快速求出 $f_n(n\ge d)$？  
注意，此时我们称 $f$ 为一个**常系数齐次线性递推数列**。  
若依然沿用矩阵乘法，当 $d$ 过大时，复杂度将无法接受。    
### SII-2 Fiduccia 算法
#### SII-2-1 特征多项式
首先，行列式定义为：
$$
\begin{vmatrix}
A_{11}&a_{12}&...&a_{1 n} \\
A_{21}&a_{22}&...&a_{2 n} \\
\vdots&\vdots&&\vdots\\
A_{n 1}&a_{n 2}&\dots&a_{nn}
\end{vmatrix}
=\displaystyle\sum_{j_1 j_2\dots j_n}(-1)^{\tau (j_1 j_2\dots j_n)}a_{1 j_1}a_{2 j_2}\dots a_{nj_n}
$$
我们记一个 $n\times n$ 的矩阵 $A$ 的特征多项式为 $p_A(x)$。  
其定义为 $x\mathrm{I}_n-A$ 的行列式的值，其中 $\mathrm{I}_n$ 为 $n$ 阶单位矩阵。
即为：
$$
p_A(x)=\det(x\mathrm{I}_n-A)=
\begin{vmatrix}
x-a_{11}&-a_{12}&\cdots&-a_{1n}\\
-a_{21}&x-a_{22}&\cdots&-a_{2n}\\
\vdots&\vdots&&\vdots\\
-a_{n1}&-a_{n2}&\cdots&x-a_{nn}\\
\end{vmatrix}
$$
需要注意的是，$x$ 既可以是普通多项式中的“形式记号”，也可以是一个矩阵，它只是一个**参量**。  
通常来说，这里的 $x$ 应当用 $\lambda$，为了更清晰地体现其多项式性质，此处仍然使用 $x$。
#### SII-2-1 Cayley-Hamilton 定理（C-H 定理）
##### 表述
实际上表述很简单：对于任意的 $n\times n$ 的矩阵 $A$，都有 $p_A(A)=0$。  

> [!error] **易错点**
> 当代入到特征多项式的定义式，即：$\det(A\mathrm{I}_n-A)$ 时，将 $A\mathrm{I}_n$ 看作是矩阵乘法，于是 $\det(A\mathrm{I}_n-A)=\det(A-A)=\det(0)=0$，看起来是一个平凡结论。  

实际上，该定理并不平凡，我们需要将 $A\mathrm{I}_n$ 看作是**标量乘法**，而非矩阵乘法，同时，如果参量为矩阵，最终得到的 $0$ 应当表述为 “$\textbf{0}$”（零矩阵，粗体），即零矩阵，而非标量 $0$。   
换句话说，在最后计算多项式值之前，矩阵 $A$ 是与一个正常的形式记号 $x$ 无异的。
##### 证明前置定义
**【伴随矩阵】**
在证明之前，仍需要一些定义：
首先定义一个 $n$ 阶矩阵 $K$ 的伴随矩阵 $K^*$ 为：
$$
\begin{bmatrix}
A_{11}&A_{21}&\cdots &A_{n1}\\
A_{12}&A_{22}&\cdots &A_{n2}\\
\vdots&\vdots&&\vdots\\
A_{1n}&A_{2n}&\cdots &A_{nn}\\
\end{bmatrix}
$$
其中 $A_{ij}$ 为 $K$ 的 $(i,j)$ 元的代数余子式，定义为 $(-1)^{i+j}M_{ij}$，即原矩阵划去第 $i$ 行和第 $j$ 列后构成的矩阵的行列式乘 $(-1)^{i+j}$。  
代数余子式的经典公式是：$|A|=\displaystyle\sum_{j=1}^n a_{ij}A_{ij}=\displaystyle\sum_{i=1}^n a_{ij}A_{ij}$。  
这里作为熟知结论，即行列式按第 $i$ 行或第 $j$ 列展开公式。
**【矩阵多项式】**
注意，矩阵多项式为**系数**是矩阵的多项式，不要混淆。  
如果一个矩阵 $A$ 中的每一个元素都是关于 $\lambda$ 的多项式，则 $A$ 必然可以被表示成：
$$
\sum_{i=0}^dA_i\lambda^i
$$
$A_0,A_1,\cdots,A_n$ 即为该矩阵多项式的系数，$d$ 为这些多项式中最高次项次数的最大值。  
不难证明，这样的分解是唯一的。
##### 证明
正式开始证明：
设 $W(x)$ 为 $x\mathrm{I}-A$ 的伴随矩阵。  
我们假定 $A$ 是 $n$ 阶矩阵。  
将 $W(x)$ 写成矩阵多项式的形式，则有 $W(x)=\displaystyle\sum_{i=0}^{n-1}W_ix^i$。  
最高次项显然为 $n-1$，因为 $W(x)$ 中的元素都是 $n$ 阶矩阵的代数余子式，少了一行和一列。  
由此得式：  
$$
\begin{aligned}
&W(x)(x\mathrm{I}-A)\\
=&\sum_{i=0}^{n-1}(W_ix^{i+1}-AW_ix^{i})\\
=&W_{n-1}x^n+\sum_{i=1}^{n-1}(W_{i-1}-AW_{i})x^i-AW_0
\end{aligned}
$$
不妨同时设 $p_A(x)=\displaystyle\sum_{i=0}^n a_ix^i$。  
将其转为矩阵，即为 $p_A(x)\mathrm{I}=\displaystyle\sum_{i=0}^n a_ix^i\mathrm{I}$。
根据伴随矩阵的母公式，即 $AA^*=\det(A)\mathrm{I}$ ，可得，$W(x)(x\mathrm{I}-A)=\det(x\mathrm{I}-A)\mathrm{I}=p_A(x)I$。  
母公式的证明直接代入矩阵乘法公式，根据行列式按行/列展开的公式易证。
因此我们有：
$$
W_{n-1}x^n+\sum_{i=1}^{n-1}(W_{i-1}-AW_{i})x^i-AW_0=\sum_{i=0}^n a_ix^i\mathrm{I}
$$
并且由于特征多项式一定是首一多项式，即 $a_n=1$，可得：
$$
\begin{bmatrix}
W_{n-1}\\
W_{n-2}-AW_{n-1}\\
W_{n-3}-AW_{n-2}\\
\vdots\\
W_{1}-AW_{2}\\
W_{0}-AW_1\\
AW_0
\end{bmatrix}
=
\begin{bmatrix}
\mathrm{I}\\
a_{n-1}\mathrm{I}\\
a_{n-2}\mathrm{I}\\
\vdots\\
a_2\mathrm{I}\\
a_1\mathrm{I}\\
a_0\mathrm{I}
\end{bmatrix}
$$
对于第 $i$ 行右乘 $A^{n-i+1}$ 得到：  
$$
\begin{bmatrix}
W_{n-1}A^n\\
W_{n-2}A^{n-1}-A^nW_{n-1}\\
W_{n-3}A^{n-2}-A^{n-1}W_{n-2}\\
\vdots\\
W_{1}A^2-A^3W_{2}\\
W_{0}A-A^2W_1\\
AW_0
\end{bmatrix}
=
\begin{bmatrix}
A^n\\
a_{n-1}A^{n-1}\\
a_{n-2}A^{n-2}\\
\vdots\\
a_2A^2\\
a_1A\\
a_0\mathrm{I}
\end{bmatrix}
$$
发现等号左边所有值相加为 $\textbf{0}$，右边所有值相加为 $p_A(A)$。  
得证。

#### SII-2-3 Fiduccia 算法
算法实现十分容易。  
对于常系数齐次线性递推数列 $f$ 来说，若 $f_i=\displaystyle\sum_{j=1}^dc_jf_{i-j}$，  
构造多项式 $\Gamma(x)=x^d-\displaystyle\sum_{i=0}^{d-1}c_{d-i}x^i$，  
则 $f_n=\displaystyle\sum_{i=0}^{d-1}[x^i](x^n \bmod \Gamma(x))f_i$。  
算法结束。

该算法在网络上可以找到大量的不严格的，感性化的证明。  
这里提供一个较为严格的证明：
对于常系数齐次线性递推数列 $f$ 来说，其转移式为：  
$$
\begin{bmatrix}
0&1&0&\cdots&0&0\\
0&0&1&\cdots&0&0\\
\vdots&\vdots&\vdots&&\vdots&\vdots\\
0&0&0&\cdots&1&0\\
0&0&0&\cdots&0&1\\
c_d&c_{d-1}&c_{d-2}&\cdots&c_2&c_1\\
\end{bmatrix}^n
\begin{bmatrix}
f_{0}\\
f_{1}\\
\vdots\\
f_{d-3}\\
f_{d-2}\\
f_{d-1}\\
\end{bmatrix}
=\begin{bmatrix}
f_{n}\\
f_{n+1}\\
\vdots\\
f_{n+d-3}\\
f_{n+d-2}\\
f_{n+d-1}\\
\end{bmatrix}
$$

记上式中的转移矩阵为 $A$。
然后，可以注意到，$f_n=\displaystyle\sum_{i=1}^dA_{1i}^nf_{i-1}$。  
也就是说，$f_n$ 仅仅与 $A^n$ 的第一行以及 $f_0,f_1,\cdots,f_{d-1}$ 有关系。    
我们设 $A^n$ 的第一行是 $\chi_1,\chi_2,\cdots,\chi_d$。  
也就有 $f_n=\displaystyle\sum_{i=1}^d\chi_if_{i-1}$。  
我们再设 $A^n=\displaystyle\sum_{i=1}^d\psi_iA^{i-1}$。    
于是得到：
$$
A^n\begin{bmatrix}
f_{0}\\
f_{1}\\
\vdots\\
f_{d-3}\\
f_{d-2}\\
f_{d-1}\\
\end{bmatrix}=\sum_{i=1}^d\psi_iA^{i-1}\begin{bmatrix}
f_{0}\\
f_{1}\\
\vdots\\
f_{d-3}\\
f_{d-2}\\
f_{d-1}\\
\end{bmatrix}
=
\begin{bmatrix}
f_{n}\\
f_{n+1}\\
\vdots\\
f_{n+d-3}\\
f_{n+d-2}\\
f_{n+d-1}\\
\end{bmatrix}
$$
故有：
$$
\begin{bmatrix}
f_{n}\\
f_{n+1}\\
\vdots\\
f_{n+d-3}\\
f_{n+d-2}\\
f_{n+d-1}\\
\end{bmatrix}
=
\sum_{i=1}^d\left(\psi_iA^{i-1}\begin{bmatrix}
f_{0}\\
f_{1}\\
\vdots\\
f_{d-3}\\
f_{d-2}\\
f_{d-1}\\
\end{bmatrix}
\right)
=
\sum_{i=1}^d\psi_i\left(A^{i-1}\begin{bmatrix}
f_{0}\\
f_{1}\\
\vdots\\
f_{d-3}\\
f_{d-2}\\
f_{d-1}\\
\end{bmatrix}\right)
=
\sum_{i=1}^d\psi_i\begin{bmatrix}
f_{i-1}\\
f_{i}\\
\vdots\\
f_{i+d-4}\\
f_{i+d-3}\\
f_{i+d-2}\\
\end{bmatrix}
$$
只取它的第一行，我们就有：
$$
f_n=\sum_{i=1}^d\psi_if_{i-1}
$$
那我们不妨认为 $\chi_i=\psi_i$。  
我们知道 $p_A(A)$ 是一个 $k$ 次多项式，然后让 $A^n$ 对 $p_A(A)$ 做带余除法，得到 $A^n=Qp_A(A)+R$。  
根据 C-H 定理有 $p_A(A)=0$，所以 $A^n=R$。  
因为 $p_A(A)$ 是一个 $k$ 次多项式，所以 $R$ 是 $(k-1)$ 次多项式。  
所以问题规约为，什么 $(k-1)$ 次多项式以 $A$ 作为形式记号，并且等于 $A^n$。  
由前面计算可得该多项式为 $\displaystyle\sum_{i=1}^d\psi_iA^{i-1}$。  
所以 $R=\displaystyle\sum_{i=1}^d\psi_iA^{i-1}$。   
因为  $p_A(x)=\det(x\mathrm{I}_d-A)$ 即为：
$$
\begin{vmatrix}
x&-1&0&\cdots&0&0\\
0&x&-1&\cdots&0&0\\
\vdots&\vdots&\vdots&&\vdots&\vdots\\
0&0&0&\cdots&-1&0\\
0&0&0&\cdots&x&-1\\
-c_d&-c_{d-1}&-c_{d-2}&\cdots&-c_2&x-c_1\\
\end{vmatrix}
$$
接下来是解行列式的基本技巧。  
我们让原行列式对最后一列展开，有：
$$
(x-c_1)
\begin{vmatrix}
x&-1&0&\cdots&0\\
0&x&-1&\cdots&0\\
\vdots&\vdots&\vdots&&\vdots\\
0&0&0&\cdots&-1\\
0&0&0&\cdots&x\\
\end{vmatrix}_{(d-1)\times(d-1)}
+
\begin{vmatrix}
x&-1&0&\cdots&0\\
0&x&-1&\cdots&0\\
\vdots&\vdots&\vdots&&\vdots\\
0&0&0&\cdots&-1\\
-c_d&-c_{d-1}&-c_{d-2}&\cdots&-c_2\\
\end{vmatrix}_{(d-1)\times(d-1)}
$$
对于第一个行列式，发现其为上三角行列式，值为 $x^{d-1}$。 
对于第二个行列式，设其为 $S_{d-1}$，将第一列展开，有 $S_n=xS_{n-1}-c_{n+1}(-1)^{n+1}(-1)^{n-1}=xS_{n-1}-c_{n+1}$。   
又因为 $S_1=-c_2$，所以 $S_{d-1}=-\displaystyle\sum_{i=2}^{d}x^{d-i}c_i$。    
因此原式为 $x^d-\displaystyle\sum_{i=0}^{d-1}x^ic^{d-i}$。  
然后把最开始式子中的形式记号 $A$ 全改为 $x$，就有：  
所以有 $\displaystyle\sum_{i=1}^d\psi_ix^{i-1}=x^n\bmod (x^d-\displaystyle\sum_{i=0}^{d-1}x^ic^{d-i})$。  
再快速幂的过程中不断进行多项式取模即可算出 $\psi$，然后与 $f_0,f_1,\cdots,f_{d-1}$ 分别相乘（或者是说“内积“）并求和即可。  
复杂度为 $O(d\log d\log n)$。  
实际上这类多项式相关的算法的复杂度应当准确表示成：$O(\mathsf{M}(d)\log n)$,，其中 $\mathsf{M}(d)$ 是多项式乘法的复杂度。  
### SII-3 Bostan-Mori 算法  
一个较新的算法，由 Alin Bostan 和 Ryuhei Mori 提出。 
用该算法解决常系数齐次线性递推问题，复杂度仍然是 $O(\mathsf{M}(d)\log n)$，但是常数更小，且更容易理解。
#### SII-3-1 有理函数远处系数求值
我们知道，一个形式幂级数的常数项若非零，则其存在乘法逆。  
之前我们讨论了求乘法逆 $\bmod x^n$ 的做法（牛顿迭代/倍增，本质相同）以及处理不可逆情况的方法（引入形式 Laurent 级数）。
Bostan-Mori 算法处理的问题是求 $[x^n]\dfrac{f(x)}{g(x)}$ 的值，其中 $n$ 可以很大。      
我们称形如此的式子为**有理函数**，换句话说，该算法本质上解决的是有理函数系数求值问题，并且可以是远处系数。
上下同乘 $g(-x)$，得 $\dfrac{f(x)g(-x)}{g(x)g(-x)}$。  
设 $v(x)=g(x)g(-x)$，则 $v(x)$ 是偶函数，则 $v$ 仅含有偶数次项，因为对于 $x^k$ 来说，只有 $k$ 是偶数，其才为偶函数。  
既然如此，则 $v(x)$ 可以被表示为某个 $u(x^2)$。
分子既不是奇函数也不是偶函数，因此我们将其奇偶次项拆开，则 $f(x)g(-x)=c_0(x)+c_1(x)$。  
$c_0(x)$ 由偶数次项构成，故可表示为 $a(x^2)$，$c_1(x)$ 由奇数次项构成，故可表示为 $xb(x^2)$。  
故原分式为：
$$
\frac{a(x^2)+xb(x^2)}{u(x^2)}=\frac{a(x^2)}{u(x^2)}+x\frac{b(x^2)}{u(x^2)}
$$
但是实际上不需要求出整个分式，只需要求出它的第 $n$ 项即可，即：
$$
[x^n]\left(\frac{a(x^2)}{u(x^2)}+x\frac{b(x^2)}{u(x^2)}\right)
$$
注意到前一项贡献偶数次项，后一项贡献奇数次项。  
因此，分类讨论 $n$ 的奇偶性。  
若 $n$ 为奇数，则 $[x^n]\left(\dfrac{a(x^2)}{u(x^2)}+x\dfrac{b(x^2)}{u(x^2)}\right)=[x^n]x\dfrac{b(x^2)}{u(x^2)}=[x^{\lfloor\frac{n}{2}\rfloor}]\dfrac{b(x)}{u(x)}$。  
若 $n$ 为偶数，则 $[x^n]\left(\dfrac{a(x^2)}{u(x^2)}+x\dfrac{b(x^2)}{u(x^2)}\right)=[x^n]\dfrac{a(x^2)}{u(x^2)}=[x^{\frac{n}{2}}]\dfrac{a(x)}{u(x)}$。
因此问题转化成了求出 $a,b,u$。  
由于每次我们仅仅需要两次多项式乘法即可将问题的 $n$ 缩小一半，所以时间复杂度 $\mathsf{M}(d)\log n$，其中 $d$ 是 $P,Q$ 中较大一个的次数。
#### SII-3-2 规约为常系数齐次线性递推
考虑将常系数齐次线性递推问题规约为有理函数系数求值问题。  
首先，不妨考虑，对于形式幂级数 $F(x),G(x)$，$\dfrac{F(x)}{G(x)}=H(x)$，那么 $H(x)$ 有没有一个递推公式？  
首先我们得到 $F(x)=G(x)H(x)$，故 $[x^n]F(x)=\displaystyle\sum_{i=0}^n[x^i]G(x)[x^{n-i}]H(x)$。  
拆开和式，得到 $[x^0]G(x)[x^n]H(x)+\displaystyle\sum_{i=1}^n[x^i]G(x)[x^{n-i}]H(x)=[x^n]F(x)$。  
所以 $[x^n]H(x)=\dfrac{[x^n]F(x)-\sum_{i=1}^n[x^i]G(x)[x^{n-i}]H(x)}{[x^0]G(x)}$。  
这就是有理函数的递归公式。  
根据这个公式，我们试图找出 $P(x),Q(x)$，使得对于常系数齐次线性递推数列 $f$ 来说，$[x^n]\dfrac{P(x)}{Q(x)}=f_n$。  
我们依然沿用 $\Gamma(x)=x^d-\displaystyle\sum_{i=0}^{d-1}c_{d-i}x^i$，根据递推式，可以构造：
$$
\begin{aligned}
P(x)&=\left(\left(\sum_{i=0}^{\infty}f_ix^i\right)x^d\Gamma(x^{-1})\right)\bmod x^d\\
Q(x)&=x^d\Gamma(x^{-1})
\end{aligned}
$$
注意，$\Gamma(x^{-1})$ 可能不是多项式，但是 $x^d\Gamma(x^{-1})$ 一定是。  
所以这保证了 $P(x),Q(x)$ 一定是正常的多项式。
所以在求解常系数齐次线性递推问题时，按上述方法构造出 $P,Q$，然后使用 Bostan-Mori 算法即可。  
依然可以继续优化常数，有 LSB-First 算法，但是并不常用。
## Senior- $\text{III}$ -常系数非齐次线性递推
### SIII-1 矩阵做法
该方法基于 Fiduccia 算法，常数可能较大。
若 $f$ 满足 $f_i=P(i)+\displaystyle\sum_{j=1}^dc_jf_{i-j}$，其中 $P$ 是 $m$ 次常系数多项式 $\displaystyle\sum_{i=0}^m p_ix^i$，则称 $f$ 是一个**常系数非齐次线性递推数列**。  
给定 $f_0,f_1,\cdots,f_{d-1}$, 要求出 $f_n$ 的值。
（矩阵上方的 $\mathsf{T}$ 代表其转置后的矩阵，方便排版）
$$
\begin{aligned}
&\begin{bmatrix}
\begin{bmatrix}
f_i\\f_{i+1}\\f_{i+2}\\\vdots\\f_{i+d-1}
\end{bmatrix}^\mathsf{T}
\begin{bmatrix}
1\\(i+1)\\(i+1)^2\\\vdots\\(i+1)^m
\end{bmatrix}^\mathsf{T}
\end{bmatrix}
\begin{bmatrix}
\begin{bmatrix}
0&0&0&\cdots&0&c_d\\
1&0&0&\cdots&0&c_{d-1}\\
0&1&0&\cdots&0&c_{d-2}\\
0&0&1&\cdots&0&c_{d-3}\\
\vdots&\vdots&\vdots&&\vdots\\
0&0&0&\cdots&1&c_1
\end{bmatrix}&
\begin{bmatrix}
0&0&0&\cdots&0&0\\
0&0&0&\cdots&0&0\\
0&0&0&\cdots&0&0\\
0&0&0&\cdots&0&0\\
\vdots&\vdots&\vdots&&\vdots\\
0&0&0&\cdots&0&0
\end{bmatrix}\\
\begin{bmatrix}
0&0&0&\cdots&0&p_0\\
0&0&0&\cdots&0&p_1\\
0&0&0&\cdots&0&p_2\\
0&0&0&\cdots&0&p_3\\
\vdots&\vdots&\vdots&&\vdots\\
0&0&0&\cdots&0&p_m
\end{bmatrix}&
\begin{bmatrix}
\binom{0}{0}&\binom{1}{0}&\binom{2}{0}&\cdots&\binom{m}{0}\\
0&\binom{1}{1}&\binom{2}{1}&\cdots&\binom{m}{1}\\
0&0&\binom{2}{2}&\cdots&\binom{m}{2}\\
0&0&0&\cdots&\binom{m}{3}\\
\vdots&\vdots&\vdots&&\vdots\\
0&0&0&\cdots&\binom{m}{m}
\end{bmatrix}
\end{bmatrix}\\
&=\begin{bmatrix}
\begin{bmatrix}
f_{i+1}\\f_{i+2}\\f_{i+3}\\\vdots\\f_{i+d}
\end{bmatrix}^\mathsf{T}
\begin{bmatrix}
1\\(i+2)\\(i+2)^2\\\vdots\\(i+2)^m
\end{bmatrix}^\mathsf{T}
\end{bmatrix}
\end{aligned}
$$
设中间的转移矩阵为 $\mathscr{B}$。
为了简便，我们设 $\mathscr{B}=\begin{bmatrix}S&T\\U&V\end{bmatrix}$。
我们仿照 Fiduccia 算法的证明思路进行拓展，即无论是齐次还是非齐次情况，只需要找到转移矩阵的特征多项式 $G$，用 $x^n \bmod G$ 的系数向量再点乘 $(f_0,f_1,\cdots,f_{siz-1})$，就可以得到 $f_n$，其中 $siz$ 为特征多项式 $G$ 的次数。  
我们希望得到 $\mathscr{B}$ 的特征多项式 $p_{\mathscr{B}}(x)$。  
根据分块矩阵的特征多项式，$p_{\mathscr{B}}(x)=p_{S}(x)p_{V}(x)-p_{T}(x)p_{U}(x)=p_{S}(x)p_{V}(x)$。  
也就是 $|x\mathrm{I}-S||x\mathrm{I}-V|=(x^d+\displaystyle\sum_{i=0}^{d-1}c_{d-i}x_i)(x-1)^{m+1}$。    
但是我们目前只有 $f_0,f_1,\cdots,f_{d-1}$，所以还需要求出 $f_d,f_{d+1},\cdots,f_{d+m}$。    
回顾原式 $f_i=P(i)+\displaystyle\sum_{j=1}^dc_jf_{i-j}$，我们不妨记 $\tau_i=P(i)$。  
于是原式可化为 $f_i=\tau_i+\displaystyle\sum_{j=1}^ic_jf_{i-j}$。（求和上界变为 $i$，因此不妨令 $j>d$ 时 $c_j=0$）  
但是上式存在问题，即当 $i<d$ 的时候 $f_i$ 是已定的，不遵循递推式。  
因此我们仅仅令 $i\ge d$ 时 $\tau_i=P(i)$，当 $i<d$ 时 $\tau_i=f_i-\displaystyle\sum_{j=1}^ic_jf_{i-j}$，这样 $i<d$ 时就能将后面的和式消掉，从而使得原式良定义。  
不妨设 $\chi(x)=\displaystyle\sum_{i=0}^{\infty}c_i,T(x)=\displaystyle\sum_{i=0}^{\infty}\tau_i,F(x)=\displaystyle\sum_{i=0}^{\infty}f_i$。  
注意到 $\displaystyle\sum_{j=1}^ic_jf_{i-j}=[x^i](\chi\times F)$。  
因此我们有 $F=T+\chi F$，即 $F=\dfrac{T}{1-\chi}$。  
虽然 $T$ 是无穷项的形式幂级数，但是由于我们仅需要 $f_d,f_{d+1},\cdots,f_{d+m}$，所以多点求值求出 $T$ 的前 $(d+m)$ 项即可。  
求出 $f_0,f_1,\cdots,f_{d-1},f_d,f_{d+1},\cdots,f_{d+m}$ 之后，算出 $x^n\bmod G$ 的系数向量再点乘 $(f_0,f_1,\cdots,f_{d-1},f_d,f_{d+1},\cdots,f_{d+m})$ 即可。
复杂度 $O(m\log^2m+\mathsf{M}(k)\log n)$。  
### SIII-2 差分做法
我们考虑前向差分算子 $\Delta$，记 $(\Delta f)(i)=f(i)-f(i-1)$，首项不变。  
为了方便，我们去掉括号，直接记作 $\Delta f(i)$。但是要注意它是“算子”，即作用在函数本身上的，从函数到函数的映射，而非作用在某个函数值上。  
那么由数学归纳法，容易证明 $\Delta^m f(i)=\displaystyle\sum_{j=0}^m\binom{m}{j}(-1)^jf(i-j)$。  
所以可得出：  
$$
\begin{aligned}
f_i&=P(i)+\sum_{j=1}^dc_jf_{i-j}\\
\implies\Delta^{m+1} f_i&=\Delta^{m+1}P(i)+\sum_{j=1}^dc_j\sum_{k=0}^{m+1}\binom{m+1}{k}(-1)^kf_{i-j-k}\\
&=\Delta^{m+1}P(i)+\sum_{j=1}^dc_j\Delta^{m+1}f_{i-j}
\end{aligned}
$$
然后，注意到对于多项式 $P(x)=\displaystyle\sum_{i=0}^m p_ix^i$ 来说，$\Delta P(x)=\displaystyle\sum_{i=0}^m p_ix^i-\displaystyle\sum_{i=0}^m p_i\sum_{j=0}^i\binom{i}{j}x^j(-1)^{i-j}$，发现 $x^m$ 项被抵消了，因此差分之后得到一个 $(m-1)$ 次多项式。  
因此，根据数学归纳法，$\Delta^{m+1}P(i)=0$。  
所以可以得到：
$$
\Delta^{m+1}f_i=\sum_{j=1}^dc_j\Delta^{m+1}f_{i-j}
$$
惊人的结果，这是一个齐次递推式。  
因此      
## Senior- $\text {IV}$ -多项式复合
多项式复合，本质上是**形式幂级数复合截断**，即求出 $f(g(x))$ 的前 $n$ 项，即 $\displaystyle\sum_{i=0}^n([x^i]f(x))g(x)^i\bmod(x^{n+1})$。   
### SIV-2 Kinoshita–Li 算法
首先最朴素的做法显然是直接按照原式计算，复杂度为 $O(n^2\log n)$，根据分块等技巧可以优化到 $O(n^2+n\sqrt{n}\log n)$，复杂度依然很高。  
R.P.Brent 和 H.T.Kung 在 1978 年发表的论文中将多项式复合优化到 $O((n\log n)^{1.5})$，但是比较复杂且常数较大。   
Yasunori Kinoshita 和李白天提出了 $O(\mathsf{M}(n)\log n)$ 做法，这一成果在 2024 年发表，可以说是较为前沿的理论。
Kinoshita-Li 算法基于二元 Bostan-Mori 算法。  
朴素的 Bostan-Mori 解决的是有理函数求远处系数的问题。  
二元有理函数亦同理。  

# PART 5 —— 形式幂级数算法扩展

<div STYLE="page-break-after: always;"></div>

# PART 6 —— 形式幂级数应用
## Application- $\text{I}$ -整式递推
### ApI-1 快速阶乘算法
关于组合数 $\dbinom{n}{m}\bmod p$ 的问题，最朴素的办法是递推公式，即杨辉三角进行 $O(nm)$ 递推。  
进一步优化就是预处理阶乘，直接根据定义计算，做到 $O(\max(n,m))$。  
当 $p$ 较小时，可以使用 Lucas 定理或 exLucas 定理进行 $O(p)$ 求解。  
如果 $n,m,p$ 是一个较大的数呢？  
依然考虑直接根据定义计算。  
我们需要更加快速的计算阶乘的方法。  
$n!$，即 $n^{\underline{n}}$，当 $n$ 为正整数时定义为 $\displaystyle\prod_{i=1}^ni$，同时有 $n!:=\Gamma(n+1),\Gamma(n)=\displaystyle\int_{0}^{\infty}t^{n-1}\mathrm{e}^{-t}\mathrm{d}t$。  
该结论直接用分部积分证明。  
此处我们仅仅考虑 $n$ 为正整数的情况。  
考虑经典技巧：分块。  
将从 $1$ 到 $n$ 的所有整数分为若干个块 $[1,\Delta],[\Delta+1,2\Delta],\cdots,[(k-1)\Delta+1,k\Delta]$。  
如果 $\Delta\nmid n$，则末尾还剩下一个长度小于 $\Delta$ 的散块。
对于每一个块内的整数，分别计算其乘积，然后再将每一个块的答案乘起来。
对于一个块 $[i\Delta+1,(i+1)\Delta]$ 来说，其乘积为 $\displaystyle\prod_{j=1}^{\Delta}(i\Delta+j)$。  
不妨定义 $f(x)=\displaystyle\prod_{j=1}^{\Delta}(i\Delta+j)$，答案就是 $\displaystyle\prod_{i=0}^{k-1}f(i\Delta)\times\displaystyle\prod_{i=k\Delta+1}^ni$。  
#### SIV-1-1 多点求值
$f(x)$ 使用分治乘法得到系数，考虑直接多点求值计算出 $f(0),f(\Delta),f(2\Delta),\cdots,f((k-1)\Delta)$，直接相乘，最后的散块暴力乘起来，复杂度 $O(\Delta\log^2\Delta+\max(k,\Delta)\log^2(\max(k,\Delta))+\Delta)$，显然当 $\Delta=\lfloor\sqrt{n}\rfloor$ 时复杂度最优为 $O(\sqrt{n}\log^2\sqrt{n})$，这等价于 $O(\sqrt{n}\log^2n)$，因为 $\log\sqrt{n}=\frac{1}{2}\log n$。

<div STYLE="page-break-after: always;"></div>


参考资料：  
丘维声《高等代数》  
杨一龙《数学在哪里》  
刘承奥《浅谈DFT在信息学竞赛中的应用》  
陈宇《转置原理的简单介绍》
吕凯风《集合幂级数的性质与应用及其快速算法》  
[OI-wiki](https://oi-wiki.org) “多项式与生成函数”部分