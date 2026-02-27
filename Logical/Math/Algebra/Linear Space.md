1. 设 $V$ 是一个非空集合，$K$ 是数域，$V$ 上规定了加法（$V^2\mapsto V$）及数量乘法（$V\times K\mapsto V$），并且满足以下八条性质：(希腊字母为 $V$ 中元素，拉丁字母为 $K$ 中元素)
	1) $\alpha+\beta=\beta+\alpha$
	2) $(\alpha+\beta)+\gamma=\alpha+(\beta+\gamma)$
	3) $\exists0\in V$, 有 $\alpha+0=\alpha$，称为零元
	4) $\forall\alpha\in V,\exists\beta\in V$，使得 $\alpha+\beta=0$，互为负元
	5) $1\alpha=\alpha$
	6) $(kl)\alpha=k(l\alpha)$
	7) $(k+l)\alpha=k\alpha+l\alpha$
	8) $k(\alpha+\beta)=k\alpha+k\beta$
	则称 $V$ 是 $K$ 上的**线性空间**。
2. 设 $V$ 是线性空间，$U\subseteq V$, 则 $U$ 是 $V$ 的一个线性子空间 $\iff U$ 对于 $V$ 的加法及乘法封闭。
3. 若向量组 $\alpha_1,\alpha_2,\dots,\alpha_s$ 有一个部分组线性相关，那么它线性相关。
4. 若向量组线性无关，则它任一部分组线性无关。
5. 含有 **$0$** 的向量组线性相关。
6. 向量组 $\alpha_1,\alpha_2,\dots,\alpha_s$ 线性相关 $\iff$ 至少有一个向量能由其他的向量线性表出。
7. 向量组 $\alpha_1,\alpha_2,\dots,\alpha_s$ 线性无关 $\iff$ 没有向量能由其他的向量线性表出。
8. 一向量组线性无关，则其延伸组也线性无关。
9. $\beta$ 能被 $\alpha_1,\alpha_2,\dots,\alpha_s$ 线性表出且方式唯一 $\iff$$\alpha_1,\alpha_2,\dots,\alpha_s$ 线性无关。
10. 设向量组 $\alpha_1,\alpha_2,\dots,\alpha_s$ 线性无关，则若 $\alpha_1,\alpha_2,\dots,\alpha_s,\beta$ 线性相关，则 $\beta$ 能被 $\alpha_1,\alpha_2,\dots,\alpha_s$ 线性表出。
11. $\alpha_1,\alpha_2,\dots,\alpha_s$ 线性无关, 且$$\begin{cases}
\beta_1=b_{11}a_1+b_{12}a_2+\dots+b_{1s}a_s\\
\dots\dots\\
\beta_s=b_{s1}a_1+b_{s2}a_2+\dots+b_{ss}a_s
\end{cases}
$$, 则向量组 $\beta_1,\beta_2,\dots,\beta_s$ 线性无关 $\iff$
$$
\begin{vmatrix}
b_{11}&b_{12}&...&b_{1s} \\
b_{21}&b_{22}&...&b_{2s} \\
\vdots&\vdots&&\vdots\\
b_{s1}&b_{s2}&\dots&b_{ss}
\end{vmatrix}\neq0
$$
（[[Determinant]] 12）