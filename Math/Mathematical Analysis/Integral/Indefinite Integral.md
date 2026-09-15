## 一、定义
1. 设区间 $I$ 上有函数 $f(x)$。若 $F(x)$ 是区间 $I$ 上的可微函数，使得 $F'(x)=f(x),\forall x\in l$，则称 $F(x)$ 为 $f(x)$ 的一个~={black}原函数=~。（[[Derivative and Differential]]）
2. 将 $f(x)$ 的所有原函数的集合称为 $f(x)$ 的~={black}不定积分=~，记作 $\displaystyle\int f(x)dx=F(x)+C$。这里的 $F(x)+C$ 表示的是 $\set{F(x)+C|C\in \mathbb{R}}$。
3. （出现在积分符号中的函数保证有原函数）有$$
\int(af(x)+bg(x))dx=a\int f(x)dx+b\int g(x)dx
$$
4. $\displaystyle\int f^{-1}(x)dx=xf^{-1}(x)-F[(f^{-1}(x))]+C,F'(x)=f(x)$
## 二、换元法
1. $$
\int f(g(x))g'(x)dx=F(g(x))+C
$$
2. 若 $\displaystyle\int f(g(x))g'(x)dx=G(x)+C$, 则 $\displaystyle\int f(x)dx=G(g^{-1}(x))+C$
## 三、分部积分法
1. $$
\int u(x)v'(x)dx=u(x)v(x)-\int u'(x)v(x)dx
$$
2. $$
\int udv=uv-\int vdu
$$
## 四、有理函数的积分方法
### 1、有理函数
1. 实系数多项式 $P(x)$ 能分解为一次因式和无实根二次因式的乘积，即$$
P(x)=A\prod_{i=1}^{s}(x-x_i)^{m_i}\prod_{i=1}^t(x^2+a_ix+b_i)^{n_i}
$$
2. 任何有理函数都能唯一地写成一个多项式和一个真分式的和。
3. 设真分式 $R(x)=\frac {P(x)}{Q(x)}$, 其中 $Q(x)=\prod_{i=1}^{s}(x-x_i)^{m_i}\prod_{i=1}^t(x^2+a_ix+b_i)^{n_i}$，那么存在一系列常数 $A_{ij},B_{ij},C_{ij}$，使得：$$
R(x)=\sum_{i=1}^s\sum_{j=1}^{m_i}\frac{A_{ij}}{(x-x_i)^j}+\sum_{i=1}^t\sum_{j=1}^{n_i}\frac{B_{ij}x+C_{ij}}{(x^2+a_ix+b_i)^j}
$$
### 2、三角函数有理式
使用万能公式（[[Triangle Basic Formulas]]）进行替换即可。