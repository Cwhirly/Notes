## 1. P2257
Problem : There are $T$ groups of querys, in each query, give two numbers $n$, $m$, print the value of 
$$
\sum_{i=1}^n\sum_{j=1}^m[\gcd(i,j)\in\mathbb{P}]
$$
Solution : 
$$
\begin{aligned}
\text{ans}&=\sum_{i=1}^n\sum_{j=1}^m[\gcd(i,j)\in\mathbb{P}]\\
&=\sum_{p\in\mathbb{P}}\sum_{i=1}^n\sum_{j=1}^m[\gcd(i,j)=p]\\
&=\sum_{p\in\mathbb{P}}\sum_{i=1}^{\lfloor\frac n p\rfloor}\sum_{j=1}^{\lfloor\frac m p\rfloor}[\gcd(ip,jp)=p]\\
&=\sum_{p\in\mathbb{P}}\sum_{i=1}^{\lfloor\frac n p\rfloor}\sum_{j=1}^{\lfloor\frac m p\rfloor}[\gcd(i,j)=1]\\
&=\sum_{p\in\mathbb{P}}\sum_{i=1}^{\lfloor\frac n p\rfloor}\sum_{j=1}^{\lfloor\frac m p\rfloor}\varepsilon(\gcd(i,j))\\
&=\sum_{p\in\mathbb{P}}\sum_{i=1}^{\lfloor\frac n p\rfloor}\sum_{j=1}^{\lfloor\frac m p\rfloor}(\mu\ast1)(\gcd(i,j))\\
&=\sum_{p\in\mathbb{P}}\sum_{i=1}^{\lfloor\frac n p\rfloor}\sum_{j=1}^{\lfloor\frac m p\rfloor}\sum_{d|\gcd(i,j)}\mu(d)\\
&=\sum_{p\in\mathbb{P}}\sum_{i=1}^{\lfloor\frac n p\rfloor}\sum_{j=1}^{\lfloor\frac m p\rfloor}\sum_{d|i,d|j}\mu(d)\\
&=\sum_{p\in\mathbb{P}}\sum_{d=1}^{\min({\lfloor\frac n p\rfloor},{\lfloor\frac m p\rfloor})}\mu(d)\sum_{i=1}^{\lfloor \frac{\lfloor\frac n p\rfloor}{d}\rfloor}\sum_{j=1}^{\lfloor \frac{\lfloor\frac m p\rfloor}{d}\rfloor}1\\
&=\sum_{p\in\mathbb{P}}\sum_{d=1}^{\min({\lfloor\frac n p\rfloor},{\lfloor\frac m p\rfloor})}\mu(d)\lfloor\frac{n}{dp}\rfloor\lfloor\frac{m}{dp}\rfloor\\
&=\sum_{p\in\mathbb{P}}\sum_{d=1}^{\min({\lfloor\frac n p\rfloor},{\lfloor\frac m p\rfloor})}\mu(d)\lfloor\frac{n}{\xi}\rfloor\lfloor\frac{m}{\xi}\rfloor\\
&=\sum_{\xi=1}^{\min(n,m)}\sum_{p\in\mathbb{P},p|\xi}\mu(\frac \xi p)\lfloor\frac{n}{\xi}\rfloor\lfloor\frac{m}{\xi}\rfloor\\
&=\sum_{\xi=1}^{\min(n,m)}\lfloor\frac{n}{\xi}\rfloor\lfloor\frac{m}{\xi}\rfloor\sum_{p\in\mathbb{P},p|\xi}\mu(\frac \xi p)\\
\end{aligned}
$$

Let $\psi(n)=\displaystyle\sum_{p\in\mathbb{P},p|n}\mu(\dfrac n p)$

Noticed that we can pretreat $\psi$ and the sum of it, so this problem can be solved with the time complexity of $O(n\log\log n+T\sqrt{n})$.  

## 2. P3327
Problem : Give two integers $n$ and $m$, get the value of $\displaystyle\sum_{i=1}^n\sum_{j=1}^md(ij)$.
Solution : 
$$
\begin{aligned}
\text{ans}&=\sum_{i=1}^n\sum_{j=1}^m\sum_{x|i}\sum_{y|j}\varepsilon(\gcd(x,y))\\
&=\sum_{x=1}^n\sum_{y=1}^m\sum_{i=1}^{\lfloor\frac n x\rfloor}\sum_{j=1}^{\lfloor\frac m y\rfloor}\varepsilon(\gcd(x,y))\\
&=\sum_{x=1}^n\sum_{y=1}^m\sum_{i=1}^{\lfloor\frac n x\rfloor}\sum_{j=1}^{\lfloor\frac m y\rfloor}\sum_{d|x,d|y}\mu(d)\\
&=\sum_{x=1}^n\sum_{y=1}^m\sum_{d|x,d|y}\mu(d)\sum_{i=1}^{\lfloor\frac n x\rfloor}\sum_{j=1}^{\lfloor\frac m y\rfloor}1\\
&=\sum_{x=1}^n\sum_{y=1}^m\sum_{d|x,d|y}\mu(d)\lfloor\frac n x\rfloor\lfloor\frac m y\rfloor\\
&=\sum_{d=1}^{\min(n,m)}\mu(d)\sum_{x=1}^{\lfloor\frac n d\rfloor}d\sum_{y=1}^{\lfloor\frac m d\rfloor}d\lfloor\frac {n} {dx}\rfloor\lfloor\frac m {dy}\rfloor\\
&=\sum_{d=1}^{\min(n,m)}\mu(d)d^2\sum_{x=1}^{\lfloor\frac n d\rfloor}\lfloor\frac {n} {dx}\rfloor\sum_{y=1}^{\lfloor\frac m d\rfloor}\lfloor\frac {m} {dy}\rfloor\\
&=\sum_{d=1}^{\min(n,m)}\mu(d)\Psi(\lfloor\frac n d\rfloor)\Psi(\lfloor\frac m d\rfloor)\\
&\Psi(n)=\sum_{i=1}^ni\lfloor \frac n i\rfloor
\end{aligned}
$$
Now we can use integer division partition to solve this problem with the time complexity of $O(n\sqrt n+T\sqrt n)$。






