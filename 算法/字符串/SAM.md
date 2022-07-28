# SAM（后缀自动机）

## SAM
- $endpos$为字符串$s$在$t$中的结束位置
- 其后缀$link$为一颗树，如图所示![[Pasted image 20220727003220.png]]

## 例题
### 处理子串

- 子串出现次数：$endpos$集合大小
- 子串长度：$node[p].len$
- 子串出现次数：差分处理


[P3804 【模板】后缀自动机](https://www.luogu.org/problemnew/show/P3804)
求出 SS 的所有出现次数不为 11 的子串的出现次数乘上该子串长度的最大值。

[P5341 [TJOI2019]甲苯先生和大中锋的字符串](https://www.luogu.com.cn/problem/P5341)
[求出现 kk 次的子串中出现次数最多的长度。

[CF802I Fake News (hard)](https://www.luogu.com.cn/problem/CF802I) 求所有子串的出现次数平方和

[CF123D String](https://www.luogu.com.cn/problem/CF123D) 求所有子串的出现次数乘 (出现次数 + 1) 之和

[P3181 [HAOI2016]找相同字符](https://www.luogu.com.cn/problem/P3181)
给定两个字符串，求出在两个字符串中各取出一个子串使得这两个子串相同的方案数。两个方案不同当且仅当这两个子串中有一个位置不同。

### 不同子串个数

 [P4070 [SDOI2016]生成魔咒](https://www.luogu.com.cn/problem/P4070)
字符集很大，在线求本质不同子串个数。

建立$SAM$,在建立的同时计算出子串个数$\sum_u(len(p)-len(p.fa))$


[CF653F Paper task](https://www.luogu.com.cn/problem/CF653F)
给定长为 $n≤5×10^5$ 的括号串，问有多少本质不同的合法括号子串。

对于每个$s[i]=')'$，查询$[1,i-1]$中合适的左括号有多少个，对于每个括号深度开一个线段树维护

### 子串匹配

[P3808 【模板】AC自动机（简单版）](https://www.luogu.com.cn/problem/P3808)问多少模式串出现过。
[P3796【模板】AC 自动机（加强版](https://www.luogu.com.cn/problem/P3796)问出现次数最多的模式串
[P5357 【模板】AC自动机（二次加强版）](https://www.luogu.com.cn/problem/P5357)问每个模式串的出现次数。

以上SAM板子即可(但会被卡MLE)

[CF235C Cyclical Quest](https://www.luogu.com.cn/problem/CF235C)给定一个主串 $s$ 和 $n$个询问串,求每个询问串的所有循环同构在主串中出现的次数总和，

询问串断环为链即可

### 第$K$小子串

[P3975 [TJOI2015]弦论](https://www.luogu.com.cn/problem/P3975)

求本质不同/位置不同的 $k$ 小子串

### LCS问题

#### 两串

[SP1811 LCS - Longest Common Substring](https://www.luogu.com.cn/problem/SP1811)。求 SS 和 TT 的最长公共子串。

#### 多串

[最长公共子串](https://www.acwing.com/problem/content/2813/)求多个串的LCS


# (后续待更新)



