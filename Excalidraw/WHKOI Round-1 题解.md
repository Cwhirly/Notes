
本套题目均由 $\color{gray}Revitalize$ 一个人出题、出题解，首次出题，或许有些做的不好的地方，可以来洛谷找我讨论，但对于因为题面等因素（非学术问题）而来的，一律拉黑并打飞。  

| 题目编号 |   T1    |       T2       |         T3          |                      T4                       |
| :--: | :-----: | :------------: | :-----------------: | :-------------------------------------------: |
|  难度  |    紫    |       紫        |          紫          |                       黑                       |
| 算法标签 | 莫比乌斯反演  | 线段树，三角函数，矩阵快速幂 | 计算几何，离线二维数点，树状数组，二分 | 快速数论变换，原根，分治，多项式多点求值，多项式取模，多项式求逆，线段树，高等代数，行列式 |
| 对标比赛 | NOIP T2 |    CSP-S T3    |      CSP-S T4       |                    NOIP T4                    |
四道题虽然评级高，但只是因为算法难度而虚高，实际上都是可做的。 
Deepseek 爆零了！
## Task 1
联立方程：
$$
\begin{cases}
y=kx+m\\
\dfrac{x^2}{a^2}+\dfrac{y^2}{b^2}=1\\
\end{cases}\\
$$ 则有
$$
\begin{aligned}
\dfrac{x^2}{a^2}+\dfrac{k^2x^2+2kmx+m^2}{b^2}=1\\
(\frac{1}{a^2}+\frac{k^2}{b^2})x^2+\frac{2km}{b^2}x+\frac{m^2-b^2}{b^2}=0\\
(\frac{b^2+k^2a^2}{a^2})x^2+2kmx+m^2-b^2=0\\
\end{aligned}
$$
由求根公式我们可以得到韦达定理：
一元二次方程 $ax^2+bx+c=0$ 的两个根 $x_1,x_2$ 满足 $x_1+x_2=-\dfrac{b}{a},x_1x_2=\dfrac c a$。
我们设 $\lambda=b^2+k^2a^2$，
则 $x_1+x_2=-a^2\dfrac{2km}{\lambda},x_1x_2=a^2\dfrac{m^2-b^2}{\lambda}$。
现在 Subtask 1，Subtask 2 就解决了。
再由求根公式我们可以得到根间距公式：$|x_1-x_2|=\dfrac{\sqrt{\Delta}}{a}$。  
所以我们就有 $|x_1-x_2|=a^2\dfrac{\sqrt{\Delta}}{\lambda},|y_1-y_2|=ka^2\dfrac{\sqrt{\Delta}}{\lambda}$ 。  
故而 $|AB|=a^2\dfrac{\sqrt{\Delta}}{\lambda}\sqrt{k^2+1}$。
$\Delta$ 就是 $b^2-4ac$ 即 $4\dfrac{b^2}{a^2}(\lambda-m^2)$。
如果你想让式子更美观就设 $\Delta'=4a^2b^2(\lambda-m^2)$，所以 $|AB|$ 就是 $\dfrac{\sqrt{\Delta'}}{\lambda}\sqrt{k^2+1}$。
还有就是 $\sqrt{\Delta}$ 在模意义下需要用到 Cipolla。
**关于这个算法，如果你感兴趣，或者你名字叫做 Sci_qud 的话可以自学（对于后者，必须自学）。**
如果没有二次剩余就输出 `998244354`。  

## Task 2
对于 Subtask 4，答案就是 0。
对于 Subtask 5，考虑先求出 $|AB|$，再根据点到直线距离公式可得面积，均值即可。
具体地：
根据硬解定理，有 $|AB|=\dfrac{\sqrt{\Delta}}{b^2+k^2a^2}\sqrt{k^2+1}$。  
并且 $l$ 到原点的距离是 $\dfrac{|ku+v|}{\sqrt{k^2+1}}$。  
$\therefore S=ab\dfrac{\sqrt{k^2u^2+2kuv+v^2}\sqrt{b^2+k^2a^2-k^2u^2-2kuv+v^2}}{b^2+k^2a^2}$

直接均值得原式最大值为 $\dfrac 1 2ab$。  
如果不可取等，即方程 $k^2u^2+2kuv+v^2=b^2+k^2a^2-k^2u^2-2kuv+v^2$ 无实根。  
即 $\Delta<0$，也即 $2u^2b^2+2v^2a^2<a^2b^2$ 时，  
原式最终可以化简为 $ab\sqrt{t-t^2}$，其中 $t=\dfrac{k^2u^2+2kuv+v^2}{b^2+k^2a^2}$。  
发现 $t$ 取 $\dfrac 1 2$ 时原式最大为 $\dfrac 1 2ab$，但因为均值不可取等，所以取不到 $\dfrac{1}{2}ab$。 
此时解一下 $\dfrac{\mathrm{d}t}{\mathrm{d}k}=0$，得 $k=\dfrac{ub^2}{a^2v}$，此时 $t=\dfrac{u^2b^2+a^2v^2}{a^2b^2}$ 最大。  
由于 $2u^2b^2+2v^2a^2<a^2b^2$ ，所以有 $\dfrac{u^2b^2+a^2v^2}{a^2b^2}<\dfrac{1}{2}$。  

![200](Pasteda.png)

发现 $x-x^2$ 在 $[0,\frac{1}{2})$ 单增，于是 $t$ 最大时有 $S$ 最大为 $\dfrac{\sqrt{(u^2b^2+a^2v^2)(a^2b^2-u^2b^2-a^2v^2)}}{ab}$。  
<div STYLE="page-break-after: always;"></div>

# <center>反抗 （power）</center>

