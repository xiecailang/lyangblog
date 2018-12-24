---
layout:     post
title:      "最长共同子序列"
subtitle:   " \"算法-动态规划\""
date:       2018-09-16 10:00:00
author:     "lang"
header-img: "http://lyang-blog-pics.oss-cn-shanghai.aliyuncs.com/post-bg-2017/0330/170330.jpg"

catalog: true
tags:
    - Tech
---

# problem

给定两个字符串，求解这两个字符串的最长公共子序列（Longest Common Sequence）。  
比如字符串1：BDCABA；字符串2：ABCBDAB  
则这两个字符串的最长公共子序列长度为4，最长公共子序列是：BCBA  
注意公共子序列并不一定是连续的

# 思路

参考[求解两个字符串的最长公共子序列](https://www.cnblogs.com/hapjin/p/5572483.html)  
将字符串$$X = {x_1, x_2, ..., x_n}$$和$$Y = {y_1, y_2, ..., y_m}$$的最长公共子序列记为$$\mathrm{LCS}(X, Y)$$  
首先考虑$$X$$和$$Y$$的最后一个元素  

1. 如果$$x_n == y_m$$，那么他们最后一个元素肯定在最长公共子序列中，我们接下去再找$$\mathrm{LCS}(X_{n-1}, Y_{m-1})$$，并且最长公共子序列 = $$\mathrm{LCS}(X_{n-1}, Y_{m-1}) + 1$$
2. 如果$$x_n != y_m$$，即最后一个元素不在最长公共子序列中。这里会有两个子问题，一个是找$$\mathrm{LCS}(X_{n - 1}, Y_m)$$，另外一个是$$\mathrm{LCS}(X_n, Y_{m - 1})$$，我们取这两个的最大值最为最长公共子序列

因此，可以写出以下的递归公式  
<center>$$LCS(X_i, Y_j) = \left\{  \begin{array}{ll}  0 & \text{if $i = 0$ or $y = 0$} \\  LCS(X_{i - 1}, Y_{j - 1}) & \text{if $X_i == Y_j$} \\  \mathrm{max}\{LCS(X_{i - 1}, Y_j), LCS(X_i, Y_{j - 1})\} & \text{else} \end{array}  \right.$$</center>  

以上明显可以使用递归来实现，但是会产生重复求解问题，原问题包含了3个子问题

1. $$\mathrm{LCS}(X_{n-1}, Y_{m-1})$$
2. $$\mathrm{LCS}(X_{n - 1}, Y_m)$$
3. $$\mathrm{LCS}(X_n, Y_{m - 1})$$

第二个子问题$$\mathrm{LCS}(X_{n - 1}, Y_m)$$包含了

1. $$\mathrm{LCS}(X_{n-2}, Y_{m-1})$$
2. $$\mathrm{LCS}(X_{n-2}, Y_{m})$$
3. $$\mathrm{LCS}(X_{n-1}, Y_{m-1})$$

显然$$\mathrm{LCS}(X_{n-1}, Y_{m-1})$$重叠了，因此可以采用动态规划求解，使用动态规划后，重叠的子问题直接通过**查表**得到，而不是重新计算一遍。

# 代码

```python
#最长公共子序列
s1, s2 = input(), input()
dp = [[0 for i in range(len(s1) + 1)] for j in range(len(s1) + 1)]
for i in range(1, len(s1) + 1):
    for j in range(1, len(s2) + 1):
        if s1[i-1] == s2[j-1]:
            dp[i][j] = dp[i - 1][j - 1] + 1
        else:
            dp[i][j] = max(dp[i - 1][j], dp[i][j - 1])
print(dp[len(s1)][len(s2)])
```
