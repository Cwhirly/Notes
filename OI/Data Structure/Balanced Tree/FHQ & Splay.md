---
难度: "6"
前置知识: 二叉搜索树
---
 I**前言**  
DeepSeek 说：“Treap 200 行内可完成，非常简洁。“
然后我看着我 50 行的代码陷入了沉思。
为了~~省篇幅~~更好地对比两种常见的平衡树（FHQ Treap & Splay Tree），放在同一文中分析。
## 一、二叉搜索树  
二叉搜索树（Binary Search Tree , BST）是一棵中序遍历为升序的二叉树。  
这是一个普及算法，在这里不做过多赘述。   
它常用来维护可重集，进行插入，删除，查找等操作。  
但是如果依次这样插入：$1,2,3,4,5,6$，那么树形就会退化为一条很长的只有右儿子的链。  
树深，也就是查找的复杂度就会变成 $O(n)$，很容易 TLE。  
所以我们需要让树深最好控制在 $O(\log n)$，也就是让树平衡。  
## 二、基础  
### Splay
这是一种常数较大的平衡树，但是 LCT 需要用到它。  
在 Splay 中，一个节点要维护以下信息：  
```cpp
struct node{int fa,ch[2],val,cnt,siz;} tr[N];
```
分别代表：该节点的父亲、两个儿子、权值、该权值出现的次数，子树大小。  
还需要两个变量 $rt$ 和 $tot$，分别表示树根的编号以及节点个数。  
注意，如果一个节点的 $cnt$ 值大于 $1$，这就代表着这个节点实际上是 $cnt$ 个节点，只是因为它们权值相等所以而共用了编号。  
还有几个基本的函数：  
```cpp
void maintain(int x){sz[x]=sz[ch[x][0]]+sz[ch[x][1]]+cnt[x];} 
bool get(int x){return x==ch[fa[x]][1];} 
void clear(int x){ch[x][0]=ch[x][1]=fa[x]=val[x]=sz[x]=cnt[x]=0;}
```
它们分别就是 `pushup`，判断一个节点是左儿子还是右儿子，以及销毁节点。  
### FHQ Treap 
这是一种轻量简洁的平衡树，且速度较优，在 OI 中大抵是最广泛的平衡树。  
先不用管它的名字是怎么来的，后面会说。  
在 Splay 中，一个节点要维护以下信息：  
```cpp
struct node{int ch[2],val,siz,hp;} st[N];
```
分别代表：该节点的两个儿子、权值、子树大小、堆编号。  
“堆编号”是什么？不用管，后面说。  
也需要两个变量 $rt$ 和 $tot$，分别表示树根的编号以及节点个数。  
注意，这里节点没有记录 $cnt$ 和 $fa$，也就是说，FHQ Treap 并不需要考虑一个节点的父亲节点，并且权值相同的节点会有不同的编号，而不是挤在同一节点内。  
基本的函数：  
```cpp
void maintain(int x){st[x].sz=st[st[x].ch[0]].sz+st[st[x].ch[1]].sz+1;} 
int crte(int v){int now=++tot;st[now]={{0,0},v,1,rnd()};return now;}
```
只有 `maintain` 和 `create`，就是新建一个节点。  
那个 `rnd` 不用管。
此处已经可看出 FHQ Treap 的简洁性。
## 三、平衡操作  
（最好配合 TOC 食用）
### Splay 
为了使树平衡，Splay 树使用了一种操作：Splay。  
Splay 操作主要通过旋转来实现，它的效果是将某一个节点旋转到根。  
#### 单旋  
分为左旋（zig）和右旋（zag），它们是很对称的。  
先勉强用文字描述一下，其实对某个点旋转就是把它的父节点“拽”下来，放到该点的下面去。  
具体地，如果一个点 $x$ 是其父节点 $p$ 的左儿子，那么就让 $p$ 成为 $x$ 的右儿子。  
原来 $x$ 的右儿子怎么办？  
让它成为现在 $p$ 的左儿子即可。  
这就是右旋。  