终于有第一道纯原创题啦！
前置知识：卷积具有结合律，交换律，然后通过**惊人的注意力**，可以注意到，$\Xi=\mu*\mu$，然后本题最难的部分就结束了。
本题又考查了大家反向推式子的能力（我习惯把它叫做：**莫比乌斯正演**）。
那就开始推式子：（长式警告）
$$
\begin{aligned}
\text{ans}&=\sum_{x=1}^n\sigma(x)\sum_{i=1}^{\lfloor\frac{n}{x}\rfloor}\sum_{j=1}^{\lfloor\frac{n}{x}\rfloor}\Xi({\gcd(i,j)})\\
&=\sum_{i=1}^{n}\sum_{j=1}^{n}\sum_{x|\gcd(i,j)}\sigma(x)\Xi(\frac{\gcd(i,j)}{x})\\
&=\sum_{i=1}^{n}\sum_{j=1}^{n}(\sigma*\Xi)(\gcd(i,j))\\
&=\sum_{i=1}^{n}\sum_{j=1}^{n}(\sigma*\mu*\mu)(\gcd(i,j))\\
&=\sum_{i=1}^{n}\sum_{j=1}^{n}(\operatorname{id}*1*\mu*\mu)(\gcd(i,j))\\
&=\sum_{i=1}^{n}\sum_{j=1}^{n}(\operatorname{id}*\mu*1*\mu)(\gcd(i,j))\\
&=\sum_{i=1}^{n}\sum_{j=1}^{n}(\varphi*\varepsilon)(\gcd(i,j))\\
&=\sum_{i=1}^{n}\sum_{j=1}^{n}\varphi(\gcd(i,j))\\
&=\sum_{d=1}^n\sum_{i=1}^{n}\sum_{j=1}^{n}[\gcd(i,j)=d]\varphi(d)\\
&=\sum_{d=1}^n\varphi(d)\sum_{i=1}^{\lfloor\frac{n}{d}\rfloor}\sum_{j=1}^{\lfloor\frac{n}{d}\rfloor}[\gcd(i,j)=1]\\
&=\sum_{d=1}^n\varphi(d)\sum_{i=1}^{\lfloor\frac{n}{d}\rfloor}\sum_{j=1}^{\lfloor\frac{n}{d}\rfloor}\sum_{x|i,x|j}\mu(x)\\
&=\sum_{d=1}^n\varphi(d)\sum_{x=1}^n\sum_{i=1}^{\lfloor\frac{n}{dx}\rfloor}\sum_{j=1}^{\lfloor\frac{n}{dx}\rfloor}\mu(x)\\
&=\sum_{d=1}^n\varphi(d)\sum_{x=1}^n\mu(x)\sum_{i=1}^{\lfloor\frac{n}{dx}\rfloor}\sum_{j=1}^{\lfloor\frac{n}{dx}\rfloor}1\\
&=\sum_{d=1}^n\varphi(d)\sum_{x=1}^n\mu(x)\lfloor\frac{n}{dx}\rfloor^2\\
&=\sum_{T=1}^n\sum_{d|T}\varphi(d)\mu(\frac{T}{d})\lfloor\frac{n}{T}\rfloor^2\\
&=\sum_{T=1}^n\lfloor\frac{n}{T}\rfloor^2\sum_{d|T}\varphi(d)\mu(\frac{T}{d})\\
&=\sum_{T=1}^n\lfloor\frac{n}{T}\rfloor^2(\varphi*\mu)(T)\\
\end{aligned}
$$
$\varphi*\mu$ 是积性函数，可以通过埃氏筛 $O(n\log n)$ 预处理，然后询问部分数论分块解决，总复杂度 $O(n\log n+T\sqrt{n})$。

代码：
```cpp
#include <bits/stdc++.h>
#define sor(i, l, r) for (int i = l; i <= r; i++)
#define int long long
using namespace std;
namespace Revitalize
{
    const int N = 1e6 + 100;
    int mu[N], nu[N], s[N], pri[N], psi[N], olhs[N], pm[N];
    bool vis[N];
    const int p = 998244853;
    inline void prework(int n)
    {
        mu[1] = 1;
        olhs[1] = 1;
        for (int i = 2; i <= n; i++)
        {
            if (!vis[i])
            {
                pri[++pri[0]] = i;
                mu[i] = -1;
                olhs[i] = i - 1;
            }
            for (int j = 1; j <= pri[0] && i * pri[j] <= n; j++)
            {
                vis[i * pri[j]] = 1;
                if (i % pri[j] == 0)
                {
                    mu[i * pri[j]] = 0;
                    olhs[i * pri[j]] = olhs[i] * pri[j];
                    break;
                }
                mu[i * pri[j]] = -mu[i];
                olhs[i * pri[j]] = olhs[i] * (pri[j] - 1);
            }
        }
        for (int i = 1; i <= n; i++)
        {
            for (int j = 1; j * i <= n; j++)
            {
                pm[i * j] += mu[i] * olhs[j];
                pm[i * j] %= p;
            }
        }
        for (int i = 1; i <= n; i++)
            pm[i] += pm[i - 1];
    }
    int n;
    inline void work()
    {
        int T;
        cin >> T;
        prework(1000005);
        while (T--)
        {
            cin >> n;
            int l = 1, r = 0, ans = 0;
            while (l <= n)
            {
                r = n / (n / l);
                ans += (pm[r] - pm[l - 1]) % p * (n / l) % p * (n / l) % p;
                ans %= p;
                l = r + 1;
            }
            cout << (ans + p) % p << "\n";
        }
    }
}
signed main()
{
    freopen("power.in","r",stdin);
    freopen("power.out","w",stdout);
    ios::sync_with_stdio(false);
    Revitalize::work();
}
```

<div STYLE="page-break-after: always;"></div>

