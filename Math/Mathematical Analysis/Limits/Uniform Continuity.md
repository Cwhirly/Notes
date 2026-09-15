与 [[Continuous]] 相似但完全不同
#### 定义：
设函数 $f(x)$ 在区间 $I$ 上有定义，若 $\forall\varepsilon>0$，$\exists\delta>0$，当 $x_1,x_2\in I$ 且 $|x_1-x_2|<\delta$ 时，有 $|f(x_1)-f(x-2)|<\varepsilon$, 则称 $f(x)$ 在 $I$ 上~={green}一致连续=~

### 定理 1：
$f(x)$ 再区间 $I$ 上一致连续 $\iff$ 对 $I$ 中满足$$
\lim_{n\to\infty}(x_n'-x_n'')=0
$$ 的任意两个数列 $\{x'_n\}$，$\{x''_n\}$，必有$$
\lim_{n\to\infty}[f(x'_n)-f(x''_n)]=0
$$
### Cantor 定理：
设函数 $f$ 在闭区间 $[a,b]$ 上连续，则 $f$ 在闭区间 $[a,b]$ 上一致连续。
### 定理 2：
设函数 $f$ 在开区间 $(a,b)$ 上连续，则 $f$ 在开区间 $(a,b)$ 上一致连续 $\iff\displaystyle\lim_{x\to a+0}f(x)$ 和 $\displaystyle\lim_{x\to b-0}f(x)$ 都存在。