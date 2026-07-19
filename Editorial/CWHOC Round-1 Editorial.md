本模拟赛旨在唤醒大家对语文学科的重视与热爱。
~~（尽管不大可能有用）~~

| 题目编号 |  T1  |  T2  |   T3   |  T4  |  T5  |
| :--: | :--: | :--: | :----: | :--: | :--: |
| 题目名称 | 醉翁亭记 | 渔家傲  |   蒹葭   | 岳阳楼记 | 回延安  |
|  难度  |  蓝   |  绿   |   紫    |  橙   |  紫   |
| 算法标签 | 主席树  | 分类讨论 | 莫比乌斯反演 |  同余  | 树链剖分 |
# T1
题目背景是初 2026 届八年级下学期期末考试的阅读理解原题。  
语文老师评价是很有区分度的题，考真正的语文思维和功底，而非盲目使用套路，符合新的中考语文命题趋势。  
出的比较好的题目有：
- 请结合文本分析“秋雨”在全文中的多重作用。  
- 从文中第四段可知欧阳修喜爱梅花，常写梅花诗，联系上下文和你对欧阳修的理解，说说欧阳修与古梅有哪些相似之处。
- 文章结尾部分，作者为什么打算从酿泉灌一瓶水，把这些水汇到龙潭去？


对于语文好的同学来说可能可以轻松驾驭，例如 CWH，他在这场考试中获得了总分和语文双 rnk1，让我们为他表示祝贺！  

---
言归正传，我们用中文翻译题面，大意为：

> 给定一颗有根树，$q$ 次询问，每次询问给出 $k,a$，问有多少对有序三元组 $(a,b,c)$，满足 $a,b$ 都是 $c$ 的祖先，且 $a,b$ 之间的距离不超过 $k$。

提前说明，我们设根节点的深度为 $1$，$H$ 为树高，节点 $a$ 的深度记为 $dep_a$，其子树大小记为 $sum_a$。 
根据第一条性质得出 $a,b,c$ 一定在同一条链上，且这条链没有转折点，即直接竖直向下延伸。  
根据第二条性质，$b$ 的位置需要进行分类讨论。  

如果 $b$ 在 $a$ 上方，则 $b$ 有 $\min(k,dep_a-1)$ 种选法，$c$ 一定在 $a$ 下方，故有 $(sum_a-1)$ 种选法。  
根据乘法原理，共有 $\min(k,dep_a-1)(sum_a-1)$ 种选法。  
如果 $b$ 在 $a$ 下方，则 $dep_b$ 的范围是：$dep_a<dep_b\leq\min(dep_a+k,H)$，同时，还要满足 $a$ 是 $b$ 的祖先。
我们要统计满足这些条件的 $b$ 的 $sum_b$ 之和。   
主席树做二维数点即可。