# <center>忏悔（confess）</center>
这个题数学证明难度思维难度说实话**真的低**，签到题了属于是。   
## 5 pts
为了防止不会写矩阵快速幂的人爆零而去蛙跳导致双腿残废，特意留了 $5$ 分。  
## 15 pts
会写矩阵快速幂但不会线段树。  
## 50 pts
不会和角公式但会矩阵快速幂且会线段树。  
## 100 pts
### STEP 1
先看查询 $f$ 的操作。  
由题可知 $f$ 是广义斐波那契数列。  
那么我们有矩阵乘法公式：
$$
\begin{bmatrix}
f_{i-1}&f_{i}
\end{bmatrix}
\times
\begin{bmatrix}
0 & x\\
1 & y\\
\end{bmatrix}
=
\begin{bmatrix}
f_{i}&f_{i+1}
\end{bmatrix}
$$
所以：
$$
f_i=
\begin{bmatrix}
1 & 1\\
\end{bmatrix}
\times
\begin{bmatrix}
0 & x\\
1 & y\\
\end{bmatrix}^{i-1}
$$
搭配矩阵快速幂可以在 $O(n\log n)$ per query 完成。  
但是仅仅这样显然是会 TLE 的。  
所以我们考虑维护一个矩阵数组 $J$。
其中，
$$
J_i=
\begin{bmatrix}
f_{a_i} & f_{a_{i+1}}\\
\end{bmatrix}
$$
对于查询操作，就是查询：$\displaystyle\sum_{i=l}^r J_i$ 即 $\begin{bmatrix}\displaystyle\sum_{i=l}^r f_{a_i} & \displaystyle\sum_{i=l+1}^{r+1} f_{a_i}\end{bmatrix}$。  
取它的第一项，即为 $\displaystyle\sum_{i=l}^r f_{a_i}$。  
对于区间加 $c$ 操作，即为：  
对所有 $l\le i\le r$，给 $J_i$ 乘上一个 $\begin{bmatrix}0 & x\\1 & y\\\end{bmatrix}^{c}$ 。  
所以这就转换为区间乘区间求和问题，也就是“【模板】线段树 2”。  
用线段树维护 $J$，可以 $O(\log n)$ per query。    
### STEP 2
那么三角函数询问怎么做？    
假设向量 $\phi=(x_0,y_0),\psi=(x_1,y_1)$。  
角 $\alpha,\beta$ 满足 $\tan(\alpha)=\dfrac{y_0}{x_0},\tan(\beta)=\dfrac{y_1}{x_1}$。
根据向量点乘的两种运算公式，我们有：
$$
\begin{aligned}
\phi\cdot\psi=x_0x_1+y_0y_1&=\sqrt{x_0^2+y_0^2}\sqrt{x_1^2+y_1^2}\cos(\alpha-\beta)\\
\frac{x_0x_1}{\sqrt{x_0^2+y_0^2}\sqrt{x_1^2+y_1^2}}+\frac{y_0y_1}{\sqrt{x_0^2+y_0^2}\sqrt{x_1^2+y_1^2}}&=\cos(\alpha-\beta)\\
\cos(\alpha)\cos(\beta)+\sin(\alpha)\sin(\beta)&=\cos(\alpha-\beta)\\
\end{aligned}
$$
因为 $\cos(-\beta)=\cos(\beta),\sin(-\beta)=-\sin(\beta)$，所以：
$$
\cos(\alpha)\cos(\beta)-\sin(\alpha)\sin(\beta)=\cos(\alpha+\beta)
$$
又因为 $\sin(x)=\cos(x-\frac{\pi}{2})$，所以有：
$$
\sin(\alpha+\beta)=\cos(\alpha+\beta-\frac{\pi}{2})=\cos (\alpha)\cos (\beta-\frac{\pi}{2})-\sin (\alpha)\sin (\beta-\frac{\pi}{2})=\cos (\alpha)\sin(\beta)+\sin (\alpha)\cos (\beta)
$$
综合起来，我们有：
$$
\begin{gather}
\sin(\alpha+\beta)=\sin(\alpha)\cos(\beta)+cos(\alpha)sin(\beta)\\
\cos(\alpha+\beta)=\cos(\alpha)\cos(\beta)-\sin(\alpha)\sin(\beta)
\end{gather}
$$
这就是**和角公式**。
所以 $\displaystyle\sum_{i=l}^r \sin(a_i+c)=\displaystyle\sum_{i=l}^r \sin(a_i)\cos(c)+cos(a_i)sin(c)=\cos(c)\displaystyle\sum_{i=l}^r \sin(a_i)+sin(c)\displaystyle\sum_{i=l}^r cos(a_i)$。
同理 $\displaystyle\sum_{i=l}^r \cos(a_i+c)=\displaystyle\sum_{i=l}^r \cos(a_i)\cos(c)-sin(a_i)sin(c)=\cos(c)\displaystyle\sum_{i=l}^r \cos(a_i)-sin(c)\displaystyle\sum_{i=l}^r sin(a_i)$。
所以我们用线段树维护 $\sin$ 和，$\cos$ 和即可。  

