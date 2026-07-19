本套题目均由 $\color{gray}Opshacom$ 一个人出题、出题解，首次出题，或许有些做的不好的地方，可以来洛谷找我讨论，但对于因为题面等因素（非学术问题）而来的，一律拉黑并打飞。  

T4 数据是大佬 $C\color{red}ollapsarr$ 提供的，感谢贡献！

| 题目编号 | T1         | T2       | T3             | T4       |
| ---- | ---------- | -------- | -------------- | -------- |
| 难度   | 灰          | 绿        | 蓝              | 紫        |
| 算法标签 | 高等代数，C++入门 | DP，博弈    | 数论，莫比乌斯反演，欧拉反演 | 线段树，位运算  |
| 对标比赛 | 未知         | CSP-J T4 | NOIP T2        | CSP-S T3 |
四道题考察的内容可能有所重复~~（可能）~~，对于拔高而言，是很有意义的。
# T1

因为代码难度与数学难度不对等，所以此题评灰。  
$$
\begin{vmatrix}
1&1&1&\dots&1\\
a_1&a_2&a_3&\dots&a_n\\
a_1^2&a_2^2&a_3^2&\dots&a_n^2\\
\vdots&\vdots&\vdots&&\vdots\\
a_1^{n-1}&a_2^{n-1}&a_3^{n-1}&\dots&a_n^{n-1}
\end{vmatrix}=\prod_{1\le j<i \le n}(a_i-a_j)
$$

考虑用数学归纳法证明上式。
不妨设：
$$
f(n)=
\begin{vmatrix}
1&1&1&\dots&1\\
a_1&a_2&a_3&\dots&a_n\\
a_1^2&a_2^2&a_3^2&\dots&a_n^2\\
\vdots&\vdots&\vdots&&\vdots\\
a_1^{n-1}&a_2^{n-1}&a_3^{n-1}&\dots&a_n^{n-1}
\end{vmatrix}
$$

打表发现 $f(2),f(3)$ 都满足。  
于是开始归纳。     
假设 $f(n-1)=\displaystyle\prod_{1\le j<i \le n-1}(a_i-a_j)$。  
然后开始推 $f(n)$。
先将第 $n-1$ 行的 $-a_n$ 倍加到第 $n$ 行上，得到：  
$$
\begin{vmatrix}
1&1&1&\dots&1\\
a_1&a_2&a_3&\dots&a_n\\
a_1^2&a_2^2&a_3^2&\dots&a_n^2\\
\vdots&\vdots&\vdots&&\vdots\\
a_1^{n-1}-a_na_1^{n-2}&a_2^{n-1}-a_na_2^{n-2}&a_3^{n-1}-a_na_3^{n-2}&\dots&0
\end{vmatrix}
$$

再将第 $n-2$ 行的 $-a_n$ 倍加到第 $n-1$ 行上，行列式我就不写了，大家可以自己推一遍。  
以此类推，最后将第 $1$ 行的 $-a_n$ 倍加到第 $2$ 行上，最后的行列式如下图所示：
$$
\begin{vmatrix}
1&1&1&\dots&1\\
a_1-a_n&a_2-a_n&a_3-a_n&\dots&0\\
a_1^2-a_1a_n&a_2^2-a_2a_n&a_3^2-a_3a_n&\dots&0\\
\vdots&\vdots&\vdots&&\vdots\\
a_1^{n-1}-a_na_1^{n-2}&a_2^{n-1}-a_na_2^{n-2}&a_3^{n-1}-a_na_3^{n-2}&\dots&0
\end{vmatrix}
$$

然后将行列式按第 $n$ 列展开，得到：  
$$
(-1)^{n+1}\times
\begin{vmatrix}
a_1-a_n&a_2-a_n&a_3-a_n&\dots&a_{n-1}-a_n\\
a_1^2-a_1a_n&a_2^2-a_2a_n&a_3^2-a_3a_n&\dots&a_{n-1}^2-a_{n-1}a_n\\
\vdots&\vdots&\vdots&&\vdots\\
a_1^{n-1}-a_na_1^{n-2}&a_2^{n-1}-a_na_2^{n-2}&a_3^{n-1}-a_na_3^{n-2}&\dots&a_{n-1}^{n-1}-a_na_{n-1}^{n-2}
\end{vmatrix}_{(n-1)\times(n-1)}
$$

右下角代表行列式的阶数是 $n-1$。  
然后先不管左边负一的幂，先化简右边行列式。  
提取每一列的公因数，得到：
$$
(a_1-a_n)(a_2-a_n)\cdots(a_{n-1}-a_n)\times
\begin{vmatrix}
1&1&1&\dots&1\\
a_1&a_2&a_3&\dots&a_n\\
a_1^2&a_2^2&a_3^2&\dots&a_n^2\\
\vdots&\vdots&\vdots&&\vdots\\
a_1^{n-2}&a_2^{n-2}&a_3^{n-2}&\dots&a_{n-1}^{n-2}
\end{vmatrix}
$$

