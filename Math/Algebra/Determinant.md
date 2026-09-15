1.  $$
\begin{vmatrix}
a_{11}&a_{12}&...&a_{1n} \\
a_{21}&a_{22}&...&a_{2n} \\
\vdots&\vdots&&\vdots\\
a_{n1}&a_{n2}&\dots&a_{nn}
\end{vmatrix}
=\displaystyle\sum_{j_1j_2\dots j_n}(-1)^{\tau(j_1j_2\dots j_n)}a_{1j_1}a_{2j_2}\dots a_{nj_n}
$$
2. 上/下三角行列式的值等于它主对角线的数的乘积
3. 设 $$\begin{pmatrix}
a_{11}&a_{21}&...&a_{n1} \\
a_{12}&a_{22}&...&a_{n2} \\
\vdots&\vdots&&\vdots\\
a_{1n}&a_{2n}&\dots&a_{nn}
\end{pmatrix}$$ 是矩阵 $$
A=\begin{pmatrix}
a_{11}&a_{12}&...&a_{1n} \\
a_{21}&a_{22}&...&a_{2n} \\
\vdots&\vdots&&\vdots\\
a_{n1}&a_{n2}&\dots&a_{nn}
\end{pmatrix}
$$ 的**转置**，记作 $A'$，则有 $|A|=|A'|$
4. $$
\begin{vmatrix}
a_{11}&a_{12}&...&a_{1n} \\
a_{21}&a_{22}&...&a_{2n} \\
\vdots&\vdots&&\vdots\\
ka_{i1}&ka_{i2}&\dots&ka_{in}\\
\vdots&\vdots&&\vdots\\
a_{n1}&a_{n2}&\dots&a_{nn}
\end{vmatrix}=
k\begin{vmatrix}
a_{11}&a_{12}&...&a_{1n} \\
a_{21}&a_{22}&...&a_{2n} \\
\vdots&\vdots&&\vdots\\
a_{i1}&a_{i2}&\dots&a_{in}\\
\vdots&\vdots&&\vdots\\
a_{n1}&a_{n2}&\dots&a_{nn}
\end{vmatrix}
$$
5. $$ \begin{vmatrix}
a_{11}&a_{12}&...&a_{1 n} \\
\vdots&\vdots&&\vdots\\
b_1+c_1&b_2+c_2&\dots&b_n+c_n\\
\vdots&\vdots&&\vdots\\
a_{n 1}&a_{n 2}&\dots&a_{nn}
\end{vmatrix}=\begin{vmatrix}
a_{11}&a_{12}&...&a_{1 n} \\
\vdots&\vdots&&\vdots\\
b_1&b_2&\dots&b_n\\
\vdots&\vdots&&\vdots\\
a_{n 1}&a_{n 2}&\dots&a_{nn}
\end{vmatrix}+\begin{vmatrix}
a_{11}&a_{12}&...&a_{1 n} \\
\vdots&\vdots&&\vdots\\
c_1&c_2&\dots&c_n\\
\vdots&\vdots&&\vdots\\
a_{n 1}&a_{n 2}&\dots&a_{nn}
\end{vmatrix}$$
6. 交换行列式的两行，行列式的值取相反数。
7. 两行成比例，行列式的值为 $0$。
8. 把一行的倍数加到另一行上，行列式的值不变。
9. $|A|=\displaystyle\sum_{j=1}^n a_{ij}A_{ij}=\displaystyle\sum_{i=1}^n a_{ij}A_{ij}$，其中 $A_{ij}=(-1)^{i+j}M_{ij}$，称为 $(i,j)$ 元的代数余子式。
10. $\displaystyle\sum_{j=1}^n a_{ij}A_{kj}=\displaystyle\sum_{i=1}^n a_{ij}A_{il}=0$，前式要求 $i\neq k$，后式要求 $j\neq l$。
11. Vandermonde 行列式 
$$
\begin{vmatrix}
1&1&1&\dots&1\\
a_1&a_2&a_3&\dots&a_n\\
a_1^2&a_2^2&a_3^2&\dots&a_n^2\\
\vdots&\vdots&\vdots&&\vdots\\
a_1^{n-1}&a_2^{n-1}&a_3^{n-1}&\dots&a_n^{n-1}
\end{vmatrix}=\prod_{1\le j<i\le n}(a_i-a_j)
$$
12. (Cramer Rule) 一个 $n$ 元 $n$ 个方程的线性方程组有唯一解 $\iff$ 它的系数矩阵 $A$ 的行列式 $|A|\neq0$。且解为： 
$$
\begin{cases}
x_1=\frac{|B_1|}{|A|},\\
x_2=\frac{|B_2|}{|A|},\\
\dots\\
x_n=\frac{|B_n|}{|A|}\\
\end{cases}
$$ 其中 $B_i$ 为 $A$ 用 $b_1,b_2,\dots,b_n$ 代替第 $i$ 列的矩阵。

