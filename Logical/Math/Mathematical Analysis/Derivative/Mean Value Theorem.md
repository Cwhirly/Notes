## 一、Fermat 定理
若 $x_0$ 是 $f$ 的极值点，且导数 $f'(x_0)$ ，则一定有 $f'(x_0)=0$

（定理二，三，四皆满足 $f$ （以及 $g$）在 $[a,b]$ 上连续 （[[Continuous]]） ，在 $(a,b)$ 上可导([[Derivative and Differential]])）
## 二、Rolle 定理
$f(a)=f(b)\implies\exists\xi\in(a,b)$，使得 $f'(\xi)=0$
## 三、Lagrange 中值定理
$\exists\xi\in(a,b)$，使得 $$f'(\xi)=\frac{f(b)-f(a)}{b-a}$$
## 四、Cauchy 中值定理
若满足条件 $g(b)-g(a)\neq0$，和 $f'^2(x)+g'^2(x)\neq0,\forall x\in(a,b)$，则 $\exists\xi\in(a,b)$，使得
$$
\frac{f(b)-f(a)}{g(b)-g(a)}=\frac{f'(\xi)}{g'(\xi)}
$$
## 五、Darboux 定理
设 $f$ 在区间 $I$ 上可微，则 $f'$ 具有介值性质。
## 六、Taylor 公式
 $$
f(x)=f(x_0)+f'(x_0)(x-x_0)+\frac{f''(x_0)}{2!}(x-x_0)^2+\dots+\frac{f^{(n)}(x_0)}{n!}(x-x_0)^n+o((x-x_0)^n) (x\to x_0)
$$ 
余项 $R_n(x)=o((x-x_0)^n)$ 称为 ~={green}Peano 余项=~（[[Orders of Infinitesimals]]）。
这个项可以为 
$$
\frac{f^{(n+1)}(\xi)}{(n+1)!}(x-x_0)^{n+1}(x_0\le\xi\le x)
$$
$$
\frac{f^{(n+1)}(\eta)}{n!}(x-\eta)^n(x-x_0)(x_0\le\eta\le x)
$$

需要注意的是，Maclaurin 公式就是 $x_0$ 取 0 时的 Taylor 公式。

