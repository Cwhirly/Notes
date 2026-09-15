---
难度: "10"
前置知识: "[[Link-Cut Tree]]"
前置知识 2: "[[Heavy Path Decomposition]]"
前置知识 3: "[[FHQ & Splay]]"
---
## 0. 前言

> “大家不要被【】学长带偏了，在没进队的时候，去学什么，Top Tree。”

某中学一骨干数学教师对于讲授必修一集合部分的前两节花费了整一周时间，令众人大为震撼。  

不过，也有观点认为，这种教学方法也造就了更深刻的理解与和更扎实的基础。

## 1. 例题
基础模板题：[P5649 Sone1](https://www.luogu.com.cn/problem/P5649)。  
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
 
SATT 可以实现森林中任一棵树的链修改/查询、子树修改/查询以及非局部搜索等操作，其复杂度由 Splay 操作保证，并根据势能分析得出其均摊复杂度不超过 $\mathcal O(\log n)$。
  
2006 年，Werneck 在 [Design and Analysis of Data Structures for Dynamic Trees](https://renatowerneck.wordpress.com/wp-content/uploads/2016/06/wer06-dissertation.pdf) 提出了第一个正确的最坏 $\mathcal{O}(\log n)$ 的实现，不过该做法实现极其复杂且常数大，所以不太容易投入实践。

注意在原论文和大部分的教程中，介绍的都是基于边的 SATT，但是在写代码时，我们通常使用基于点的 SATT。本文会在实际操作的板块对两种进行区分，在学习其结构是还是讲解基于边的 SATT。

## 3. 静态 Top Tree
先讲静态的。
考虑线段树如何维护序列的区间操作。
我们把一个区间拆成成 $\mathcal{O}(\log n)$ 个由递归分治得到是线段树区间，维护每一个线段树区间的信息，然后用其维护的具有结合率的运算进行合并，得到答案。  

将这个思想照搬到树上，可以通过树链剖分将树转化成序列，也可以通过建立 Top Tree 的方法。

### I. 簇合并 
一个簇（Top Cluster）是一个连通的**边集**（即不包含边集所连接的点），并且最多只有两个点有不在簇内的边，即最多只有两个点连向簇外。

我们称这些点为**界点**，两个界点之间的边为**簇路径**。 

树根被钦定后，我们便可以称深度较浅的界点为**上界点**，较深的界点为**下界点**。若簇中只有一个点连向簇外，则可以任意钦定一个叶子作为下界点。  

根据定义，显然有如下性质：
+ 如果点 $u$ 有儿子 $v$、边 $(u,v)$ 在某个簇内，且 $v$ 不在该簇的簇路径上，则 $v$ 子树内的所有边均在簇内。

说明这个性质使容易的，Top Tree 中的界点是簇连接外界的“唯一出口”：只要沿着簇内的一条边 $(u,v)$ 走进了一个子树，且该子树里面没有界点，那么这个子树就是一条死路，里面的所有节点都无法通向其他簇，整个子树就必须完全被该簇打包吞并。

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
收缩的方法还是 Compess 和 Rake，只不过它们。
#### 1. Compress
Compress 操作指定一个度数为 $2$ 的点 $x$，与点 $x$ 相邻的那两个点记为 $y,z$，我们连一条新边 $(y,z)$；将点 $x$、边 $(x,z)$、边 $(x,y)$ 的信息放到 $(y,z)$ 中储存，并删去它们。  

如图所示。

![](https://oi-wiki.org/ds/images/top-tree1.svg)
#### 2. Rake
Rake 操作指定一个度为 $1$ 的点 $x$，而且与点 $x$ 相邻的点 $y$ 的度数需大于 $1$，设点 $y$ 的另一个邻点为 $z$，我们将点 $x$、边 $xy$ 的信息放入边 $yz$ 中储存，并删去它们。如图所示。

![](https://oi-wiki.org/ds/images/top-tree2.svg)

---
其实两种刻画方式只不过是相反的顺序罢了。  
### III. 建立树结构
建立一棵新的二叉树，将一个簇看作新树上的一个节点，仅包含一条边的簇为新树的叶子，通过 Compress 或 Rake 操作形成的大簇所代表的节点是合并前的两个小簇的父节点。最终形成的这棵树就是原树的 **Top Tree**。

![](https://cdn.luogu.com.cn/upload/image_hosting/ue9ifr12.png)

左边部分即原树，不带箭头的圆弧指 Compress 操作，带箭头的指 Rake 操作。

我们可以认为原树是一棵无根树，也可以认为 $e$ 是树根，右边部分为原树的 Top Tree, 其中形如 $xy$ 的节点指该节点代表的原树中簇的上下界点分别为 $x,y$，当然如果认为原树是无根树，则这个“界点”同样具有前后顺序，只不过是人为定的罢了。  

方点表示该节点代表的簇是仅有一条边组成，或由两个小簇 Compress 形成的，非叶子方点称为 Compress Node；圆点表示该节点代表的簇是由一个小簇 Rake 到另一个小簇上形成的, 称为 Rake Node。

由于一个簇中的两个界点之间存在先后顺序，所以 Top Tree 里的左右儿子顺序是不能调换的，对于 Compress Node，其左儿子为“更靠上”的小簇；对于 Rake Node，其左儿子在原树中的“箭头”指向右儿子。当然这个不涉及什么性质，所以让所有 Rake Node 把左右儿子关系做个对称也没有影响。

注意这仅仅是一个示意图，为了更好的体现出 Top Tree 上每一层所代表的收缩而绘制的，实际上**我们不需要那些重复的度数为 $2$ 的非根点**，比如那若干个 $hi$ 和最顶上的那个作为 Compress Node 的 $gi$。仅仅保留最上面的有效节点即可。


的定义是### IV. 应用
该以怎样的收缩方法构建这棵 Top Tree？考虑重链剖分，得到这样一个结构。

![]()https://cdn.luogu.com.cn/upload/image_hosting/xuifalte.png)



References
+ 

黑色的代表重链，三角形代表一棵轻子树。

对于递归过程中的每一个子树，采取这样的过程：先把重链剖出来，然后把每个轻子树递归处理，处理完成后，将所有轻子树 Rake 到重链上的相应叶簇上，我们就得到了一个竹节状的结构。

![](https://cdn.luogu.com.cn/upload/image_hosting/7vx2tpw7.png)

图中每一个回旋镖状物就是一个 Rake 了该条重边的上方点的全部轻子树的簇，对于轻子树递归处理。  

得到该结构后，如同线段树一样的进行递归 Compress，结束构建。

Rake 的过程是同样的的，即若一个节点有多个轻子树，那么同样采用类似线段树的方法，先两个为一组互相 Rake，得到轻子树数一半的新簇，然后再两两 Rake，这样就算一个节点有 $\mathcal O(n)$ 个轻儿子，也可以保证 Rake Node 构成的部分深度不超过 $\mathcal O(\log n)$。  

但实际上这个方法的复杂度是伪的。

由于轻子树不是最大的子树，所以轻子树大小不会超过整棵树大小的一半，所以从根往下走，最多只能走 $\mathcal O(\log n)$ 个轻边，就会走到子树大小为 $1$ 的叶子，而这也就意味着，最多只能走 $\mathcal{O}(\log n)$ 条重链。每条重链贡献 $\mathcal{O}(\log n)$ 的 Top Tree 上深度，那么它们在 Top Tree 上拼起来深度就达到了 $O(\log^2 n)$。    

一个显然的 Hack：

![](https://cdn.luogu.com.cn/upload/image_hosting/nthf6y6i.png)

认为 $n$ 是某个 $2$ 的次幂并取 $k=\log{n}$，那么上图所显示的树的 Top Tree 单单 Compress Node 的深度贡献就达到了 $\sum_{i=1}^{\log n}(\log n-i)=\frac{\log^2n}{2}-\log n=\mathcal{O}(\log^2 n)$。  

因此，考虑全局平衡二叉树的思想。在 Compress 重链和 Rake 轻子树的时候做带权二分，而非严格二分，权值即为轻子树的大小。

此处仅仅距离 Rake 时的带权二分，Compress 时的带权二分是完全同理的。

考虑一个具有 $x$ 个轻儿子的原树节点 $u$，这些轻子树的 Top Tree 均已被建出，这 $x$ 棵 Top Tree 的根编号（注意是 Top Tree 中的编号而非原树中的）分别为 $t_1,t_2,\cdots,t_{x-1},t_x$。在前面的错误做法中，我们的二分方法是，对于一个在 Top Tree 上涵盖了 $t_{[l,r]}$ 作为后代的 Rake Node $\alpha$，其左儿子和右儿子分别涵盖 $t_{[l,\lfloor\frac{l+r}{2}\rfloor]},t_{(\lfloor\frac{l+r}{2}\rfloor,r]}$。  

而“带权二分”的方法是，不按点的数量折半，而是寻找一个 $\xi\in[l,r]$，使得 $t_{[l,\xi]}$ 中所有点对应的原树点的子树大小之和与 $t_{(\xi,r]}$ 中所有点对应的原树点的子树大小之和尽可能接近，即：该 $\xi$ 的选取使这两个值的差的绝对值尽可能小，实际上写法是简单的，一边加轻儿子一边更新前缀和，直到这个前缀和刚刚大于 $u$ 全体轻子树和的一半，停止循环，并将已计算的前缀递归进左儿子，剩余部分递归进右儿子。

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

然后考虑全局树高的证明。

考虑原树中任意一个叶子节点 $u$（初始大小 $w=1$），它在 Top Tree 中一路向上跳到全局根（总大小 $W=n$）：

路径上会依次经过多棵局部二叉树（可能是轻儿子合并的 Rake 树，也可能是重链压缩的 Compress 树）。

以下所说的节点均指 Top Tree 中的。

设刚开始跳到路径上第 $j$ 棵局部树的“叶子”，即“入口”上时，目前跳到的节点的子树所代表的原树中簇大小为 $S_j^{\text{in}}$，跳到这棵局部树的“出口”时目前点的子树在原树上的对应簇大小为 $S_j^{\text{out}}$。由引理可知：在第 $j$ 棵局部树内的深度 $\le2\log\left(\frac{S_j^{\text{out}}}{S_j^{\text{in}}}\right)+O(1)$，当从第 $j$ 棵树跳到上一级第 $j+1$ 棵树时，上一级的“入口”簇必然包含了当前簇，即 $S_{j+1}^{\text{in}} \ge S_j^{\text{out}}$。

根据重链剖分性质，从叶到根最多跨越 $\mathcal O(\log n)$ 条重链（即只有 $\mathcal O(\log n)$ 棵局部树，常数项总和为 $\mathcal O(\log n)$）。

将沿途所有局部树内的深度累加：
$$
\begin{aligned}
\text{Global Depth}&=\sum_{j}\left(2\log\left(\frac{S_j^{\text{out}}}{S_j^{\text{in}}}\right)+\mathcal O(1)\right)\\
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

下面举一些经典应用，具体细节实现参考各题题解，这里不做赘述：
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

有趣的是，对原树三度化的方法，是 Frederickson 在 1985 年及后续论文中提出的原始 Topology Tree。但是其实现非常复杂且效率低下。Alstrup 等人在 1997 年提出的 Top Tree，其设计目标就是提供更简单、更容易处理信息的接口，但是这个方法也并不简洁，所有后面 SATT 才应运而生。

何为三度化？前文所述的静态 Top Tree 是一颗二叉树，现在我们要将其改为三叉树。 

每个节点除了有左右儿子之外，再引入**中儿子**，而 Compress Node 和 Rake Node 的中儿子意义不同：
+ Compress Node 的中儿子一定是一个 Rake Node, Rake Node 的中儿子一定是 Compress Node。
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

Splay 前要执行从 Top Tree 的根（而非 Compress Tree 的根）到 $x$ 的下传标记，且为了方便以后的操作，需要设立一个哨兵节点 $\text{goal}$ 以处理有时只需旋转到特定节点的情况。

由于 Splay 操作不改变中序遍历的性质，它不会使得原树的树形产生任何变化。

### IV. Access 相关操作
`access(x)` 将 SATT 上的节点 $x$ 旋转到整个 SATT 的根节点，不改变原树结构。  

我们考虑先把 $x$ 到 SATT 根的路径“打通”，使得 $x$ 和 SATT 根处于同一个 Compress Tree 中，然后再执行一次 Splay 把 $x$ 转到根部。
#### 1. Splice
Splay 操作只能在同一棵 Compress Tree 内实现，所以 Splice 的作用就是，将两个靠  Rake Tree 接在一起的 Compress Tree 打通。  

打通后产生的新的 Compress Tree 显然不一定会包含原先两棵 Compress Tree 的全部节点，不过这没有影响，我们仅仅关心那个要旋转的点 $x$ 是否被新的 Compress Tree 囊括进去。  

例如，如果我们想要打通 Rake Node $\alpha$ 所在 Rake Tree 下方的 Compress Tree 和上方的 Compress Tree：
1. 先将 $\alpha$ Splay 到其所在 Rake Tree 的根，此时 $\alpha$ 的父亲 $f$ 一定是一个 Compress Node，中儿子为 $\alpha$。
2. 将 $f$ Splay 到其所在 Compress Tree 的根。
3. 如果 $f$ 有右儿子，那就让

## Refrences
+ [[1] OI-wiki](https://oi-wiki.org/ds/top-tree)
+ [[2] Autre: 简明 TopTree 指南](https://www.luogu.com/article/arezeyav)
+ [[3] 251sec: 这个世界就是一棵巨大的 SATT](https://www.luogu.com.cn/article/bh8zlk4j)
+ [[4] negiizhao: Top tree 相关东西的理论、用法和实现](https://negiizhao.blog.uoj.ac/blog/4912)


```
EsU1 7NTN X1Ve rN2u wQtT tVXE mxGH 8cGr 4euP g32w gFrR 6UF1
```
