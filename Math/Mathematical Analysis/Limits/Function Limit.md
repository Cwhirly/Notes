### 一、定义

#### 1. 函数极限
设函数在 $(x_0-\delta,x_0)\cup(x_0,x_0+\delta)$ 内有定义，其中 $\delta>0$ ,如果存在实数 A，使得对任意的 $\varepsilon>0$，都存在正实数 $t$，满足对 $\forall x\in(x_0-t,x_0)\cup(x_0,x_0+t)$，有 $|f(x)-A|<\varepsilon$ 成立，则称**当 $x$ 趋近于 $x_0$ 时，$f(x)$ 的极限为 $A$**   ,记作 $\displaystyle\lim_{x\to x_0}{f(x)}=A$。我们称 $(x_0-\delta,x_0)\cup(x_0,x_0+\delta)$ 为 $x_0$ 处的**去心邻域**，记作 $U_0(x_0,\delta)$ .需要注意的是，不需要 $f(x)$ 在 $x_0$ 处有定义，就可以定义 $\displaystyle\lim_{x\to x_0}{f(x)}$。  

特点：
1) $\displaystyle\lim_{x\to x_0}{f(x)}$ 与 $f(x_0)$ 无关
2) 我们熟知的基本初等函数，大部分都连续，参见[[Continuous]]

#### 2. 右极限
设函数在 $(x_0,x_0+\delta)$ 内有定义，其中 $\delta>0$ ,如果存在实数 $A$，使得对任意的 $\varepsilon>0$，都存在正实数 $t$，满足对任意的 $x\in(x_0,x_0+t)$，有 $|f(x)-A|<\varepsilon$ 成立，则称**当 $x$ 趋近于 $x_0$ 时，$f (x)$ 的右极限为 A**   ,记作 $\displaystyle\lim_{x\to x_0^+}{f(x)}=A$ 或 $f(x_0+0)=A$
#### 3. 左极限
设函数在 $(x_0-\delta,x_0)$ 内有定义，其中 $\delta>0$ ,如果存在实数 $A$，使得对任意的 $\varepsilon>0$，都存在正实数 $t$，满足对任意的 $x\in(x_0-t,x_0)$，有 $|f(x)-A|<\varepsilon$ 成立，则称**当 $x$ 趋近于 $x_0$ 时，$f(x)$ 的左极限为 $A$**   ,记作 $\displaystyle\lim_{x\to x_0^-}{f(x)}=A$ 或 $f(x_0-0)=A$

定理：设 $f(x)$ 在 $U_0(x_0)$ 上有定义，则存在 $\displaystyle\lim_{x\to x_0}{f(x)}=A$ 的充要条件为：
			$f(x_0+)=f(x_0-)=A$
#### 4. 趋于正无穷的函数极限
设函数在 $(u,+\infty)$ 内有定义，如果存在实数 $A$，使得对任意的 $\varepsilon>0$，都存在实数 $t>u$，满足对任意的 $x>t$，有 $|f(x)-A|<\varepsilon$ 成立，则称**当 $x$ 趋近于 $+\infty$ 时，$f (x)$ 的极限存在且等于 $A$**   ,记作 $\displaystyle\lim_{x\to +\infty}{f(x)}=A$ 或 $f(x)\to A,x\to+\infty$

#### 5. 趋于负无穷的函数极限
设函数在 $(-\infty,u)$ 内有定义，如果存在实数 $A$，使得对任意的 $\varepsilon>0$，都存在实数 $t<u$，满足对任意的 $x<t$，有 $|f(x)-A|<\varepsilon$ 成立，则称**当 $x$ 趋近于 $-\infty$ 时，$f (x)$ 的极限存在且等于 $A$**   ,记作 $\displaystyle\lim_{x\to -\infty}{f(x)}=A$ 或 $f(x)\to A,x\to-\infty$

### 二、定理
#### 1. Heine 归结原理
设 $x_0,\delta,A\in \mathbb{R}$, 函数 $f(x)$ 在 $x_0$ 的某个去心邻域 $U_0(x_0,\delta)$ 有定义，如果对任何满足 $x_n\to x_0$ 的序列 $\{x_n\}\subset U_0(x_0,\delta)$，都有 $f(x_n)\to A$，则称 $x\to x_0$ 时，函数 $f(x)$ 的极限为 $A$。
### 三、性质
1. 如果 $\displaystyle\lim_{x\to x_0}{f(x)}$ 存在，则该极限值是唯一的。
2. 设 $\displaystyle\lim_{x\to x_0}{f(x)}=A$，$\displaystyle\lim_{x\to x_0}{g(x)}=B$ ,如果存在 $\delta>0$,，使得当 $x\in U_0(x,\delta)$ 时 $f(x)\le g(x)$，则 $A\le B$ 。
3. 函数极限具有四则运算
4. 夹逼原理 (同数列极限)
5. Cauchy 收敛准则：
		函数 $f$ 在点 $a$ 有极限的充要条件是：对每一个给定的 $\varepsilon>0$ 存在 $\delta>0$, 使得对于在 $O_\delta(a)-\{a\}$ 中的每一对点 $x',x''$ 满足不等式 $|f(x')-f(x'')|<\varepsilon$ 