```cpp
#include<bits/stdc++.h>
#define sor(i,l,r) for(int i=l;i<=r;i++)
#define int long long
using namespace std;
namespace luuia {
    const int __SIZE=(1<<21)+1;
char ibuf[__SIZE],*iS,*iT,obuf[__SIZE],*oS=obuf,*oT=oS+__SIZE-1,_c,qu[55];int __f,qr,_eof;
#define Gc()(iS==iT?(iT=(iS=ibuf)+fread(ibuf,1,__SIZE,stdin),(iS==iT?EOF:*iS++)):*iS++)
inline void flush(){fwrite(obuf,1,oS-obuf,stdout),oS=obuf;}inline void gc(char &x){x=Gc();}
inline void pc(char x){*oS++=x;if(oS==oT)flush();}
inline void pstr(const char *s){int __len=strlen(s);for(__f = 0;__f< __len;++__f)pc(s[__f]);}
inline void gstr(char *s){for(_c=Gc();_c<32||_c>126||_c==' ';)_c=Gc();
for(;_c>31&&_c<127&&_c!=' '&&_c!='\n'&&_c!='\r';++s,_c=Gc())*s=_c;*s=0;}
template<class I>inline bool read(I &x){_eof=0;
for(__f=1,_c=Gc();(_c<'0'||_c>'9')&&!_eof;_c=Gc()){if(_c=='-')__f=-1;_eof|=_c==EOF;}
for(x=0;_c<='9'&&_c>='0'&&!_eof;_c=Gc())x=x*10+(_c&15),_eof|=_c==EOF;x*=__f;return !_eof;}
template<class I>inline void print(I x){if(!x)pc('0');if(x<0)pc('-'),x=-x;
while(x)qu[++qr]=x%10+'0',x/= 10;while(qr)pc(qu[qr--]);}struct Flusher_{~Flusher_(){flush();}}io_flusher_;
}using luuia::pc;using luuia::gc;using luuia::pstr;using luuia::gstr;using luuia::read;using luuia::print;
namespace Revitalize
{
    const int N=4e5+5;
    int n,m;
    struct edge{
        int to,nxt;
    }e[N<<1];
    int head[N],cnt;
    inline void add(int u,int v){
        e[++cnt].to=v;
        e[cnt].nxt=head[u];
        head[u]=cnt;
    }
    int num,dfn[N],rnk[N],dep[N],fa[N],sum[N];
    void dfs(int u){
        sum[u]=1;
        dfn[u]=++num;
        rnk[dfn[u]]=u;
        for(int i=head[u];i;i=e[i].nxt){
            int v=e[i].to;
            if(v==fa[u]) continue;
            fa[v]=u;
            dep[v]=dep[u]+1;
            dfs(v);
            sum[u]+=sum[v];
        }
    }
    int root[N];
    struct node{
        int ls,rs,d,del;
    }st[N*40];
    int cnnt=0;
    void update(int l,int r,int &pre,int c,int id){
        int p=++cnnt;
        st[p]=st[pre];
        st[p].d+=c;
        ++st[p].del;
        pre=p;
        if(l==r) return ;
        int mid=(l+r)>>1;
        if(id<=mid) update(l,mid,st[pre].ls,c,id);
        else update(mid+1,r,st[pre].rs,c,id);
    }
    int query(int l,int r,int s,int t,int k){
        if(s==t) return st[r].d-st[l].d;
        int mid=(s+t)>>1;
        if(k<=mid) return query(st[l].ls,st[r].ls,s,mid,k);
        else return st[st[r].ls].d-st[st[l].ls].d+query(st[l].rs,st[r].rs,mid+1,t,k);
    }
    int query2(int l,int r,int s,int t,int k){
        if(s==t) return st[r].del-st[l].del;
        int mid=(s+t)>>1;
        if(k<=mid) return query2(st[l].ls,st[r].ls,s,mid,k);
        else return st[st[r].ls].del-st[st[l].ls].del+query2(st[l].rs,st[r].rs,mid+1,t,k);
    }
    inline void work(){
        read(n),read(m);
        sor(i,1,n-1){
            int u,v;
            read(u),read(v);
            add(u,v);
            add(v,u);
        }
        dep[1]=1;
        dfs(1);
        int len=-1;
        sor(i,1,n) len=max(len,dep[i]);
        sor(i,1,n){
            root[i]=root[i-1];
            update(1,len,root[i],sum[rnk[i]],dep[rnk[i]]);
        }   
        while(m--){
            int a,k;
            read(a),read(k);
            cout<<(min(dep[a]-1,k)*sum[a]-min(dep[a]-1,k))+query(root[dfn[a]-1],root[dfn[a]+sum[a]-1],1,len,min(len,dep[a]+k))-sum[a]-query2(root[dfn[a]-1],root[dfn[a]+sum[a]-1],1,len,min(len,dep[a]+k))+1<<"\n";
        }
    }
}
signed main()
{
    ios::sync_with_stdio(false);
    cin.tie(0),cout.tie(0);
    return Revitalize::work(),0;
}
```


