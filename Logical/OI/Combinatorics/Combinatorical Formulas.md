1. (定义) $$
\binom{n}{m}=\binom{n-1}{m}+\binom{n-1}{m-1}
$$
2. $$
\sum_{i=0}^n \binom{n}{i}^2=\binom{2n}{n}
$$
3. (Catalan 数) $$
H_n=\sum_{i=0}^{n-1}H_iH_{n-1-i}=\frac{\binom{2n}{n}}{n+1}
$$
4. (二项式定理) $$
(xa+yb)^n=\sum_{i=0}^n \binom{n}{i}(xa)^i(yb)^{n-i}
$$
5. (Lucas 定理) $$
\binom{n}{m}\bmod p=\binom{\lfloor\frac{n}{p}\rfloor}{\lfloor\frac{m}{p}\rfloor}\times\binom{n\bmod p}{m\bmod p}\bmod p
$$
6. (Vandermonde 卷积)   
$$
\sum_{i=0}^k\binom{n}{i}\binom{m}{k-i}=\binom{m+n}k{}
$$
7. 
$$
\binom{n}{r}\binom{r}{k}=\binom{n}{k}\binom{n-k}{r-k}
$$
8. 
$$
\sum_{i=0}^n\binom{n-i}{i}=F_{n+1}
$$
9. (李善兰恒等式)
$$
\binom{n+k}{k}^2=\sum_{j=0}^k\binom{k}{j}^2\binom{n+2k-j}{2k}
$$
10. (二项式反演)
$$
g_n=\sum_{i=0}^nf_i\binom{n}{i}\implies f_n=\sum_{i=0}^n(-1)^{n-i}g_i\binom{n}{i}
$$
11. (Binominal Transform) $$
\sum_{i=0}^nf(i)\binom{n}{i}=\sum_{i=0}^nn^{\underline{i}}\frac{\Delta^if(0)}{i!}
$$