我们发现左边乘积其实是 $(-1)^{n-1}\displaystyle\prod_{i=1}^{n-1}(a_n-a_i)$ 。  
右边那个行列式就是 $f(n-1)$ 。

所以最后总结一下，得到 $f(n)=(-1)^{n+1}\times(-1)^{n-1}\times\displaystyle\prod_{i=1}^{n-1}(a_n-a_i)\times\displaystyle\prod_{1\le j<i \le n-1}(a_i-a_j)=\prod_{1\le j<i \le n}(a_i-a_j)$。
那式子推出来了，写代码就很容易了。  
最后补充一下，这个行列式叫做**范德蒙德行列式**，有兴趣的可以上网查一下，还是挺有意思的。

```cpp
#include<bits/stdc++.h>
#define sor(i,l,r) for(int i=l;i<=r;i++)
#define int long long
using namespace std;
namespace Revitalize{
    const int N=1e4,M=1004535809;
    int n,a[N];
    inline void work(){
        cin>>n;
        sor(i,1,n) cin>>a[i];
        int ans=1;
        sor(i,2,n) sor(j,1,i-1) ans=(ans*(a[i]-a[j]))%M;
        cout<<ans;
    }
}
signed main(){
    ios::sync_with_stdio(false);
    cin.tie(0),cout.tie(0);
    return Revitalize::work(),0;
}
```

~~代码这么短，评红其实都可以。~~
# T2
一道博弈 DP 题。  
题目短，实现容易，应该是最简单的一道了吧。  
设 $dp_i$ 表示还剩 $i$ 个石子时，先手最多能取多少个。  
考虑当先手取了 $a_j$ 个石子后，另一个人变成此时的先手开始取，此时另一个人最多取 $dp_{i-a_j}$ 个石子，剩下的 $i-dp_{i-a_j}$ 个石子就是这个人的了。  
所以要让 $i-dp_{i-a_j}$ 最大化。  
那么转移式就可以推出来了。

$$dp_i=\max_{j=1}^n(i-dp_{i-a_j})$$

代码很好写。  
```cpp
#include<bits/stdc++.h>
#define sor(i,l,r) for(int i=l;i<=r;i++)
#define int long long
using namespace std;
namespace Revitalize{
    const int N=5e5;
    int n,k,a[N],dp[N];
    inline void work(){
        cin>>n>>k;sor(i,1,k) cin>>a[i];
        sor(i,1,n) sor(j,1,k) dp[i]=(i<a[j])?dp[i]:max(dp[i],i-dp[i-a[j]]);
        cout<<dp[n]; 
    }
}
signed main(){
    ios::sync_with_stdio(false);
    cin.tie(0),cout.tie(0);
    return Revitalize::work(),0;
}
```