# T2   
题意：给你一段区间，每次询问某个区间 $[l,r]$ 是否可以被切成 $k\ge 2$ 段子区间，使得这些子区间的异或和相等。
## 100 pts
不想思考暴力做法了……直接说正解吧。  
首先注意到一个性质，我们发现 $3$ 个异或和相等的连续子区间可以被合并为一个子区间，异或和与原来 $3$ 小段的异或和都相等，因为很明显 $x\oplus x\oplus x=x$。  
所以本质上 $k$ 就只有 $2,3$ 两个取值，因为如果 $k>3$ 则必然可以通过 $3$ 段一合并最终归约到 $k=2,3$。  
那么分讨即可。  
### $k=2$
也就是存在 $x\in(l,r)$，使得 $s_x\oplus s_{l-1}=s_r\oplus s_{x}$。  
$s$ 是前缀异或和。  
消去 $s_x$，那就是 $s_{r}=s_{l-1}$ 时，则必可以切成两段。  
### $k=3$
也就是存在 $x,y\in(l,r),x<y$，使得 $s_x\oplus s_{l-1}=s_y\oplus s_{x}=s_r\oplus s_y$。  
那就是：
$$
\begin{cases}
x<y\\
s_{l-1}=s_y  \\
s_x=s_r
\end{cases}
$$
维护一个形如 `map<int,set<int> > mp` 的东西，其中 `mp[i]` 就存的是由所有的满足 $s_x=i$ 的下标 $x$ 所构成的 `set`。  
根据贪心思想，我们必然要找到 $(l,r)$ 内最小的 $x$，使得 $s_x=s_r$，那就在 `mp[s[r]]` 里面 `lower_bound` 一下即可。  
然后如果没有 $x$ 则直接表示不能切成 $3$ 段，否则再在 $(x,r)$ 中还是用 `lower_bound` 寻找 $y$，使得 $s_y=s_{l-1}$ 即可。    

每次判断就看 $k=2$ 和 $k=3$ 的条件满足其中一个就可以输出 `poly`，两个都不行就输出 `modui`。  
```cpp
#include <bits/stdc++.h>
using namespace std;
const int maxn=2e5+5;
int T,n,m,x,y;
int a[maxn],b[maxn],c[maxn],len;
map<int,vector<int> > mp;
vector<int>::iterator posx,posy;
int main(){
	scanf("%d",&T);
	while(T--){
		scanf("%d%d",&n,&m);
		mp.clear();
		for(int i=1;i<=n;++i) scanf("%d",a+i),a[i]^=a[i-1],mp[a[i]].emplace_back(i);
		while(m--){
			scanf("%d%d",&x,&y);
			if(a[y]==a[x-1]){
				puts("YES");
				continue;
			}
			posy=lower_bound(mp[a[x-1]].begin(),mp[a[x-1]].end(),y);
			posx=lower_bound(mp[a[y]].begin(),mp[a[y]].end(),x);
			if(posy==mp[a[x-1]].begin()||posx==mp[a[y]].end()) puts("NO");
			else --posy,puts(*posx<*posy&&*posx>=x&&*posy<=y?"YES":"NO");
		}
		putchar('\n');
	}
}
```

