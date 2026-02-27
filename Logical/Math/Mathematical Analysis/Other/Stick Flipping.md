---
前置知识: "[[Derivative and Differential]]"
前置知识 2: "[[Triangle Basic Formulas]]"
---

2026 届 F15 班
鲁泊成

---
## 目录

问题提出与建模------------------------------1
圆求导与导数截距 ---------------------------2
两种方法证伪圆弧 ---------------------------3
问题证明与解决------------------------------4

---

# 一、问题提出与建模

![[Pasted image 20250216102949.png]]
如上图所示，一根木棍贴着墙正在向下滑动，直到落到地面为止。木棍在滑动的过程中扫过了一个图形，该图形由一对直角邻边和一个曲边组成（即为图中黑色部分），那么这个曲边到底是什么曲线？

我在刚开始时有三种设想：
1. 双曲线
2. 抛物线的一部分
3. 一个四分之一圆

![[Pasted image 20250210222019.png]]
可以将墙角和地面看作是y轴与x轴，

此时可以很明显的看出，上图中双曲线（$y=\dfrac{1}{x}$）永远也不会与两条坐标轴相交，但木棍却可以靠在墙边或落在地上，所以第一个猜想双曲线肯定是不成立的。
![[Pasted image 20250210222131.png]]
此时有人可能会想：那么如果将双曲线向斜下方平移一下（如左下图所示）不就好了？这个问题我们将会在后面解决。

事实上，这个猜想其实也是错的。

再看抛物线。
我们注意到，木棍的每一个状态都能关于y=x找到一个对称的状态，所以这条曲线也是一个轴对称图形，它的对称轴是y=x.
显然，抛物线不满足这个条件。

---
# 二、圆求导与导数截距

![[Pasted image 20250210230936.png]]

 假设有一辆汽车，它行驶的路程与时间的图像如上图所示，路程对于时间的函数是 $f$，求它在 $x$ 秒时的速度是多少？
我们可以这样算：先算 $x\sim x+1$ 这个时间段的平均速度，这很好算，物理课讲过，$\dfrac{s_总}{t_总}$，即：$\dfrac{f(x+1)-f(x)}{1}$，分子是这段时间内行驶的路程，分母是行驶的时间：$1$ 秒。
然后算 $x\sim x+0.5$ 这个时间段的平均速度：$\dfrac{f(x+0.5)-f(x)}{0.5}$。以此类推，直到分母无限趋近于 $0$。
此时算出的速度就是 $x$ 秒这一瞬间的速度了！设分母为 $h$，我们要让 $h$ 趋近于 $0$，写作 $h\to0$。

这个式子就变成了 $\displaystyle\lim_{h\to0}\dfrac{f(x+h)-f(x)}{h}$ 。它就是：
在 $h$ 无限趋近于零时，右边式子的值。
那么，是否可以直接将 $h=0$ 代入 $\dfrac{f(x+h)-f(x)}{h}$ 呢？ 显然是不可以的，分母不能为 $0$。那该怎么求呢？举个例子：求 $y=kx$ 这个函数的导数，$k$ 为常数，这是一个简单的一次函数。即：$f(x)=kx$。此时，
$$
\lim_{h\to0}\frac{f(x+h)-f(x)}{h}=\lim_{h\to0}\frac{kx+kh-kx}{h}=\lim_{h\to0}\frac{kh}{h}=k
$$

所以说，这个函数的导函数是 $k$!（导函数就是这个函数的导数构成的函数，例如原本的函数 $f(x)$ 的导函数就表示为 $f’(x)$，$f’(x)$ 为 $f$ 在 $x$ 点上的导数）

通常我们还有一种表达方式 $f’(x)=\dfrac{\mathrm{d}y}{\mathrm{d}x}$。（$\mathrm{d}y$，$dx$ 是一个整体，不是指它们的商），它就代表着（$y$（也就是 $f(x)$）随之发生的变化的量）除以（$x$ 产生极其微小的变化量），$x$ 的变化其实就是 $h$，它无限趋近于 $0$。

