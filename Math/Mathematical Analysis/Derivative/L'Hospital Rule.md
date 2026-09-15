---
前置知识: "[[Function Limit]]"
---
保证 $f,g$ 在 $a$ 的一去心邻域 $U_0(a,\delta)$ 内可导（[[Derivative and Differential]]）

---
## $\frac 0 0$ 型
若函数 $f$，$g$ 满足：
1) $\displaystyle\lim_{x\to a}f(x)=\displaystyle\lim_{x\to a}g(x)=0$
2) $\forall x\in U_0(a,\delta),g'(x)\neq0$
3) $\displaystyle\lim_{x\to a}\frac{f'(x)}{g'(x)}=A$ （$A\in{\mathbb{R}}\cup\set{+\infty,-\infty}$） 
则有$$
\lim_{x\to a}\frac{f(x)}{g(x)}=\lim_{x\to a}\frac{f'(x)}{g'(x)}=A
$$
---
## $\frac \infty \infty$ 型
若函数 $f$，$g$ 满足：
1) $\displaystyle\lim_{x\to a}f(x)=\displaystyle\lim_{x\to a}g(x)=\infty$
2) $\forall x\in U_0(a,\delta),g'(x)\neq0$
3) $\displaystyle\lim_{x\to a}\frac{f'(x)}{g'(x)}=A$ （$A\in{\mathbb{R}}\cup\set{+\infty,-\infty}$） 
则有 $$
\lim_{x\to a}\frac{f(x)}{g(x)}=\lim_{x\to a}\frac{f'(x)}{g'(x)}=A
$$
