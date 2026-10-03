由于不是全家桶而是半家桶，故 Family 变成了 Fml。
# 一、Stern-Brocot Tree
## 0. 前言

> Stern–Brocot 树是一种维护分数的优雅的结构，包含所有不同的正有理数．这个结构分别由 Moritz Stern 在 1858 年和 Achille Brocot 在 1861 年独立发现．

上文出自 OI-wiki。

## 1. DIVCNT1 
[SP26073 DIVCNT1](https://www.luogu.com.cn/problem/SP26073)。
题目极其简洁，即求出：
$$\sum_{i=1}^n\sigma_0(i)$$
的值，其中 $\sigma_0(i)$ 是 $i$ 的因数个数。  

直接欧拉筛然后暴力求和显然开销巨大，考虑展开 $\sigma_0(i)=\sum_{d|i}1$，于是原式化简为：
$$
\begin{aligned}
&\sum_{i=1}^n\sigma_0(i)\\
=&\sum_{i=1}^n\sum_{d|i}1\\
=&\sum_{d=1}^n\sum_{i=1}^{\lfloor\frac{n}{d}\rfloor}1\\
=&\sum_{d=1}^n\left\lfloor\frac{n}{d}\right\rfloor\\
\end{aligned}
$$
数论分块即可做到 $\mathcal{O}(\sqrt{n})$。
### I. 数论分块
还是说一下吧，普遍意义上数论分块解决的是形如 $\displaystyle\sum_{i=1}^nf(i)g(\left\lfloor\dfrac{n}{i}\right\rfloor)$ 的问题，当然也可以扩展到上取整、高维的情况，思想本质是一致的。  

$\left\lfloor\dfrac{n}{i}\right\rfloor$ 只有 $\mathcal{O}(\sqrt{n})$ 个不同的取值，对于 $i\le\sqrt n$，最多只有 $\sqrt n$ 种不同的 $\left\lfloor\dfrac{n}{i}\right\rfloor$，而对于 $i>\sqrt n$，必然有 $\left\lfloor\dfrac{n}{i}\right\rfloor\le\sqrt{n}$，也最多只有 $\sqrt n$ 种不同的取值。又因为 $\left\lfloor\dfrac{n}{i}\right\rfloor$ 是非严格单调递减的，所以使 $\left\lfloor\dfrac{n}{i}\right\rfloor$ 取值相同的 $i$ 都是一个个连续段，因此我们可以逐连续段处理。  

对于同一个极长连续段 $[l,r]$，$\left\lfloor\dfrac{n}{l}\right\rfloor=\left\lfloor\dfrac{n}{l+1}\right\rfloor=\cdots=\left\lfloor\dfrac{n}{r}\right\rfloor$，所以这些 $g$ 值是相等的，不妨用 $g_l$ 来表示，记 $f$ 的前缀和为 $F$，则这段连续段的贡献即为 $(F_r-F_{l-1})g_l$。  

因此只要我们能 $\mathcal O(1)$ 计算 $F,g$，就可以 $\mathcal{O}(\sqrt{n})$ 计算 $\displaystyle\sum_{i=1}^nf(i)g(\left\lfloor\dfrac{n}{i}\right\rfloor)$。  

对于一个极长连续段 $[l,r]$，容易说明 $r=\min\left(n,\left\lfloor\dfrac{n}{\left\lfloor\frac{n}{l}\right\rfloor}\right\rfloor\right)$。若存在下一个连续段，则下一个连续段的 $l$ 即为当前连续段的 $r+1$。  

对于本题，$f=1,F=g=\mathrm{id}$，数论分块容易处理。

### II. 正解
但是本题的 $n$ 高达 $2^{63}$，我们需要更快的做法。  

考虑 $\displaystyle\sum_{d=1}^n\left\lfloor\frac{n}{d}\right\rfloor$ 的几何意义。考虑反比例函数 $y=\dfrac{n}{x}$ 的图像。

![[Pasted image 20261002202119.png]]

绿线即为 $y=\dfrac{n}{x}$ 的图像，而根据要求的式子，答案则是该图像下**正整数点**的数量，即可视作为蓝线下的面积, 毕竟一个正整数格点和一个单位方格是一一对应的。

![[Pasted image 20261002201519.png]]

如上图所示，数论分块其实就是在同高的蓝框内拆矩形，并快速计算矩形面积。

现在我们考虑优化。

我们先假定 $n$ **不是完全平方数**，然后对原式进行拆分， $\displaystyle\sum_{d=1}^n\left\lfloor\frac{n}{d}\right\rfloor=\displaystyle\sum_{d=1}^{\lfloor\sqrt{n}\rfloor}\left\lfloor\frac{n}{d}\right\rfloor+\displaystyle\sum_{d=\lfloor\sqrt{n}\rfloor+1}^{n}\left\lfloor\frac{n}{d}\right\rfloor$，而：
$$
\begin{aligned}
&\sum_{d=\lfloor\sqrt{n}\rfloor+1}^{n}\left\lfloor\frac{n}{d}\right\rfloor\\
=&\sum_{d=\lfloor\sqrt{n}\rfloor+1}^{n}\sum_{i=1}^{\lfloor\frac{n}{d}\rfloor}1\\
=&\sum_{i=1}^{\lfloor\sqrt{n}\rfloor}\sum_{\lfloor\sqrt{n}\rfloor<d\le n\land \left\lfloor\frac{n}{d}\right\rfloor\ge i}1\\
=&\sum_{i=1}^{\lfloor\sqrt{n}\rfloor}\sum_{\lfloor\sqrt{n}\rfloor<d\le n\land \left\lfloor\frac{n}{i}\right\rfloor\ge d}1\\
=&\sum_{i=1}^{\lfloor\sqrt{n}\rfloor}\sum_{d=\lfloor\sqrt{n}\rfloor+1}^{\left\lfloor\frac{n}{i}\right\rfloor}1\\
=&\sum_{i=1}^{\lfloor\sqrt{n}\rfloor}(\left\lfloor\frac{n}{i}\right\rfloor-\lfloor\sqrt{n}\rfloor)\\
=&\sum_{i=1}^{\lfloor\sqrt{n}\rfloor}\left\lfloor\frac{n}{i}\right\rfloor-(\lfloor\sqrt{n}\rfloor)^2\\
\end{aligned}
$$
代入原式，得到 $\displaystyle\sum_{d=1}^n\left\lfloor\frac{n}{d}\right\rfloor=2\displaystyle\sum_{d=1}^{\lfloor\sqrt{n}\rfloor}\left\lfloor\frac{n}{d}\right\rfloor-(\lfloor\sqrt{n}\rfloor)^2$。对于 $n$ 是完全平方数的情况，这个式子是完全一致的，只是推导过程有一部分加一减一的变化。  

因此我们将问题规约为求 $\displaystyle\sum_{d=1}^{\lfloor\sqrt{n}\rfloor}\left\lfloor\frac{n}{d}\right\rfloor$ 的值。

由于 $\displaystyle\sum_{d=\lfloor\sqrt{n}\rfloor+1}^{n}\left\lfloor\frac{n}{d}\right\rfloor=\displaystyle\sum_{i=1}^{\lfloor\sqrt{n}\rfloor}\left\lfloor\frac{n}{i}\right\rfloor-(\lfloor\sqrt{n}\rfloor)^2$，则 $\displaystyle\sum_{i=1}^{\lfloor\sqrt{n}\rfloor}\left\lfloor\frac{n}{i}\right\rfloor=\displaystyle\sum_{d=\lfloor\sqrt{n}\rfloor+1}^{n}\left\lfloor\frac{n}{d}\right\rfloor+(\lfloor\sqrt{n}\rfloor)^2$。我们要算的即为下图的正整数点个数，或可视为面积。

![[Pasted image 20261003154808.png]]

注意“面积”是辅助理解的，实际上我们数的是正整点数量，同时我们发现正整点对应着一个单位正方形的右上角。  

考虑构造一个凸包，如下图所示。

![[Pasted image 20261003165024.png]]

这个凸包的定义是 $\mathrm{conv}\{(x,y)\in\mathbb{Z}^2|xy>n,\left\lfloor\frac{n}{\lfloor\sqrt{n}\rfloor}\right\rfloor\le x\le n+1,1\le y\le\lfloor\sqrt{n}\rfloor+1\}$ 其必然存在。

这个凸包具有如下性质：
+ 凸包下边界的每一个顶点都是整点，且这些整点都在 $y=\dfrac{n}{x}$ 的严格上方。
+ 凸包下边界与蓝框边界之间（不含两个边界上的整点）不存在任何整点。
+ 在符合上述条件的前提下，该凸包下边界从左上到右下每一条向量的斜率绝对值都尽可能的大。

:::info[三条性质为何成立？]
+ 性质一直接根据定义得到。  
+ 性质二也几乎直接根据定义得到，在矩形 $[1,n]\times[1,\lfloor\sqrt{n}\rfloor]$ 中，所有的点要么在凸包内，要么在蓝框内，即我们的计算范围内。 
+ 性质三可以做反证，如果对于某条向量的右端点，不取使其斜率最大的点，那根据凸包的性质，这个点必然在凸包内部而不在凸包边界上。
:::

由于后面的步骤，我们希望构成凸包下边界的所有向量都满足，其 $\Delta x$ 与 $\Delta y$ 互质。  

如果有不互质的，我们会人为的将其拆成 $\gcd(\Delta x,\Delta y)$ 条长度相等，方向相同的向量即可，参考上图所示凸包下边界的第一条与第二条向量，斜率相同但被拆成两条。  

这样一是保证了后面 Stern-Brocot Tree 对其的维护，二是使得一个向量内仅端点为整点。  

为什么凸包的上边界是 $y=\lfloor\sqrt{n}\rfloor+1$，左边界是 $x=\left\lfloor\frac{n}{\lfloor\sqrt{n}\rfloor}\right\rfloor$ ？ 参考上图，第一条向量的设置看起来十分多余。

我们的计算逻辑是，对于每个向量 $\vec{v}$，计算其“左边”“上开下闭”区域的整点数，  

![[Pasted image 20261003183126.png]]

对于上图，第一至四条向量处理的分别为灰线、绿线、橙线、紫线上的正整数点。  

因此第一条向量实际上是必要的。

