---
难度: "10"
前置知识: "[[Link-Cut Tree]]"
前置知识 2: "[[Heavy Path Decomposition]]"
前置知识 3: "[[FHQ & Splay]]"
---
## 0. 前言
【暂无】
## 1. 例题
模板题：[P5649 Sone1](https://www.luogu.com.cn/problem/P5649)。  
给你一棵 $n$ 个节点的有根树，点带权，有 $q$ 次操作，分为十二种：  

- `0 x y` 表示将 $x$ 的子树中所有点权都改为 $y$；  
- `1 x` 表示把树根换为 $x$ 节点；  
- `2 x y z` 表示把 $x$ 到 $y$ 简单路径上所有点权改为 $z$；  
- `3 x` 表示询问 $x$ 的子树中最小权值；   
- `4 x` 表示询问 $x$ 的子树中最大权值；   
- `5 x y` 表示将 $x$ 的子树中所有点权都增加 $y$；  
- `6 x y z` 表示将  $x$ 到 $y$ 简单路径上所有点权加上 $z$；  
- `7 x y` 表示询问 $x$ 到 $y$ 简单路径上的最小权值；   
- `8 x y` 表示询问 $x$ 到 $y$ 简单路径上的最大权值；  
- `9 x y` 表示把 $x$ 的父亲换为 $y$，若 $y$ 在 $x$ 的子树里则忽略此操作；  
- `10 x y` 表示询问 $x$ 到 $y$ 简单路径上的点权和；  
- `11 x` 表示询问 $x$ 的子树中点权和。

## 2. 基本信息
Self-Adjusting Top Tree，以下简称 SATT，是 2005 年 Tarjan 和 Werneck 在他们的论文 Self-Adjusting Top Trees 中提出的一种基于 Top Tree 理论的维护完全动态森林的数据结构，简称为 SATT。
 
SATT 可以实现森林中任一棵树的链修改/查询、子树修改/查询以及非局部搜索等操作，其复杂度由 Splay 操作保证。  

根据势能分析可得出其均摊复杂度不超过 $\mathcal O(\log n)$。
  
2006 年，Werneck 在 [Design and Analysis of Data Structures for Dynamic Trees](https://renatowerneck.wordpress.com/wp-content/uploads/2016/06/wer06-dissertation.pdf) 中给出了第一个直接的最坏 $\mathcal O(\log ⁡n)$ Top Tree 实现。此前 Alstrup 等人的 Top Tree 已可以建立在 Topology Tree 之上实现最坏 $\mathcal O(\log ⁡n)$ ，但那并非独立的 Top Tree 实现。

注意原始 Top Tree/SATT 是基于边簇的，但在竞赛实现中，对于点权树问题，常直接采用“Compress Node 对应原树节点”的点式实现；本文后半部分主要讲这种实现。

## 3. 静态 Top Tree
先讲静态的。
考虑线段树如何维护序列的区间操作。
我们把一个区间拆成成 $\mathcal{O}(\log n)$ 个由递归分治得到是线段树区间，维护每一个线段树区间的信息，然后用其维护的具有结合率的运算进行合并，得到答案。  

将这个思想照搬到树上，可以通过树链剖分将树转化成序列，也可以通过建立 Top Tree 的方法。

### I. 簇合并 
一个簇（Top Cluster）是一个连通的**边集**（即不包含边集所连接的点），并且最多只有两个点有不在簇内的边，即最多只有两个点连向簇外。

我们称这些点为**界点**，两个界点之间的唯一简单路径称为**簇路径**。 

树根被钦定后，我们便可以称深度较浅的界点为**上界点**，较深的界点为**下界点**。若簇中只有一个点连向簇外，则可以任意钦定一个簇内部的一个原树叶子作为下界点。  

根据定义，显然有如下性质：
+ 在有根树中，如果点 $u$ 有儿子 $v$、边 $(u,v)$ 在某个簇内，且 $v$ 不在该簇的簇路径上，则 $v$ 子树内的所有边均在簇内。

说明这个性质是容易的，Top Tree 中的界点是簇连接外界的“唯一出口”：只要沿着簇内的一条边 $(u,v)$ 走进了一个子树，且该子树里面没有界点，那么这个子树就是一条死路，里面的所有节点都无法通向其他簇，整个子树就必须完全被该簇打包吞并。

一个大簇是由若干个更小的簇合并而来的，由此便可以进行递归分解和合并，递归的终点是仅包含一条边的簇，不妨称这种最小簇为“**叶簇**”。

簇之间的合并有两种方法：
#### 1. Compress
若簇 $X$ 的下界点和簇 $Y$ 的上界点相同，则使得合并后的更大簇 $Z$ 包含 $X,Y$ 中的所有边，其上界点为 $X$ 的上界点，下界点为 $Y$ 的下界点。可以直观的理解为：将两个上下相邻的簇直接通过一个公共界点拼接在一起。

如图所示。

![](https://cdn.luogu.com.cn/upload/image_hosting/41irquvl.png)
#### 2. Rake
若簇 $X,Y$ 的上界点相同，且 $Y$ 的下界点为叶子（即 $Y$ 中包含了某一个子树内的所有边），则使合并后的更大簇 $Z$ 包含 $X,Y$ 中的所有边，其上下界点与 $X$ 的上下界点完全一致。可以直观的理解为：将 $Y$ 直接“扫”到 $X$ 上。

如图所示。

![](https://cdn.luogu.com.cn/upload/image_hosting/1jy1uiro.png)

---
有了这两种合并，一棵树初始每个边各自成簇，一定存在某种合并方式，将所有簇最终合并成包含所有边的“根簇”。

Compress 和 Rake 合并信息的方式往往是不同的，需要分开实现。
### II. 树收缩
这是另一种刻画方式。
对于任意一棵树，我们都可以运用**树收缩**理论来将它收缩为一条边。
收缩的方法还是 Compress 和 Rake。
#### 1. Compress
Compress 操作指定一个度数为 $2$ 的点 $x$（这里的度数指当前正在进行收缩的树中的度数，而非原树度数），与点 $x$ 相邻的那两个点记为 $y,z$，我们连一条新边 $(y,z)$；将点 $x$、边 $(x,z)$、边 $(x,y)$ 的信息放到 $(y,z)$ 中储存，并删去它们。  

如图所示。

![](https://oi-wiki.org/ds/images/top-tree1.svg)
#### 2. Rake
Rake 操作指定一个度为 $1$ 的点 $x$，而且与点 $x$ 相邻的点 $y$ 的度数需大于 $1$，设点 $y$ 的另一个邻点为 $z$，我们将点 $x$、边 $xy$ 的信息放入边 $yz$ 中储存，并删去它们。如图所示。

![](https://oi-wiki.org/ds/images/top-tree2.svg)

---
这两种刻画描述的是同一套收缩过程的两个视角：前者自底向上描述簇如何合并，后者直接描述原树中的点/边如何被逐步消去。
### III. 建立树结构
建立一棵新的二叉树，将一个簇看作新树上的一个节点，仅包含一条边的簇为新树的叶子，通过 Compress 或 Rake 操作形成的大簇所代表的节点是合并前的两个小簇的父节点。最终形成的这棵树就是原树的 **Top Tree**。

![](https://cdn.luogu.com.cn/upload/image_hosting/ue9ifr12.png)

左边部分即原树，不带箭头的圆弧指 Compress 操作，带箭头的指 Rake 操作。

我们可以认为原树是一棵无根树，也可以认为 $e$ 是树根，右边部分为原树的 Top Tree, 其中形如 $xy$ 的节点指该节点代表的原树中簇的上下界点分别为 $x,y$，当然如果认为原树是无根树，则这个“界点”同样具有前后顺序，只不过是人为定的罢了。  

方点表示该节点代表的簇是仅有一条边组成，或由两个小簇 Compress 形成的，非叶子方点称为 Compress Node；圆点表示该节点代表的簇是由一个小簇 Rake 到另一个小簇上形成的, 称为 Rake Node。

由于一个簇中的两个界点之间存在先后顺序，所以 Top Tree 里的左右儿子顺序是不能调换的，对于 Compress Node，其左儿子为“更靠上”的小簇；对于 Rake Node，其左儿子在原树中的“箭头”指向右儿子。当然这个不涉及什么性质，所以让所有 Rake Node 把左右儿子关系做个对称也没有影响。

注意这仅仅是一个示意图，图中的部分节点只是为了展示每一轮收缩而人为画出的中间状态，并不是 Top Tree 实际需要存储的节点。例如图中的若干 $h_i$ 以及最上方作为 Compress Node 的 $g_i$ 都属于这种“展开后的中间节点”。实际实现只需保留真正参与层次结构的节点即可。
### IV. 应用
该以怎样的收缩方法构建这棵 Top Tree？考虑重链剖分，得到这样一个结构。

![]()https://cdn.luogu.com.cn/upload/image_hosting/xuifalte.png)



References
+ 

黑色的代表重链，三角形代表一棵轻子树。

对于递归过程中的每一个子树，采取这样的过程：先把重链剖出来，然后把每个轻子树递归处理，处理完成后，将所有轻子树 Rake 到重链上的相应叶簇上，我们就得到了一个竹节状的结构。

![](https://cdn.luogu.com.cn/upload/image_hosting/7vx2tpw7.png)

图中每一个回旋镖状物就是一个 Rake 了该条重边的上方点的全部轻子树的簇，对于轻子树递归处理。  

得到该结构后，如同线段树一样的进行递归 Compress，结束构建。

Rake 的过程是同样的，即若一个节点有多个轻子树，那么同样采用类似线段树的方法，先两个为一组互相 Rake，得到轻子树数一半的新簇，然后再两两 Rake，这样就算一个节点有 $\mathcal O(n)$ 个轻儿子，也可以保证 Rake Node 构成的部分深度不超过 $\mathcal O(\log n)$。  

但实际上以上方法的复杂度是双 $\log$ 的，并不优。

由于轻子树不是最大的子树，所以轻子树大小不会超过整棵树大小的一半，所以从根往下走经过的路径最多只能包含 $\mathcal O(\log n)$ 个轻边，就会走到子树大小为 $1$ 的叶子，而这也就意味着，最多只能走 $\mathcal{O}(\log n)$ 条重链。每条重链贡献 $\mathcal{O}(\log n)$ 的 Top Tree 上深度，那么它们在 Top Tree 上拼起来深度就达到了 $\mathcal O(\log^2 n)$。    

一个显然的 Hack：

![](https://cdn.luogu.com.cn/upload/image_hosting/nthf6y6i.png)

令 $n=2^k$，那么上图所显示的树的 Top Tree 单单 Compress Node 的深度贡献就达到了 $\sum_{i=1}^{\log n}(\log n-i)=\frac{\log^2n-\log n}{2}=\mathcal{O}(\log^2 n)$。  

因此，考虑全局平衡二叉树的思想。在 Compress 重链和 Rake 轻子树的时候做带权二分，而非严格二分，权值即为轻子树的大小。

此处仅仅举例 Rake 时的带权二分，Compress 时的带权二分是完全同理的，注意二者的带权二分缺一不可，如果其中任意一个使用普通二分，就可能导致复杂度退化。

考虑一个具有 $x$ 个轻儿子的原树节点 $u$，这些轻子树的 Top Tree 均已被建出，这 $x$ 棵 Top Tree 的根编号（注意是 Top Tree 中的编号而非原树中的）分别为 $t_1,t_2,\cdots,t_{x-1},t_x$。在前面的错误做法中，我们的二分方法是，对于一个在 Top Tree 上涵盖了 $t_{[l,r]}$ 作为后代的 Rake Node $\alpha$，其左儿子和右儿子分别涵盖 $t_{[l,\lfloor\frac{l+r}{2}\rfloor]},t_{(\lfloor\frac{l+r}{2}\rfloor,r]}$。  

而“带权二分”的方法是，不按点的数量折半，而是寻找一个 $\xi\in[l,r]$，使得它是第一个使前缀和达到总权值一半的位置。实际上写法是简单的，一边加轻儿子一边更新前缀和，直到这个前缀和刚刚大于 $u$ 全体轻子树和的一半，停止循环，并将已计算的前缀递归进左儿子，剩余部分递归进右儿子。

然后使 $t_{[l,\xi]}$ 作为 $\alpha$ 左儿子的后代，使 $t_{(\xi,r]}$ 作为 $\alpha$ 右儿子的后代，并以此递归分治下去即可。  

对于 Compress 时，我们定义一个重链上的点的权值是其轻子树大小之和 $+1$（此为它本身），以同样的方法构建每一个 Compress Node。

先感性理解，这个二分方法虽然在局部贡献的深度就会变大，但是这却造成了全局的平衡性。我们发现，以这种带权二分的方法，节点的深度会基本呈现出“权值越大的节点深度越小”的性质，于是可以认为，对于权值比较大的点，那么它的目前所在簇的 Top Tree 高度也会大一些，但是由于前面的性质，这棵较深的“子 Top Tree”会在带权二分的过程中“较浅”的接入局部，所以它更高的高度就在全局角度被“弥补”了。

然而注意这种方法大概率不会找到真正最优的划分, 我们并没有重排轻子树顺序。例如对于 $[1,2,3,4]$，以这种方法划分的结果是 $[1,2\ |\ 3,4]$, 然而实际上最优的划分是 $[1,4\ |\ 2,3]$。显然如果想要得到最优的划分，需要做一个背包，但是这个时间复杂度无法接受。事实上，我们可以证明这种不严格的划分不会导致整棵 Top Tree 深度的退化。

严格证明并不困难。

首先我们需要证明：对于局部的 Rake 树，从任何一个轻叶子（即某个原树节点轻子树的 Top Tree 的根）向上走到局部 Rake 树根的深度受到其权重比例的严格控制。

> [!abstract] 引理
> 设当前待合并的序列总权重为 $W=\sum_{i=l}^r w_i$。按前缀和寻找满足第一个满足 $\sum_{i=l}^{\xi}w_i\ge\frac{W}{2}$ 的带权中点 $\xi$ 进行二分分治。
> 那么对于其中任意一个权重为 $w_i$ 的点，其在生成的这棵局部树中的深度 $d_i$ 满足 $d_i\le2\log\left(\dfrac{W}{w_i}\right)+\mathcal O(1)$。

证明考虑，每次二分将区间划分为 $[l, \xi]$ 与 $(\xi,r]$，两部分的权重分别为 $W_L$ 与 $W_R$。由“带权中点”定义，右半部分显然满足 $W_R\le\frac{W}{2}$。

对于左半部分：
  * 若 $i \neq \xi$，因为 $\xi$ 是第一个使前缀和 $\ge\frac{W}{2}$ 的位置，所以 $i$ 所在的区间 $[l, \xi - 1]$ 的总权重严格 $<\frac{W}{2}$；
  * 若 $i = \xi$，在下一次分治中 $\xi$ 就会作为单点直接成为叶子（不再继续细分）。

因此，在局部树中每向下走至多 $2$ 步，包含节点 $i$ 的子区间总权重至少减半（$\le \frac{W}{2}$），或者节点 $i$ 已经被分离为叶子。
引理得证，初始权重为 $W$，当区间权重缩减到 $w_i$ 时必然成为单点叶子，因此向下走的步数至多为 $2\log(W)-2\log(w_i)+\mathcal O(1)=2\log\left(\frac{W}{w_i}\right)+\mathcal O(1)$。

---
然后考虑全局树高的证明。

考虑原树中任意一个叶子节点 $u$（初始大小 $w=1$），它在 Top Tree 中一路向上跳到全局根（总大小 $W=n$）：

路径上会依次经过多棵局部二叉树（可能是轻儿子合并的 Rake 树，也可能是重链压缩的 Compress 树）。

以下所说的节点均指 Top Tree 中的节点。

设刚开始跳到路径上第 $j$ 棵局部树的“叶子”，即“入口”上时，目前跳到的节点的子树所代表的原树中簇大小为 $S_j^{\text{in}}$，跳到这棵局部树的“出口”时目前点的子树在原树上的对应簇大小为 $S_j^{\text{out}}$。由引理可知：在第 $j$ 棵局部树内的深度 $\le2\log\left(\frac{S_j^{\text{out}}}{S_j^{\text{in}}}\right)+\mathcal O(1)$，当从第 $j$ 棵树跳到上一级第 $j+1$ 棵树时，上一级的“入口”簇必然包含了当前簇，即 $S_{j+1}^{\text{in}} \ge S_j^{\text{out}}$。

根据重链剖分性质，从叶到根最多跨越 $\mathcal O(\log n)$ 条重链（即只有 $\mathcal O(\log n)$ 棵局部树，常数项总和为 $\mathcal O(\log n)$）。

将沿途所有局部树内的深度累加：
$$
\begin{aligned}
\text{Ans}&=\sum_{j}\left(2\log\left(\frac{S_j^{\text{out}}}{S_j^{\text{in}}}\right)+\mathcal O(1)\right)\\
&\le2\sum_{j}\left(\log S_j^{\text{out}}-\log S_j^{\text{in}}\right)+\mathcal O(\log n)\\
&\le2\left(\log S_{\text{root}}-\log S_{\text{leaf}}\right)+\mathcal O(\log n) \\
&=2\log\left(\frac{n}{1}\right)+\mathcal O(\log n)\\
&=\mathcal{O}(\log n)
\end{aligned}
$$
从第二步到第三步的裂项相消是关键。

得证。

---
根据这个建树方法，就可以像线段树一样，极其方便地解决很多树上问题。

下面举一些经典应用，具体细节这里不做赘述，以下各题题解区均存在 Top Tree 做法及其代码实现：
#### 1. 维护最大独立集
例题为[【模板】动态 DP](https://www.luogu.com.cn/problem/P4719)。
对于每个簇，分别维护簇内独立集不包含两个界点、只包含上界点、只包含下界点、两个节点均包含的最大权值。  

#### 2. 维护直径
例题为 [[CEOI2019] Dynamic Diameter](https://www.luogu.com.cn/problem/P6845)。  
每个簇维护该簇内离上界点最远的点与上界点的距离，离下界点最远的点与下界点的距离（明显地，离上界点最远的点不一定是下界点，反之亦然），簇路径长度以及簇内直径即可。  

#### 3. 维护邻域
例题为 [【模板】点分树 | 震波](https://www.luogu.com.cn/problem/P6329)
分别维护簇内到两个界点距离最远的点到该界点的距离 $p,q$，和距离不超过 $1,2,3,\cdots,p-1$（及 $q-1$）的所有点的信息和。这样合并两个簇的复杂度为 $\mathcal O(\mathrm{size})$，其中 $\mathrm{size}$ 为簇的大小。
单点修改，此时就不要考虑正常的靠合并更新信息的方法，而是把包含该点的 $\mathcal{O}(\log n)$ 个簇直接全部拎出来。此时发现如果暴力修改的话，由于簇内维护的是前缀和状物，所以每个簇内修改最高达到了 $\mathcal{O}(\mathrm{size})$，所以我们每一个簇内用一个树状数组来维护，簇内信息的修改就变成了 $\mathcal{O}(\log\mathrm{size})$，总复杂度 $\mathcal{O}(n\log n+q\log^2n)$，实际上常数和代码难度都优于点分树的。
## 4. Self-Adjusting Top Tree
步入正题。
### I. 树结构 
我们任意假定一种树链剖分方法，并认为一个点有**零或一**个重儿子，即不要求分解为极长路径。（注意此处“重儿子”是人为钦定的，与子树大小或高度无关，你也可以把它称作“实儿子”之类，后文中若无特殊说明，“重儿子”“重链”等称谓均为人为钦定）  

得到树链剖分后，再以先前的方法在重链上 Compress，在轻子树 Rake，合并方法任意，不需要带权二分。

#### 1. Compress Tree
与先前不同的是，现在 Compress Node 与 Rake Node 的含义有些许变化，且叶子簇需要单拎出来作为第三种节点。  

在原先的二叉静态 Top Tree 中，我们给每个 Compress Node 的命名方法是用其上下节点来表示，而现在，我们需要用簇中的一个“合并点”来代指一个簇，即：两个小簇在 Compress 成一个大簇时, 以两个小簇之间交叠的那个公共点作为大簇的“代表”。

我们拿一条已经把所有轻儿子都 Rake 过了的重链 $\text{a-b-c-d-e}$ 来举例：

![](https://cdn.luogu.com.cn/upload/image_hosting/e1guvxbo.webp)

其中形如 $\text{Nx}$ 的点即为原树节点 $\text x$ 在 Top Tree 上的对应点，而下方的以矩形表示的叶子簇记录每一条单边。

这个仅由 Compress Node 和叶子簇节点构成的二叉树称为 **Compress Tree**。

Compress Tree 依然需要满足“其中序遍历符合原树中的深度顺序”这一性质，因此左右儿子是不对称的。
#### 2. Rake Tree
首先我们重新定义 Rake Node 的逻辑。

之前我们对 Rake Node 的定义是一个“中间量”，代表若干个轻子树合并后的结果；而现在，我们依然认为一个 Rake Node 是若干个轻子树的集合, 但是与原先不同的是，它还存储着这个集合中的某一个特定轻子树的信息，不妨称这个特定的轻子树为该集合的“代表元”。

这就意味着现在每一个 Rake Node 都有一个轻子树与之对应，所以 Rake $k$ 个轻子树原先需要 $(2k-1)$ 个节点，现在只需要 $k$ 个，这 $k$ 个节点以任意方法拼成一棵二叉树（当然如果原树的不同儿子之间有顺序限制，你就不能这样，此时你可能甚至不能让 Compress Node 有中儿子，需要换写法），左右儿子不区分，每一个节点所代表的轻子树的集合即为其左右儿子代表的集合以及它自己的并，这个集合的代表元即为它自己。

![[Pasted image 20260911205330.png]]

原树中轻儿子编号 $\text{a,b,c,d,e}$ 分别对应 Rake Tree 上的 $\alpha,\beta,\chi,\delta,\varepsilon$。

例如，$\delta$ 代表的就是 $\text{a,b,c,d,e}$ 的子树构成的集合，而这个集合的“代表元”就是 $\text d$ 的子树；$\chi$ 代表的就是 $\text{a,b,c}$ 的子树构成的集合，而这个集合的“代表元”就是 $\text c$ 的子树。

这个二叉结构被称作 **Rake Tree**。
#### 3. 三度化
注意这是**对 Top Tree 进行三度化**而非对于原树。

有趣的是，对原树三度化的方法，是 Frederickson 在 1985 年及后续论文中提出的原始 Topology Tree。但是其实现非常复杂且效率低下。Alstrup 等人在 1997 年提出的 Top Tree，其设计目标就是提供更简单、更容易处理信息的接口，但是这个方法也并不简洁，所以后面 SATT 才应运而生。

> [!note] 需明确的一点  
> 原始 SATT 为了支持有序邻接表，在概念表示上允许 Compress Node 存在两个 foster child，因此一个节点最多可以有四个孩子。本文只处理普通的无序树（指儿子之间无序），两个方向的轻子树没有顺序要求，因此可以采用单中儿子的三度化表示。这正是后文所使用的结构。

何为三度化？前文所述的静态 Top Tree 是一颗二叉树，现在我们要将其改为三叉树。 

每个节点除了有左右儿子之外，再引入**中儿子**，而 Compress Node 和 Rake Node 的中儿子意义不同：
+ Compress Node 的中儿子若存在，一定是一个 Rake Node, Rake Node 的中儿子若存在，一定是 Compress Node。
+ 一个节点的左右儿子的类型跟它本身类型相同。
+ Compress Node $x$ 的中儿子 $\mu$ 维护原树中节点 $x$ 的全部轻子树构成的集合。
+ Rake Node $\alpha$ 的中儿子 $m$ 维护 $\alpha$ 所维护的集合的代表元的重链。

![[Pasted image 20260915165050.png]]

以上图例中，你会发现，在合并的最后（$\text{cd,Nj}$ Compress 到 $\text{Nc}$），$\text{Nc}$ 已经是一个包含所有边的根簇，但是在其上又补充了一个 $\text{Nk}$，还是一个根簇，看起来有些无用，但实际上这样做的原因其实是为了标定原树的根，否则我们将无法处理有根树的树根信息。

最终我们容易发现，SATT 就是若干棵 Compress Tree 和 Rake Tree 通过中儿子边连接起来的结果。
#### 4. 维护点的 SATT
在代码中，我们也会根据题目的需要去选择 SATT 维护边还是点，例如文章开头给出的例题 Sone1 中我们就使用维护点的 SATT，其实这何刚才所说的维护边的 SATT 并无本质不同，只是此时我们就不再区分叶子簇与 Compress 节点，它们都可以用“$\text{Nx}$”来表示。  

此时的 Compress 的过程也略有分别，维护边的 SATT 中 Compress 是一个二元操作, 且两簇之间有公共点；而维护点的 SATT 中 Compress 是一个**三元操作**：一个 Compress Node $x$ 的左右儿子所维护的簇并无交集，甚至不相邻，而这处于两个簇中间、将它们分离开的节点，正是原树上的 $x$。 

所以在向上合并信息的时候，我们需要合并三个信息，左儿子，右儿子，以及 $x$ 本身（可以认为此处的“$x$ 本身”其实包括 $x$ 的轻儿子，毕竟所有轻儿子已经在先前被 Rake 了）。

至于两个单点之间的合并，就认为左右儿子中有一个是空即可。

把上一节的图例改成维护点的 SATT，毫无疑问这样的实现是更加简洁清晰的，我们甚至**不需要将 SATT 的根节点单独置顶以对应原树根**，原树根就是顶端 Compress Tree 的最左节点。

![[Pasted image 20260915170229.png]]

其实看到这个结构你可以发现，这完全就是把 LCT 并列的虚边改成二叉的 Rake Node 来维护轻子树而已，然而就是这一个简单的操作，使得 SATT 做到了子树修改，因为这个改动使得下传标记能做到 $\mathcal{O}(1)$，树高的复杂度还没有本质改变，SATT 的高明与强大之处正在于此。  

不仅如此，它的思想的可扩展性，也让它能处理相当多 LCT 能力范围之外的问题。
### II. 维护信息
#### 1. 节点信息
以最开始的例题 Sone1 为例，我们需要维护子树信息和路径信息。

```cpp
struct node {
	int son[3], fa, val;
	dat pth, str;
	tag tpth, tstr;
	int rev;
	node() {
		son[0] = son[1] = son[2] = fa = val = 0;
		pth = str = dat();
	}
};
```

$\text{son,fa,val}$ 分别代表节点的三个儿子（左中右对应下标 $0,1,2$），父亲，与节点自己存储的值，显然 Rake Node 的 $\text{val}$ 是无意义的。

注意在节点结构体内部一般不需要规定节点的类型，因为在操作节点时，我们通过给每个函数传参的方法可以得知要操作的节点类型。并且，为了方便，在实践中我们通常给 Compress Node 在 SATT 上编号为 $1\sim n$，恰好对应原树上的 $n$ 个点, 这样编号 $>n$ 的节点就一定是 Rake Node。

$\text{dat,tag}$ 类型分别代表要维护的信息以及修改时作为懒标记的信息，$\text{rev}$ 是左右翻转懒标记，但是不被放在 $\text{tag}$ 类型里，因为它的功能不是维护真正的修改操作，这点我们会在后文提到。

$\text{pth,str}$ 分别存储的是重链信息和轻子树信息：
+ 对于 Compress Node $x$, 其 $\text{pth}$ 存储的是**以 $x$ 为根的 Compress Tree** 的信息总和，由于仅仅包含 Compress Node 的总和信息，所以在原树上的表现就是维护了某条重链中的一段的信息总和，例如对于上一节的示意图，$Nc$ 的 $\text{pth}$ 维护的就是 $cd,Nc,ch,Nh,jh,Nj,jk$ 的信息总和。
+ 对于 Compress Node $x$, 其 $\text{str}$ 存储的是Top Tree 上 $x$ 的 Compress Tree 子树中的所有节点的中子树的信息总和。在原树上的表现是，$x$ 的 $\text{pth}$ **所维护的重链段上全部节点的全部轻子树中所有节点**的信息总和，例如对于上一节的示意图，$Nc$ 的 $\text{str}$ 维护的就是 $\alpha,\delta,\varepsilon$ 的子树中所有节点的信息总和。
+ 对于 Rake Node $\alpha$，其 $\text{pth}$ 没有意义。
+ 对于 Rake Node $\alpha$，其 $\text{str}$ 是 $\alpha$ 在 Top Tree 上的**整棵子树的信息总和**（包括中儿子），在原树上的表现为某一个点的某几棵轻子树（可能是所有，也可能是一个）的信息总和, 例如对于上一节的示意图，$\alpha$ 的 $\text{str}$ 维护的就是 $\alpha,bc,Nb,ab,\beta,ce$ 的信息总和。

对于 $\text{tag}$ 来说，就是把修改标记打在这些位置上。
#### 2. 合并信息
即 `pushup` 的实现。

```cpp
void pushup(int &x, int typ) {
	if (typ)
		str(x) = str(son(x, 0)) * str(son(x, 1)) * str(son(x, 2)) *
		pth(son(x, 2));
	else {
		str(x) = str(son(x, 0)) * str(son(x, 1)) * str(son(x, 2));
		pth(x) = pth(son(x, 0)) * pth(son(x, 1)) *
		dat(1, val(x), val(x), val(x));
	}
}
```

$\text{typ}$ 为 $x$ 的类型，值为 $0$ 表示 $x$ 是 Compress Node，为 $1$ 表示 $x$ 是 Rake Node。

乘号是重载运算符，用来表示两个 $\text{dat}$ 之间的运算。

对于 Rake Node，只有其子树信息需要维护，根据定义，它的整棵子树都需要被包括在内，所以需要左中右儿子的信息都需要合并。由于 Rake Node 的中儿子只能是 Compress Node，又 Compress Node 的 $\text{str}$ 是仅包括其 Compress Tree 子树中所有点中儿子，所以还需要加上 $x$ 中儿子的 $\text{pth}$ 以补上这个中儿子的 Compress Tree 子树中节点的信息。

对于 Compress Node $x$，$\text{str}$ 直接合并左中右儿子即可，左右儿子相当于“递归处理”，中儿子是 Rake Node，所以这个中儿子的 $\text{str}$ 即为 $x$ 的整棵中子树，符合定义。

而 Compress Node 对于 $\text{pth}$ 的维护恰好体现了维护点的 SATT 的 Compress 是三元合并的特点，除了要合并左右儿子的信息，还需要合并自己这个单点的信息，这样才能把左右儿子完全接上。
#### 3. 下传标记
先考虑，一个点被打上了一个 $\text{tpth}$ 或 $\text{tstr}$ 的标记 (Tag of PaTH, Tag of SubTRee) 之后，单单这个点本身（不考虑儿子）的各项值的变化。注意此处 $\text{tpth}$ 和 $\text{tstr}$ 的标记不等价于原问题中的路径修改和子树修改，只有后文我们讲到将树旋转以及重划轻重边之后，路径修改和子树修改才能归约到 $\text{tpth}$ 和 $\text{tstr}$ 的标记的改变。而现在，这两个标记所做的事情，就是给这个节点的 $\text{pth}$ 和 $\text{str}$ 所管辖的范围（前文已经讨论过这些范围是什么）中的全部节点打上标记而已。 

```cpp
void ptht(int x, tag t) {
	val(x) = val(x) * t;
	tpth(x) = tpth(x) * t;
	pth(x) = pth(x) * t;
}
void strt(int x, tag t) {
	tstr(x) = tstr(x) * t;
	str(x) = str(x) * t;
}
```

这两个函数即为给 Top Tree 上节点 $x$ 打上标记时他本身的变化，其实理解起来是平凡的，唯一要注意的就是根据 Compress 的三元合并性质，在打上路径标记的时候，其本身的 $\text{val}$ 值也会发生改变。

有了这两个“单点维护”函数后，`pushdown` 的逻辑其实就是 `pushup` 反过来。

```cpp
void pushdown(int &x, int typ) {
	if (typ) {
		if (!tstr(x).empty()) {
			strt(son(x, 0), tstr(x));
			strt(son(x, 1), tstr(x));
			strt(son(x, 2), tstr(x));
			ptht(son(x, 2), tstr(x));
			tstr(x) = tag();
		}
	} else {
		if (rev(x)) {
			trev(son(x, 0));
			trev(son(x, 1));
			rev(x) = 0;
		}
		if (!tpth(x).empty()) {
			ptht(son(x, 0), tpth(x));
			ptht(son(x, 1), tpth(x));
			tpth(x) = tag();
		}
		if (!tstr(x).empty()) {
			strt(son(x, 0), tstr(x));
			strt(son(x, 1), tstr(x));
			strt(son(x, 2), tstr(x));
			tstr(x) = tag();
		}
	}
}
```

中间多了那个对于 $\text{rev}$ 标记的维护，发现它干的事情其实就是翻转左右儿子，那不难想到给整棵 Compress 子树打的 $\text{rev}$ 标记就是翻转子树内每个节点的左右儿子关系，从而使该子树的中序遍历完全翻转。

如果你学过 LCT，你就可以知道这一步其实是为换根做准备，我们后面会提到。
#### 4. 递归下传
由于在我们实现的 SATT 中，Compress Node 和原树节点编号相同，所以每次都可以精准定位一个 Compress Node。然而，如果要从这个 Compress Node 向上跳，而它的祖先的标记还都没下传，这是不对的，因此我们需要一个从根开始精准地在一条根链做下传标记直到某个节点的操作。

实现是容易的。

```cpp
void pdrt(int x, int typ) {
	if (!isrt(x))
		pdrt(fa(x), typ);
	pushdown(x, typ);
}
```
### III. Splay 相关操作
Splay 操作是 LCT 保证复杂度以及操作简便性的核心，在 SATT 中也不例外。

这里不过多赘述 Splay 的具体旋转方法，如果有忘记可参考[我的 LCT 笔记相关部分](https://www.luogu.com.cn/article/nmwplwa3)。
#### 1. 单旋  

```cpp
bool dir(int x) { return x == son(fa(x), 1); }
void rotate(int x, int typ) {
	int f = fa(x), g = fa(f), lr = dir(x), y = son(x, lr ^ 1);
	if (g)
		son(g, son(g, 2) == f ? 2 : dir(f)) = x;
	son(x, lr ^ 1) = f;
	son(f, lr) = y;
	if (y)
		fa(y) = f;
	fa(f) = x;
	fa(x) = g;
	pushup(f, typ);
	pushup(x, typ);
}
```

与正常旋转的差异在于对于中儿子的处理，毕竟这里旋转的时候仅仅操作的是 Compress Tree，与中儿子的边是不会有影响的。
#### 2. 双旋

```cpp
bool isrt(int x) { return (x != son(fa(x), 0)) && (x != son(fa(x), 1)); }
void splay(int x, int typ, int goal = 0) {
	pdrt(x, typ);
	for (int i; i = fa(x), !isrt(x) && i != goal; rotate(x, typ))
		if (fa(i) != goal && !isrt(i))
			rotate(dir(x) ^ dir(i) ? x : i, typ);
}
```

注意这里对于是否为 Compress Tree 根的判定，一个点如果是其父亲的中儿子，再往上跳相当于走了一条“虚边”，就不属于该 Compress Tree 内了。

Splay 前需要把当前节点所在的局部 Splay 树根到 $x$ 的路径上的标记全部下传。由于跨越中儿子时，父节点上的标记已经在此前下传过程中被转移到对应的中儿子根，因此这里不需要继续递归到整个 Top Tree 根。

为了方便以后的操作，需要设立一个哨兵节点 $\text{goal}$ 以处理有时只需旋转到特定节点的情况。

由于 Splay 操作不改变中序遍历的性质，它不会使得原树的树形产生任何变化。

### IV. Access 相关操作
`access(x)` 将 SATT 上的节点 $x$ 旋转到整个 SATT 的根节点，不改变原树连边结构，只改变原树的轻重边划分方式，使得原树上节点 $x$ 与原树根处于同一重链上。

我们考虑先把 $x$ 到 SATT 根的路径“打通”，使得 $x$ 和 SATT 根处于同一个 Compress Tree 中，然后再执行一次 Splay 把 $x$ 转到根部。
#### 1. Splice
Splay 操作只能在同一棵 Compress Tree 内实现，所以 Splice 的作用就是，将两个靠  Rake Tree 接在一起的 Compress Tree 打通。  

打通后产生的新的 Compress Tree 显然不一定会包含原先两棵 Compress Tree 的全部节点，不过这没有影响，我们仅仅关心那个要旋转的点 $x$ 是否被新的 Compress Tree 囊括进去。  

例如，如果我们想要打通 Rake Node $\alpha$ 所在 Rake Tree 下方的 Compress Tree 和上方的 Compress Tree：
1. 先将 $\alpha$ Splay 到其所在 Rake Tree 的根，此时 $\alpha$ 的父亲 $f$ 一定是一个 Compress Node，中儿子为 $\alpha$。
2. 将 $f$ Splay 到其所在 Compress Tree 的根。
3. 这一步是核心：
	+ 如果 $f$ 有右儿子，那就让 $f$ 的右儿子跟 $\alpha$ 中儿子互换，这样 $\alpha$ 下面的这棵 Compress Tree 就被调到上面去了。
	+ 如果 $f$ 没有右儿子，那直接把 $\alpha$ 的中儿子接到 $f$ 的右儿子处即可。然而此时 $\alpha$ 没有中儿子了，那这个 Rake Node 就没什么本质作用了，可以把它删掉, 具体怎么删掉一个 Rake Node 需要 Delete 操作，下一节将会进行说明。

对于第 3 步，第一种情况其实就是更改原树上节点 $f$ 的重儿子，第二种情况其实就是原树上 $f$ 根本没有重儿子，于是直接设置一个重儿子。

```cpp
void splice(int x) {
	splay(x, 1);
	int g = fa(x);
	splay(g, 0);
	pushdown(x, 1);
	if (son(g, 1)) {
		swap(fa(son(x, 2)), fa(son(g, 1)));
		swap(son(x, 2), son(g, 1));
	} else {
		setf(son(x, 2), fa(x), 1);
		del(x);
	}
	pushup(x, 1);
	pushup(g, 0);
}
```

其实 1、2 两步叫作 Local Splay，第 3 步才是真正的 Splice 操作，但是方便起见都算作一起了。
#### 2. Delete
删除的逻辑很简单：如果 $\alpha$ 有左子树，那就在其所在 Rake Tree 上找到它左子树的最右节点，也就是 $\alpha$ 的前驱，然后将该前驱 $\beta$ Splay 到 $\alpha$ 的左儿子位置，此时 $\beta$ 没有右儿子，那就直接把 $\alpha$ 的右子树接到 $\beta$ 右儿子处，这样 $\alpha$ 就没有右子树了，我们直接删掉这个节点，然后让 $\beta$ 向 $f$ 连边，顶替它的位置即可。如果 $\alpha$ 没有左子树，那更简单了，直接删掉 $\alpha$，用它的右儿子接替它的位置即可。

```cpp
void setf(int x, int f, int typ) {
	if (x) fa(x) = f;
	son(f, typ) = x;
}
void del(int x) {
	if (son(x, 0)) {
		int ls = son(x, 0);
		pushdown(ls, 1);
		while (son(ls, 1)) {
			ls = son(ls, 1);
			pushdown(ls, 1);
		}
		splay(ls, 1, x);
		setf(son(x, 1), ls, 1);
		setf(ls, fa(x), 2);
		pushup(ls, 1);
		pushup(fa(x), 0);
	} else
		setf(son(x, 1), fa(x), 2);
	clear(x);
}
```

#### 3. Access
我们希望 $x$ 在 Access 完成后无重儿子，即**为重链底部节点**。而 $x$ 在最后又是 SATT 的根，因此它显然最终没有右儿子（注意前面我们说过用 SATT 维护树时原树上任一个点都可以没有重儿子，因此重链底部节点不一定是叶子）。

先将 $x$ Splay 到局部 Compress Tree 的根，然后看它有没有右儿子，如果没有，那这就是预期效果，如果有，我们应当直接把其右儿子断掉，并在 $x$ 正下方（也就是其中儿子所在的）的 Rake Tree 中新建立一个 Rake Node 来拼接它与它被断掉的右儿子。

右儿子断开之后，考虑 $x$ 的父亲，由于 $x$ 已是其 Compress Tree 的根，故 $x$ 的父亲 $\alpha$ 必定是一个 Rake Node，所以可以对 $\alpha$ 进行一次 Splice 操作。

此时 $x$ 所在的 Compress Tree 就跟 $\alpha$ 上面的那个 Compress Tree 打通了。由于 Splice 中有一个 `splay(g, 0)` 的操作，所以此时打通之后，$x$ 现在的父亲，即 $g$，就是目前 Compress Tree 的根。于是当前处理的位置往上跳到 $g$，让 $g$ 的父亲再 Splice，再向上跳，以此类推，直到完全打通到根节点。  

注意，这个循环过程中，一直都是 Rake Node 在执行 Splice，且向上跳的是“当前处理的指针”，真正要操作的 $x$ 还在最底下没有动，所以我们要执行一次 Global Splay，由于此时 $x$ 已经和 SATT 的根在同一 Compress Tree 内，所以 Splay 会使得其直接转到根。

由于 $x$ 的右子树已被切断，且每次 Splice 的时候，下面那棵 Compress  Tree 都是往上面 Compress Node 的右儿子处接的，所以打通之后 $x$ 就自然是整个 Compress Tree 中最右的一个，即原树中那条“根重链”最底端的节点。

```cpp
void access(int x) {
        splay(x, 0);
        int y = x;
        if (son(x, 1)) {
            int nd = newnode();
            setf(son(x, 2), nd, 0);
            setf(son(x, 1), nd, 2);
            son(x, 1) = 0;
            setf(nd, x, 2);
            pushup(nd, 1);
            pushup(x, 0);
        }
        while (fa(x)) {
            splice(fa(x));
            x = fa(x);
            pushup(x, 0);
        }
        splay(y, 0);
    }
```

我们其实也可以不在最后才进行 Global Splay，而是每次 Splice 之后就将 $x$ 向上 Splay 一下，这样循环结束后 $x$ 自然直接成为根。

在实践中这种方法常数更小，甚至比最终 Global Splay 的方法总时长快了近 $400\mathrm{ms}$，但是由于前面的方法在复杂度的严格证明上更加简洁，所以原论文中使用的是前面的做法。  

```cpp
void access(int x) { // 优化版
        splay(x, 0);
        int y = x;
        if (son(x, 1)) {
            int nd = newnode();
            setf(son(x, 2), nd, 0);
            setf(son(x, 1), nd, 2);
            son(x, 1) = 0;
            setf(nd, x, 2);
            pushup(nd, 1);
            pushup(x, 0);
        }
        while (fa(x)) {
            splice(fa(x));
            // x = fa(x);
            splay(x, 0); // +++
            pushup(x, 0);
        }
        // splay(y, 0);
    }
```

实际代码中只有三行的变化。
### V. 更改原树结构
#### 1. 换根
将原树看成一个有向树（内向外向随意）后，容易观察出将 $x$ 换为根的本质其实就是把从 $x$ 到旧根的路径上的所有边反向。

又由于 Compress Tree 的中序遍历反映了重链的深度顺序，所以将某个 Compress Tree 中所有点的左右儿子调换，其中序遍历就会完全翻转，从而这条重链的深度顺序完全翻转。 

因此，先将 $x$ 执行一次 Access，此时 $x$ 所在的 Compress Tree 恰好就完全代表了 $x$ 到现在的根的路径。将这棵 Compress Tree 中所有点的左右儿子调换，就将 $x$ 到根的这条链的深度顺序完全翻转，从而 $x$ 就变成了原树的根。

整棵子树翻转左右儿子的操作，就交给前面所说的 $\text{rev}$ 标记来实现。

Makeroot 的代码极其好写。

```cpp
void trev(int x) {
	swap(son(x, 0), son(x, 1));
	rev(x) ^= 1;
}
void makeroot(int x) {
	access(x);
	trev(x);
}
```
#### 2. Expose
Expose 操作的目的更加简单，`expose(x,y)` 将 $x$ 到 $y$ 的路径单独剖成一棵 Compress Tree，即一条重链，不要求 $x,y$ 具有祖先与后代的关系。  

这个操作其实就是为了辅助链上操作，它将链操作转化成了一棵 Compress Tree 的整体操作，实现也很简单，先换根为 $x$，再对 $y$ 执行 Access 即可。

理解和实现都是平凡的。

```cpp
void expose(int x, int y) {
	makeroot(x);
	access(y);
}
```

注意由于链操作不改变原树结构，所以 Expose 并修改/查询信息后，应当将根换回来。
#### 3. Link/Cut
Link 操作连接两个不连通的点，新增一条边; Cut 操作删除一条边。注意单纯的 Link/Cut 操作是针对**无根树**的。

对于 Link 操作，我们先对 $x$ 执行 Access，使得 $x$ 在 SATT 上没有右儿子。然后将 $y$ 换为其所在树根，直接在 SATT 上将 $y$ 接到 $x$ 右儿子处即可。

```cpp
void link(int x, int y) {
	access(x);
	makeroot(y);
	setf(y, x, 1);
	pushup(x, 0);
}
```

对于 Cut 操作，先 Expose 这两个点形成一棵仅有两点的 Compress Tree，然后在 SATT 上断开唯一一条边即可。

```cpp
void cut(int x, int y) {
	expose(x, y);
	clear(son(x, 1));
	fa(x) = son(y, 0) = 0;
	pushup(y, 0);
}
```

但是由于题目维护的是有根树，所以在题目中 Link 操作为把 $x$ 的父亲换为 $y$ 的操作，从而我们需要先得知 $x$ 的原树父亲是谁。

Access 了 $x$ 之后，其在原树上的父亲就是其所在 Compress Tree 的前驱的对应节点。

```cpp
int cutfa(int x) {
	access(x);
	int y = son(x, 0);
	while (son(y, 1))
		pushdown(y, 0), y = son(y, 1);
	son(x, 0) = fa(son(x, 0)) = 0;
	pushup(x, 0);
	return y;
}
```

该函数同时实现了切断原树中 $x$ 与其父亲的关系和返回其父亲编号。  

对于更改父亲操作，也就极其容易了。

```cpp
int findrt(int x) {
	access(x);
	while (son(x, 0)) {
		pushdown(x, 0);
		x = son(x, 0);
	}
	splay(x, 0);
	return x;
}
 void changefa(int x, int y) {
	if (x == y || x == rt)
		return;
	int z = cutfa(x);
	if (findrt(x) == findrt(y))
		link(x, z);
	else
		link(x, y);
	makeroot(rt);
}
```

注意上面的 Findrt 操作，其实就是找到 $x$ 所在原树的根，用来辅助判断两点是否连通，`cutfa(x)` 后，$x$ 与原树剩余部分暂时分成两个连通块。若 $y$ 位于原来 $x$ 的子树中，那么 $y$ 会与 $x$ 处于同一个连通块；此时不能直接 `link(x,y)`，否则会成环，因此必须恢复原边 `(x,z)`。反之 $y$ 位于 $x$ 的子树之外，则直接把 $x$ 连接到 $y$ 即可。

Findrt 实现逻辑就是 Access 后找到顶端 Compress Tree 的最左节点。

### VI. 查询与修改
其实有了前面的大量铺垫，这一块是呼之欲出的。
#### 1. 路径操作
Expose 之后为该路径对应的 Compress Tree 根打上路径标记或返回它的路径信息即可。

```cpp
dat qpth(int x, int y) {
	expose(x, y);
	dat ans = pth(y);
	makeroot(rt);
	return ans;
}
void upth(int x, int y, tag t) {
	expose(x, y);
	ptht(y, t);
	makeroot(rt);
}
```

#### 2. 子树操作
将 $x$ Access 之后，其只有轻子树，故其子树信息即为其在 SATT 上中子树以及它自己的信息总和，这样就完全涵盖了以 $x$ 为根的整棵子树。

```cpp
dat qstr(int x) {
	access(x);
	dat ans = str(son(x, 2)) * dat(1, val(x), val(x), val(x));
	makeroot(rt);
	return ans;
}
void ustr(int x, tag t) {
	access(x);
	strt(son(x, 2), t);
	val(x) = val(x) * t;
	pushup(x, 0);
	makeroot(rt);
}维护
```

### VII. 完整代码
在[云剪贴板](https://www.luogu.com.cn/paste/pg8oru9s)中查看。
## 5. 仙人掌
### I. 静态仙人掌
后文中“仙人掌”均指边仙人掌，即所有边都最多出现在一个简单环中的无向连通图。

如果一个无向图的每个连通块都是个仙人掌，且不存在自环，我们就称之为沙漠或荒漠。
#### 1. Twist
如果直接对着一棵仙人掌进行 Compress 和 Rake 的收缩操作，你会发现那些环最终都被 Compress 成了两个点之间有两条重（chóng）边。 

Compress 和 Rake 均无法处理这种情况，故我们引入一种新的操作：Twist，它的作用是将两条重边并为一条。

![](https://pic.imgdb.cn/item/6573d3dac458853aef160b20.png)

#### 2. 环的收缩
有了 Twist，静态仙人掌的收缩过程就和树一样了：把环的两个界点之间的两条路径分别 Compress 成两个簇，再用一次 Twist 把它们并成一个环簇，于是"带环的路径"又变成了一条普通路径，可以继续参与后面的 Compress/Rake。

但这里有一个顺序上的约束：

> [!note] Twist 之前必须先 Rake
> Twist 的两个儿子必须是"干净的路径"。如果环上的点还挂着环外的子树，这些子树必须在 Twist **之前**就 Rake 到环上，因为 Twist 结点只有两个孩子，两条路径之间不能夹着别的边。
> negiizhao 的原话是："如果要维护边的顺序，还需要处理环的两个界点在环内的子树。类似 compress，这可以在 twist 之前进行 rake，并成为 twist 结点的两个 foster child。然而，并不能 expose 环内和环外的顶点，因为 twist 的两条边之间不能有其他边。"

这也是仙人掌 Top Tree 和树的 Top Tree 在结构上唯一需要额外小心的地方：Rake 与 Twist 有先后依赖——先把旁支收拾干净，再 Twist。

#### 3. 静态仙人掌簇的信息
对于"两点间最短路"这一类查询，一个簇需要记录的东西其实很少：两个界点 $u,v$、它们之间最短路的长度 $d$、以及簇内点的聚合量。三种合并分别做：

+ **Compress**：两段路径首尾相接，$d=d_a+d_b$；
+ **Rake**：$d$ 不变，只把旁支的聚合量并进来；
+ **Twist**：$d=\min(d_a,d_b)$，并在 $d_a=d_b$ 时打上一个"最短路不唯一"的标记。

最后这个标记必须一路往上传：如果最终包含 $u,v$ 的那个簇被打上了标记，就说明 $u,v$ 之间的最短路不唯一，查询应该直接返回 $-2$。这正是动态版本里 $flag$ 字段的来源。

### II. 动态仙人掌
#### 1. 从树到仙人掌
前面的 SATT 只能维护树，想搬到仙人掌上，唯一的额外困难就是环：树上两点之间的路径唯一，所以在 Compress 时我们从不担心"该往哪边走"；而仙人掌上同一个环的两个点之间有两条路径，路径查询必须在环上二选一。也就是说，仙人掌相对于树**只多出一种东西**：

> 存在一对端点，它们之间有不止一条路径。

根据边仙人掌的性质（每条边至多属于一个简单环），把每个环缩成一个点之后，仙人掌就变成一棵树，也就是块割点树。这说明仙人掌上的任意一条简单路径，都是在块割点树的路径上"每经过一个环时在该环的两条路径里选一条"。于是除了环那一层需要多做一次决策，其余部分与树完全一样。

回忆环上的情形：设环上两个界点是 $u,v$，它们把环分成两条边不相交的路径 $P_1,P_2$。显然 $d(P_1)$ 和 $d(P_2)$ 中较小的一条就是 $u\to v$ 的最短路；若二者相等，则最短路不唯一。

> [!note] Twist
> negiizhao 在《用于仙人掌的 top trees》中的说法是：树收缩是不够用的，因为解决不了环的问题；于是需要引入第三种收缩操作 **Twist**，把两条端点相同的边合并为一条。相应地，Top Tree 中会多出一种结点：**Twist 结点，它的两个孩子就是原来的那两条边**。
> 这里的"两条边"其实是两个**簇**：环的两个端点把环分成两条边不相交的路径，两条路径分别用一棵 Compress Tree 表示（每个簇的两个界点就是环的两个端点），再把它们 Twist 成一条"边"。这样一条"带环的路径"就被收缩为一条普通路径，继续用 Compress Tree 维护即可。

![[Cactus-Twist.png]]

上图左边就是一个最简单的环：$u,v$ 把环分成 $u-a-b-v$ 与 $u-c-v$ 两条路径；中间是它们各自的 Compress Tree 表示；右边用一个 Twist 结点把两个簇并成一个环簇，它在整条"带环路径"里扮演一条权值为 $\min(d_1,d_2)$ 的边。

再加一句在本实现里很关键的话：**Twist 的两条路径之间不能再夹其它边**。因为 Twist 结点只有两个孩子，如果一个顶点在环内还挂着别的子树，这些子树必须在 Twist 之前就 Rake 起来挂到环上。这一点在后文的 `retwist` 里会直接看到。

本文的实现按 negiizhao 提示的简化版来做：**点式 + 三叉树**。

+ 不维护边在点周围的顺序，所以 Twist 结点只需要两个儿子（不需要 foster child）；
+ 用"Compress Node 对应原树顶点"的点式实现，于是簇的信息天然带上了界点的点权；
+ 仍然共用一片数组存 Splay 森林，靠一个 `type` 字段区分四类结点。

#### 2. 四类结点
代码给每个结点存了一个 `type`，一共四种：

| 符号 | 名字 | 对应原图的东西 |
| --- | --- | --- |
| `c` | Compress Node | 原图的一个顶点（点权存在 `val` 里） |
| `b` | base 结点 / 叶簇 | 原图的一条边（`dis` 就是边权） |
| `r` | Rake Node | 一组轻子树（旁支） |
| `t` | Twist Node | 一个环（两个孩子是环的两条路径） |

![[Cactus-Structure.png]]

+ `c` 结点是主链（Compress Tree）上的结点，同时是原图顶点的代表，`val` 是它的点权。它有三个儿子槽位：`ls/rs` 是簇的两端，`ms` 是挂在这个顶点上的轻子树集合（Rake Tree 的根）。
+ `b` 结点是**叶子**，代表"恰好一条边"的簇。它的 `psiz=0`、`tsiz=0`，只提供两个界点与长度：一条边本身不含内部点。
+ `r` 结点与前一节的 Rake Node 语义完全一致：中儿子是**代表元**（一棵 Compress Tree，代表某条轻子树所在的重链），左右儿子是其余轻子树。`pushup` 时先把自己中儿子的整条路径 `move` 成旁支信息，再把左右儿子 rake 进来。
+ `t` 结点是一个环，两个儿子是环上两条路径对应的簇，界点相同，`cir=1`。

顺带说明一个设计：界点并不单独存储。每个 `b` 结点自带两个端点，Compress 时左簇的 $u$ 与右簇的 $v$ 拼成新的界点对，于是信息一路合并下来，一个簇的界点自然就是它两端 `b` 结点的端点。

> [!note] `isrt` 的含义
> 这份代码里 `isrt(x)` 写的是 `type(x) != type(fa(x))`，它并不是在判断"$x$ 是不是左右儿子"，而是在判断**这条父子边是不是 Splay 边**。类型相同的父子关系才是 Splay 树内部的边；类型不同的父子关系（比如 `c` 的父亲是 `r`，或者 `c` 的儿子是 `b`、`t`）相当于 LCT 里的虚边，属于 Top Tree 的结构性连边，不参与旋转。
> 正因如此，`c/b/r/t` 才能共用同一个数组：`pushup` 对 `son[0]/son[1]` 一视同仁地当作"簇的两端"，信息既能沿 Splay 边流动，也能沿结构边流动。
> 在这份实现中，只有 `c-c` 与 `r-r` 之间会出现 Splay 边，`b`、`t` 在 Splay 意义下永远是叶子（这一条是对随机数据实测得到的）。

#### 3. 簇的信息与四种运算
一个簇 $C$ 需要记录这些东西：
$$F(C)=(u,v,d,P,T,flag)$$

+ $u,v$：簇的两个界点；
+ $d$：两个界点之间最短路（也就是簇路径）的长度；
+ $P$：簇路径上的点集，用 `psiz,pmin,psum` 表示；
+ $T$：簇内不在簇路径上的点集，用 `tsiz,tmin,tsum` 表示；
+ $flag$：最短路是否不唯一。

在实现里 $P$ 是这样攒出来的：Compress Tree 子树里的每个 `c` 结点恰好贡献一次自己的 `val`（`compress` 的第三个参数或单点簇的 `insert`），而一个簇的两个界点正好是这棵子树里最外层的 `c` 结点，所以它们也被算在 $P$ 里。$T$ 则是"挂在这些结点上的所有旁支，加上环上被舍弃的那条路"。于是恒有 $P\cup T$ 等于簇内所有点、$P\cap T=\varnothing$——这解释了为什么只需要维护两套聚合量：`query1/add1` 只看 $P$，`query2/add2` 只看 $T$（配合 `ms`），两边几乎天然对应。

![[Cactus-Merge.png]]

四种运算就是簇上的四则运算：

```cpp
dat compress(const dat &a, const dat &b, int w) {
    return {a.u, b.v, a.dis + b.dis,
            a.tsiz + b.tsiz, min(a.tmin, b.tmin), a.tsum + b.tsum,
            a.psiz + b.psiz + 1, min({a.pmin, b.pmin, w}),
            a.psum + b.psum + w, a.flag || b.flag};
}
dat rake(const dat &a, const dat &b) {
    return {a.u, a.v, a.dis,
            a.tsiz + b.tsiz, min(a.tmin, b.tmin), a.tsum + b.tsum,
            a.psiz, a.pmin, a.psum, a.flag};
}
dat twist(dat a, dat b) {
    if (a.dis > b.dis)
        swap(a, b);
    if (a.dis == b.dis)
        return {a.u, a.v, a.dis,
                a.tsiz + b.tsiz, min(a.tmin, b.tmin), a.tsum + b.tsum,
                a.psiz + b.psiz, min(a.pmin, b.pmin), a.psum + b.psum, 1};
    b = b.move();
    return {a.u, a.v, a.dis,
            a.tsiz + b.tsiz, min(a.tmin, b.tmin), a.tsum + b.tsum,
            a.psiz, a.pmin, a.psum, a.flag};
}
dat move() const {
    return {u, v, dis, tsiz + psiz, min(tmin, pmin), tsum + psum,
            0, MAX, 0, flag};
}
```

+ **Compress**：两段首尾相接的路径拼起来，$w$ 是中间那个顶点。$d=d_a+d_b$，$P=P_a\cup P_b\cup\{w\}$，$T=T_a\cup T_b$，$flag=flag_a\lor flag_b$。
+ **Rake**：把旁支 $b$ 挂到主链 $a$ 上。路径完全不变（$u,v,d,P$ 全取 $a$），$T=T_a\cup T_b$。
+ **Twist**：两个界点相同的簇合成一个环。若 $d_a>d_b$ 就交换，于是 $a$ 是较短的那条；$d_a=d_b$ 时两条都是最短路，$flag=1$；否则 $d=d_a$、$P=P_a$，而另一条整条路径都变成旁支，对它做一次 `move` 即可：$T=T_a\cup T_b\cup P_b$。
+ **Move**：$P\to T$，把"原本在路径上的东西"整体变成"旁支"，即把 $p$ 三个量并进 $t$ 三个量后清空 $p$。

> [!note] 为什么 rake 里不合并 $P$
> 代码里 `rake(a,b)` 完全没有把 $b$ 的 $P$ 加进去，这不是漏写：`rake` 的第二个参数只可能是 `r` 结点（`pushup` 中写作 `rake(f(rs(x)), f(ms(x)))`），而 `r` 结点在 `pushup` 的第一步就把中儿子的路径 `move` 掉了，所以它的 $P$ 恒为空（`psiz=0, pmin=INF`）。
> 同理，`rake` 的结果 `flag` 只取 `a.flag`：$b$ 内部的最短路是否唯一，与当前簇路径是否唯一无关。

#### 4. pushup
三种结点的 `pushup` 就是把上面的运算按层次拼起来。

```cpp
void pushup(int x) {
    if (type(x) == 'c') {
        if (ls(x) && rs(x)) {
            if (ms(x))
                f(x) = compress(f(ls(x)), rake(f(rs(x)), f(ms(x))), val(x));
            else
                f(x) = compress(f(ls(x)), f(rs(x)), val(x));
        } else {
            if (ls(x) || rs(x))
                f(x) = f(ls(x) | rs(x));
            else
                f(x) = {x, x, 0, 0, MAX, 0, 0, MAX, 0, 0};
            f(x).insert(val(x));
            if (ms(x))
                f(x) = rake(f(x), f(ms(x)));
        }
        cir(x) = cir(ls(x)) | cir(rs(x));
    } else if (type(x) == 'r') {
        f(x) = f(ms(x)).move();
        if (ls(x))
            f(x) = rake(f(x), f(ls(x)));
        if (rs(x))
            f(x) = rake(f(x), f(rs(x)));
    } else if (type(x) == 't') {
        f(x) = twist(f(ls(x)), f(rs(x)));
        cir(x) = 1;
    }
}
```

+ `c`：左右儿子都在时，先把中儿子 rake 到右儿子上（也可以 rake 到左儿子上，结果一样），再与左儿子、以及自己 `val` 做 Compress；只有一个儿子时用 `f(ls(x)|rs(x))` 这种写法借空结点 0 省掉一次判断，然后 `insert` 自己；一个儿子都没有就是单点簇。最后 `cir` 沿主链取或。
+ `r`：中儿子是代表元的 Compress Tree，它的路径现在整体变成旁支，所以先 `move`，再把左右儿子 rake 进来。注意 `r` 结点自身不提供任何点，`val` 对它没有意义。
+ `t`：两个儿子就是环的两条路径，直接 `twist`，并标记 `cir=1`。

其中 `cir` 表示"这个簇里是否含环"，只在 `link` 判合法性时用到。

#### 5. 懒标记与 pushdown
懒标记有两个维度：

```cpp
struct tag {
    int tlaz, plaz;
    tag tree() { return {tlaz, 0}; }
    tag move() { return {tlaz, tlaz}; }
    friend tag operator*(const tag &x, const tag &y) {
        return {x.tlaz + y.tlaz, x.plaz + y.plaz};
    }
};
```

`tlaz` 加到簇内**所有**点上（对应 `add2`），`plaz` 只加到**簇路径上**的点（对应 `add1`）。

为什么必须分成两维？对一个环簇来说，"所有点"和"最短路上的点"不是同一个集合：若环的两条路径长度不等，只有较短的那条在 $P$ 里，但两条都在 $T$ 里。子树加要给两条都加，最短路加只能加较短的那条，一个标记没办法同时表达这两种意思。

标记作用到 `dat` 上由重载的 `dat * tag` 完成：

```cpp
friend dat operator*(const dat &x, const tag &y) {
    return {x.u, x.v, x.dis,
            x.tsiz, x.tmin == MAX ? MAX : x.tmin + y.tlaz,
            x.tsum + 1ll * x.tsiz * y.tlaz,
            x.psiz, x.pmin == MAX ? MAX : x.pmin + y.plaz,
            x.psum + 1ll * x.psiz * y.plaz, x.flag};
}
```

`pushlaz` 除了更新 `dat`，还要同步维护真正的点权：`if (type(x) == 'c') val(x) += y.plaz;`。因为 `f` 只是摘要，`val` 才是原图的点权，不一起改的话下次 `pushup` 会把它按旧值重新算进去。

![[Cactus-Pushdown.png]]

`pushdown` 按结点类型分流：

```cpp
if (type(x) == 'c') {
    pushlaz(ls(x), laz(x));
    pushlaz(rs(x), laz(x));
    pushlaz(ms(x), laz(x).tree());
} else if (type(x) == 'r') {
    int w = laz(x).tlaz;
    for (int c = 0; c < 3; c++)
        pushlaz(son(x, c), {w, 0});
    pushlaz(ms(x), {0, w});
} else if (type(x) == 't') {
    int w = f(ls(x)).dis - f(rs(x)).dis;
    pushlaz(ls(x), w <= 0 ? laz(x) : laz(x).move());
    pushlaz(rs(x), w >= 0 ? laz(x) : laz(x).move());
}
```

+ `c`：左右儿子都在簇路径上，拿走完整标记；中儿子是整棵 Rake Tree，整块都属于旁支，所以只能拿走 `laz.tree()`，也就是只保留 `tlaz`。
+ `r`：Rake 结点整体都是旁支，所以 `tlaz` 要发给所有儿子；但它的**中儿子是一条路径**，对这一份 `tlaz` 来说，中儿子既要加到树信息、又要加到路径信息，所以中儿子除 `{w,0}` 之外还要额外收到 `{0,w}`。这就是 `r` 的 `pushdown` 看起来像重复发了一次标记的原因。
+ `t`：环上的点归 $P$ 还是 $T$ 要看哪条路径短。代码用 $w=f(ls).dis-f(rs).dis$ 判断：
  + $w<0$：左儿子是簇路径，拿完整标记；右儿子整条都是旁支，拿 `laz.move()`，把 `plaz` 全部转成 `tlaz`；
  + $w>0$：对称；
  + $w=0$：两条都是最短路，两边都拿完整标记（`w<=0` 与 `w>=0` 同时成立）。

  这三个分支与 `twist` 里的比较是同一件事在两个位置的体现。

#### 6. access、splice 与 retwist
`access(x)` 与 LCT 几乎一样：先把 $x$ 旋到根，把它原来的右儿子（更深的那些点）连同中儿子打包成一个新的 Rake 结点挂回去，然后不断 `splice` 向上。差别只在循环内部：

```cpp
for (; splay(x), fa(x);) {
    if (type(fa(x)) == 'r')
        splice(fa(x));
    else
        retwist(x);
}
```

父亲是 `r`，说明要跨过一棵 Rake Tree，走普通的 `splice`；父亲是 `t`，说明路径要穿过一个环，走 `retwist`。

`splice` 就是前一节 SATT 的 Splice：拆掉一个 Rake 结构，让路径从上面那棵 Compress Tree 继续延伸。核心是三行：

```cpp
int a = ms(x), b = rs(y);
setf(a, y, 1);
setf(b, x, 2);
pushup(x);
```

把 $x$ 的中儿子接到 $y$ 的右儿子（让 $y$ 多一个"重儿子"），再把 $y$ 原来的右儿子丢进 $x$ 的中儿子。它改变的是"重链怎么划"，不是原图。

`retwist` 是整份代码最难读的部分，但它做的事一句话就能概括：

> **splice 到环上会改变环的两个端点，于是原来的 Twist 分解失效，必须把 Twist 拆开重新组织。**

![[Cactus-Retwist.png]]

对应 negiizhao 的原话是："这种情形会改变环的端点，与非环上的 splice 相比，我们还需要先对环的两个原端点进行 local splay。"代码里

```cpp
int a = f(x).u, b = f(x).v;
splay(b);
if (ls(b) == y) {
    splice(fa(b));
    return retwist(x, p);
}
splay(a);
```

正是对环的两个原端点 $a,b$ 做 local splay。而

```cpp
if (rs(b)) {
    int z = newnode('r');
    setf(rs(b), z, 2), rs(b) = 0;
    setf(ms(b), z, 0);
    pushup(z), setf(z, b, 2);
}
```

是把 $b$ 上多出来的旁支先 Rake 成一个新的 Rake 结点 $z$（`rs(b)` 当 $z$ 的中儿子、`ms(b)` 当 $z$ 的左儿子）。之所以必须这么做，就是前面强调的那句"**Twist 的两条路径之间不能再夹别的边**"：要把 $b$ 重新作为 Twist 的儿子，就得先把它收拾成一条干净的路径。

其余那些 `setf/pushdown/pushup/pushrev` 只是在 Splay 树上把新的父子关系摆好，**并没有修改原图**。这一点与 LCT 改变 preferred path 的性质完全一致：`retwist` 改变的是"表示"，不是图。

#### 7. link 与 cut

```cpp
bool link(int x, int y, int w) {
    if (x == y)
        return 0;
    makeroot(x);
    if (findroot(y) == x) {
        if (cir(x))
            return 0;
        fa(rs(x)) = 0;
        splay(y), setf(y, x, 1);
        int z = newnode('t'), e = newnode('b');
        f(e) = {x, y, w, 0, MAX, 0, 0, MAX, 0, 0};
        pushdown(y);
        setf(ls(y), z, 0);
        setf(e, z, 1);
        pushup(z), setf(z, y, 0);
        pushup(y), pushup(x);
    } else {
        access(x), access(y);
        int e = newnode('b');
        f(e) = {y, x, w, 0, MAX, 0, 0, MAX, 0, 0};
        setf(e, x, 0);
        pushup(x), setf(x, y, 1);
        pushup(y);
    }
    return 1;
}
```

+ $x=y$ 直接非法（自环）。
+ `makeroot(x)` 之后 `findroot(y) != x` 说明两点不连通：直接造一个 `b` 结点当 $x$ 的左儿子，再把 $x$ 挂成 $y$ 的右儿子，没有环。
+ 否则加边一定会成环。此时先看 `cir(x)`：如果这条根路径上已经含环，再加一条边会让某些边属于两个环，不再合法，直接失败——这就是 `cir` 唯一的用途。合法的话，先断开 $x$ 的右儿子（保证 $x,y$ 是路径的两端），再造**一个 `t` 结点和一个 `b` 结点**：`b` 就是新边，`t` 的左儿子是从 $y$ 上剥下来的旧路径、右儿子是新的 `b`，最后把 `t` 挂回 $y$ 的左边。

这与 negiizhao 说的"对于 $\mathrm{link}(v,w)$ 加入一个 Twist 结点 $(u,v)$ 和一个 base 结点 $(u,v)$"完全一致。

`cut` 与之对偶：

```cpp
bool cut(int x, int y, int w) {
    if (x == y)
        return 0;
    makeroot(x);
    if (findroot(y) != x)
        return 0;
    access(y);
    fa(ls(y)) = 0;
    splay(x), setf(x, y, 0), pushup(y);
    if (type(rs(x)) == 'b') {
        if (f(rs(x)).dis != w)
            return 0;
        clear(rs(x)), rs(x) = 0;
        fa(x) = ls(y) = 0;
        pushup(x), pushup(y);
        return 1;
    } else if (type(rs(x)) == 't') {
        int z = rs(x), k = 0;
        pushdown(z);
        if (type(son(z, k)) != 'b' || f(son(z, k)).dis != w)
            k = 1;
        if (type(son(z, k)) != 'b' || f(son(z, k)).dis != w)
            return 0;
        setf(son(z, !k), x, 1), pushup(x), pushup(y);
        clear(z);
        return 1;
    } else
        return 0;
}
```

先判连通，然后把 $x\to y$ 的路径暴露成"$x$ 的左儿子是 $y$"的形态。

+ 若 `rs(x)` 是 `b` 结点且 `dis == w`，说明要删的是一条普通边，直接回收它。
+ 若 `rs(x)` 是 `t` 结点，就在它的两个儿子里找那条权值为 $w$ 的 `b` 结点；把另一条路径接回 $x$ 的右儿子处，然后回收整个 `t`——这就是"环消失"。
+ 其余情况说明这条边上带环、或者权值对不上，失败。

注意这里只比对了 `dis`：题面保证只要存在权值为 $w$ 的边就随便删一条，而同一个 `(x,y,w)` 在仙人掌里出现的次数不影响正确性。

#### 8. 查询与修改
四个操作短得可怕，因为所有困难都已经压缩进 `pushup` 里了。

```cpp
pair<int, ll> pquery(int x, int y) {
    makeroot(x);
    if (findroot(y) != x)
        return {-1, -1};
    if (f(x).flag)
        return {-2, -2};
    return {f(x).pmin, f(x).psum};
}
pair<int, ll> tquery(int x, int y) {
    makeroot(x);
    if (findroot(y) != x)
        return {-1, -1};
    splay(y);
    return {min(f(ms(y)).tmin, val(y)), f(ms(y)).tsum + val(y)};
}
bool pupdate(int x, int y, int w) {
    makeroot(x);
    if (findroot(y) != x)
        return 0;
    if (f(x).flag)
        return 0;
    return pushlaz(x, {0, w}), 1;
}
bool tupdate(int x, int y, int w) {
    makeroot(x);
    if (findroot(y) != x)
        return 0;
    splay(y), val(y) += w;
    return pushlaz(ms(y), {w, 0}), pushup(y), 1;
}
```

+ `query1`：`makeroot(v)` 之后 `findroot(u)`，不连通返回 `-1 -1`，`flag` 为真返回 `-2 -2`，否则直接取根的 $P$。换根之后 $v$ 被旋到根，它的簇恰好就是 $v\to u$ 这条路径，两个端点都是这棵 Compress Tree 里的 `c` 结点，所以 $P$ 正好是整条路径上的点。
+ `query2`：换根为 $v$、`findroot(u)`、`splay(u)` 之后，$u$ 的右儿子方向已经被剥空，**$u$ 的所有后代恰好全在它的中儿子里**，这就是"子仙人掌 $u$"，最后把 $u$ 自己的点权补上。
+ `add1`：直接给整条路径打 `{0,w}`。
+ `add2`：先 `splay(u)` 把结构显式暴露出来，给中儿子打 `{w,0}`，再单独处理 $u$ 自己，最后 `pushup(u)`。

#### 9. 完整代码
```cpp
#include <bits/stdc++.h>
using namespace std;
using ll = long long;

namespace IO {
    const int __SIZE = (1 << 20) + 1;
    char ibuf[__SIZE], *iS, *iT, obuf[__SIZE],
        *oS = obuf, *oT = oS + __SIZE - 1, _c, qu[55];
    int __f, qr, _eof;
#define Gc()                                                                   \
    (iS == iT ? (iT = (iS = ibuf) + fread(ibuf, 1, __SIZE, stdin),             \
                 (iS == iT ? EOF : *iS++))                                     \
              : *iS++)
    void flush() {
        fwrite(obuf, 1, oS - obuf, stdout);
        oS = obuf;
    }
    void gc(char &x) {
        x = Gc();
    }
    void pc(char x) {
        *oS++ = x;
        if (oS == oT)
            flush();
    }
    void pstr(const char *s) {
        int __len = strlen(s);
        for (__f = 0; __f < __len; ++__f)
            pc(s[__f]);
    }
    void gstr(char *s) {
        for (_c = Gc(); _c < 32 || _c > 126 || _c == ' ';)
            _c = Gc();
        for (; _c > 31 && _c < 127 && _c != ' ' && _c != '\n' && _c != '\r';
             ++s, _c = Gc())
            *s = _c;
        *s = 0;
    }
    template <class I> bool read(I &x) {
        _eof = 0;
        for (__f = 1, _c = Gc(); (_c < '0' || _c > '9') && !_eof; _c = Gc()) {
            if (_c == '-')
                __f = -1;
            _eof |= _c == EOF;
        }
        for (x = 0; _c <= '9' && _c >= '0' && !_eof; _c = Gc()) {
            x = x * 10 + (_c & 15), _eof |= _c == EOF;
        }
        x *= __f;
        return !_eof;
    }
    template <class I> void print(I x) {
        if (!x)
            pc('0');
        if (x < 0) {
            pc('-');
            x = -x;
        }
        while (x) {
            qu[++qr] = x % 10 + '0', x /= 10;
        }
        while (qr)
            pc(qu[qr--]);
    }
    struct Flusher_ {
        ~Flusher_() { flush(); }
    } io_flusher_;
} // namespace IO
using IO::gstr;
using IO::pc;
using IO::print;
using IO::pstr;
using IO::read;

class DynamicCactus {
private: /* const */
    static constexpr const int MAX = INT_MAX;

public: /* Types */
    struct tag {
        int tlaz, plaz;
        tag() { tlaz = plaz = 0; }
        tag(int tlaz, int plaz) : tlaz(tlaz), plaz(plaz) {}
        bool empty() { return (tlaz == 0 && plaz == 0); }
        tag tree() { return {tlaz, 0}; }
        tag move() { return {tlaz, tlaz}; }
        friend tag operator*(const tag &x, const tag &y) {
            return {x.tlaz + y.tlaz, x.plaz + y.plaz};
        }
    };
    struct dat {
        int u, v, dis;
        int tsiz, tmin, psiz;
        ll tsum, psum;
        int pmin;
        bool flag;
        dat() {
            u = v = dis = tsiz = psiz = 0, tmin = pmin = INT_MAX;
            tsum = psum = 0, flag = 0;
        }
        dat(int u, int v, int dis, int tsiz, int tmin, ll tsum, int psiz,
            int pmin, ll psum, bool flag)
            : u(u), v(v), dis(dis), tsiz(tsiz), tmin(tmin), psiz(psiz),
              tsum(tsum), psum(psum), pmin(pmin), flag(flag) {}
        void rev() { swap(u, v); }
        void insert(int w) { psiz++, pmin = min(pmin, w), psum += w; }
        dat move() const {
            return {u, v, dis, tsiz + psiz, min(tmin, pmin), tsum + psum,
                    0, INT_MAX, 0, flag};
        }
        friend dat operator*(const dat &x, const tag &y) {
            return {x.u, x.v, x.dis,
                    x.tsiz, x.tmin == INT_MAX ? INT_MAX : x.tmin + y.tlaz,
                    x.tsum + 1ll * x.tsiz * y.tlaz,
                    x.psiz, x.pmin == INT_MAX ? INT_MAX : x.pmin + y.plaz,
                    x.psum + 1ll * x.psiz * y.plaz, x.flag};
        }
    };
    struct node {
        char type, rev, cir;
        int son[3], fa, val;
        dat f;
        tag laz;
        node() { type = rev = cir = 0, son[0] = son[1] = son[2] = fa = val = 0; }
    };

public: /* Basic */
    vector<node> st;
    vector<int> trash;
    DynamicCactus(int n) {
        st.reserve(5 * n + 1024);
        trash.reserve(n + 64);
        st.resize(n + 1), st[0].f.tmin = st[0].f.pmin = MAX;
        for (int i = 1; i <= n; i++)
            type(i) = 'c';
    }
    int newnode(char c) {
        int id;
        if (!trash.empty())
            id = trash.back(), trash.pop_back();
        else
            id = st.size(), st.push_back(node());
        return type(id) = c, id;
    }
    void clear(int x) { st[x] = node(), trash.push_back(x); }
    node &operator[](int x) { return st[x]; }
    void setval(int x, int w) { val(x) = w, pushup(x); }

private: /* Aux */
    int &son(int x, int y) { return st[x].son[y]; }
    int &fa(int x) { return st[x].fa; }
    int &val(int x) { return st[x].val; }
    char &rev(int x) { return st[x].rev; }
    char &cir(int x) { return st[x].cir; }
    char &type(int x) { return st[x].type; }
    dat &f(int x) { return st[x].f; }
    tag &laz(int x) { return st[x].laz; }
    int &ls(int x) { return son(x, 0); }
    int &rs(int x) { return son(x, 1); }
    int &ms(int x) { return son(x, 2); }
    bool isrt(int x) { return type(x) != type(fa(x)); }
    bool dir(int x) { return son(fa(x), 1) == x; }
    void setf(int x, int f, int typ) {
        if (x)
            fa(x) = f;
        son(f, typ) = x;
    }

private: /* Push */
    dat compress(const dat &a, const dat &b, int w) {
        return {a.u, b.v, a.dis + b.dis,
                a.tsiz + b.tsiz, min(a.tmin, b.tmin), a.tsum + b.tsum,
                a.psiz + b.psiz + 1, min({a.pmin, b.pmin, w}),
                a.psum + b.psum + w, a.flag || b.flag};
    }
    dat twist(dat a, dat b) {
        if (a.dis > b.dis)
            swap(a, b);
        if (a.dis == b.dis)
            return {a.u, a.v, a.dis,
                    a.tsiz + b.tsiz, min(a.tmin, b.tmin), a.tsum + b.tsum,
                    a.psiz + b.psiz, min(a.pmin, b.pmin), a.psum + b.psum, 1};
        b = b.move();
        return {a.u, a.v, a.dis,
                a.tsiz + b.tsiz, min(a.tmin, b.tmin), a.tsum + b.tsum,
                a.psiz, a.pmin, a.psum, a.flag};
    }
    dat rake(const dat &a, const dat &b) {
        return {a.u, a.v, a.dis,
                a.tsiz + b.tsiz, min(a.tmin, b.tmin), a.tsum + b.tsum,
                a.psiz, a.pmin, a.psum, a.flag};
    }
    void pushup(int x) {
        if (type(x) == 'c') {
            if (ls(x) && rs(x)) {
                if (ms(x))
                    f(x) = compress(f(ls(x)), rake(f(rs(x)), f(ms(x))), val(x));
                else
                    f(x) = compress(f(ls(x)), f(rs(x)), val(x));
            } else {
                if (ls(x) || rs(x))
                    f(x) = f(ls(x) | rs(x));
                else
                    f(x) = {x, x, 0, 0, MAX, 0, 0, MAX, 0, 0};
                f(x).insert(val(x));
                if (ms(x))
                    f(x) = rake(f(x), f(ms(x)));
            }
            cir(x) = cir(ls(x)) | cir(rs(x));
        } else if (type(x) == 'r') {
            f(x) = f(ms(x)).move();
            if (ls(x))
                f(x) = rake(f(x), f(ls(x)));
            if (rs(x))
                f(x) = rake(f(x), f(rs(x)));
        } else if (type(x) == 't') {
            f(x) = twist(f(ls(x)), f(rs(x)));
            cir(x) = 1;
        }
    }
    void pushrev(int x) {
        if (!x)
            return;
        f(x).rev();
        swap(ls(x), rs(x)), rev(x) ^= 1;
    }
    void pushlaz(int x, tag y) {
        if (!x)
            return;
        laz(x) = laz(x) * y;
        f(x) = f(x) * y;
        if (type(x) == 'c')
            val(x) += y.plaz;
    }
    void pushdown(int x) {
        if (rev(x)) {
            pushrev(ls(x));
            pushrev(rs(x));
            rev(x) = 0;
        }
        if (laz(x).empty())
            return;
        if (type(x) == 'c') {
            pushlaz(ls(x), laz(x));
            pushlaz(rs(x), laz(x));
            pushlaz(ms(x), laz(x).tree());
        } else if (type(x) == 'r') {
            int w = laz(x).tlaz;
            for (int c = 0; c < 3; c++)
                pushlaz(son(x, c), {w, 0});
            pushlaz(ms(x), {0, w});
        } else if (type(x) == 't') {
            int w = f(ls(x)).dis - f(rs(x)).dis;
            pushlaz(ls(x), w <= 0 ? laz(x) : laz(x).move());
            pushlaz(rs(x), w >= 0 ? laz(x) : laz(x).move());
        }
        laz(x) = tag();
    }
    void pdrt(int x) {
        if (!isrt(x))
            pdrt(fa(x));
        pushdown(x);
    }

private: /* Rot */
    void rotate(int x) {
        int y = fa(x), z = fa(y), k = dir(x);
        if (z)
            *find(st[z].son, st[z].son + 3, y) = x;
        fa(x) = z;
        setf(son(x, !k), y, k), setf(y, x, !k);
        pushup(y);
    }
    void splay(int x) {
        pdrt(x);
        for (int y; y = fa(x), !isrt(x); rotate(x))
            if (!isrt(y))
                rotate(dir(x) ^ dir(y) ? x : y);
        pushup(x);
    }

private: /* Access */
    void del(int x) {
        int y = fa(x);
        pushdown(x);
        if (ls(x)) {
            int z = ls(x);
            fa(z) = 0;
            for (; rs(z); z = rs(z))
                pushdown(z);
            splay(z);
            setf(rs(x), z, 1);
            pushup(z);
            setf(z, y, 2);
        } else
            setf(rs(x), y, 2);
        pushup(y), clear(x);
    }
    void splice(int x) {
        splay(x);
        int y = fa(x);
        splay(y);
        pushdown(y), pushdown(x);
        if (fa(y) && type(fa(y)) == 't')
            return retwist(y, ms(x));
        int a = ms(x), b = rs(y);
        setf(a, y, 1);
        setf(b, x, 2);
        pushup(x);
        if (!b)
            del(x);
        else
            pushup(y);
    }
    void retwist(int x, int p = 0) {
        int y = fa(x);
        splay(fa(y));
        pushdown(y);
        int a = f(x).u, b = f(x).v;
        splay(b);
        if (ls(b) == y) {
            splice(fa(b));
            return retwist(x, p);
        }
        splay(a);
        fa(rs(a)) = 0;
        splay(b), setf(b, a, 1), pushup(b);
        pushdown(a), pushdown(b);
        pushdown(y), pushdown(x);
        if (p)
            pushdown(ms(x));
        int k = dir(x);
        setf(ls(x), y, k);
        if (rs(b)) {
            int z = newnode('r');
            setf(rs(b), z, 2), rs(b) = 0;
            setf(ms(b), z, 0);
            pushup(z), setf(z, b, 2);
        }
        setf(son(y, !k), b, 0);
        pushrev(rs(x));
        setf(rs(x), b, 1);
        pushup(b), setf(b, y, !k);
        pushup(y), setf(y, x, 0);
        if (p)
            del(fa(p));
        setf(p, x, 1);
        pushup(x), setf(x, a, 1);
        pushup(a);
    }
    void access(int x) {
        splay(x);
        if ((!fa(x) || type(fa(x)) != 't') && rs(x)) {
            int y = newnode('r');
            pushdown(x);
            setf(ms(x), y, 0);
            setf(rs(x), y, 2), rs(x) = 0;
            setf(y, x, 2);
            pushup(y), pushup(x);
        }
        for (; splay(x), fa(x);) {
            if (type(fa(x)) == 'r')
                splice(fa(x));
            else
                retwist(x);
        }
    }
    void makeroot(int x) { access(x), pushrev(x); }
    int findroot(int x) {
        access(x);
        for (; ls(x); x = ls(x))
            pushdown(x);
        return splay(x), x;
    }

public: /* Link-Cut */
    bool link(int x, int y, int w) {
        if (x == y)
            return 0;
        makeroot(x);
        if (findroot(y) == x) {
            if (cir(x))
                return 0;
            fa(rs(x)) = 0;
            splay(y), setf(y, x, 1);
            int z = newnode('t'), e = newnode('b');
            f(e) = {x, y, w, 0, MAX, 0, 0, MAX, 0, 0};
            pushdown(y);
            setf(ls(y), z, 0);
            setf(e, z, 1);
            pushup(z), setf(z, y, 0);
            pushup(y), pushup(x);
        } else {
            access(x), access(y);
            int e = newnode('b');
            f(e) = {y, x, w, 0, MAX, 0, 0, MAX, 0, 0};
            setf(e, x, 0);
            pushup(x), setf(x, y, 1);
            pushup(y);
        }
        return 1;
    }
    bool cut(int x, int y, int w) {
        if (x == y)
            return 0;
        makeroot(x);
        if (findroot(y) != x)
            return 0;
        access(y);
        fa(ls(y)) = 0;
        splay(x), setf(x, y, 0), pushup(y);
        if (type(rs(x)) == 'b') {
            if (f(rs(x)).dis != w)
                return 0;
            clear(rs(x)), rs(x) = 0;
            fa(x) = ls(y) = 0;
            pushup(x), pushup(y);
            return 1;
        } else if (type(rs(x)) == 't') {
            int z = rs(x), k = 0;
            pushdown(z);
            if (type(son(z, k)) != 'b' || f(son(z, k)).dis != w)
                k = 1;
            if (type(son(z, k)) != 'b' || f(son(z, k)).dis != w)
                return 0;
            setf(son(z, !k), x, 1), pushup(x), pushup(y);
            clear(z);
            return 1;
        } else
            return 0;
    }

public: /* Query */
    pair<int, ll> pquery(int x, int y) {
        makeroot(x);
        if (findroot(y) != x)
            return {-1, -1};
        if (f(x).flag)
            return {-2, -2};
        return {f(x).pmin, f(x).psum};
    }
    pair<int, ll> tquery(int x, int y) {
        makeroot(x);
        if (findroot(y) != x)
            return {-1, -1};
        splay(y);
        return {min(f(ms(y)).tmin, val(y)), f(ms(y)).tsum + val(y)};
    }

public: /* Update */
    bool pupdate(int x, int y, int w) {
        makeroot(x);
        if (findroot(y) != x)
            return 0;
        if (f(x).flag)
            return 0;
        return pushlaz(x, {0, w}), 1;
    }
    bool tupdate(int x, int y, int w) {
        makeroot(x);
        if (findroot(y) != x)
            return 0;
        splay(y), val(y) += w;
        return pushlaz(ms(y), {w, 0}), pushup(y), 1;
    }
};

int main() {
    int n, m;
    read(n), read(m);
    DynamicCactus tril(n);
    for (int i = 1, x; i <= n; i++)
        read(x), tril.setval(i, x);
    for (int x, y, z; m--;) {
        static ll w;
        static char op[99];
        gstr(op), read(x), read(y);
        if (*op == 'l')
            read(z), pstr(tril.link(x, y, z) ? "ok\n" : "failed\n");
        else if (*op == 'c')
            read(z), pstr(tril.cut(x, y, z) ? "ok\n" : "failed\n");
        else if (*op == 'q' && op[5] == '1')
            tie(x, w) = tril.pquery(x, y), print(x), pc(' '), print(w), pc('\n');
        else if (*op == 'q')
            tie(x, w) = tril.tquery(x, y), print(x), pc(' '), print(w), pc('\n');
        else if (*op == 'a' && op[3] == '1')
            read(z), pstr(tril.pupdate(x, y, z) ? "ok\n" : "failed\n");
        else
            read(z), pstr(tril.tupdate(x, y, z) ? "ok\n" : "failed\n");
    }
    return 0;
}
```

#### 10. 复杂度
`splice` 的次数仍然均摊 $\mathcal O(\log n)$，与树上的分析相同（只是常数略大），每次操作均摊 $\mathcal O(\log n)$，于是总复杂度 $\mathcal O((n+m)\log n)$。结点回收保证了辅助结点总数始终是 $\mathcal O(n+m)$，不会随操作次数增长，空间复杂度 $\mathcal O(n+m)$。

#### 11. 空间：辅助结点数到底要多少
这份实现只开一片结点池（原实现是定长数组，改写版是 `vector` + 预分配），所以必须知道辅助结点最多有多少个。四类结点分别数：

+ `c`：每个原图顶点恰好一个，共 $n$ 个，初始化之后不再增删；
+ `b`：每条边一个。沙漠里两个点之间最多两条重边，而"环"给连通块带来的额外边数不超 Cactus-Merge. Png过环数，于是边数 $\le 2n-2$；
+ `t`：每个环一个，环数 $\le n-1$（"顶点 $1$ 与其余每个点连两条重边"这种 2-环扇可以把两个界同时取到）；
+ `r`：Rake 结点只会出现在轻子树的位置上。每个点最多一个重儿子，所以轻边数 $\le n-1$，而所有 Rake Tree 的内部结点数不超过它们的叶子数，即不超过轻边数，于是 $r\le n-1$。

四项相加：
$$n+(2n-2)+(n-1)+(n-1)=5n-4.$$

这个界是**紧**的：$n=50000$ 时用 2-环扇的数据实测，池子高水位正好是 $249996=5n-4$。而普通随机大数据只会用到 $3.5n$ 左右，因为"边数取满"和"环数取满"和"轻边取满"很难同时发生。

所以预分配 $5n$ 个结点就够了，**完全不需要跟着 $m$ 走**：结点数只取决于"当前这张图"，与操作次数无关（$b$、$t$ 随 link/cut 增删，$r$ 随 access 变动的也只是位置而不是数量）。这一点在卡空间的时候很关键——把池子按 $n$ 开，$\mathcal O(n)$ 的空间就写死了。

## References
+ [[1] OI-wiki](https://oi-wiki.org/ds/top-tree)
+ [[2] Autre: 简明 TopTree 指南](https://www.luogu.com/article/arezeyav)
+ [[3] 251sec: 这个世界就是一棵巨大的 SATT](https://www.luogu.com.cn/article/bh8zlk4j)
+ [[4] negiizhao: Top tree 相关东西的理论、用法和实现](https://negiizhao.blog.uoj.ac/blog/4912)
+ [[5] ExplodingKonjac: Top Tree 相关理论扯淡](https://www.cnblogs.com/ExplodingKonjac/p/17890636.html)