主要内容来自《具体数学》和《组合数学》

## 1. 特征方程方法

如何求斐波拉契数列的通项公式？这里的斐波拉契数列定义为满足：

$$\begin{aligned}f_0=&0\\f_1=&1\\f_i=&f_{i-1}+f_{i-2}\end{aligned}$$

的数列 $\langle f_n\rangle$。

仔细观察我们可以猜测该数列是若干等比数列的和，于是我们设其中一个等比数列公比为 $\lambda$，就应满足：

$$\lambda^2=\lambda+1$$

解方程得 $\lambda=\dfrac{1\pm\sqrt 5}{2}$，即黄金分割率 $\phi\approx1.61803$ 和 $1-\phi\approx-0.61803$，为了方便我们将 $1-\phi$ 记作 $\hat \phi$。（注：黄金分割率也有人定义为 $\dfrac{\sqrt 5 -1}{2}=-\hat \phi\approx 0.61803$。）

然后我们来算一下系数，令 $f_n=c_1\phi^n+c_2\phi^n$，得方程组：

$$\begin{cases}f_0=c_1+c_2=0\\f_1=\phi c_1+\hat\phi c_2=1\end{cases}$$

解得 $c_1=\dfrac{1}{\phi-\hat\phi}=\dfrac{1}{\sqrt5},c_2=\dfrac{1}{\hat\phi-\phi}=-\dfrac{1}{\sqrt 5}$，从而有 $f_n=\dfrac{\phi^n-\hat\phi^n}{\sqrt 5}$。

由于实际上在 $n\ge0$ 时 $\hat\phi^n/\sqrt5 <\dfrac{1}{2}$，所以也有 $f_n=\left\lfloor\dfrac{\phi^n}{\sqrt 5}+\dfrac{1}{2}\right\rfloor$（$n=0$ 时 $\dfrac{1}{\sqrt5}+\dfrac{1}{2}\approx0.947213595499958<1$，公式仍然成立）。

也因此，有 $\lim\limits_{n\to\infty}f_n/f_{n-1}=\phi$。

显然的是，这个方法是可以推广的，所以我们已经解决了这类问题，当然也有明显的问题，那就是我们需要解高次方程，没关系！利用一些注意力，这个方法还是很有用的。

但是我们来考虑数列 $\langle g_n\rangle$，它满足：

$$\begin{aligned}g_0=&0\\g_1=&1\\g_i=&2g_{i-1}-g_{i-2}\end{aligned}$$

这看起来和斐波拉契数列没啥区别，无非是换了系数，可不难注意到，$g_n=n$，很明显这并不是等比数列的和。

我们来套用一下之前的方法，设公比为 $\lambda$，有：

$$\lambda^2=2\lambda-1$$

所以 $\lambda=1$。

再设 $g_n=c$，得方程组

$$\begin{cases}g_0=c=0\\g_1=c=1\end{cases}$$

显然无解。

看来这个方法有点问题，我们并不能保证最后得出的方程组有唯一解。

但只要稍加改动也能使其有效，具体来说，我们设 $f_n=(c_1+c_2n)\lambda^n=c_1+c_2n$

然后便得到了 $f_n=n$。

让我们总结一下简单的情况吧，对于数列 $\langle h_n\rangle$，若满足：

$$h_n=\sum_{j=1}^{k}a_jh_{n-j},n\ge k,a_k\not=0$$

且 $h_0,h_1,\dots,h_{k-1}$ 有初值。

令复数 $q_1,q_2,\dots,q_k$ 为多项式方程 $q^k-\sum_{j=1}^{k}a_{j}q^{k-j}=0$ 的 $k$ 个根（算重根），若关于 $c_1,c_2,\dots,c_k$ 的方程组

$$\begin{cases}c_1+c_2+\dots+c_k&=h_{0}\\q_1c_1+q_2c_2+\dots+q_kc_k&=h_1\\q_1^2c_1+q_2^2c_2+\dots+q_k^2c_k&=h_2\\&\vdots\\q_1^{k-1}c_1+q_2^{k-1}c_2+\dots+q_k^{k-1}c_k&=h_k\end{cases}$$