# T3  
（这里先假设 $n=m$）
让你求 
$$
\prod_{i=1}^n\prod_{j=1}^nf(\gcd(i,j))
$$
直接开始推式子：
$$
\begin{aligned}
\text{ans}&=\prod_{i=1}^n\prod_{j=1}^nf(\gcd(i,j))\\
&=\prod_{d=1}^n\prod_{i=1}^n\prod_{j=1}^nf(d)^{[\gcd(i,j)=d]}\\
&=\prod_{d=1}^nf(d)^{\tiny\displaystyle\sum_{i=1}^n\sum_{j=1}^n[\gcd(i,j)=d]}\\
&=\prod_{d=1}^nf(d)^{\tiny\displaystyle\sum_{i=1}^{\lfloor\frac{n}{d}\rfloor}\sum_{j=1}^{\lfloor\frac{n}{d}\rfloor}[\gcd(i,j)=1]}\\
&=\prod_{d=1}^nf(d)^{\tiny\displaystyle\sum_{i=1}^{\lfloor\frac{n}{d}\rfloor}\sum_{j=1}^{\lfloor\frac{n}{d}\rfloor}\sum_{x|i,x|j}\mu(x)}\\
&=\prod_{d=1}^nf(d)^{\tiny\displaystyle\sum_{x=1}^{\lfloor\frac{n}{d}\rfloor}\mu(x)\sum_{i=1}^{\lfloor\frac{n}{dx}\rfloor}\sum_{j=1}^{\lfloor\frac{n}{dx}\rfloor}1}\\
&=\prod_{d=1}^nf(d)^{\tiny\displaystyle\sum_{x=1}^{\lfloor\frac{n}{d}\rfloor}\mu(x)\lfloor\frac{n}{dx}\rfloor^2}\\
&=\prod_{d=1}^nf(d)^{\tiny\displaystyle\sum_{x=1}^{\lfloor\frac{n}{d}\rfloor}\mu(x)\lfloor\frac{n}{dx}\rfloor^2}\\
\end{aligned}
$$
设 $R(x)=\displaystyle\sum_{i=1}^x\mu(i)\lfloor\dfrac{n}{i}\rfloor^2$，可以预处理。  
原式则为
$$
\prod_{d=1}^nf(d)^{R(\lfloor\frac{n}{d}\rfloor)}
$$
那么数论分块，复杂度 $O(n\sqrt{n}+T\sqrt{n}\log P)$。  
过不了！  
那么就不能这么考虑。  
转变，设 $c=dx$，则原式为：
$$
\begin{aligned}
&=\prod_{d=1}^nf(d)^{\tiny\displaystyle\sum_{x=1}^{\lfloor\frac{n}{d}\rfloor}\mu(\frac{c}{d})\lfloor\frac{n}{c}\rfloor^2}\\
&=\prod_{c=1}^n\prod_{d|c}f(d)^{\mu(\frac{c}{d})\lfloor\frac{n}{c}\rfloor^2}\\
&=\prod_{c=1}^n(\prod_{d|c}f(d)^{\mu(\frac{c}{d})})^{\lfloor\frac{n}{c}\rfloor^2}\\
\end{aligned}
$$
很明显中间括号内的东西可以 $O(n\log n)$ 利用埃氏筛的思想暴力处理，外层数论分块，总复杂度 $O(n\log n+T\sqrt{n}\log P)$，可以过。
```cpp
#include<iostream>
#include<cstdio>
#include<cstdlib>
#include<cstring>
#include<cmath>
#include<algorithm>
#include<set>
#include<map>
#include<vector>
#include<queue>
using namespace std;
#define MOD 1000000007
#define MAX 1000000
inline int read()
{
    int x=0,t=1;char ch=getchar();
    while((ch<'0'||ch>'9')&&ch!='-')ch=getchar();
    if(ch=='-')t=-1,ch=getchar();
    while(ch<='9'&&ch>='0')x=x*10+ch-48,ch=getchar();
    return x*t;
}
int fpow(int a,int b)
{
    int s=1;
    while(b){if(b&1)s=1ll*a*s%MOD;a=1ll*a*a%MOD;b>>=1;}
    return s;
}
int f[MAX+10],pri[MAX],tot;
int g[MAX+10];
int inv[MAX+10];
int F[MAX+10];
int mu[MAX+10];
bool zs[MAX+10];
int n,m;
void pre()
{
    f[1]=g[1]=F[0]=F[1]=1;
    mu[1]=1;zs[1]=true;
    for(int i=2;i<=MAX;++i)
    {
        f[i]=(f[i-1]+f[i-2])%MOD;
        g[i]=fpow(f[i],MOD-2);F[i]=1;
        if(!zs[i])pri[++tot]=i,mu[i]=-1;
        for(int j=1;j<=tot&&i*pri[j]<=MAX;++j)
        {
            zs[i*pri[j]]=true;
            if(i%pri[j])mu[i*pri[j]]=-mu[i];
            else{break;}
        }
    }
    for(int i=1;i<=MAX;++i)
    {
        if(!mu[i])continue;
        for(int j=i;j<=MAX;j+=i)
            F[j]=1ll*F[j]*(mu[i]==1?f[j/i]:g[j/i])%MOD;
    }
    for(int i=2;i<=MAX;++i)F[i]=1ll*F[i]*F[i-1]%MOD;
}
int main()
{
    pre();
    int T=read();
    while(T--)
    {
        n=read(),m=read();
        if(n>m)swap(n,m);
        int i=1,j,inv,ans=1;
        while(i<=n)
        {
            j=min(n/(n/i),m/(m/i));
            inv=1ll*F[j]*fpow(F[i-1],MOD-2)%MOD;
            ans=1ll*ans*fpow(inv,1ll*(n/i)*(m/i)%(MOD-1))%MOD;
            i=j+1;
        }
        printf("%d\n",(ans+MOD)%MOD);
    }
    return 0;
}


```

# T4
显然就是求 $am^2 + bm+ c$ 中的 $b$，其中 $b,c<m$。  
所以答案就是 $\lceil\dfrac{10n \bmod m^2}{m}\rceil$。  
时间复杂度 $O(n)$，期望得分 100pts。
```cpp
#include<bits/stdc++.h>
using namespace std;
#define int long long
#define intt __int128
#define endl '\n'
int n,m;
int Q(int a,int b,int m){
    int res=1;
    while(b){
        if(b&1) res=res*a%m;
        a=a*a%m;
        b>>=1;
    }
    return res%m;
}
signed main()
{
    ios::sync_with_stdio(false);
    cin.tie(0);cout.tie(0);
    cin>>n>>m;
    int r=Q(10,n,m),tmp=(Q(10,n,m*m)-r+m*m)%(m*m);
    int q=(tmp/m)%m;
    cout<<q;
    return 0;
}
```

