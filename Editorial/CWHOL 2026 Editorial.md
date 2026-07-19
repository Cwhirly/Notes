
|          | A                  | B              | C                  |
| -------- | ------------------ | -------------- | ------------------ |
| 中文名      | 四月天                | 蜀道难            | 希望有羽毛和翅膀           |
| 英文名      | april              | road           | wing               |
| Luogu 评级 | ~={yellow}普及/提高-=~ | ~={cyan}提高=~   | ~={black}省选/NOI-=~ |
| CF 评级    | 1500               | 2000           | 2900               |
| 分类       | 数学                 | 图论，数学          | 树论，数学              |
| 算法标签     | 数学证明               | Tarjan, Ad-hoc | Kruskal 重构树，FWT    |
| 侧重点      | 思维                 | 算法+思维          | 算法                 |
| 题目来源     | EOlymp             | Codeforces     | Revitalize+Pulsar  |
| std      | EOlymp             | Revitalize     | Revitalize         |
| 题解       | Revitalize         | Revitalize     | Revitalize         |
| 预期通过率    | 0.7                | 0.3            | 0                  |

|          | D                 | E                 | F                      |
| -------- | ----------------- | ----------------- | ---------------------- |
| 中文名      | 追忆                | 孔乙己               | 滕王阁序                   |
| 英文名      | recall            | Kong              | elas                   |
| Luogu 评级 | ~={green}普及+/提高=~ | ~={blue}提高+/省选-=~ | ~={navy}NOI/NOI+/CTS=~ |
| CF 评级    | 1700              | 2400              | 3500                   |
| 分类       | Ad-hoc            | 动态规划，树论           | 图论，数学                  |
| 算法标签     | Ad-hoc            | 长链剖分优化 DP         | 各类生成函数技巧               |
| 侧重点      | 思维                | 算法                | 算法+思维+代码               |
| 题目来源     | Codeforces        | CF+Revitalize     | Revitalize             |
| std      | Pulsar            | Revitalize        | Revitalize             |
| 题解       | Codeforces        | Revitalize        | Revitalize             |
| 预期通过率    | 0.5               | 0.2               | 0                      |