有唯一解，则 $h_n=\sum_{j=1}^{k}c_jq_j^n$。

如何判定该方程组有唯一解呢？应用一点基础的线性代数知识，该方程组的系数矩阵

$$\left[\begin{matrix}1&1&\dots&1\\q_1&q_2&\dots&q_k\\q_1^2&q_2^2&\dots&q_k^2\\\vdots&\vdots&\ddots&\vdots\\q_1^{k-1}&q_2^{k-1}&\dots&q_k^{k-1}\end{matrix}\right]$$

为范德蒙德矩阵，其行列式为

$$\prod_{1\le i,j \le k}(q_j-q_i)$$

在 $q_1,q_2,\dots,q_k$ 互不相同时有唯一解。

范德蒙德矩阵逆并无简单形式，不过可以使用 FFT 快速计算。（哈哈，不会）

我们得到的多项式方程 $q^k-\sum_{j=1}^{k}a_{j}q^{k-j}=0$ 被称作**特征方程**，其 $k$ 个根 $q_1,q_2,\dots,q_k$
被称为**特征根**，数列 $h_n$ 的递推关系被称作**常系数线性递推关系**。

这就够用了，稍加改进，它便完全能解决常系数齐次线性递推了（假设你会解高次方程）。

## 2.（普通）生成函数法

对于数列 $\langle a_n\rangle$，函数

$$F(z)=\sum_{i=0}^{\infty}a_iz^i$$

被称作数列 $\langle a_n\rangle$ 的**生成函数 (GF)**，这里我们用 $[z^n]F(z)$ 表示 $z^n$ 的系数，即 $a_n$。

现在我们来考虑斐波拉契数列的生成函数，令其为 $F(z)$，可以得到

$$z^2F(z)+zF(z)+z=F(z)$$

所以：

$$F(z)=\dfrac{z}{1-z-z^2}$$

这该怎么解出系数？

我们考虑生成函数 $\dfrac{\alpha}{1-\beta x}$，用等比数列求和得 $\dfrac{\alpha}{1-\beta x}=\sum\limits_{n=0}^{\infty}\alpha\beta^nx^n$。

出于某种信任，我们相信 $F(z)=\dfrac{c_1}{1-\lambda_1z}+\dfrac{c_2}{1-\lambda_2z}$。

稍微算一下就有 $c_1=\dfrac{1}{\sqrt 5},c_2=-\dfrac{1}{\sqrt 5},\lambda_1=\phi,\lambda_2=\hat\phi$，所以

$$F(z)=\dfrac{1}{\sqrt 5}\left(\dfrac{1}{1-\phi z}-\dfrac{1}{1-\hat\phi z}\right)=\sum_{n=0}^{\infty}\dfrac{1}{\sqrt 5}(\phi^n-\hat\phi^n)x^n$$

就得到了通项，这正是正确的结果。

你可能会关心生成函数是否收敛，有的生成函数在 $z\not=0$ 时永远发散，斐波拉契数列的生成函数只有在 $\vert x\vert<1$ 时收敛，但这并不妨碍我们处理它。可以证明，关于生成函数的大部分操作永远有效，而不必关心是否收敛。

现在，我们来考查一下常系数线性递推数列的生成函数。省略若干细节，我们总能得到生成函数 $F(z)=\dfrac{P(z)}{Q(z)}+R(z)$，$P(z),Q(z),R(z)$ 都是整式，且 $P(z)$ 的次数小于 $Q(z)$ 的次数。

## 3. 无重根的有理展开定理



$$
\begin{aligned}
&=\sum_{i=1}^a\sum_{j=1}^b\sum_{k=1}^c\sigma_0(ijk)\\
&=\sum_{i=1}^a\sum_{j=1}^b\sum_{k=1}^c\sum_{x|i}\sum_{y|jk}\varepsilon(\gcd(x,y))\\
&=\sum_{i=1}^a\sum_{j=1}^b\sum_{k=1}^c\sum_{x|i}\sum_{y|jk}\sum_{d|x,d|y}\mu(d)\\
&=\sum_{x=1}^a\sum_{i=1}^{\lfloor\frac{a}{x}\rfloor}\sum_{j=1}^b\sum_{k=1}^c\sum_{y|jk}\sum_{d|x,d|y}\mu(d)\\
&=\sum_{x=1}^a\sum_{j=1}^b\sum_{k=1}^c\sum_{y|jk}\sum_{d|x,d|y}\mu(d)\lfloor\frac{a}{x}\rfloor\\
&=\sum_{j=1}^b\sum_{k=1}^c\sum_{y|jk}\sum_{d|y}\mu(d)\sum_{x=1}^{\lfloor\frac{a}{d}\rfloor}\lfloor\frac{a}{xd}\rfloor\\
&=\sum_{j=1}^b\sum_{k=1}^c\sum_{d|jk}\mu(d)\sigma_0(\frac{jk}{d})\sum_{x=1}^{\lfloor\frac{a}{d}\rfloor}\lfloor\frac{a}{xd}\rfloor\\
\end{aligned}
$$