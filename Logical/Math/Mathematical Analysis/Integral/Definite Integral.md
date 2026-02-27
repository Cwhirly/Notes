# 一、Riemann 积分
## 1. 定义
设 $f(x)$ 在闭区间 $[a,b]$ 有定义，$a<b$
1) 称点集 $\Delta=\{x_0,x_1,\dots,x_n\}$ 为 $[a,b]$ 的一个**分划**，若：$a=x_0<\cdots<x_n=b$,
   记 $\Delta x_i=x_i-x_{i-1},i=1,2,\cdots,n,\lambda(\Delta)=\displaystyle\max_{1\le i\le n}\Delta x_i$ ，表示细分的程度。
2) 设 $\Delta=\{x_0,x_1,\dots,x_n\}$ 为 $[a,b]$ 的一个分划，任取 $\xi_i\in[x_{i-1},x_i]$，则称 $\displaystyle\sum_{i=1}^nf(\xi_i)\Delta x_i$ 为 $f$ 在 $[a,b]$ 的分划 $\Delta$ 上的一个~={black}Riemann 和=~。
3) 若存在 $I\in\mathbb{R},\forall\varepsilon>0,\exists\delta>0$，对 $\lambda(\Delta)<\delta$ 的分划 $\Delta$ 和·其上的任意 Riemann 和 $\displaystyle\sum_{i=1}^nf(\xi_i)\Delta x_i$，都有 $|\displaystyle\sum_{i=1}^nf(\xi_i)\Delta x_i-I|<\varepsilon$，则称 $f$ 在区间 $[a,b]$ 上 Riemann 可积或简称~={cyan}可积=~。记为 $f\in R[a,b]$ 称 $I$ 为 $f$ 在区间 $[a,b]$ 上的 ~={cyan}Riemann 积分=~或~={black}定积分=~，记为 $\displaystyle\int_a^bf(x)\mathrm{d}x=I$。
## 2. 定理
1. 如果定积分存在，则唯一。
2. 若 $f(x)\in R[a,b]$，则 $f(x)$ 在 $[a,b]$ 上有界。
3. 如果 $f(x)\in C[a,b]$，则 $f(x)\in R[a,b]$。
4. 如果 $f(x)$ 在 $[a,b]$ 上单调，则 $f(x)\in R[a,b]$。
# 二、Darboux 理论
## 1. 定义
1.  $f(x)$ 在区间 $[a,b]$ 上有定义且有界，$\Delta=\{x_0,x_1,\dots,x_n\}$ 为 $[a,b]$ 的一个分划。 $a=x_0<\cdots<x_n=b$，令
$$
M_i=\sup_{x\in[x_{i-1},x_i]}\{f(x)\},m_i=\inf_{x\in[x_{i-1},x_i]}\{f(x)\}
$$
	并考虑以下的和式：
	$$
\overline{S}(\Delta)=\sum_{i=1}^n M_i\Delta x_i,\underline{S}(\Delta)=\sum_{i=1}^n m_i\Delta x_i
$$
	$\overline{S}(\Delta)$，$\underline{S}(\Delta)$ 分别称为 $f(x)$ 得关于分划 $\Delta$ 的 ~={cyan}Darboux 上和=~与 ~={cyan}Darboux 下和=~。
2. 若 $\Delta'$，$\Delta''$ 都是区间 $[a,b]$ 的分划，且 $\Delta'\subset\Delta''$，则称 $\Delta''$ 是 $\Delta'$ 的~={magenta}细分=~。
3. $$
\underline{\int_a^b}f(x)\mathrm{d}x=\sup_{\Delta}(\underline{S}(\Delta)),\overline{\int_a^b}f(x)\mathrm{d}x=\inf_{\Delta}(\overline{S}(\Delta))
	$$ 称 $\underline{\displaystyle\int_a^b}f (x)\mathrm{d}x,\overline{\displaystyle\int_a^b}f (x)\mathrm{d}x$ 分别为 $f(x)$ 在区间 $[a,b]$ 上的~={cyan}下积分=~和~={cyan}上积分=~。
4. $\omega_i=M_i-m_i$ 为 $f(x)$ 在 $[x_{i-1},x_i]$ 上的~={magenta}振幅=~。
## 2. 定理
1. 若 $\Delta'$，$\Delta''$ 都是区间 $[a,b]$ 的分划，且 $\Delta'\subset\Delta''$，则有：$$
\overline{S}(\Delta')\ge\overline{S}(\Delta''),\underline{S}(\Delta')\le\underline{S}(\Delta'')
$$
2. 若 $\Delta'$，$\Delta''$ 都是区间 $[a,b]$ 的分划,，则有 $\underline{S}(\Delta')\le\overline{S}(\Delta'')$。
3. （Darboux 定理）设函数 $f(x)$ 在区间 $[a,b]$ 上有界，则$$
\lim_{\lambda(\Delta)\to0}\overline{S}(\Delta)=\overline{\int_a^b}f(x)\mathrm{d}x,\lim_{\lambda(\Delta)\to0}\underline{S}(\Delta)=\underline{\int_a^b}f(x)\mathrm{d}x
$$
4. 设函数 $f(x)$ 在区间 $[a,b]$ 上有界，则 $f(x)\in R[a,b]$ 的充要条件是：$$
\overline{\int_a^b}f(x)\mathrm{d}x=\underline{\int_a^b}f(x)\mathrm{d}x
$$
5. 设函数 $f(x)$ 在区间 $[a,b]$ 上有界，则下面三个结论等价：
	1) $f(x)\in R[a,b]$。
	2) $\forall\varepsilon>0$，$\exists[a,b]$ 的一个分划 $\Delta$，使得 $\displaystyle\sum_{i=1}^n \omega\Delta x_i<\varepsilon$。
	3) $\forall\varepsilon>0$，$\forall\sigma>0$，$\exists[a,b]$ 的一个分划 $\Delta$，使得 $\omega_i\ge\varepsilon$ 的的小区间的长度总和小于 $\sigma$。
# 三、定积分的性质
声明：在无特殊说明情况下，假设 $f(x)$ 在积分区间内有定义，可积。
## 1. 初等性质
1. $$
\int_a^bf(x)\mathrm{d}x=\int_a^cf(x)\mathrm{d}x+\int_c^bf(x)\mathrm{d}x
$$
2. $$
\int_a^b(nf(x)+mg(x))\mathrm{d}x=n\int_a^bf(x)\mathrm{d}x+m\int_a^bg(x)\mathrm{d}x
$$
3. $$
|\int_a^bf(x)\mathrm{d}x|\le\int_a^b|f(x)|\mathrm{d}x
$$
## 2. 微积分基本定理
1. （Newton-Leibniz 公式）设函数 $f(x)$ 在区间 $[a,b]$ 内有原函数 $F(x)$，则 $\displaystyle\int_a^b f(x)\mathrm{d}x=F(b)-F(a)$，记为 $F(x)|_a^b:=F(b)-F(a)$。
2. (微积分基本定理) 设 $f(x)$ 在 $x_0\in[a,b]$ 连续，记 $F(x)=\displaystyle\int_a^x f(t)\mathrm{d}t$，则 $F'(x_0)=f(x_0)$。
# 四、定积分的计算
套用 Newton-Leibniz 公式，其余与不定积分（[[Indefinite Integral]]）大体相同。