**感谢参与 CWHOC 2026！**  
任何人如有疑问或建议，请先确保您有能力使用的 AI 无法解决您的问题，再洛谷私信出题人，请勿使用暴戾语言。   
出题人洛谷账号链接：[Revitalize](https://luogu.com.cn/user/553192)

---
## A：四月天（april）
### Hint 1
输入量极小，可以先尝试打表寻找规律，然后尝试证明。
### Hint 2
考虑表中质数情况及质数之次幂的规律。
### Hint 3
注意到原式关于 $n$ 是积性的。
### Solution
直接模拟的复杂度是 $O(n+m)$，可以拿到 $20$ 分。
我们定义原式为 $F(n,m)$。  
先考虑 $n$ 是质数的情况。  
发现 $\binom{n}{1}=n$，所以所有的 $F(n,m)$ 不可能大于 $n$。  
考虑 $\binom{n}{m}$ 的结构，即为 $\frac{n!}{(n-m)!m!}$，而由于 $n-m<n,m<n$，所以 $(n-m)!,m!$ 中不可能含有 $n$ 这个质因子，所以 $\binom{n}{m}$ 中的 $n$ 质因子必定会被保留。  
所以对于所有 $m<n$，$F(n,m)$ 不可能小于 $n$，只能全部等于 $n$。  
现在我们已经可以拿到 $30$ 分。
然后考虑 $n$ 是质数 $p$ 的 $k$ 次幂的情况。  
打表发现，$F(p^k,m)=p^{k-\lfloor\log_pm\rfloor}$，下面我们考虑证明。  
要证明这个性质，我们核心需要利用 **Kummer 定理** 或者 **Lucas 定理的推论**。
（定义 $v_p(x)$ 为 $x$ 中 $p$ 质因子的次数，读作 $x$ 的 $p$ -进估值（$p$ -adic valuation））来进行严格证明。）
首先根据 Kummer 定理，$\binom{n}{i}$ 能被 $p$ 整除的最高次数 $v_p\left(\binom{n}{i}\right)$，等于在 $p$ 进制下计算 $i + (n-i)$ 时发生的**进位次数**。
对于 $F(p^k, m)$ 中的项 $\binom{p^k}{i}$，其中 $1 \le i \le m < p^k$：
- 在 $p$ 进制下，$p^k$ 写做 $1$ 后面跟 $k$ 个 $0$。
- 由于 $i < p^k$，设 $v_p(i) = \alpha$（即 $i$ 的末尾有 $\alpha$ 个 $0$）。
- 计算 $p^k - i$ 时，从第 $\alpha$ 位向更高位借位，直到最高位。
- **结论：** $v_p\left(\binom{p^k}{i}\right) = k - v_p(i)$。
由于 $\binom{p^k}{1}=p^k$，所以所有的 $F(p^k,m)$ 都是 $p$ 的某个 $r_m$ 次幂，故而：
$$
r_m=\min_{i=1}^mv_p\left(\binom{p^k}{i}\right)=\min_{i=1}^m(k-v_p(i))=k-\max_{i=1}^mv_p(i)
$$
而能取到 $\max_{i=1}^mv_p(i)$ 的 $i$ 正是小于 $m$ 的离 $m$ 最近的 $p$ 的次幂。  
也就是 $\lfloor\log_pm\rfloor$。  
所以我们就可以证明 $F(p^k,m)=p^{k-\lfloor\log_pm\rfloor}$。  
现在我们可以拿到 $45$ 分。  
最后证明 $F(n,m)$ 关于 $n$ 积性。
有一个非常优美的结论：
$$F(n,m) = \frac{n}{\gcd(n, \text{lcm}(1, 2, \cdots, m))}$$
只要证明了上述等式，由于 $L_m = \text{lcm}(1, 2, \cdots, m)$ 是一个与 $n$ 无关的常数，其积性便显而易见。
首先我们知道：
$$v_p(L_m) = \max_{1 \le i \le m} v_p(i) = \lfloor \log_p m \rfloor$$
我们把要证明的式子根据质因数分解拆开，就等价于，对任意质数 $p$，均有：
$$v_p(F(n,m))=\max(0,v_p(n)-v_p(L_m))$$
设 $v_p(n) = k$（即 $n = p^k \cdot c$，且 $p \nmid c$）。  
分两种情况讨论：
#### 情况 1：$m \ge p^k$
此时 $v_p(L_m) = \lfloor \log_p m \rfloor \ge k$。所以 $\max(0, k - v_p(L_m)) = 0$。
因为 $p^k \le m$，所以在最大公约数的序列中必然包含 $i = p^k$ 这一项。  
单独考察 $\binom{n}{p^k}$ ：
$$\binom{n}{p^k} = \frac{n}{p^k} \binom{n-1}{p^k-1} = c \binom{n-1}{p^k-1}$$
由于 $p \nmid c$，有 $v_p(c) = 0$。接着分析后面的组合数：
因为 $n \equiv 0 \pmod{p^k}$，所以 $n-1 \equiv -1 \pmod{p^k}$。
这意味着在 $p$ 进制下，$n-1$ 的最低 $k$ 位全是 $p-1$。而 $p^k-1$ 在 $p$ 进制下刚好也是连续的 $k$ 个 $p-1$。
根据 Lucas 定理（或者由 Kummer 定理可知减法不产生借位），我们在 $p$ 进制下计算对应的组合数时，最低的 $k$ 位分别对应 $\binom{p-1}{p-1} = 1$，不会被 $p$ 整除，因此：
$$\binom{n-1}{p^k-1} \not\equiv 0 \pmod p \implies v_p\left(\binom{n-1}{p^k-1}\right) = 0$$
这就说明 $v_p\left(\binom{n}{p^k}\right) = 0$。既然序列中存在一项的估值为 $0$，那么整个 $\gcd$ 的 $p$ -进估值必然为 $0$，与公式完美吻合。
#### 情况 2：$m < p^k$
此时 $v_p(L_m) = \lfloor \log_p m \rfloor < k$。我们要证明的理论最小估值应为 $k - v_p(L_m)$。
对于任意 $1 \le i \le m$，由于 $m < p^k$，必有 $i < p^k$，即 $i-1 < p^k-1$。
利用吸收公式：
$$\binom{n}{i} = \frac{n}{i} \binom{n-1}{i-1}$$
两边同时取 $p$ -进估值可得：
$$v_p\left(\binom{n}{i}\right) = v_p(n) - v_p(i) + v_p\left(\binom{n-1}{i-1}\right) = k - v_p(i) + v_p\left(\binom{n-1}{i-1}\right)$$
在 $p$ 进制表示下，$i-1$ 的位数不超过 $k$ 位，且每一位上的数字显然都 $\le p-1$。
如情况 1 所述，$n-1$ 的最低 $k$ 位全是 $p-1$。所以 $i-1$ 的每一位数字都不超过 $n-1$ 对应位上的数字。
再次由 Lucas 定理可得：
$$\binom{n-1}{i-1}\not\equiv0\pmod p\implies v_p\left(\binom{n-1}{i-1}\right)=0$$
因此，对于所有的 $1 \le i \le m$，均有严格的等式：
$$v_p\left(\binom{n}{i}\right) = k - v_p(i)$$
我们对其求最小值：
$$\min_{i=1}^m v_p\left(\binom{n}{i}\right)=k-\max_{i=1}^mv_p(i)=k - v_p(L_m)$$
这与公式 $\max(0, k - v_p(L_m))$ 再次完全吻合。
综合以上两种情况，我们严格证明了对于所有素数 $p$：
$$v_p(F(n,m)) = \max(0, v_p(n) - v_p(L_m))$$
把它“合并”起来，等价于那个“优美的等式”：
$$F(n,m) = \frac{n}{\gcd(n, L_m)}$$
由于 $a$ 与 $b$ 互素，常数 $L_m$ 与它们乘积的最大公约数可以被完全拆分：
$$\gcd(ab, L_m) = \gcd(a, L_m) \cdot \gcd(b, L_m)$$
将其代入 $F$ 的闭式解公式中：
$$
F(ab,m)=\frac{ab}{\gcd(ab,L_m)}=\left(\frac{a}{\gcd(a,L_m)}\right)\left(\frac{b}{\gcd(b,L_m)}\right) = F(a,m)F(b,m)
$$
所以得出对于相同的 $m$，$F(n,m)$ 关于 $n$ 必然是积性函数。
所以先对 $n$ 进行质因数分解，然后对于每一个质因子的幂分别处理最后乘在一起即可。 
具体的时间复杂度取决于质因数分解的效率，如果使用普通的方法即为 $\mathcal{O}(\sqrt{n})$，使用 Pollard-rho 即可做到 $\mathcal{O}(n^{\frac{1}{4}})$。  
然而卡这个没有任何实际价值，所以 $\mathcal{O}(\sqrt{n})$ 即可通过本题。
## B：蜀道难（road）
### Hint 1
考虑原图是一棵树的情况。  
### Hint 2
考虑环上的两个点。
### Hint 3
**任意一个长度为 $|R|$ 的环 $R$ 上随便剖出一条长为 $|R|-2$ 的链，链的异或和都是 $0$。**
不难发现，若 $u,v$ 之间有连边，并且其还有其他路径可互相到达的话，就会构成一个环。  
由于同一个点对 $u,v$，从 $u$ 到 $v$ 的所有路径的点权异或和都是同一个值，所以说 $p_u\oplus p_v$ 的值等于 $p_u\oplus p_v$ 再异或上环上所有其他节点的异或和。  
这就意味着环上所有其他节点的异或和是 $0$。 
对于这个环上的任意相邻点对来说都是一样的，所以这个性质得证。
### Hint 4
**任意环的异或和都是 $0$。**
相当于把 Hint 3 做了推广。  
如果环 $R$ 的异或和不是零，又因为 Hint 3，所以这意味着每一个相邻点对的异或和都不是 $0$。
但是，你考虑环上两个点 $u,v$ 之间隔了一个点 $c$ 的情况，那么显然 $p_c$ 等于环上 $u,v,c$ 的补的异或和。  
我们让 $p_c$ 和环上 $u,v,c$ 的补同时异或上 $p_u$，显然环上 $u,v,c$ 的补会和 $u$ 构成一条长为 $|R|-2$ 的链，异或和为零。  
然而 $p_c\oplus p_u$ 根据前面的反设，不是 $0$，所以得到矛盾。  
因此任意环的异或和都是 $0$。
### Hint 5
**任意奇环上的所有点权都是 $0$。**
首先，根据 Hint 4，显然奇环上的所有点点权都相等。  
所以只能全都是 $0$，因为是奇数个。
### Hint 6
**任意偶环上的所有点权都相等。**
这个直接反证即可得到。
### Solution
最终得到推论：
1. 如果某个奇环和偶环有交，那么两个环的并上的所有点都是 0。
2. 如果某个偶环和偶环有交，那么两个环的并上的所有点权都相等。
有了这几个性质，答案呼之欲出。  
对原图跑 Tarjan 求出每一个边双连通分量，因为边双连通分量本质上就是若干环的并。
对于每一个边双联通分量，如果不是二分图，那么这个边双上所有点都是 $0$。  
如果是二分图，那么这个边双上所有点权都相等。  
判二分图就是找奇环，BFS 黑白染色即可。  

最后，关于已经钦定点权的点，那个其所在的整个边双的点权都与它相等。（除非它不是 $0$ 但是其所在边双有奇环，此时原图不合法，填充方案数为 $0$）  
没有钦定的就乘法原理即可。  
所以最终答案是 $V$ 的【没被钦定的二分图边双个数】次方。
## D：追忆（recall）
### Solution
不难看出以下性质成立：
-   对于某个子序列 $T$ 和下标 $i$ 满足 $i \notin T$，假设对 $T$ 的查询结果为 $0$，而对 $T \cup {i}$ 的查询结果为 $x \neq 0$。那么，我们获得信息 $a_i=x$。
为利用这一性质，我们维护一个已知查询结果为 $0$ 的子序列 $S$。考虑如下子过程：
-   查询子序列 $S \cup {i}$ 并得到结果 $x$。
-   若结果非零，则我们知道 $a_i=x$。
-   否则，令 $S \leftarrow S \cup {i}$。
若对 $i=1,2,\ldots,2n$ 依次执行此子过程，我们便能获得所有**第二次出现**的 $a_i$ 值的信息。现在需要找出其余 $n$ 个元素的值。
注意此时子序列 $S$ 恰好包含 $1,2,\ldots,n$ 各**一次**，而 $[1,2n] \setminus S$ 也**同样如此**。因此，我们令 $S \leftarrow ([1,2n] \setminus S)$，并再次对所有尚未发现的下标执行该子过程。子过程的每次运行都会返回一个非零结果，并在额外 $n$ 次查询中给出 $n$ 个未发现元素的值。
因此，该解法恰好使用 $3n$ 次查询（或者如果你愿意，也可以是 $3n-1$ 次）来发现全部 $2n$ 个元素的值。
### Bonus
调用函数 `bonus("The past is never dead, it’s not even past.")` 即可直接通过。 
如何猜出彩蛋：根据题目名称“追忆（recall）”，不难联想到“过去”，再根据最大重复出现数的定义名称 “$\mathrm{RL}$” 容易注意到其全称为 “**R**usty **L**ake”，结合“过去”即可得到该字符串。  
## E：孔乙己（Kong）
### Hint 1
整棵树最多在第【树高】时刻变为全 $0$，同时考虑每个节点的贡献。
### Hint 2
考虑树形 DP，并思考朴素做法，然后考虑优化。  
### Solution
首先，显然的做法是，设 $dp_{u,c}$ 为：时间为 $c$ 时，节点 $u$ 的值。  
直接根据题面即可显然得到 $dp_{u,0}=a_u,dp_{u,c}=[\displaystyle\sum_{v\in\mathrm{son}_u}dp_{v,c-1}\in \text{odd}]$。  
最终所求答案即为 $\displaystyle\sum_{i=1}^n\sum_{c}dp_{u,c}$。  
我们可以简单地发现，该式直接暴力转移在的最坏复杂度是 $O(n^2)$，在原图是一条链的情况下取到该复杂度。  
可以将 $[\displaystyle\sum_{v\in\mathrm{son}_u}dp_{v, c-1}\in \text{odd}]$ 视作与之完全等价的 $\displaystyle\bigoplus_{v\in\mathrm{son}_u}dp_{v,c-1}$。  
由于异或具有结合律，故我们可以直接将推导式改为 $dp_{u,c}=\displaystyle\bigoplus_{\substack{\mathrm{dis}(v,u)=c \\  v\in\mathrm{subtr}_u}}a_v$，发现这是一个可直接计算的式子。  
因此我们考虑直接设 $f_u=\displaystyle\sum_{c}dp_{u,c}$，有 $f_u=\displaystyle\sum_{c}\displaystyle\bigoplus_{\substack{\mathrm{dis}(v,u)=c \\  v\in\mathrm{subtree}_u}}a_v$。    
这个式子对于每一个节点，直接暴力计算，复杂度等于 $\mathcal{O}(\displaystyle\sum_{i=1}^n |\mathrm{subtree}_i|)$。  
可以证明，对于一个用 Prüfer 序列构造出的随机树，这个式子等于 $\mathcal O(n^{\frac 3 2})$。  
证明比较困难且繁琐，这里就不放了，感兴趣的同学请自行查阅[原论文]([https://www.cambridge.org/core/journals/journal-of-the-australian-mathematical-society/article/on-the-height-of-trees/C30DF2A9C26541526D847E0A0D46F2A8](https://www.cambridge.org/core/journals/journal-of-the-australian-mathematical-society/article/on-the-height-of-trees/C30DF2A9C26541526D847E0A0D46F2A8))。    
然而，如果原树退化成一条链，容易得出该复杂度会变成 $\mathcal{O}(n^2)$，无法接受。  
如果你不会太多算法，容易得到一个简单的办法，考虑一个“孤链压缩”的优化方法，如果一个点只有一个儿子，那么此时显然可以直接用儿子的信息转移，不需要重新计算，发现这种优化方法可以避免在极其长的一条单链上做无效的计算。  
这可以帮助我们多通过几个点，然而卡这种优化的方法很简单，只需要一条链，链上每个点再多挂一个儿子就可以避免被优化。  
如果你学得算法比较多，可以发现此时其实已经几乎是长链剖分优化 DP 的模板了。  
我们回到 $dp_{u,c}=\displaystyle\bigoplus_{v\in\mathrm{son}_u}dp_{v,c-1}$ 这一步。  
先对树做长链剖分，要求同一条长链上的节点的 DFN 序是连续的。
此时 用到一个技巧，我们不把 $dp$ 开成一个二维数组，而是考虑建立一个
## F：滕王阁序（elas）
### Hint 1
注意到题目名称翻转后得到 “sale”