```cpp
#include <bits/stdc++.h>
#define sor(i, l, r) for (int i = l; i <= r; i++)
#define int long long
using namespace std;
namespace fastio{
    /*快读，暂时无法展示*/
}
using fastio::gc;
using fastio::gstr;
using fastio::pc;
using fastio::print;
using fastio::pstr;
using fastio::read;
names  pace Revitalize
{
    const int N = 2e5 + 10, P = 1e9 + 7;
    int a[N], X, Y;
    class SegmentTree_with_Matrix
    {
    public:
        struct Matrix
        {
            int a[2][2];
            Matrix operator*(const Matrix &x) const
            {
                Matrix z;
                z.a[0][0] = (a[0][0] * x.a[0][0] + a[0][1] * x.a[1][0]) % P;
                z.a[1][0] = (a[1][0] * x.a[0][0] + a[1][1] * x.a[1][0]) % P;
                z.a[0][1] = (a[0][0] * x.a[0][1] + a[0][1] * x.a[1][1]) % P;
                z.a[1][1] = (a[1][0] * x.a[0][1] + a[1][1] * x.a[1][1]) % P;
                return z;
            }
            Matrix operator+(const Matrix &x) const
            {
                Matrix z;
                z.a[0][0] = (a[0][0] + x.a[0][0]) % P;
                z.a[0][1] = (a[0][1] + x.a[0][1]) % P;
                z.a[1][0] = (a[1][0] + x.a[1][0]) % P;
                z.a[1][1] = (a[1][1] + x.a[1][1]) % P;
                return z;
            }
            Matrix operator^(const int &x) const
            {
                Matrix I, res;
                I.a[0][0] = 1, I.a[0][1] = 0, I.a[1][0] = 0, I.a[1][1] = 1;
                res.a[0][0] = 0, res.a[0][1] = X, res.a[1][0] = 1, res.a[1][1] = Y;
                int tmp = x;
                while (tmp)
                {
                    if (tmp & 1)
                        I = I * res;
                    res = res * res;
                    tmp >>= 1;
                }
                return I;
            }
        } __;
        struct node
        {
            Matrix d;
            int tag;
            // node() { d.a[0][0] = d.a[0][1] = d.a[1][0] = d.a[1][1] = 0; tag=0;}
        } st[N << 2];
        inline void pushup(int p)
        {
            st[p].d = st[p << 1].d + st[p << 1 | 1].d;
        }
        inline void pushdown(int p)
        {
            if (!st[p].tag)
                return;
            st[p << 1].tag += st[p].tag;
            st[p << 1 | 1].tag += st[p].tag;
            st[p << 1].d = st[p << 1].d * (__ ^ st[p].tag);
            st[p << 1 | 1].d = st[p << 1 | 1].d * (__ ^ st[p].tag);
            st[p].tag = 0;
        }

    public:
        inline void calc()
        {
            __.a[0][0] = 0;
            __.a[0][1] = 1;
            __.a[1][0] = X;
            __.a[1][1] = Y;
        }
        void build(int l, int r, int p)
        {
            if (l == r)
            {
                int tmp2 = a[l] - 1;
                Matrix tmp = {{{1, 1}, {0, 0}}}, tmp1 = __ ^ tmp2;
                st[p].d = tmp * tmp1;
                return;
            } // 1  1 2 3 5
            int mid = (l + r) >> 1;
            build(l, mid, p << 1);
            build(mid + 1, r, p << 1 | 1);
            pushup(p);
        }
        void update(int l, int r, int s, int t, int p, int c, Matrix C)
        {
            if (l <= s && t <= r)
            {
                st[p].d = st[p].d * C;
                st[p].tag += c;
                return;
            }
            int mid = (s + t) >> 1;
            pushdown(p);
            if (l <= mid)
                update(l, r, s, mid, p << 1, c, C);
            if (r > mid)
                update(l, r, mid + 1, t, p << 1 | 1, c, C);
            pushup(p);
        }
        Matrix query(int l, int r, int s, int t, int p)
        {
            if (l <= s && t <= r)
            {
                return st[p].d;
            }
            pushdown(p);
            int mid = (s + t) >> 1;
            Matrix ans;
            ans.a[0][0] = ans.a[0][1] = ans.a[1][0] = ans.a[1][1] = 0;
            if (l <= mid)
                ans = ans + query(l, r, s, mid, p << 1);
            if (r > mid)
                ans = ans + query(l, r, mid + 1, t, p << 1 | 1);
            return ans;
        }
    } mat;
    inline double SIN(double arc) { return sin(arc); }
    inline double COS(double arc) { return cos(arc); }
    class SEG
    {
#define sin sn
#define cos cs
    private:
        struct node
        {
            double cos, sin;
            int tag;
        } st[N << 1];
        inline void record(int p, double x, double y)
        {
            double tmp1 = st[p].sin, tmp2 = st[p].cos;
            st[p].sin = 1.0 * tmp1 * y + 1.0 * tmp2 * x;
            st[p].cos = 1.0 * tmp2 * y - 1.0 * tmp1 * x;
        }
        inline void pushup(int p)
        {
            st[p].sin = st[p << 1].sin + st[p << 1 | 1].sin;
            st[p].cos = st[p << 1].cos + st[p << 1 | 1].cos;
        }
        inline void pushdown(int p)
        {
            if (st[p].tag)
            {
                st[p << 1].tag += st[p].tag;
                st[p << 1 | 1].tag += st[p].tag;
                record(p << 1, SIN((double)(st[p].tag)), COS((double)(st[p].tag)));
                record(p << 1 | 1, SIN((double)(st[p].tag)), COS((double)(st[p].tag)));
                st[p].tag = 0;
            }
        }

    public:
        void build(int l, int r, int p)
        {
            if (l == r)
            {
                st[p].sin = SIN((double)(a[l]));
                st[p].cos = COS((double)(a[l]));
                return;
            }
            int mid = (l + r) >> 1;
            build(l, mid, p << 1);
            build(mid + 1, r, p << 1 | 1);
            pushup(p);
        }
        double querysin(int l, int r, int s, int t, int p)
        {
            if (l <= s && t <= r)
                return st[p].sin;
            int mid = (s + t) >> 1;
            double res = 0;
            pushdown(p);
            if (l <= mid)
                res += querysin(l, r, s, mid, p << 1);
            if (r > mid)
                res += querysin(l, r, mid + 1, t, p << 1 | 1);
            return res;
        }
        double querycos(int l, int r, int s, int t, int p)
        {
            if (l <= s && t <= r)
                return st[p].cos;
            int mid = (s + t) >> 1;
            double res = 0;
            pushdown(p);
            if (l <= mid)
                res += querycos(l, r, s, mid, p << 1);
            if (r > mid)
                res += querycos(l, r, mid + 1, t, p << 1 | 1);
            return res;
        }
        void update(int l, int r, int s, int t, int p, int c)
        {
            if (l <= s && t <= r)
            {
                st[p].tag += c;
                record(p, SIN((double)(c)), COS((double)(c)));
                return;
            }
            int mid = (s + t) >> 1;
            pushdown(p);
            if (l <= mid)
                update(l, r, s, mid, p << 1, c);
            if (r > mid)
                update(l, r, mid + 1, t, p << 1 | 1, c);
            pushup(p);
        }
#undef sin
#undef cos
    } tree;
    int n, m;
    inline void work()
    {
        mat.calc();
        read(n), read(m), read(X), read(Y);
        for (int i = 1; i <= n; i++)
        {
            read(a[i]);
        }
        mat.build(1, n, 1);
        tree.build(1, n, 1);
        while (m--)
        {
            int op, l, r;
            read(op), read(l), read(r);
            if (l > r)
            {   
                swap(l, r);
            }
            if (op == 1)
            {
                int c;
                read(c);
                mat.update(l, r, 1, n, 1, c, mat.__ ^ c);
                tree.update(l,r,1,n,1,c);
            }
            if (op == 2)
            {
                cout << mat.query(l, r, 1, n, 1).a[0][0] << "\n";
            }
            if (op == 3)
            {
                cout << 0.3562 << "\n";
            }
            if (op == 4)
            {
                cout <<fixed<< setprecision(4) << tree.querysin(l, r, 1, n, 1) << "\n";
            }
            if (op == 5)
            {
                cout <<fixed<< setprecision(4) << tree.querycos(l, r, 1, n, 1) << "\n";
            }
        }
    }
}
signed main()
{
    ios::sync_with_stdio(false);
    cin.tie(0), cout.tie(0);
    return Revitalize::work(), 0;
}
```

<div STYLE="page-break-after: always;"></div>