这种表示方法更加的简单，直观，它也被称为微分。

在几何上看，$f(x)$ 上某个点的导数其实就是函数在这个点上的切线。
这里有一些基本的导数公式：
$$
\begin{aligned}
f’(axb)&=abx^{b-1}\\
f’(\sin x)&=\cos x\\
f’(\cos x)&= -\sin x\\
f’(\tan x)&= \sec^2x\\
f’(\cot x)&= -\csc^2x \\
f’(\sec x)&=\sec x\tan x\\
f’(\csc x)&= -\csc x\cot x\\
f’(a^x)&=a^x\ln a\\
f’(\log_ax)&=\frac{1}{x \ln a}\\
\dfrac{\mathrm{d}(uv)}{\mathrm{d}x}&=v\dfrac{\mathrm{d}u}{\mathrm{d}x}+u\dfrac{\mathrm{d}v}{\mathrm{d}x}\\
\dfrac{\mathrm{d}\frac u v}{\mathrm{d}x}&=\dfrac{v\dfrac{\mathrm{d}u}{\mathrm{d}x}-u\dfrac{\mathrm{d}v}{\mathrm{d}x}}{v^2}\\
\end{aligned}
$$
最后一个公式最重要：
$y$ 是 $u$ 的函数，$u$ 是 $x$ 的函数，那么 $\dfrac{\mathrm{d}y}{\mathrm{d}x}=\dfrac{\mathrm{d}y}{\mathrm{d}u}\dfrac{\mathrm{d}u}{\mathrm{d}x}$。

![[Pasted image 20250211045543.png]]
终于可以猜想圆弧了！
我们假设木棍的长度是 $1$，该圆弧所在的圆的半径也是 $1$。
所以这个圆的方程是 $x^2+y^2=1$ 吗？并不是。因为这个圆的圆心不是原点，所以要把 $x^2+y^2=1$ 向右上平移一格，得到
$$
(x-1)^2+(y-1)^2=1 
$$
然后，我们发现，木棍的每一个状态都是这条曲线的切线，也就是导数！那么圆的导数该怎么求呢？
先拆开，$(x-1)^2+(y-1)^2=x^2+y^2-2x-2y+2=1$

