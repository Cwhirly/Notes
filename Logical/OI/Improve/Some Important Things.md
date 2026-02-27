开 longlong   
关注输入顺序  
加 endl   
线段树 build 不加  `` if (l<=mid) `` 
广搜加 vis 
注意函数有没有 return ，没有要用 void
 long long 下的 max 初始值为 $10^{18}$
线段树是 ``if(r>mid)`` 而不是 `else`
线段树一定要分清 $l$，$r$ 与 $s$，$t$！  
数组开大！
`string` 在前面加是 $O(n)$ 的。     