# <center>灵魂（soul）</center>
题面这么短，你是不是以为这题很简单？
题解这么短，你是不是以为这题很简单？
代码这么短，你是不是以为这题很简单？
实际上这题原题是黑。
首先，考虑二分答案，每次二分判断离原点小于等于某个距离的直线的个数与 $k$ 的大小关系。  
那么当我们二分出了一个 $r$ 时，可以做一个以原点为圆心，$r$ 为半径的圆，然后计算出有多少个直线与圆有交点（包括相交和相切），这个数量就是离原点小于等于 $r$ 的直线的个数。  
我们有一个定理：  
圆 $O$ 外有两个点 $A,B$，直线 $AB$ 与圆有交点当且仅当弧 $A_1A_2,B_1B_2$ 相交但不包含，其中直线 $AA_1,AA_2,BB_1,BB_2$ 与圆 $O$ 相切。  
所以我们对于每个点都做两条圆的切线，然后形成 $n$ 个弧，计算这 $n$ 个弧中相交但不包含的弧有多少对。  
看到环自然想到破环为链，但是破环的时候可能会有弧被断掉，怎么办？  
我们注意到，假设有两个弧 $AB,CD$ 相交但不包含，那么弧 $BA,CD$ 也必然相交但不包含。  
所以对于那些被断掉的弧，就把它变成圆上与它相对的那个补弧即可。  
问题就转变为，直线上有一些线段，问有多少对线段相交但不包含。  
那么在插入某条线段时 $[l,r]$ 时，你就需要找到所有满足左端点在 $[0,l)$ 内而右端点在 $[l,r]$ 内或左端点在 $[l,r]$ 内而右端点在 $(r,len]$ 内的线段的数量，$len$ 就是右端点最大的线段的右端点。  
这显然主席树可以秒掉。  
但是主席树常数大，你会发现这其实就是二维数点，那其实我们不需要在线，只需要分别按照左端点、右端点排序然后树状数组处理即可。  
在处理之前你需要离散化。  
复杂度 $O(n\log r\log V)$，其中 $r$ 是二分上限，$V$ 是离散化后的值域。
恭喜你切了一道连 Jiangly 都没有赛时通过的 Div1 F 题！
```cpp
#include<bits/stdc++.h>
#define For(i,a,b) for(register int i=(a);i<=(b);++i)
#define Rep(i,a,b) for(register int i=(a);i>=(b);--i)
//#define double long double
using namespace std;
inline int read()
{
    char c=getchar();int x=0;bool f=0;
    for(;!isdigit(c);c=getchar())f^=!(c^45);
    for(;isdigit(c);c=getchar())x=(x<<1)+(x<<3)+(c^48);
    if(f)x=-x;return x;
}
#define maxn 200005
const double pi=3.14159265358979323846,eps=1e-8;
int n,m,l[maxn],r[maxn];
double a[maxn],b[maxn],t[maxn],tl[maxn],tr[maxn];
bool vis[maxn];
int c[maxn],p[maxn],k;
bool cmp(int x,int y){return l[x]<l[y];}
inline void add(int x,int y){
	for(;x<=m;x+=x&-x)c[x]+=y;
}
inline int ask(int x){
	int res=0;
	for(;x;x^=x&-x)res+=c[x];
	return res;
}
inline int ask(int l,int r){return ask(r)-ask(l-1);}

long long check(double rd)
{
	long long res=0,in=0; m=0;k=0;
	For(i,1,n)vis[i]=0; 
	For(i,1,n){
		double x=a[i],y=b[i],dis=sqrt(x*x+y*y);
		if(dis<rd+eps){in++;vis[i]=1;continue;}
		double ang=atan2(y,x),dt=acos(rd/dis);
		tl[i]=ang-dt,tr[i]=ang+dt;
		if(tl[i]<-pi)tl[i]+=pi*2;
		if(tr[i]>pi)tr[i]-=pi*2;
		if(tl[i]>tr[i])swap(tl[i],tr[i]);
		t[++m]=tl[i],t[++m]=tr[i];
	//	printf("%d %.4Lf %.4Lf\n",i,tl[i],tr[i]); 
	}
	sort(t+1,t+m+1);
	For(i,1,n){
		if(vis[i])continue;
		p[++k]=i;
		l[i]=lower_bound(t+1,t+m+1,tl[i])-t;
		r[i]=lower_bound(t+1,t+m+1,tr[i])-t;
	}
	memset(c,0,sizeof c);
	sort(p+1,p+k+1,cmp);
    res=1ll*n*(n-1)/2;
	For(i,1,k){
		int u=p[i];
		res-=ask(l[u],r[u]);
		add(r[u],1);
	}
	//printf("chk %.8Lf %lld\n",rd,res);
	return res;
}

int main()
{
//	freopen("my.out","w",stdout);
	n=read();long long kk;cin>>kk;
	For(i,1,n)cin>>a[i]>>b[i];
	double l=0,r=20000;
	while(r-l>eps){
		double mid=(l+r)/2;
		if(check(mid)>=kk)r=mid;
		else l=mid;
	}
	printf("%.8lf\n",l);
    return 0;
}
```
<div STYLE="page-break-after: always;"></div>

# <center>封建（janefly）</center>
作为本场比赛最激动人心的压轴题，是道好题。  
## 30 pts
首先，范德蒙德行列式你应该已经会证了：

$$
\begin{vmatrix}
1&1&1&\dots&1\\
a_1&a_2&a_3&\dots&a_n\\
a_1^2&a_2^2&a_3^2&\dots&a_n^2\\
\vdots&\vdots&\vdots&&\vdots\\
a_1^{n-1}&a_2^{n-1}&a_3^{n-1}&\dots&a_n^{n-1}
\end{vmatrix}=\prod_{1\le j<i \le n}(a_i-a_j)
$$