由于斜率的定义，我们必须要给方程两边同时对 $x$ 求导，具体怎么做？只需要给两边同乘上一个 $\dfrac{\mathrm{d}}{\mathrm{d}x}$  (再次说明，$\dfrac{\mathrm{d}y}{\mathrm{d}x}$ 是一个整体，分子单独的” $\mathrm{d}$”只不过是把方程两边的式子当成 $y$ 了而已）

$\dfrac{\mathrm{d}x^2}{\mathrm{d}x}+\dfrac{\mathrm{d}y^2}{\mathrm{d}x}-2\dfrac{\mathrm{d}x}{\mathrm{d}x}-2\dfrac{\mathrm{d}y}{\mathrm{d}x}+0=0$ (常数的导数=0）

$2x +\dfrac{\mathrm{d}y^2}{\mathrm{d}x}-2\dfrac{\mathrm{d}y}{\mathrm{d}x}=2$

$\dfrac{\mathrm{d}y^2}{\mathrm{d}x}$ 怎么算？设 $u=y^2$ ,算 $\dfrac{\mathrm{d}u}{\mathrm{d}x}$。根据上一页那个重要的公式，得：

$\dfrac{\mathrm{d}y^2}{\mathrm{d}x}=\dfrac{\mathrm{d}u}{\mathrm{d}x}=\dfrac{\mathrm{d}u}{\mathrm{d}x}\dfrac{\mathrm{d}y}{\mathrm{d}x}=2y\dfrac{\mathrm{d}y}{\mathrm{d}x}$
代入上式，得

 $2x+2y\dfrac{\mathrm{d}y}{\mathrm{d}x}-2\dfrac{\mathrm{d}y}{\mathrm{d}x}=2$

$x+y\dfrac{\mathrm{d}y}{\mathrm{d}x}-\dfrac{\mathrm{d}y}{\mathrm{d}x}=1$

$\dfrac{\mathrm{d}y}{\mathrm{d}x}=-\dfrac{x-1}{y-1}$
所以圆上每一个点 $(x,y)$ 的切线的斜率是 $-\dfrac{x-1}{y-1}$！
这个对一个方程求导的方法叫做：隐函数求导。
那么该怎么证明木棍形成的曲线是不是圆弧呢？
思考：导数是求切线的斜率，那是否能将切线的 $x$ 轴和 $y$ 轴的截距也算出来呢？
假设我们有一个一次函数 $y=kx+b$。
现在我们只知道k,且该函数经过点 $(x_0,y_0)$，求该函数的 $x$ 轴，$y$ 轴截距？
很简单一个问题。首先代入方程，得 $y_0=kx_0+b$，$b=y_0-kx_0$。
$x$ 轴截距其实是 $y=0$ 时 $x$ 的值。即 $0=kx+b$，
$x=-\dfrac{b}{k}=\dfrac{kx_0-y_0}{k}$。
其实此时的 $y_0=f(x_0)$。

因为我们只研究圆的下半部分，所以 $y=f(x)=1-\sqrt{2x-x^2}$。（通过解二次方程）  
所以可以得到：
$k=-\dfrac{x-1}{y-1}=\dfrac{x-1}{\sqrt{2x-x^2 }}$  
$b=y-kx=1-\sqrt{2x-x^2}- \dfrac{x^2-x}{\sqrt{2x-x^2}} = 1-\dfrac{x\sqrt{2x-x^2 }}{2x-x^2}=\dfrac{2x-x^2-x\sqrt{2x-x^2}}{2x-x^2}=\dfrac{2-x-\sqrt{2x-x^2}}{2-x}$
$x$ 轴截距就是 
$-\dfrac b k=\dfrac{-b}{k}=\dfrac{2-x-\sqrt{x-x^2}}{x-2}\dfrac{\sqrt{2x-x^2}}{x-1}=\dfrac{2\sqrt{2x-x^2}-x\sqrt{2x-x^2}-2x+x^2}{x^2-3x+2}=\dfrac{x-\sqrt{2x-x^2}}{x-1}$
得到 $x$ 轴截距与 $y$ 轴截距后，神奇的一步就来了。

---
# 三、两种方法证伪圆弧

我们在得到了这个四分之一圆每个点上的 $x$ 轴截距（$a$）和 $y$ 轴截距（$b$）后，只需要证明 $a^2+b^2\equiv1$ 即可，但它等于 $1+\dfrac{x^3-4x^2+5x-2\sqrt{2x-x^2}}{(x^2-3x+2)^2}$，很明显，第二项的那个复杂式子分子都不是零，所以第二项不恒为 $0$，所以原式不恒为 $1$，不是个圆弧！

![[Pasted image 20250211152602.png]]

我到后来才发现，其实可以很简单的用几何证明！
由于这是一个圆弧，所以 $AB=1$，
又木棍的长度为 $1$，所以 $CD=1$，
假设木棍现在刚好移动到了中间的位置，
那么 $AO=\dfrac1 2$ , 又 $OB=\sqrt 2$ ,
所以 $AB=\sqrt2-\dfrac{1}{2}\neq1$，矛盾。

---
# 四、问题证明与解决
经过观察我们发现：
  >假如我们固定了一条直线 $L$（下图蓝色），它的式子是 $x=a$,
  >哪个木棍的状态与L这条竖线的相交点  $y$ 最大，       
  >那么这条曲线在经过 $a$ 时。
  >曲线那个点的纵坐标也就是那个最大的 $y$。

![[屏幕截图 2025-02-15 155117.png]]

看来还是用几何的方法比较好。

![[Pasted image 20250215160610.png]]

假设木棍是 $AB$，那么 $AB=1$。它与地面的夹角是 $\theta$，与曲线的交点是 $C$，$C$ 对应的横坐标是 $X$，纵坐标是 $Y$,设 $OX=x$， $OY=y$。
所以 $YC=x,CX=y$。
 通过三角函数的定义可知，
$AO=\sin\theta,BO=\cos\theta,\dfrac{AO}{BO}=\tan\theta$。
又因为 $AO//CX,YC//OB,\angle O=90°$，
$\Delta AYC \sim \Delta CXB \sim \Delta AOB$。
所以 $\dfrac{AY}{YC}=\tan\theta$。
所以 $\dfrac{\sin\theta-y}{x}=\tan\theta$。
所以 $y=\sin \theta-x\tan\theta$。
刚开始时，木棍不和直线 $L(x=a)$ 相交。
木棍逐渐滑动，直到与 $L$ 交于 $x$ 轴。然后继续滑动，与 $L$ 的交点越来越高，直到一个最大值（临界点） $P(a,b)$，如果此时继续滑动，那么木棍与 $L$ 的交点反而会变低，直到木棍落到地上，与 $L$ 交于 $(a,0)$。那么这条曲线必然过 $(a,b)$。因为 $P$ 点再往上就不会有线经过了。所以 $P$ 必然在木棍扫过的图形的边线上，也就是在这条曲线上！我们只需要知道每一对 $(a,b)$ 的关系，就能得出 $x ,y$ 的关系式了！就像下图：

![[Pasted image 20250216092003.png]]

（图中 $x$ 轴为时间轴，$y$ 轴为与 $L$ 的交点的纵坐标）
现在，$y$ 怎样能最大呢？
求导。有一个关于导数的极值定理可以解决这个问题。
关于极值定理，曾经有一道很早的思考题运用了它，就是那个折无盖长方体求最大体积的那一道，我们将求三次式最值问题转化成了解二次方程。

>极值定理：
> $c$ 在**开区间** $(a,b)$ 内，若 $f(c)$ 为 $f$ 在这个区间内的最值，那么 $f$ 在 $c$ 点上
>导数（ $f’(c)$ ) 为 $0$ 或不存在。

我特意强调了开区间，因为如果是闭区间，那么很可能函数在端点上有最值，不一定导数是 $0$，但开区间没有端点。
现在~~不太严谨地~~证明一下。假设是最大值，那么函数必然经历了先爬坡再下坡的过程。而函数上升时导数肯定为正，下降时为负，而由上升转为下降，导数由正转到负的那个转折点，即“山顶”，最大值，导数则必然是 $0$ ！
不难看出，最小值也是同理。
所以就对刚才那个式子 $y=\sin\theta-x\tan\theta$ 求导（把 $\theta$ 看作常数），得到 $y’=\cos\theta-\dfrac{x}{\cos^2\theta}$
在 $y’=0$ 时，$y$ 取最大值。此时 $\cos\theta=\sqrt[3]{x}$。
因为 $\sin^2\theta+\cos^2\theta=1$，
所以 $\sin\theta=\sqrt{1-\cos^2\theta}=\sqrt{1-\sqrt[3]{x^2}}=(1-x^{\frac 2 3})^{\frac 1 2}$。
所以 
$$
\begin{aligned}
y&=\sin\theta-x\tan\theta\\
&=\sin\theta-x\frac{\sin\theta}{\cos\theta}\\
&=\sin\theta(1-\frac{x}{\cos\theta})\\
&=(1-x^{\frac 2 3})^{\frac 1 2}(1-\frac{x}{x^{\frac 1 3}})\\
&=(1-x^{\frac 2 3})^{\frac 1 2}(1-x^{\frac 2 3})\\
&=(1-x^{\frac 2 3})^{\frac 3 2}\\
\end{aligned}
$$
所以最后的曲线就是 $x^{\frac 2 3}+y^{\frac 2 3}=1$ 在第一象限的部分。
![[Pasted image 20250216093945.png]]

---
## 感谢观看！