左旋就是把上文中的“左”字和“右”字互换即可。  

这是 OI-wiki 中的演示图。    
![](https://oi-wiki.org/ds/images/splay-zig.svg)
可以这么想象：有一个手将 $x$ “拎”了起来，$p$ 因为重力而下垂，掉到下面，顺便“抢”来了 $x$ 的一个子节点。  

单旋有一个性质：单选不改变树的中序遍历（注意：此处“中序遍历”指遍历权值）。  
这是很显然的。
下面是代码，它将左旋和右旋写进了同一函数中，并不好看。
```cpp
inline void rotate(int x){
		int f=fa[x],gf=fa[f],lr=get(x);
		ch[f][lr]=ch[x][lr^1];
		if(ch[x][lr^1]) fa[ch[x][lr^1]]=f;
		ch[x][lr^1]=f;
		fa[f]=x;
		fa[x]=gf;
		if(gf) ch[gf][f==ch[gf][1]]=x;
		maintain(f);
		maintain(x);
}
```

这是一个很好看~~（块状）~~但只是图一乐的写法。

```cpp
void rotate (int x) {
int y=fa[x],s=get(x);
int z=fa[y],t=get(y);
ch[y][s]= ch[x][s^1];
if( ch[x] [ s ^ 1 ] )
fa[ch[x] [s ^ 1]] =y;
ch[ x ] [ s ^ 1 ] =y;
fa[y] = x; fa[x] = z;
if (z) ch[z] [t] = x;
mainta(y);mainta(x);}
```
#### 双旋
##### zig-zig/zag-zag
设 $p=fa_x,g=fa_p$  当 $x,p,g$ “共线”，即 $x,p$ **同为**左儿子或右儿子时，我们应该先转一下 $p$，再转一下 $x$，如此图。  
![](https://oi-wiki.org/ds/images/splay-zig-zig.svg) 
这样我们就可以把 $x$ 转到上面了。   
注意到一个性质，两次旋转同为左旋/右旋。  
所以就叫做“zig-zig”或“zag-zag”。  
##### zig-zag/zag-zig  
![](https://oi-wiki.org/ds/images/splay-zig-zag.svg)
设 $p=fa_x,g=fa_p$  当 $x,p,g$ “不共线”，即 $x,p$ 一个是左儿子，一个是右儿子时，我们应该转两下 $x$，如此图。  
注意到一个性质，两次旋转一次是左旋，一次是右旋。  
所以就叫做“zag-zig”或“zig-zag”。  
#### Splay 操作  
通过若干次双旋和零或一次单旋将 $x$ 旋转到根。  
为什么最后可能有一次单旋?  
因为我们发现，双旋要判断 $x,p,g$ 是否“共线”，但当 $x$ 为根的儿子时，$g$ 不存在，所以只需单旋即可。  
代码：
```cpp
inline void splay(int x){
		rt=x;
		while(fa[x]){
				int f=fa[x],g=fa[fa[x]];
				if(g){
						if(get(f)==get(x)) rotate(f);
						else rotate(x);
				}
				rotate(x);
		}
}
```  

这是易于理解的代码，但很丑陋，可以这么写：  

```cpp
inline void splay(int x){
    for(int f=fa[x];f=fa[x];rotate(x)){
        if(fa[f]) rotate((get(f)==get(x))?f:x);
    }
    rt=x;
}
```

最好在理解第一个代码后再看第二个代码。
### FHQ Treap
FHQ Treap 不需要旋转操作，所以它也被称为“无旋 Treap”。  
~~是的，还有“有旋 Treap”，但我不会。~~  
那么 Treap 是什么意思?  
其实是 “Tree” 与 “Heap” 的结合，因此它也被称为“树堆”。  
现在说回之前说节点所维护的那个 $hp$，也就是堆编号。  
我们发现在建立节点时，$hp$ 被赋成了 `rnd()`。  
这一使用了 `mt19937` 来生成随机数。  
使用的时候就是 `mt19937 val(time(0))`，至于那个 `val` 就是一个类似变量名或函数名的东西，给它起什么名字都行。  
使用的时候就调用 `val()` 即可。  
据说比 `rand` 要好一些，因为 `rand` 的生成范围是 `short`，而它的生成范围是 `int`。（应该是这样？）
所以也就是说每个点的 $hp$ 是一个随机值。  
它是干什么用的？后面说。  
先看 FHQ Treap 的基本两项操作。
#### 分裂  
顾名思义，这个操作是把一棵平衡树分裂成两个树。  
具体有两种分裂方法。  
##### 按值分裂
譬如说，要按权值 $v$ 来分裂，分裂成的两棵树其中一棵所有节点的权值都小于等于 $v$，另一棵权值都大于 $v$。  
具体其实就是递归实现，如果根的权值小于等于 $v$，递归分裂右子树，否则递归分裂左子树。   
$x,y$ 是两棵树的根节点编号，$p$ 是要分裂的子树的根的编号。

```cpp
void sptv(int p,int v,int &x,int &y){
		if(!p){x=y=0;return ;}
		if(v<st[p].val){y=p,sptv(ls(p),v,x,ls(p));}
		else{x=p,sptv(rs(p),v,rs(p),y);}
		maintain(p);
}
```
##### 按排名分裂
譬如说，要按排名 $rnk$ 来分裂，分裂成的两棵树其中一棵所有节点的权值在这棵树中的排名都小于等于 $rnk$，另一棵都大于 $rnk$。  
具体还是递归实现，如果根的权值的排名小于等于 $rnk$，递归分裂右子树，否则递归分裂左子树。   
$x,y$ 是两棵树的根节点编号，$p$ 是要分裂的子树的根的编号。  
```cpp
void spts(int p,int v,int &x,int &y){
		if(!p){x=y=0;return ;}
		if(st[ls(p)].sz+1>v){y=p,spts(ls(p),v,x,ls(p));}
		else{x=p,spts(rs(p),v-st[ls(p)].sz-1,rs(p),y);}
		maintain(p);
}
```
注意排名的定义（`if` 条件中的加一）
#### 合并
首先需要说明，在合并以 $x,y$ 为根的两棵树时，需要保证 $x$ 中节点的权值全部小于 $y$ 中节点的权值。  
这里合并不能乱合并，而是要按照 $hp$ 合并，最终合并下来的树应该让每个节点的 $hp$ 满足堆的性质，即其 BFS 序满足 $hp$ 递增。
```cpp
int mrge(int x,int y){
	if(!x||!y) return x^y;
	if(st[x].hp>st[y].hp){rs(x)=mrge(rs(x),y),maintain(x);return x;}
	else{ls(y)=mrge(x,ls(y)),maintain(y);return y;}
}
```
$x \operatorname{xor} y$ 其实返回的是两个中非零的一个。  
函数返回最后合并出的树的根节点编号。  
为什么要让它满足堆的性质？  
请看接下来部分。
### 平衡性证明
#### Splay
我们可以使用势能分析法得出一次 Splay 操作的均摊复杂度是 $O(\log n)$。  
总复杂度是均摊 $O(m\log n)$。
#### FHQ Treap
它的复杂度证明过于复杂（至少我看不懂），所以感性理解。  
由于 $hp$ 随机，所以成为链或深度极大的概率很小。  
所以均摊 $O(m\log n)$，但常数比 Splay 好。  
## 四、插入
操作：*向可重集中插入一个数 $x$*。
### Splay
+ 如果树是空的，那太简单了，直接新建一个节点作为根，权值为 $x$ 即可。
+ 树不空，从树根往下找，如果 $x$ **小于**现在节点的权值，就往左儿子走；**大于**就往右儿子走；如果等于，就让目前节点的 $cnt$ 加 $1$，代表该节点的位置又多了一个同权值的点。
+ 要是走到叶子还没碰到哪个节点的权值为 $x$，就在叶子下面新建一个节点，权值为 $x$，若 $x$ 小于现在节点的权值，新节点就作为它的左儿子，大于就是右儿子。 
注意，最后一定要执行 Splay 操作，把被修改的节点旋转到根。  
这么做的目的是保证树平衡。  
代码：  
```cpp
inline void insert(int k){
	if(!root){
		val[++tot]=k;cnt[tot]++;
		root=tot;maintain(tot);
		return ;
	}
	int now=root,f=0;
	while(true){
		if(val[now]==k){
			++cnt[now];
			maintain(now);maintain(f);
			splay(now);break;
		}
		f=now;now=ch[now][val[now]<k];
		if(!now){
			val[++tot]=k;++cnt[tot];
			fa[tot]=f;ch[f][val[f]<k]=tot;
			maintain(tot);maintain(f);
			splay(tot);break;
		}
	}
}
```
### FHQ Treap
异常的简短。  
只需要先把数按值 $x$ 分裂，然后新建一个节点（作为一棵新树），最后把三棵树合并即可。    
```cpp
inline void insert(int v){sptv(rt,v,x,y);rt=mrge(mrge(x,crte(v)),y);}
```
只有一行。    
顺便提一嘴，$x,y$ 可以定义成全局变量。  
### 五、根据值查询排名  
操作：*在可重集中查找一个数 $x$ 的排名，它定义为小于 $x$ 的数的个数加 $1$*。  
### Splay 
从树根往下走，如果往左子树走，就直接走，如果往右子树走，那么答案加上左子树的大小，因为左子树里的所有节点的权值都小于 $x$。
最后走到某个节点的权值等于 $x$，~~套路化地~~把目前节点 Splay 到根，然后返回 $ans+1$。  
如果树中根本没有节点的权值等于 $x$，那就不用 Splay 了（也无法 Splay 啊）。  
```cpp
inline int rank(int k){
	int ans=0,now=root;
	while(1){
		if(k<val[now]) now=ch[now][0];
		else{
			ans+=sz[ch[now][0]];
			if(!now) return ans+1;
			if(k==val[now]){splay(now);return ans+1;}
			ans+=cnt[now];now=ch[now][1];
		}
	}
}
```
### FHQ Treap
先按值 $x-1$ 分裂，然后答案就是分裂出的前一棵树的大小加一。  
最后再合并回去。  
还是一行（其实后面的操作全是一行）。  
```cpp
inline int vtor(int v){sptv(rt,v-1,x,y);int ans=st[x].sz+1;rt=mrge(x,y);return ans;}
```
### 六、根据排名查询值
操作：*在可重集中查找排名为 $k$ 的数*。  
### Splay 
其实就只用到二叉搜索树的性质。  
还是往下走，看在左子树还是在右子树，最后使得所有左子树的大小加起来是 $k$。
逻辑比较简单，直接看注释。  
```cpp
inline int askrank(int k){
	int now=root;
	while(true){
		if(ch[now][0]&&k<=sz[ch[now][0]]) now=ch[now][0];//有左子树且排名小于当前节点的排名就往左子树走
		else{//否则
			k-=cnt[now]+sz[ch[now][0]];//排名为k的数对应的节点一定是now或其右子树
			//相当于在其右子树中寻找排名为k-cnt[now]-sz[ch[now][0]]的数
			if(k<=0){//说明k就是now的排名（now的排名<=cnt[now]+sz[ch[now][0]])
				splay(now);//照常Splay
				return val[now];//返回答案
			}
			else now=ch[now][1];//k不是now的权值，只能往右子树走了
		} //右儿子一定存在，否则就没有排名为k的数了
	}
}
```
### FHQ Treap
按排名分裂。  
返回右边那个树的第一个值即可。  
```cpp
inline int rtov(int r){spts(rt,r-1,x,y);spts(y,1,y,z);int ans=st[y].val;rt=mrge(mrge(x,y),z);return ans;}
```