这道题把范德蒙德行列式的最后一行从 $n-1$ 次幂变成了 $n$ 次幂。
即算出：
$$
\begin{vmatrix}
1&1&1&\dots&1\\
a_1&a_2&a_3&\dots&a_n\\
a_1^2&a_2^2&a_3^2&\dots&a_n^2\\
\vdots&\vdots&\vdots&&\vdots\\
a_1^{n-2}&a_2^{n-2}&a_3^{n-2}&\dots&a_n^{n-2}\\
a_1^{n}&a_2^{n}&a_3^{n}&\dots&a_n^{n}
\end{vmatrix}
$$
处理方法是，构造一个 $(n+1)$ 阶行列式：
$$
\begin{aligned}
&\begin{vmatrix}
1&1&1&\dots&1&\color{cyan}1\\
a_1&a_2&a_3&\dots&a_n&\color{cyan}\lambda\\
a_1^2&a_2^2&a_3^2&\dots&a_n^2&\color{cyan}\lambda^2\\
\vdots&\vdots&\vdots&&\vdots&\color{cyan}\vdots\\
\color{cyan}a_1^{n-1}& \color{cyan}a_2^{n-1}&\color{cyan}a_3^{n-1}&\dots&\color{cyan}a_n^{n-1}&\color{cyan}\lambda^{n-1}\\
a_1^{n}& a_2^{n}&a_3^{n}&\dots&a_n^{n}&\color{cyan}\lambda^n
\end{vmatrix}\\
=&\prod_{i=1}^n(\lambda-a_i)\times\prod_{1\le i<j\le n}(a_j-a_i)\\
=&(\sum_{i=0}^n\xi_i\lambda^i)\times\prod_{1\le i<j\le n}(a_j-a_i)\\
=&\sum_{i=0}^n(\lambda^i\xi_i\prod_{1\le i<j\le n}(a_j-a_i))\\
=&\sum_{i=0}^{n}\lambda^iA_{i,n}
\end{aligned}
$$
所以我们说明了 $A_{i,n}=\xi_i\displaystyle\prod_{1\le i<j\le n}(a_j-a_i))$，其中 $\xi_i$ 是多项式 $\displaystyle\prod_{i=1}^n(\lambda-a_i)$ 中 $\lambda^i$ 的系数。 
我们发现 $-A_{n-1,n}$ 就是我们要求的原式。  
所以 $-A_{n-1,n}=-\xi_{n-1}\displaystyle\prod_{1\le i<j\le n}(a_j-a_i))=\left(\displaystyle\sum_{i=1}^na_i\right)\times\left(\displaystyle\prod_{1\le i<j\le n}(a_j-a_i))\right)$。  
这就是原式化简后的值。   
那就是范德蒙德行列式再乘上所有 $a_i$ 的和。  
所以只需要考虑范德蒙德行列式如何快速计算。
## 100 pts
### NTT
FFT 把单位根变成原根（对于 $998244353$ 来说，原根是 $3$）就是 NTT。
### 多项式求逆
多项式的同余与数论的同余同理。  
我们有多项式 $f(x)$，现在我们想求 $g(x)$，满足 $f(x)g(x)\equiv 1 \pmod {x^n}$
首先我们得求出 $h(x)$ ，满足 $f(x)h(x)\equiv 1\pmod{x^{\lceil\frac{n}{2}\rceil}}$  
很明显因为 $f(x)g(x)\equiv 1 \pmod {x^n}$，所以 $\forall m\le n,f(x)g(x)\equiv 1 \pmod {x^m}$ 。  
所以 
$$
f(x)g(x)\equiv 1\pmod{x^{\lceil\frac{n}{2}\rceil}}
$$
做差得到 
$$
f(x)(g(x)-h(x))\equiv 0\pmod{x^{\lceil\frac{n}{2}\rceil}}
$$
两边同除去一个 $f(x)$ 得 
$$
g(x)-h(x)\equiv 0\pmod{x^{\lceil\frac{n}{2}\rceil}}
$$
两边平方得 
$$
g(x)^2-2g(x)h(x)+h(x)^2\equiv 0\pmod{x^{n}}
$$
移项得 
$$
g(x)^2\equiv 2g(x)h(x)-h(x)^2\pmod{x^{n}}
$$
合并同类项 
$$
g(x)^2\equiv h(x)(2g(x)-h(x))\pmod{x^{n}}
$$
两边同除一个 $g(x)$ （模意义下相当于同乘一个 $f(x)$）
$$
g(x)\equiv h(x)(2-h(x)f(x))\pmod{x^{n}}
$$
$h(x)$ 就递归算下去就好了，然后 $h(x)f(x)$ 可以用 NTT $O(n\log n)$ 解决。  
总复杂度是 $O(n\log n)$，这个是用主定理算的，可以在 OI-wiki 上学一下。  

（题外话，怎么感觉跟做初中题一样）   
（恭喜，你现在就可以切一道紫题了）
### 多项式取模
求出 $f(x)$ 除以 $g(x)$ 的商 $Q(x)$ 和余式 $r(x)$。  
设 $f$ 的次数为 $n$，$g$ 的次数为 $m$，则自然有 $Q$ 的次数为 $n-m$，$r$ 的次数不超过 $m-1$。  
那么我们可以认定 $r$ 的次数就是 $m-1$，高次项没有就使系数为 $0$ 即可。
定义：
$$
\tilde{F}(x)=x^{c}F(\frac{1}{x})
$$

此处 $c$ 就是 $F$ 的次数。
手玩一下，发现其实就是 $F$ 的系数翻转的结果。
那么根据式子：$f(x)=Q(x)g(x)+r(x)$，我们替换 $x$ 为 $\frac 1 x$ 得：
$$
f(\frac 1 x)=g(\frac 1 x)Q(\frac 1 x)+r(\frac 1 x)
$$
等式两边同乘一个 $x^n$ 得：  
$$
\begin{aligned}
x^nf(\frac 1 x)&=x^ng(\frac 1 x)Q(\frac 1 x)+x^nr(\frac 1 x)\\
&=x^mg(\frac 1 x)x^{n-m}Q(\frac 1 x)+x^{n-m+1}x^{m-1}r(\frac 1 x)\\
&=\tilde{g}(x)\tilde{Q}(x)+x^{n-m+1}\tilde{r}(x)\\
&=\tilde{f}(x)
\end{aligned}
$$
也就是
$$
\tilde{f}(x)=\tilde{g}(x)\tilde{Q}(x)+x^{n-m+1}\tilde{r}(x)
$$
我们发现放到模 $x^{n-m+1}$ 意义下可以把 $\tilde{r}(x)$ 项干掉。
$$
\tilde{f}(x)\equiv\tilde{g}(x)\tilde{Q}(x)\pmod{x^{n-m+1}}
$$  
那 $\tilde{Q}(x)$ 就是 $\tilde{f}(x)\tilde{g}^{-1}(x)$。  
做多项式求逆即可得出 $\tilde{Q}(x)$，翻转系数得 $Q(x)$，回代到原式得 $r(x)$。  
复杂度依然是 $O(n\log n)$。
### 多项式多点求值  
给你 $n$ 次多项式 $f(x)$ 和 $n$ 个数 $a_1,a_2,\cdots,a_n$。  
让你求出 $f(a_1),f(a_2),\cdots,f(a_n)$ 的值。  
很显然 $O(n^2)$ 可以暴力完成。  
优化策略：
将这 $n$ 个数分成两部分：
$$
\begin{aligned}
&A_0=\set{a_1,a_2,\cdots,a_{\lfloor\frac{n}{2}\rfloor}}\\
&A_1=\set{a_{\lfloor\frac{n}{2}\rfloor+1},\cdots,a_{n-1},a_n}\\
\end{aligned}
$$
构造辅助多项式 $t_0(x)=\displaystyle\prod_{a_i\in A_0}(x-a_i),t_1(x)=\displaystyle\prod_{a_i\in A_1}(x-a_i)$。  
很显然，当 $x\in A_0$ 时，$t_0(x)=0$。  
对于 $f(x),t_0(x)$ 做一个带余除法，得到 $f(x)=t_0(x)Q_0(x)+r_0(x)$。  
当 $t_0(x)\neq0$ 时就是正常带余除法，$t_0(x)=0$ 时则有 $f(x)=r_0(x)$。  

