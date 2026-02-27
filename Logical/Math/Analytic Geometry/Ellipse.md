本质是 [[Circle]] 经过仿射变换后的结果。
点到直线距离公式：
$$
\dfrac{|ax+by+c|}{\sqrt{a^2+b^2}}
$$
硬解定理：
$$
\begin{aligned}
&\begin{cases}
y=kx+m\\
\dfrac{x^2}{a^2}+\dfrac{y^2}{b^2}=1\\
\end{cases}\\
\\
&\begin{cases}
\delta=b^2+k^2a^2\\
x_1+x_2=-\dfrac{2a^2mk}{\delta}\\
x_1x_2=a^2\dfrac{m^2-b^2}{\delta}\\
y_1+y_2=\dfrac{2b^2m}{\delta}\\
y_1y_2=\dfrac{b^2(m^2-a^2k^2)}{\delta}\\
x_1y_2+x_2y_1=\frac{-2a^2b^2k}{\delta}\\
(x_1-c)(x_2-c)=\dfrac{f(c)}{\delta}\\
\Delta=4a^2b^2(\delta-m^2)\\
|AB|=\dfrac{\sqrt{\Delta}}{\delta}\sqrt{k^2+1}
\end{cases}\\
\end{aligned}
$$
联立所得二次方程：
$$
\begin{aligned}
(a^2k^2+b^2)x^2+2a^2kmx+a^2(m^2-b^2)=0\\
(a^2k^2+b^2)y^2-2b^2my+b^2(m^2-a^2k^2)=0
\end{aligned}
$$
焦点弦：
$$
l=\frac{b^2}{a+c\cos\theta}=\frac{\frac{b^2}{a}}{1+e\cos\theta}
$$
$$
AB=\frac{\frac{2b^2}{a}}{1-e^2\cos^2\theta}
$$

面积大多用弦长和到原点的距离算。
$PF_1F_2$ 的面积：（ $\angle F_1PF_2=\alpha$ ）([[Triangle Basic Formulas]])
$$
S=b^2\tan\frac{\alpha}{2}
$$
若设 $y=kx+b$，先讨论斜率不存在的情况。
根据实际情况考虑设 $y=kx+b$ 或 $x=ty+c$。

通常来说，联立 
$$
\begin{cases}
\frac{x^2}{a^2}+\frac{y^2}{b^2}=1\\
y=kx+b
\end{cases}
$$
使用均值放缩：
$$
\frac{n}{\displaystyle\sum_{i=1}^n\frac{1}{a_i}}\le\sqrt[n]{\prod_{i=1}^{n}a_i}\le\frac{\displaystyle\sum_{i=1}^na_i}{n}\le\sqrt{\frac{\displaystyle\sum_{i=1}^na_i^2}{n}}
$$
要注意是否可以取等。

中点弦（二次曲线 $Ax^2+Cy^2+Dx+Ey+F=0$，弦 $l$ 中点为 $(x_P,y_P)$）：
$$
\begin{aligned}
l:&Ax_Px+Cy_Py+D\dfrac{x_P+x}{2}+E\dfrac{y_P+y}{2}+F=Ax_P^2+Cy_P^2+Dx_P+Ey_P+F\\

\end{aligned}
$$

$S_{\triangle OAB}=\dfrac{1}{2}|x_Ay_B+x_By_A|$

$P\not \in \mathscr{C}$，过 $P$ 的动直线交 $\mathscr{C}$ 于 $A,B(A\neq B)$，过 $A,B$ 的两条切线交于 $Q$，则 $Q$ 的轨迹在直线
$$
Ax_Px+Cy_Py+D\dfrac{x_P+x}{2}+E\dfrac{y_P+y}{2}+F=0
$$

$P\not \in \mathscr{C}$，过 $P$ 作 $\mathscr{C}$ 的切线，切点为 $A,B$，$AB:$
$$
Ax_Px+Cy_Py+D\dfrac{x_P+x}{2}+E\dfrac{y_P+y}{2}+F=0
$$


