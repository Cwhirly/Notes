## 三角函数
### 初中定义 ：
设 $\alpha$ 是一个锐角，那么：  
有一个 $Rt\triangle ABC$，若 $\angle B=90\degree,\angle A=\alpha$，则 $\sin(\alpha)=\dfrac{BC}{AC},\cos(\alpha)=\dfrac{AB}{AC}$。  
### 高中定义：
#### 弧度制
$x\degree$ 在弧度制下的表示为 $\dfrac{\pi x}{180\degree}$。
#### 单位圆
平面直角坐标系中，圆心为 $(0,0)$，半径为 $1$ 的圆。  
#### 三角函数
一个点 $x$，从 $(1,0)$ 出发，沿着单位圆逆时针绕 $(0,0)$ 旋转，走过 $\theta$ 的单位长度后，其纵坐标即为 $\sin(\theta)$，横坐标即为 $\cos(\theta)$。
#### 诱导公式
$$
\begin{aligned}
\sin(\theta)&=\sin(\theta+2k\pi)\\
\cos(\theta)&=\cos(\theta+2k\pi)\\
(k\in\mathbb{Z})\\
\sin(\frac{\pi}{2}-\theta)&=\cos(\theta)\\
\sin(\pi-\theta)&=\sin(\theta)\\
\cos(\pi-\theta)&=-\cos(\theta)\\
\sin(\theta)&=-\sin(-\theta)\\
\cos(\theta)&=\cos(-\theta)
\end{aligned}
$$
#### 图像

![[Pasted image 20250327200124.png]]
![[Pasted image 20250327200154.png]]
（Windows 11 自带计算器绘制）
其他公式：[[Triangle Basic Formulas]]

## 向量
### 高中定义
向量是一种有大小，有方向的量。
相当于一条有向线段，但不在乎起点的位置。  
也就是说，一个向量平移后仍与原向量相等。    
所以**有时**为了方便计算，可以假定所有向量的起点都是 $(0,0)$。
一个平面向量 $\alpha$ 可以用一个实数点对 $(x,y)$ 来表示。  
这表示，将 $\alpha$ 平移使得起点为 $(0,0)$，$\alpha$ 此时的终点即为 $(x,y)$。
#### 加法 
设 $\alpha=(x_0,y_0),\beta=(x_1,y_1)$。  
则 $\alpha+\beta=(x_0+x_1,y_0+y_1)$。  
在几何中，还可以用“平行四边形法则”理解向量加法。  
![[Pasted image 20250327201423.png]]
如图，在平行四边形 $ABCD$ 中， $\overrightarrow{AB}+\overrightarrow{AC}=\overrightarrow{AD}$。(Geogebra 绘制)
### 减法
定义 $-\alpha$ 为与 $\alpha$ 大小相同，但方向相反的向量。
$\alpha-\beta=\alpha+(-\beta)$。
#### 模长
向量 $\alpha$ 的模长记为 $|\alpha|$，即为 $\alpha$ 的大小（长度）。
若 $\alpha=(x,y)$，则 $|\alpha|=\sqrt{x^2+y^2}$。
#### 点积（数量积）
$\alpha$ 与 $\beta$ 的点积记为 $\alpha\cdot\beta$。
$\alpha\cdot\beta=|\alpha||\beta|\cos\langle\alpha,\beta\rangle$。
其中 $\langle\alpha,\beta\rangle$ 为将 $\alpha,\beta$ 平移到同一起点时，它们的夹角。  
同时，若 $\alpha=(x_0,y_0),\beta=(x_1,y_1)$，则 $\alpha\cdot\beta=x_0x_1+y_0y_1$。  

本道题所需要的**全部**前置数学知识如上文所示。

>[!example] 拓展（与题目无关）
>设 $V$ 是一个非空集合，$K$ 是数域，$V$ 上规定了加法（$V^2\mapsto V$）及数量乘法（$V\times K\mapsto V$），并且满足以下八条性质：(希腊字母为 $V$ 中元素，拉丁字母为 $K$ 中元素)
>1) $\alpha+\beta=\beta+\alpha$
>2) $(\alpha+\beta)+\gamma=\alpha+(\beta+\gamma)$
>3) $\exists0\in V$, 有 $\alpha+0=\alpha$，称为零元
>4) $\forall\alpha\in V,\exists\beta\in V$，使得 $\alpha+\beta=0$，互为负元
>5) $1\alpha=\alpha$
>6) $(kl)\alpha=k(l\alpha)$
>7) $(k+l)\alpha=k\alpha+l\alpha$
>8) $k(\alpha+\beta)=k\alpha+k\beta$
> 
>则称 $V$ 是 $K$ 上的**线性空间**。  
平面向量构成的集合即为 $\mathbb{R}$ 上的线性空间