# T5
回延安要求全文背诵。  

---
看到这个题，树上的路径操作并查询，那不就是树剖么？心里顿时有了底。可是仔细读题，发现并不是单纯地路径操作，还会涉及到对于每个点邻边的修改。这怎么搞？貌似单纯直白的树剖并不能解决问题。这条路能否走通呢？

其实是能的。我们对问题进行一个简单而巧妙的转化，就能解决！如果我们在每次操作的时候，将修改路径上的点全部染上一种**独一无二的颜色**，判断重边就只需要判断两端点颜色是否相同即可。端点颜色相同为重边（即流速快的边），否则为轻边（即流速慢的边）。

如果您懂了，就可以跳过下面这段话。

没有太懂的话，具体的讲一讲，因为要将一条路径上的边修成重边，那么路径点染同色的操作就可以保证这条路径上**点同色且所有边两端点同色**，这样这些边就符合重边的判定了！而且更妙的是，由于这些点被染成的是一种**独一无二的颜色**，是一定不与前面的颜色重复的，所以所有染色点连边的另一端一定与它的颜色不同，这样子这些边就自然而然的变成了轻边。这个转化不得不说是很精妙的。

于是，问题变成了：

如何统计树的一段路径上**同色相邻**点对的数量？（相邻：保证两点有边；同色：保证是重边）

我们知道，本题的 `pushup` 是不可交换的，换句话说，$a$ 和 $b$ 进行合并的结果与 $b$ 和 $a$ 进行合并的结果是不一样的。  
那么我们处理完某条重链的答案后，如何“加“到总答案里面去？    
我们需要知道上一个被处理的重链处于的路径段与此次处理的重链所在的路径段是否相同。    
如果不相同：  
	说明这两条重链不相接，于是就不用管，直接加；    
如果相同，那么我们考察：    
	已知跳重链是从下往上跳的，而越往下的节点 DFN 序越大。  
	也就是说此次处理的重链一定是接在上次处理的重链的“左边”的。      
	于是我们就需要关注此次处理的重链的“右端点“与上次处理的重链的“左端点”是否颜色相等。  
	如果相等，把此次的答案加到总答案里然后减一。同 `pushup`。  
注意，刚才说的所谓“左”，“右”，都是在**以 DFN 序为下标的序列**，即线段树所维护的那个序列上所考虑的。    

好了，现在返回去，是时候该想想怎么判断上一个被处理的重链处于的路径段与此次处理的重链所在的路径段是否相同了。    
简单！开两个变量 $ans1$，$ans2$，分别记录：    
现在的路径段上的**前一个**被处理的重链的左端点的颜色，      
另一个路径段上的**前一个**被处理的重链的右端点的颜色。    
最后 LCA 再单独处理——它与两个路径段都相接。

