z# 运动学
$\vec{r}$ 位矢.
$\dot{\vec{r}}=\dfrac{\mathrm{d}\vec{r}}{\mathrm{d}t}=\vec{v}$.  
$\ddot{\vec{r}}=\dfrac{\mathrm{d}\dot{\vec{r}}}{\mathrm{d}t}=\vec{a}$.  
（常用变换：$\ddot{\vec{r}}=\dfrac{\mathrm{d}\dot{r}}{\mathrm{d} r}\dfrac{\mathrm{d}r}{\mathrm{d}t}=\dfrac{\mathrm{d}\dot{r}}{\mathrm{d}r}\dot{r}$）  
### 三种常用坐标
#### 一、直角坐标（右手系）
$$
\vec{r}=(x,y,z)
$$
$$
\vec{v}=\dfrac{\mathrm{d}x}{\mathrm{d}t}\hat{x}+\dfrac{\mathrm{d}y}{\mathrm{d}t}\hat{y}+\dfrac{\mathrm{d}z}{\mathrm{d}t}\hat{z}
$$
$$\vec{a}=\dfrac{\mathrm{d}^2x}{\mathrm{d}t^2}\hat{x}+\dfrac{\mathrm{d}^2y}{\mathrm{d}t^2}\hat{y}+\dfrac{\mathrm{d}^2z}{\mathrm{d}t^2}\hat{z}$$  
此变换成立因为 $\{e_i\}$ 为不变基矢.
#### 二、极坐标
$$
\vec{r}=r\hat{r}
$$   
$$
\therefore\vec{v}=\dfrac{\mathrm{d}\vec{r}}{\mathrm{d}t}=\dot{r}\hat{r}+r\dfrac{\mathrm{d}\hat{r}}{\mathrm{d}t}=\dot{r}\hat{r}+r\dot{\theta}\hat{\theta}
$$  
($\dfrac{\mathrm{d}\hat{r}}{\mathrm{d}t}=\vec{\omega}\times\hat{r}=\dot{\theta}\hat{\theta},\dfrac{\mathrm{d}\hat{\theta}}{\mathrm{d}t}=-\dot{\theta}\hat{r}$)  
$$
\begin{aligned}
\therefore\vec{a}&=\ddot{r}\hat{r}+\dot{r}\dot{\theta}\hat{\theta}+\dot{r}\dot{\theta}\hat{\theta}+r\ddot{\theta}\hat{\theta}-r\dot{\theta}^2\hat{r}\\
&=(\ddot{r}-\dot{\theta}^2)\hat{r}+(r\ddot{\theta}+2\dot{r}\dot{\theta})\hat{\theta}
\end{aligned}
$$
（柱坐标只需添加 $\dot{z}\hat{z}$）  

