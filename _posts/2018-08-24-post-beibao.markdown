---
layout:     post
title:      "背包问题"
subtitle:   " \"Leetcode\""
date:       2018-08-24 10:00:00
author:     "lang"
header-img: "http://lyang-blog-pics.oss-cn-shanghai.aliyuncs.com/post-bg-2017/0330/170330.jpg"

catalog: true
tags:
    - Tech
---

# Problem

动态规划求解背包问题  

    输入：
    Knapsack Max weight : W = 10 (units)
    Total items         : N = 4
    Values of items     : v[] = {10, 40, 30, 50}
    Weight of items     : w[] = {5, 4, 6, 3}

# 思路

[参考本文](http://www.importnew.com/13072.html)  

items | w_0 | w_1 |w_2 |w_3 |w_4 |w_5 |w_6 |w_7 |w_8 |w_9 |w_10
item 0| 0 | 0 |0 |0 |0 |0 |0 |0 |0 |0 |0
item 1| 0 | 0 |0 |0 |0 |10 | | | | |
item 2|  |  | | | | | | | | |
item 3|  |  | | | | | | | | |
item 4|  |  | | | | | | | | |

算法流程：

1. 初始化dp[n+1][m+1] = 0, dp表示放第i个物品时可以达到的最大价值
2. 从第2行开始判断
    1. 背包容量w_j, j = 0, 1, ..., 10. w_j是否允许放当前物品i，即weight[i] \< w_j
        1. 不能存放，将前一行i-1的最大价值复制到当前的dp[i][j]
        2. 可以存放，计算存放当前物品后剩余容量remain = w_j - weight[i],可以知道dp[i-1][remain]保存的就是容量为remain可以存放的最大价值。比较value[i] + dp[i-1][remain]和前一行的最大价值dp[i-1][j]，取最大值作为dp[i][j]的值
    2. 循环上面的步骤一直到放完所有物品
3. 显然，最大价值就是dp最后一个元素

# 代码

```python
#背包问题
[n, w] = [int(i) for i in input().split()]
values = [int(i) for i in input().split()]
weight = [int(i) for i in input().split()]

dp = [([0] * (w+1)) for i in range(n+1)]

for i in range(1,n+1,1):
    for j in range(1,w+1,1):
        if weight[i - 1] <= j:
            remain_w = j - weight[i - 1]
            dp[i][j] = max(values[i - 1] + dp[i - 1][remain_w], dp[i - 1][j])
        else:
            dp[i][j] = dp[i-1][j]
print(dp)
```