同理可得当 $x\in A_1$ 时 $f(x)=r_1(x)$。
所以我们将这个问题分治成了两个子问题，即对于 $A_0,A_1$ 中的所有点，分别求出对于 $r_0(x),r_1(x)$ 的值。
也就是说，假设对于次数与点的个数为 $n$ 时，设 $T(n)$ 为多项式多点求值的复杂度，则 
$$
T(n)=2T(\frac n 2)+O(n\log n)
$$
在分治的过程中，每次计算的规模减半并执行两次，每次向下递归的复杂度为 $O(n\log n)$ ——这是多项式取模所带来的复杂度。  
根据主定理，$T(n)=O(n\log^2n)$。  
中间我们似乎遗漏了一步：那些辅助多项式 $t$ 怎么求？  
我们可以使用线段树的思想，发现每个 $t$ 都是形如 $\displaystyle\prod_{a_i\in S}(x-a_i)$ 的，而根据分治时的原理，每个 $t$ 所对应的 $S$ 集合都是线段树上的一个节点的管辖范围。  
那么在线段树上从下往上合并即可，每次合并时乘法就用 NTT。  
复杂度还是 $O(n\log^2n)$。  
#### 优化
Elegia 有一种矩阵转置的做法，依然是 $O(n\log^2n)$，但是不需要多项式取模，且常数更小，可以参考相关博客。  
~~**据说** Zhoukangyang 有一种未知 $O(n\dfrac{\log n}{\log\log n})$ 解法，不知道是真的假的，但是**据说**多项式多点求值的复杂度最好也只能是 $O(n\log^2 n)$。~~
### 线段树
#### 朴素做法
知道了前面的前置知识后，我们就可以解决原问题了。  
首先建立一棵线段树，管辖范围为 $[l,r]$ 的节点存储多项式：
$$
\prod_{i=l}^r(x-a_i)
$$
建树的方法与之前算辅助多项式 $t$ 的方法是一样的。  
现在回过来看原式，我们要求的是
$$
\begin{aligned}
&\prod_{1\le i<j\le n}(a_j-a_i)\\
=&\prod_{j=1}^n\prod_{i=1}^{j-1}(a_j-a_i)\\
\end{aligned}
$$
于是对于每一个 $a_j$，我们让它为 $x$，它的贡献就是 $\displaystyle\prod_{i=1}^{j-1}(x-a_i)$。  
那么就把 $[1,j-1]$ 在线段树上拆出 $O(\log n)$ 个节点，然后把 $x$ 带进这些节点存的多项式里去，然后将这些值乘起来即可。    
但是这个方法的复杂度依然是 $O(n^2)$。
#### 优化
所以我们转而去考虑线段树上的每一个节点会有多少个 $a_j$ 作为 $x$ 时被用到。  
于是我们就每次拆 $[1,j-1]$ 的时候不直接带入多项式求值，而是在每个结点处维护一个 vector，每次拆到这个节点就往它的 vector 里面插入一个 $a_j$，最终对于每一个节点，对它的 vector 中的数进行多项式多点求值，这样复杂度就是 $O(n\log^3n)$，不一定通过本题。  
#### 优化 2
线段树变成树状数组，可以优化常数。   
但是复杂度不变。  
#### 优化 3
说实话我也不知道这个方法和树状数组比哪个更快：  
直接对原序列分治，中间的交叉部分多项式多点求值维护，仍旧是 $O(n\log^3n)$。