注意 $\vec{v}=\dfrac{\mathrm{d}\vec{r}}{\mathrm{d}t}$ 并非 $\dfrac{\partial \vec{r}}{\partial t}$.  
如 $r+ut=A\cos\theta$,    
$r=A\cos\theta-ut$,  
求导后会存在 $\dot{\theta}$.  
若写成 $\dfrac{\partial}{\partial t}$ 形式需添加全导数项.  
$$
\vec{v}_x=\frac{\mathrm{d}x}{\mathrm{d}t}=\frac{\partial x}{\partial y}\frac{\partial y}{\partial t}+\frac{\partial x}{\partial z}\frac{\partial z}{\partial t}+\frac{\partial x}{\partial t}
$$
（流体描述）
#### 三、自然坐标
$s$ 为路程.  
$$\vec{v}=\dfrac{\mathrm{d}s}{\mathrm{d}t}\hat{\tau},\mathrm{d}\tau=\mathrm{d}\vec{\theta}\times\hat{\tau}
$$
$$
\begin{aligned}
\therefore \vec{a}&=\frac{\mathrm{d}^2s}{\mathrm{d}t^2}\hat{\tau}+\frac{\mathrm{d}s}{\mathrm{d}t}\cdot\frac{\mathrm{d}\theta}{\mathrm{d}t}\hat{n}\\
&=\frac{\mathrm{d}^2s}{\mathrm{d}t^2}\hat{\tau}+\frac{v^2}{\rho}\hat{n}
\end{aligned}
$$
$\rho$ 为曲率半径.  
$$
\begin{aligned}
\mathrm{d}s&=\sqrt{1+y'^2}\mathrm{d}x\\
&=\rho\mathrm{d}\theta\\
&=\rho|[\arctan y'(x)-\arctan y'(x+\mathrm{d}x)]|\\
&=\rho\frac{|\mathrm{d}y'|}{1+y'^2}\\
\therefore\rho&=\frac{(1+y'^2)^{\frac 3 2}}{|y''|}
\end{aligned}
$$
选取坐标系的奇技淫巧：
1. 若可轻松写出 $x(t),y(t),z(t)$ 参数方程，即选直角坐标系.  
2. 有固定转轴且有 $\dot{\theta}$ 或 $\dot{r}$，即选极坐标.  
3. $\vec{v}$ 已知或 $\rho$ 易求且轨迹易求，即选自然坐标. 
### 相对运动 ：
$$
\begin{aligned}
\vec{r}&=\vec{r_o}+\vec{r_c}\\
\therefore\frac{\mathrm{d}\vec{r}}{\mathrm{d}t}&=\frac{\mathrm{d}(\vec{r_o}+\vec{r_c})}{\mathrm{dt}}=\vec{v_o}+\vec{v_c}=\vec{v}\\
\end{aligned}
$$
同理，  
$$\vec{a}=\vec{a_o}+\vec{a_c}$$
### 转动换系
（这里是采取转动理论证明）
基矢
$$
e_i(t+\mathrm{d}t)=(\delta_{ij}+{\mathbf{\Phi}_i}^j(t))e_j(t)=e_i(t)+\mathrm{d}t{\mathbf{\Omega}^j}_ie_j(t)
$$
这里 $\mathbf{\Phi}$ 为无穷小转动矩阵.  
同时泰勒展开，对 $e_i$ 保留至一阶：
$$
\begin{aligned}
&e_i(t+\mathrm{d}t)=e_i(t)+\frac{\mathrm{d}e_i(t)}{\mathrm{d}t}\mathrm{d}t\\
\therefore & \frac{\mathrm{d}e_i}{\mathrm{d}t}={\mathbf{\Omega}^j}_ie_j\equiv-{\mathbf{\Omega}_i}^je_j
\end{aligned}
$$
记 $\{\overline{e_i}\}$ 是一组不变基矢，$\{e_i\}$ 是一组转动基矢，即有：
$$
\begin{aligned}
&e_i(t)={\mathbf{R}_i}^j(t)\overline{e_j}\\
\therefore&\frac{\mathrm{d}e_i}{\mathrm{d}t}=\frac{\mathrm{d}{\mathbf{R}_i}^k}{\mathrm{d}t}\overline{e_k}=\frac{\mathrm{d}{\mathbf{R}_i}^k}{\mathrm{d}t}{\mathbf{R}^j}_ke_j\\
\Rightarrow&{\mathbf{\Omega}_i}^j={\mathbf{R}_i}^k\frac{\mathrm{d}{\mathbf{R}^j}_k}{\mathrm{d}t}\\
\Rightarrow&\mathbf{\Omega}=\mathbf{R}\frac{\mathrm{d}\mathbf{R}^{\mathsf{T}}}{\mathrm{d}t}
\end{aligned}
$$
易得 $\mathbf{\Omega}$ 为反对称矩阵.  
$$
\mathbf{\Omega}=\begin{pmatrix}0&-\omega^3&\omega^2\\\omega^3&0&-\omega^1\\-\omega^2&\omega&0\end{pmatrix}\equiv\omega^1\mathbf{J}_1+\omega^2\mathbf{J}_2+\omega^3\mathbf{J}_3
$$
($\mathbf{J}_i$ 为无穷小转动生成元)