```cpp
#include<bits/stdc++.h>
using namespace std;
namespace Opshacom{
    const int N=400005;
    int n,m;
    struct edge{
        int to,nxt;
    }e[N];

    int head[N],cnt;
    inline void add(int u,int v){
        e[++cnt].to=v;
        e[cnt].nxt=head[u];
        head[u]=cnt;
    }
    class HPD{
        private:
            struct node{
                int d,tag,l,r;
                bool is;
            }st[N];
            inline void pushup(node &res,node resl,node resr){
                res.d=resl.d+resr.d+(resl.r==resr.l);
                res.l=resl.l;
                res.r=resr.r;
            }
            inline void pushdown(int p,int l,int r,int mid){
                if(st[p].is){
                    st[p<<1].is=st[p<<1|1].is=1;
                    st[p<<1].d=mid-l;
                    st[p<<1|1].d=r-mid-1;
                    st[p<<1].l=st[p<<1|1].l=st[p].tag;
                    st[p<<1].r=st[p<<1|1].r=st[p].tag;
                    st[p<<1].tag=st[p<<1|1].tag=st[p].tag;
                    st[p].tag=st[p].is=0;
                }
            }
        public:
            int dep[N],num,dfn[N],fa[N],sz[N],top[N],son[N];
            inline void clear(){
                memset(dep,0,sizeof(dep));
                memset(dfn,0,sizeof(dfn));
                memset(fa,0,sizeof(fa));
                memset(sz,0,sizeof(sz));
                memset(top,0,sizeof(top));
                memset(son,0,sizeof(son));
                memset(head,0,sizeof(head));
                for(int i=0;i<N;i++) e[i]={0,0},st[i]={0,0,0,0,0};
                num=cnt=0;
            }
            void update(int l,int r,int s,int t,int p,int c){
                if(l<=s&&t<=r){
                    st[p].d=t-s;
                    st[p].l=st[p].r=c;
                    st[p].tag=c;
                    st[p].is=1;
                    return;
                }
                int mid=(s+t)>>1;
                pushdown(p,s,t,mid);
                if(l<=mid) update(l,r,s,mid,p<<1,c);
                if(r>mid) update(l,r,mid+1,t,p<<1|1,c);
                pushup(st[p],st[p<<1],st[p<<1|1]);
            }
            node query(int l,int r,int s,int t,int p){
                if(l<=s&&t<=r){
                    //cout<<s<<" "<<t<<" "<<p<<" "<<st[p].d<<endl;
                    return st[p];
                }
                int mid=(s+t)>>1;
                pushdown(p,s,t,mid);
                if(r<=mid) return query(l,r,s,mid,p<<1);
                if(l>mid) return query(l,r,mid+1,t,p<<1|1);
                node tmp1=query(l,r,s,mid,p<<1);
                node tmp2=query(l,r,mid+1,t,p<<1|1);
                node res;
                pushup(res,tmp1,tmp2);
                return res;
            }
            void build1(int u){
                sz[u]=1;
                son[u]=-1;
                for(int i=head[u];i;i=e[i].nxt){
                    int v=e[i].to;
                    if(v==fa[u]) continue;
                    fa[v]=u;
                    dep[v]=dep[u]+1;
                    build1(v);
                    sz[u]+=sz[v];
                    if(son[u]==-1||sz[v]>sz[son[u]]) son[u]=v;
                }
            }
            void build2(int u,int tp){
                top[u]=tp;
                dfn[u]=++num;
                if(son[u]==-1) return ;
                build2(son[u],tp);
                for(int i=head[u];i;i=e[i].nxt){
                    int v=e[i].to;
                    if(v!=fa[u]&&v!=son[u]) build2(v,v);
                }
            }
            void change(int u,int v,int c){
                while(top[u]!=top[v]){
                    if(dep[top[u]]<dep[top[v]]) swap(u,v);
                    update(dfn[top[u]],dfn[u],1,n,1,c);
                    u=fa[top[u]];
                }
                if(dfn[u]>dfn[v]) swap(u,v);
                update(dfn[u],dfn[v],1,n,1,c);
            }
            int ask(int u,int v){
                int res=0;
                int ans1=-1e9,ans2=-1e9;
                while(top[u]!=top[v]){
                    if(dep[top[u]]<dep[top[v]]) swap(u,v),swap(ans1,ans2);
                    node tmp=query(dfn[top[u]],dfn[u],1,n,1);
                    res+=tmp.d;
                    if(tmp.r==ans1) res++;
                    ans1=tmp.l;
                    u=fa[top[u]];
                }
                if(dfn[u]>dfn[v]) swap(u,v),swap(ans1,ans2);
                node tmp=query(dfn[u],dfn[v],1,n,1);
                res+=tmp.d;
                if(tmp.l==ans1) res++;
                if(tmp.r==ans2) res++;
                return res;
            }
    }tr;
    inline void work(){
        tr.clear();
        cin>>n>>m;
        for(int i=1;i<n;i++){
            int x,y;
            cin>>x>>y;
            add(x,y);
            add(y,x);
        }
        tr.build1(1);
        tr.build2(1,1);
        for(int i=1;i<=n;i++) tr.update(tr.dfn[i],tr.dfn[i],1,n,1,-tr.dfn[i]);
        for(int i=1;i<=m;i++){
            int op;
            int u,v;
            cin>>op;
            if(op==1){
                cin>>u>>v;
                tr.change(u,v,i);
            }
            else{
                cin>>u>>v;
                cout<<tr.ask(u,v)<<"\n";
            }
        }
    }
}
int main(){
    ios::sync_with_stdio(false);
    int t;
    cin>>t;
    while(t--) Opshacom::work();
    return 0;
}
```