这道题时一道 ATcoder 的原题，如果看不懂可以再看一看别人的题解。  
原题链接：[Stones](https://www.luogu.com.cn/problem/AT_abc270_d)。

# T3

~~永离，莫反（同“返”，返回）。~~


$30$ 分没有拿到的（~~考虑退役~~）自行反思。  
此题有常见的 $2$ 种做法。
**莫比乌斯反演**

$$
\begin{aligned}
\text{ans}&=\sum_{i=1}^n\sum_{j=1}^n\gcd(i,j)\\
&=\sum_{d=1}^n\sum_{i=1}^n\sum_{j=1}^nd[\gcd(i,j)=d]\\
\end{aligned}
$$

我们设 $I=di,J=dj$。  
我们可以将枚举 $i,j$ 转向枚举 $I,J$，因为只有 $i,j$ 都为 $d$ 的倍数时它才构成贡献，所以就只枚举 $d$ 的倍数即可。
$$
\begin{aligned}
&=\sum_{d=1}^nd\sum_{i=1}^n\sum_{j=1}^n[\gcd (i, j)=d]\\
&=\sum_{d=1}^nd\sum_{I=1}^{n}\sum_{J=1}^{n}[\gcd (I, J)=d]\\
&=\sum_{d=1}^nd\sum_{i=1}^{\lfloor\frac{n}{d}\rfloor}\sum_{j=1}^{\lfloor\frac{n}{d}\rfloor}[\gcd (id, jd)=d]\\
&=\sum_{d=1}^nd\sum_{i=1}^{\lfloor\frac{n}{d}\rfloor}\sum_{j=1}^{\lfloor\frac{n}{d}\rfloor}[\gcd (i, j)=1]\\
&=\sum_{d=1}^nd\sum_{i=1}^{\lfloor\frac{n}{d}\rfloor}\sum_{j=1}^{\lfloor\frac{n}{d}\rfloor}\varepsilon(\gcd (i, j))\\
&=\sum_{d=1}^nd\sum_{i=1}^{\lfloor\frac{n}{d}\rfloor}\sum_{j=1}^{\lfloor\frac{n}{d}\rfloor}(\mu*1)(\gcd (i, j))\\
&=\sum_{d=1}^nd\sum_{i=1}^{\lfloor\frac{n}{d}\rfloor}\sum_{j=1}^{\lfloor\frac{n}{d}\rfloor}\sum_{x|gcd(i,j)}\mu(x)\\
&=\sum_{d=1}^nd\sum_{i=1}^{\lfloor\frac{n}{d}\rfloor}\sum_{j=1}^{\lfloor\frac{n}{d}\rfloor}\sum_{x|i,x|j}\mu(x)\\
\end{aligned}
$$

下一步如果没学过的话比较难想。  
我们考虑在外层枚举 $x$。  
因为 $i,j$ 需要是 $x$ 的倍数，所以我们设 $u=u'x,v=v'x$，转而去枚举 $u',v'$,，因为 $u,v\le\lfloor\dfrac{n}{d}\rfloor$，所以 $u',v'\le\lfloor\dfrac{n}{dx}\rfloor$，推完发现内层 $\mu(x)$ 与 $x$ 无关，相当于公因式提到前面来。
$$
\begin{aligned}
&=\sum_{d=1}^nd\sum_{x=1}^{\lfloor\frac{n}{d}\rfloor}\sum_{u'=1}^{\lfloor\frac{n}{dx}\rfloor}\sum_{v'=1}^{\lfloor\frac{n}{dx}\rfloor}\mu(x)\\
&=\sum_{d=1}^nd\sum_{x=1}^{\lfloor\frac{n}{d}\rfloor}\mu(x)\sum_{u'=1}^{\lfloor\frac{n}{dx}\rfloor}\sum_{v'=1}^{\lfloor\frac{n}{dx}\rfloor}1\\
&=\sum_{d=1}^nd\sum_{x=1}^{\lfloor\frac{n}{d}\rfloor}\mu(x)(\lfloor\frac{n}{dx}\rfloor)^2\\
\end{aligned}
$$
设 $\Psi(\xi)=\displaystyle\sum_{x=1}^{\xi}\mu(x)(\lfloor\dfrac{\xi}{x}\rfloor)^2$。  
然后我们可以在 $O(n\sqrt{n})$ 的时间复杂度内预处理出所有的 $\Psi$。  
具体怎么预处理下文再说。  
然后原式就是：
$$
\begin{aligned}
&=\sum_{d=1}^nd\Psi(\lfloor\frac{n}{d}\rfloor)\\
\end{aligned}
$$
这个式子可以 $O(\sqrt{n})$ 求。  
于是这道题就可以在 $O(n\sqrt{n})$ 的复杂度内做了，可以拿 $40$ 分。  

最后说怎么 $O(\sqrt{n})$ 求 $\displaystyle\sum_{i=1}^nf(i)g(\lfloor\dfrac{n}{i}\rfloor)$。  
我们先打出一个 $n=8$ 关于 $\lfloor\dfrac{n}{i}\rfloor$ 的表。  

| $i$                         | $1$ | $2$ | $3$ | $4$ | $5$ | $6$ | $7$ | $8$ |
| --------------------------- | --- | --- | --- | --- | --- | --- | --- | --- |
| $\lfloor\frac{n}{i}\rfloor$ | $8$ | $4$ | $2$ | $2$ | $1$ | $1$ | $1$ | $1$ |
发现 $2$ 与 $1$ 有重复。  
当你 $n$ 取更大，表打更大的时候，会发现会有更多的数重复。  
考虑 $i$ 在那个范围时 $\lfloor\dfrac{n}{i}\rfloor$ 值一样。  
计算得如果 $\lfloor\dfrac{n}{i}\rfloor\neq\lfloor\dfrac{n}{i-1}\rfloor$, 则当 $j$ 取 $i\sim\lfloor\dfrac{n}{\lfloor\frac{n}{i}\rfloor}\rfloor$ 时，$\lfloor\dfrac{n}{j}\rfloor$ 的值相等。  
表打大一点，上式应该可以自己手推出来。  

所以，形式化的说，我们可以把 $[1,n]$ 切成若干个段，每个段内的 $\lfloor\dfrac{n}{i}\rfloor$ 值相等。  
如果某个段的左端点是 $l$，那右端点就是 $\lfloor\dfrac{n}{\lfloor\frac{n}{l}\rfloor}\rfloor$。  
通过一些简单的数学证明，我们可以发现段数不超过 $\lfloor2\sqrt{n}\rfloor$。  
因此就可以写出这样的代码。
```cpp
int l=1,r=0,ans=0;
while(l<=n){
	r=n/(n/l);
	ans+=(s[r]-s[l-1])*g[l];
}
return ans;
```
其中 $s$ 是 $f$ 的前缀和。  
这个方法就是著名的**整除分块**或**数论分块**。  

这道题正解做法：
**欧拉反演**  
$$
\begin{aligned}
\text{ans}&=\sum_{i=1}^n\sum_{j=1}^n\gcd(i,j)\\
&=\sum_{i=1}^n\sum_{j=1}^nid(\gcd(i,j))\\
&=\sum_{i=1}^n\sum_{j=1}^n(\varphi*1)(\gcd(i,j))\\
&=\sum_{i=1}^n\sum_{j=1}^n\sum_{d|\gcd(i,j)}\varphi(d)\\
&=\sum_{i=1}^n\sum_{j=1}^n\sum_{d|i,d|j}\varphi(d)\\
&=\sum_{d=1}^n\sum_{i=1}^{\lfloor\frac{n}{d}\rfloor}\sum_{j=1}^{\lfloor\frac{n}{d}\rfloor}\varphi(d)\\
&=\sum_{d=1}^n\varphi(d)\sum_{i=1}^{\lfloor\frac{n}{d}\rfloor}\sum_{j=1}^{\lfloor\frac{n}{d}\rfloor}1\\
&=\sum_{d=1}^n\varphi(d)(\lfloor\frac{n}{d}\rfloor)^2\\
\end{aligned}\\
$$  
然后直接暴力跑可以 $80$ 分，带个整除分块就是 $100$ 分。  
复杂度 $O(\sqrt{n})$。
代码：
```cpp
#include<bits/stdc++.h>
using namespace std;
int pri[100005],olhs[100005],nn,cnt;
bool vis[100005];
long long sum[100005];
void prime(int n){
	olhs[1]=1;
	for(int i=2;i<=n;i++){
		if(!vis[i]){
			olhs[i]=i-1;
			pri[++cnt]=i;
		}
		for(int j=1;i*pri[j]<=n&&j<=cnt;j++){
			int t=i*pri[j];
			vis[t]=1;
			if(i%pri[j]==0){
				olhs[t]=olhs[i]*pri[j];
				break;
			}
			else olhs[t]=olhs[i]*(pri[j]-1);
		}
	}
	for(int i=1;i<=n;i++){
	    sum[i]=sum[i-1]+olhs[i];
	}	
}
int main(){
	ios::sync_with_stdio(false);
	cin>>nn;
	prime(nn);
	long long an=0,l=1,r=0;
	while(l<=nn){
		r=n/(n/l);
	    an+=(sum[r]-sum[l-1])*(n/l)*(n/l);
	}
	cout<<an;
	return 0;
}
```

# T4

数据结构，但是像大模拟。  

（~~Collapsarr 造了 100 个测试点，调了一下午，出题人表示如果 std 有锅就当场退役，结果 std 与暴力小样例测试不一样，最后发现是 Collapsarr 暴力写挂了，然后开始造数据测，结果发现用 std 跑出来的数据 std 自己过不了，以为 UB 了，最后发现是 Collapsarr 生成 .in 数据的代码以及 `\n` 以及 Collapsarr 没读题面的锅，差点退役，不过似乎 std 中间也挂了一下，严格来说还是要退役。让 DeepSeek 做发现跑得比 std 快。~~ ）

不要看上面这段话，没用。

这道题是线段树的模板。  
反思为什么没有通过。  

首先，考虑这么一个问题。
给一个序列，**单点修改**，每次询问给一个 $l,r$，要在 $[l,r]$ 内找到一段连续的和最大的区间，输出这个和。

注意是单点修改，如果是区间加的话好像要用什么“KTT”，太高深了，[黑题](https://www.luogu.com.cn/problem/P5693)。  
此处一个线段树足矣。  
每个节点要维护 $4$ 个信息：  
$lans$，$rans$，$ans$，$d$。  
让我分别来解释一下 (这里用结构体版线段树来写）。  
$st_{p(lans)}$：$p$ 的管辖区间中经过左端点的最大子段和。  
$st_{p(rans)}$：$p$ 的管辖区间中经过右端点的最大子段和。  
$st_{p(ans)}$：$p$ 的管辖区间中最大子段和。  
$st_{p(d)}$：$p$ 的管辖区间中的区间和。 

然后，`build` 和 `update` 都很简单，和正常线段树没啥区别。  
主要 `pushup` 和 `query` 要重点说一下。  
**①pushup**  
区间和就直接把左儿子和右儿子的区间和加起来就行了。  
设节点 $p$ 的管辖区间为 $[s,t]$，$2p$ 的管辖区间是 $[s,mid]$，$2p+1$ 的管辖区间是 $[mid+1,t]$。  
那么 $st_{p(lans)}$, 即  
$$
\max\{\displaystyle\sum_{i=s}^{r}a_i | s\le r\le t\}
$$  
分为 $2$ 种情况：  
1. $r>mid$
2. $r\le mid$
   
对于第一种情况，
```cpp
st[p].lans = st[p<<1].d+st[p<<1|1].lans;
```  
对于第二种情况，
```cpp
st[p].lans = st[p<<1].lans;
```  
所以 
```cpp
st[p].lans = max(st[p<<1].d+st[p<<1|1].lans,st[p<<1].lans);
```  

同理，对于 $rans$，有 
```cpp
st[p].rans = max(st[p<<1|1].d+st[p<<1].rans,st[p<<1|1].rans);
```  

现在主要处理 $st_{p(ans)}$，即为：  
$$
\max\{\displaystyle\sum_{i=l}^{r}a_i | s\le l\le r\le t\}
$$     
其中 $p$ 的管辖区间为 $[s,t]$。  
分为 $3$ 种情况：  
1. $r\le mid$
2. $l>mid$
3. $l\le mid<r$

对于第一种情况，
```cpp
st[p].ans = st[p<<1].ans;
```  
对于第二种情况，
```cpp
st[p].ans = st[p<<1|1].ans;
```  
对于第三种情况，
```cpp
st[p].ans = st[p<<1].rans + st[p<<1|1].lans;
```

所以
```cpp
st[p].ans = max(max(resl.ans,resr.ans),resl.rans+resr.lans);
```  

这里重点说一下第三种情况，即和为最大子段和的区间横跨了 $p$ 的管辖区间的左半段和右半段。
![](https://cdn.luogu.com.cn/upload/image_hosting/twlptp54.png)  
这个区间（即 $[l,r]$ ）**在 $mid$ 前的部分**因为右端点是 $mid$ 且满足局部最优，所以它的区间和等于 $st_{2p(rans)}$。    
同理，这个区间**在 $mid$ 后的部分**的区间和等于 $st_{2p+1(lans)}$。    
所以在这种情况下 $[l,r]$ 的区间和为 $st_{2p(rans)}+st_{2p+1(lans)}$。  

最后贴一下 `pushup` 的代码：
```cpp
inline SegmentTree pushup(SegmentTree &res,SegmentTree &resl,SegmentTree &resr) {
    res.d=resl.d+resr.d;
    res.lans=max(resl.lans,resl.d+resr.lans);
    res.rans=max(resr.rans,resr.d+resl.rans);
    res.ans=max(max(resl.ans,resr.ans),resl.rans+resr.lans);
    return res;
}
```
稍微解释一下，`SegmentTree` 是结构体名。  
还有，此处 `pushup` 的参数不是简单的一个 $p$，也就是说在 `build` 和 `update` 中执行 `pushup` 的时候就不是简单地调用 `pushup(p)`。  
而是：
```cpp
st[p] = pushup(st[p],st[p<<1],st[p<<1|1])
```

为什么要如此，其实是为了 `query` 更方便。
**②query**
`query` 在此处返回的是一个 `SegmentTree` 类型的变量，每次询问输出：`query(l,r,1,n,1)_{ans}`。  
$l$，$r$，是询问区间的端点，`query(l,r,s,t,p)` 的概念是：

> $[l,r]\cap[s,t]$ 的最大子段和，其中 $[s,t]$ 为节点 $p$ 的管辖区间。  

当 $[s,t]\subseteq[l,r]$ 时，返回 $st_{p(ans)}$。  
设 $mid=\lfloor\dfrac{s+t}{2}\rfloor$，  
特别地，  
当 $[l,r]\subseteq[s,mid]$ 时，返回 
```cpp
query(l,r,s,mid,p<<1);
```  
当 $[l,r]\subseteq[mid+1,t]$ 时，返回 
```cpp
query(l,r,mid+1,t,p<<1|1);
```  
如果前面这几种情况都不满足，那么表明 $[l,r]$ 横跨了 $[s,t]$ 的左半段与右半段，于是将 $[l,r]$ 以 $mid$ 为分界点劈成两段，分别查询这两段的答案，即：  
```cpp 
query(l,r,s,mid,p<<1);
``` 
以及  
``` cpp 
query(l,r,mid+1,t,p<<1|1);
```
设它们俩分别为 $tmpl$，$tmpr$，最后用 `pushup` 把它们合并起来得到一个 $res$ 返回即可。  
即:  
```cpp
SegmentTree tmpl,tmpr,res;
tmpl=query(l,r,s,mid,p<<1);
tmpr=query(l,r,mid+1,t,p<<1|1);
return pushup(res,tmpl,tmpr);
```
这也就是我要把 `pushup` 写成三个参数形式的原因。

最后的最后，上代码。(AC code of SP1716)    

```cpp
#include<bits/stdc++.h>
#define int long long
using namespace std;
const int N=4000005;
struct SegmentTree{
	int d,lans,rans,ans;
    SegmentTree(){
        d=0;
        lans=rans=ans=-1e18;
    }
}st[N];
int a[N];
inline SegmentTree pushup(SegmentTree &res,SegmentTree &resl,SegmentTree &resr) {
    res.d=resl.d+resr.d;
    res.lans=max(resl.lans,resl.d+resr.lans);
    res.rans=max(resr.rans,resr.d+resl.rans);
    res.ans=max(max(resl.ans,resr.ans),resl.rans+resr.lans);
    return res;
}
SegmentTree query(int l,int r,int s,int t,int p){
    if(l<=s&&t<=r) return st[p];
    int mid=s+((t-s)>>1);
    if(r<=mid) return query(l,r,s,mid,p<<1);
    if(l>mid) return query(l,r,mid+1,t,p<<1|1);
    SegmentTree tmpl,tmpr,res;
    tmpl=query(l,r,s,mid,p<<1);
    tmpr=query(l,r,mid+1,t,p<<1|1);
    return pushup(res,tmpl,tmpr);
}
void add(int l,int c,int s,int t,int p){
    if(s==t){
        st[p].lans=st[p].rans=st[p].d=st[p].ans=c;
        return ;
    }
    int mid=s+((t-s)>>1);
    if(l<=mid) add(l,c,s,mid,p<<1);
    else add(l,c,mid+1,t,p<<1|1);
    st[p]=pushup(st[p],st[p<<1],st[p<<1|1]);
}
void build(int l,int r,int p){
    if(l==r){
        st[p].lans=st[p].rans=st[p].d=st[p].ans=a[l];
        return ;
    }
    int mid=l+((r-l)>>1);
    build(l,mid,p<<1);
    build(mid+1,r,p<<1|1);
    st[p]=pushup(st[p],st[p<<1],st[p<<1|1]);
}

int n,m;
signed main(){
	ios::sync_with_stdio(false);
    cin>>n;
    for(int i=1;i<=n;i++) cin>>a[i];
    build(1,n,1);
    cin>>m;
    for(int i=1;i<=m;i++){
        int op;
        cin>>op;
        if(!op){
            int x,y;
            cin>>x>>y;
            add(x,y,1,n,1);
        }
        else{
            int x,y;
            cin>>x>>y;
            cout<<query(x,y,1,n,1).ans<<endl;
        }
    }
	return 0;
} 
```
我们对线段树的理解通过此题又加深了一步，`query` 事实上也是需要“广义的 `pushup`”的。  

（是的，粘贴了我以前写的博客）  

回过来看这个题。  
我们考虑对于每个数二进制拆位成 $30$ 位，不足 $30$ 位的补上前导 $0$。  
#### 操作 1
区间或。    
考虑先把 $k$ 二进制拆位。  
对于是 $1$ 的位，维护该位的的线段树进行区间修改为 $1$ 的操作。  
对于是 $0$ 的位，不管。  
#### 操作 2
区间与。    
考虑先把 $k$ 二进制拆位。  
对于是 $0$ 的位，维护该位的的线段树进行区间修改为 $0$ 的操作。  
对于是 $1$ 的位，不管。 
#### 操作 3
区间异或。    
考虑先把 $k$ 二进制拆位。  
对于是 $1$ 的位，维护该位的的线段树进行区间 $1$ 变 $0$，$0$ 变 $1$ 的操作。  
对于是 $0$ 的位，不管。
#### 操作 4
区间和。      
对于维护第 $k$ 位的线段树，$ans\gets ans+2^k\times query$。  
$query$ 就是正常的区间和查询。  
#### 操作 5
化简式子，发现要求的是：  
从 $a_l$ 到 $a_r$，找到一个区间的最大的子区间，使得该子区间的所有数的第 $k$ 位都是 $1$。  
那不就是求出维护第 $k$ 位的线段树的区间最大子段和吗？
#### 操作 6
化简式子，发现要求的是：  
从 $a_l$ 到 $a_r$，找到一个区间的最大的子区间，使得该子区间的所有数的第 $k$ 位都是 $0$。  
那不就是求出维护第 $k$ 位的线段树的区间最大全零子段吗？  
这玩意的原理其实与最大子段和差不多吧。  

现在可能有人会提出疑问，不是说最大子段和不能区间修改吗？

谁说不能修改了？我说不能区间加，没说不能区间覆盖……  
你会发现直接用懒标记维护就可以。  
因为可以 $O(1)$ 修改线段树某个节点的值。  

首先区间全赋为一操作，那就是前缀最大和，后缀最大和，最大子段和以及区间和都改成区间长度，前缀全 0 子段最大长度，后缀全 0 子段最大长度，区间最长全 0 子段长度都设为 0 就好了，最后再打上 tag 就好。  
区间全赋 0 操作反过来就行。  
区间取反就是把维护的关于 1 的信息与关于 0 的信息 `swap` 一下，区间和更新一下，打上 tag，再反转 0 的 tag 与 1 的 tag 就好了。  
非常简单。  
接下来就是写代码了，一定要仔细。  
```cpp
#include<bits/stdc++.h>
#define int long long
using namespace std;
const int N=800005;int a[N];
struct SEG{
    struct SegmentTree{
        int d,lans,rans,ans,len,l0,r0,a0;
        bool tag0,tag1,tag;
    }st[N];
    inline void pushdown(int p){
        if(st[p].tag0){
            st[p<<1].tag0=st[p].tag0;
            st[p<<1|1].tag0=st[p].tag0;
            st[p<<1].d=0;
            st[p<<1|1].d=0;
            st[p<<1].lans=0;
            st[p<<1|1].lans=0;
            st[p<<1].rans=0;
            st[p<<1|1].rans=0;
            st[p<<1].ans=0;
            st[p<<1|1].ans=0;
            st[p<<1].l0=st[p<<1].len;
            st[p<<1|1].l0=st[p<<1|1].len;
            st[p<<1].r0=st[p<<1].len;
            st[p<<1|1].r0=st[p<<1|1].len;
            st[p<<1].a0=st[p<<1].len;
            st[p<<1|1].a0=st[p<<1|1].len;
            st[p].tag0=0;
            st[p<<1].tag1=0;
            st[p<<1|1].tag1=0;
            st[p<<1].tag=0;
            st[p<<1|1].tag=0;
            st[p].tag=0;
        }
        else if(st[p].tag1){
            st[p<<1].tag1=st[p].tag1;
            st[p<<1|1].tag1=st[p].tag1;
            st[p<<1].d=st[p<<1].len;
            st[p<<1|1].d=st[p<<1|1].len;
            st[p<<1].lans=st[p<<1].len;
            st[p<<1|1].lans=st[p<<1|1].len;
            st[p<<1].rans=st[p<<1].len;
            st[p<<1|1].rans=st[p<<1|1].len;
            st[p<<1].ans=st[p<<1].len;
            st[p<<1|1].ans=st[p<<1|1].len;
            st[p<<1].l0=0;
            st[p<<1|1].l0=0;
            st[p<<1].r0=0;
            st[p<<1|1].r0=0;
            st[p<<1].a0=0;
            st[p<<1|1].a0=0;
            st[p].tag1=0;
            st[p<<1].tag0=0;
            st[p<<1|1].tag0=0;
            st[p<<1].tag=0;
            st[p<<1|1].tag=0;
            st[p].tag=0;
        }
        else if(st[p].tag){
            st[p<<1].tag^=1;
            st[p<<1|1].tag^=1;
            swap(st[p<<1].lans,st[p<<1].l0);
            swap(st[p<<1].rans,st[p<<1].r0);
            swap(st[p<<1].ans,st[p<<1].a0);
            swap(st[p<<1].tag0,st[p<<1].tag1);
            st[p<<1].d=st[p<<1].len-st[p<<1].d;
            swap(st[p<<1|1].lans,st[p<<1|1].l0);
            swap(st[p<<1|1].rans,st[p<<1|1].r0);
            swap(st[p<<1|1].ans,st[p<<1|1].a0);
            swap(st[p<<1|1].tag0,st[p<<1|1].tag1);
            st[p<<1|1].d=st[p<<1|1].len-st[p<<1|1].d;
            st[p].tag^=1;
        }
    }
    inline SegmentTree pushup(int p) {
        st[p].d=st[p<<1].d+st[p<<1|1].d;
        st[p].len=st[p<<1].len+st[p<<1|1].len;
    //---------------------------------------------------------------------------------------------------------
        if(st[p<<1].d==st[p<<1].len) st[p].lans=max(st[p<<1].lans,st[p<<1].d+st[p<<1|1].lans);
        else st[p].lans=st[p<<1].lans;
       if(st[p<<1|1].d==st[p<<1|1].len)st[p].rans=max(st[p<<1|1].rans,st[p<<1|1].d+st[p<<1].rans);
        else st[p].rans=st[p<<1|1].rans;
        st[p].ans=max(max(st[p<<1].ans,st[p<<1|1].ans),st[p<<1].rans+st[p<<1|1].lans);
    //--------------------------------------------------------------------------------------------------------
        if(st[p<<1].d==0) st[p].l0=max(st[p<<1].l0,st[p<<1].len+st[p<<1|1].l0);
        else st[p].l0=st[p<<1].l0;
        if(st[p<<1|1].d==0) st[p].r0=max(st[p<<1|1].r0,st[p<<1|1].len+st[p<<1].r0);
        else st[p].r0=st[p<<1|1].r0;
        st[p].a0=max(max(st[p<<1].a0,st[p<<1|1].a0),st[p<<1].r0+st[p<<1|1].l0);
    //--------------------------------------------------------------------------------------------------------
        return st[p];
    }
    SegmentTree query(int l,int r,int s,int t,int p){
        if(l<=s&&t<=r) return st[p];
        int mid=s+((t-s)>>1);
        pushdown(p);
        if(r<=mid) return query(l,r,s,mid,p<<1);
        if(l>mid) return query(l,r,mid+1,t,p<<1|1);
        SegmentTree tmpl,tmpr,res;
        tmpl=query(l,r,s,mid,p<<1);
        tmpr=query(l,r,mid+1,t,p<<1|1);
        res.d=tmpl.d+tmpr.d;
        res.len=tmpl.len+tmpr.len;
        if(tmpl.d==tmpl.len) res.lans=max(tmpl.lans,tmpl.d+tmpr.lans);
        else res.lans=tmpl.lans;
        if(tmpr.d==tmpr.len) res.rans=max(tmpr.rans,tmpr.d+tmpl.rans);
        else res.rans=tmpr.rans;
        res.ans=max(max(tmpl.ans,tmpr.ans),tmpl.rans+tmpr.lans);
        if(tmpl.d==0) res.l0=max(tmpl.l0,tmpl.len+tmpr.l0);
        else res.l0=tmpl.l0;
        if(tmpr.d==0) res.r0=max(tmpr.r0,tmpr.len+tmpl.r0);
        else res.r0=tmpr.r0;
        res.a0=max(max(tmpl.a0,tmpr.a0),tmpl.r0+tmpr.l0);
        return res;
    }
    int querysum(int l,int r,int s,int t,int p){
        if(l<=s&&t<=r) return st[p].d;
        int mid=s+((t-s)>>1),sum=0;
        pushdown(p);
        if(l<=mid) sum+=querysum(l,r,s,mid,p<<1);
        if(r>mid) sum+=querysum(l,r,mid+1,t,p<<1|1);
        return sum;
    }
    void all0(int l,int r,int s,int t,int p){
        if(l<=s&&t<=r){
            st[p].lans=st[p].rans=st[p].d=st[p].ans=0;
            st[p].l0=st[p].r0=st[p].a0=st[p].len;
            if(st[p].tag) st[p].tag=0;
            if(st[p].tag1) st[p].tag1=0;
            st[p].tag0=1;
            return ;
        }
        int mid=s+((t-s)>>1);
        pushdown(p);
        if(l<=mid) all0(l,r,s,mid,p<<1);
        if(r>mid) all0(l,r,mid+1,t,p<<1|1);
        st[p]=pushup(p);
    }
    void all1(int l,int r,int s,int t,int p){
        if(l<=s&&t<=r){
            st[p].d=st[p].lans=st[p].rans=st[p].ans=st[p].len;
            st[p].l0=st[p].r0=st[p].a0=0;
            if(st[p].tag) st[p].tag=0;
            if(st[p].tag0) st[p].tag0=0;
            st[p].tag1=1;
            return ;
        }
        int mid=s+((t-s)>>1);
        pushdown(p);
        if(l<=mid) all1(l,r,s,mid,p<<1);
        if(r>mid) all1(l,r,mid+1,t,p<<1|1);
        st[p]=pushup(p);
    }
    void Xor(int l,int r,int s,int t,int p){
        if(l<=s&&t<=r){
            swap(st[p].lans,st[p].l0);
            swap(st[p].rans,st[p].r0);
            swap(st[p].ans,st[p].a0);
            swap(st[p].tag1,st[p].tag0);
            st[p].d=st[p].len-st[p].d;
            st[p].tag^=1;
            return ;
        }
        int mid=s+((t-s)>>1);
        pushdown(p);
        if(l<=mid) Xor(l,r,s,mid,p<<1);
        if(r>mid) Xor(l,r,mid+1,t,p<<1|1);
        st[p]=pushup(p);
    }
    void build(int l,int r,int p,int d){
        if(l==r){
            int tmp=(a[l]>>d)&1;
            if(tmp==1) st[p].lans=st[p].rans=st[p].d=st[p].ans=tmp;
            if(tmp==0){
                st[p].l0=st[p].r0=st[p].a0=1;
                st[p].d=0;
            }
            st[p].len=1;
            return ;
        }
        int mid=l+((r-l)>>1);
        build(l,mid,p<<1,d);
        build(mid+1,r,p<<1|1,d);
        st[p]=pushup(p);
    }
}digit[32];
int n,m;
signed main(){
    freopen("warlord.in","r",stdin);
    freopen("warlord.out","w",stdout);
    ios::sync_with_stdio(false);cin>>n>>m;
    for(int i=1;i<=n;i++) cin>>a[i];
    for(int i=0;i<=31;i++) digit[i].build(1,n,1,i);
    vector<bool> v;
    while(m--){
        v.clear();int op,l,r,k;cin>>op>>l>>r;
        if(op==1){
            cin>>k;while(k){v.push_back(k&1);k>>=1;}  
            while(v.size()<31){v.push_back(0);}      
            for(int i=0;i<v.size();i++) if(v[i]) digit[i].all1(l,r,1,n,1);
        }
        else if(op==2){
            cin>>k;while(k){v.push_back(k&1);k>>=1;}  
            while(v.size()<31){v.push_back(0);}      
            for(int i=0;i<v.size();i++) if(!v[i]) digit[i].all0(l,r,1,n,1);
        }
        else if(op==3){
            cin>>k;while(k){v.push_back(k&1);k>>=1;}
            while(v.size()<31){v.push_back(0);}        
            for(int i=0;i<v.size();i++) if(v[i]) digit[i].Xor(l,r,1,n,1);
        }
        else if(op==4){
            int ans=0;
            for(int i=0;i<=31;i++) ans+=(1<<i)*digit[i].querysum(l,r,1,n,1);
            cout<<ans<<"\n";
        }
        else if(op==5){
            cin>>k;
            cout<<digit[k].query(l,r,1,n,1).ans<<"\n";
        }
        else if(op==6){
            cin>>k;
            cout<<digit[k].query(l,r,1,n,1).a0<<"\n";
        }
        else cout<<"MikeRachel\n";
    }
    return 0;
}
```
最后，关于题面再次声明，题目描述纯属虚构。