在 $8$ 秒时限以及 $O_5$ 优化下，足以通过本题。
```cpp
#include <bits/stdc++.h>
#define int long long
#define sor(i, l, r) for (int i = l; i <= r; i++)
using namespace std;
/*快读*/

namespace POLY
{
    const long long N = 2e6 + 5, P = 998244353, Y = 3, I = 332748118;
    inline long long Q(long long a, long long b)
    {
        long long res = 1;
        while (b)
        {
            if (b & 1)
                res = res * a % P;
            a = a * a % P, b >>= 1;
        }
        return res % P;
    }
    inline long long Inv(long long &__x)
    {
        long long tmp = P - 2;
        return Q(__x, tmp);
    }
    int __Binary_reverse[N], gp[N], igp[N];
    class poly
    {
    public:
        vector<long long> a;
        poly() {}
        poly(long long __n) { a.resize(__n); }
        poly operator-(const poly &__tmpa) const
        {

            long long __n = a.size(), __m = __tmpa.a.size();
            long long nm = max(__n, __m);
            poly ans(nm);
            for (register int i = 0; i < nm; ++i)
            {
                ans.a[i] = ((((i < __n) ? a[i] : 0) - ((i < __m) ? __tmpa.a[i] : 0)) + P) % P;
            }
            return ans;
        }

    private:
        inline void NTT(bool __)
        {
            long long len = a.size();
            for (register int i = 0; i < len; ++i)
            {
                if (i < __Binary_reverse[i])
                {
                    swap(a[i], a[__Binary_reverse[i]]);
                }
            }
            for (register int r = 1, base = (1 << 19) / (r << 1); r < len; r <<= 1, base >>= 1)
            {
                for (register int i = 0; i < len; i += (r << 1))
                {
                    long long YG = 1;
                    for (register int j = 0; j < r; j++)
                    {
                        long long __I = a[j | i], __II = (__ ? gp[base * j] : igp[base * j]) * a[j | r | i] % P;
                        a[j | i] = (__I + __II + P) % P;
                        a[j | r | i] = (__I - __II + P) % P;
                    }
                }
            }
            if (!__)
            {
                long long invtmp = Q(len, P - 2);
                for (register int i = 0; i < len; ++i)
                {
                    a[i] = a[i] * invtmp % P;
                }
            }
        }
        poly operator|(const poly &__tmpa) const
        {
            poly __tmpmulx = *this, __tmpmuly = __tmpa;
            int len = 1, L = 0, __n = a.size();
            while (len < __n << 1)
            {
                len <<= 1;
                L++;
            }
            for (register int i = 0; i < len; ++i)
                __Binary_reverse[i] = (__Binary_reverse[i >> 1] >> 1) | ((i & 1) << (L - 1));
            __tmpmulx.a.resize(len);
            __tmpmuly.a.resize(len);
            __tmpmulx.NTT(1);
            __tmpmuly.NTT(1);
            poly res(len);
            for (register int i = 0; i < len; ++i)
                res.a[i] = __tmpmuly.a[i] * ((2 - __tmpmulx.a[i] * __tmpmuly.a[i] % P + P) % P) % P;
            res.NTT(0);
            res.a.resize(max(a.size(), __tmpa.a.size()));
            return res;
        }

    public:
        poly operator*(const poly &__tmpa) const
        {
            poly __tmpmulx = *this, __tmpmuly = __tmpa;
            int __n = max(a.size(), __tmpa.a.size());
            long long len = 1, L = 0;
            while (len < a.size() + __tmpa.a.size())
            {
                len <<= 1;
                L++;
            }
            for (register long long i = 1; i < len; ++i)
                __Binary_reverse[i] = (__Binary_reverse[i >> 1] >> 1) | ((i & 1) << (L - 1));
            __tmpmulx.a.resize(len);
            __tmpmuly.a.resize(len);
            __tmpmulx.NTT(1);
            __tmpmuly.NTT(1);
            poly res(len);
            for (register long long i = 0; i < len; ++i)
                res.a[i] = (__tmpmulx.a[i] * __tmpmuly.a[i] % P + P) % P;
            res.NTT(0);
            res.a.resize(a.size() + __tmpa.a.size() - 1);
            return res;
        }
        poly operator&(const poly &__tmpa) const
        {
            poly __tmpmulx = *this, __tmpmuly = __tmpa;
            int __n = max(a.size(), __tmpa.a.size());
            int nnn = a.size(), mmm = __tmpa.a.size();
            long long len = 1, L = 0;
            while (len < nnn)
            {
                len <<= 1;
                L++;
            }
            for (register long long i = 1; i < len; ++i)
                __Binary_reverse[i] = (__Binary_reverse[i >> 1] >> 1) | ((i & 1) << (L - 1));
            __tmpmulx.a.resize(len);
            __tmpmuly.a.resize(len);
            __tmpmulx.NTT(1);
            __tmpmuly.NTT(1);
            poly res(len);
            for (register long long i = 0; i < len; ++i)
                res.a[i] = ((__tmpmulx.a[i] * __tmpmuly.a[i] % P) + P) % P;
            res.NTT(0);
            poly ans(nnn - mmm + 1);
            sor(__i, mmm - 1, nnn - 1) ans.a[__i - mmm + 1] = res.a[__i];
            return ans;
        }
        inline void Print(long long deg)
        {
            for (register int i = 0; i < deg; ++i)
                print((a[i] + P) % P), pc(' ');
            pc('\n');
        }

        poly operator~() const
        {
            poly __f = *this;
            long long __n = __f.a.size();
            poly ans(__n);
            long long dep = 1;
            ans.a[0] = Q(__f.a[0], P - 2);
            while (dep < __n)
            {
                ans = __f | ans;
                ans.a.resize(dep << 1);
                dep <<= 1;
            }
            ans.a.resize(__n);
            return ans;
        }
        poly operator/(poly g) const
        {
            poly f = *this;
            reverse(f.a.begin(), f.a.end());
            reverse(g.a.begin(), g.a.end());
            int n = f.a.size(), m = g.a.size();
            g.a.resize(n - m + 1);
            poly tmp = ~g;
            poly q = tmp * f;
            reverse(q.a.begin(), q.a.begin() + n - m + 1);
            return q;
        }
        poly operator%(poly g) const
        {
            poly f = *this;
            reverse(g.a.begin(), g.a.end());
            poly q = f / g;
            q.a.resize(f.a.size() - g.a.size() + 1);
            poly res = f - q * g;
            return res;
        }
    } F, G, t[N << 2];
    int ans[N];
    long long f[N], nn, pro[N];
    void build(int l, int r, int p)
    {
        if (l == r)
        {
            t[p].a.push_back(1ll);
            t[p].a.push_back(P - f[l]);
            return;
        }
        int mid = (l + r) >> 1;
        build(l, mid, p << 1);
        build(mid + 1, r, p << 1 | 1);
        t[p] = t[p << 1] * t[p << 1 | 1];
    }
    void multipoint_evel(int l, int r, int p, poly __f)
    {
        if (l == r)
        {
            ans[l] = __f.a[0];
            return;
        }
        poly r1 = __f & t[p << 1 | 1], r2 = __f & t[p << 1];
        int mid = (l + r) >> 1;
        multipoint_evel(l, mid, p << 1, r1);
        multipoint_evel(mid + 1, r, p << 1 | 1, r2);
    }
    int get_ans(int l, int r, int p)
    {
        if (l == r)
        {
            return 1;
        }
        int mid = (l + r) >> 1;
        int ansl = get_ans(l, mid, p << 1), ansr = get_ans(mid + 1, r, p << 1 | 1);
        int res = ansl * ansr % P;
        poly tmpl = t[p << 1], tmpr = t[p << 1 | 1];
        int con=tmpl.a[mid - l + 1],cong=tmpl.a[mid - l];
        if(mid-l+1>r-mid)
            tmpl.a.pop_back();
        poly F = tmpl * ~tmpr;
        F.a.resize(r - mid);
        multipoint_evel(mid + 1, r, p << 1 | 1, F);
        if(mid-l+1>r-mid){
            sor(i, mid + 1, r)
            {
                res *= ((f[i] * ans[i]%P + cong)  % P * f[i] % P + con)%P;
                res %= P;
            }
        }
        else{
            sor(i, mid + 1, r)
            {
                res *= ((f[i] * ans[i] % P + tmpl.a[mid-l+1]) % P + P) % P;
                res %= P;
            }
        }
        return res%P;
    }
    inline void work()
    {
        int tm = clock();
        int tmpg = Q(Y, (P - 1) / (1 << 19)), tmpig = Q(I, (P - 1) / (1 << 19));
        gp[0] = igp[0] = 1;
        sor(i, 1, (1 << 19))
        {
            gp[i] = tmpg * gp[i - 1] % P;
            igp[i] = tmpig * igp[i - 1] % P;
        }
        read(nn);
        int sum = 0;
        for (register int i = 1; i <= nn; ++i)
            read(f[i]), sum = (sum + f[i]) % P;
        build(1, nn, 1);
        print((get_ans(1, nn, 1)*sum)%P);
        pc('\n');
    }
}
signed main()
{
    freopen("janefly.in", "r", stdin);
    freopen("janefly.out", "w", stdout);
    ios::sync_with_stdio(false);
    cin.tie(0), cout.tie(0);
    return POLY::work(),0;
}
```

