---
layout:     post
title:      "爬楼梯问题"
subtitle:   " \"算法\""
date:       2018-09-11 10:00:00
author:     "lang"
header-img: "http://lyang-blog-pics.oss-cn-shanghai.aliyuncs.com/post-bg-2017/0330/170330.jpg"

catalog: true
tags:
    - Tech
---

有M阶楼梯，一次可以爬1阶或者2阶：  
<center>$$f(n) = f(n-1) + f(n-2)$$</center>

每次可以爬1/2/3阶  
<center>$$f(n) = f(n-1) + f(n-2) + f(n-3)$$</center>

每次可以爬1/2/3/.../k阶  
<center>$$f(n) = f(n-1) + f(n-2) + f(n-3) + ... + f(n - k) = \sum_{i = 1}^{k}f(n - i)$$</center>

每次可以爬$$2^0/2^1/2^2/.../2^k$$  
<center>$$f(n) = f(n-2^0) + f(n-2^1) + f(n-2^2) + ... + f(n - 2^k) = \sum_{i = 0}^{k}f(n - 2^i)$$</center>

```python
#爬楼梯，2^n
M = int(input())
N = []
for i in range(M):
    N.append(int(input()))
import math
for i in range(M):
    cur_n = N[i]
    dp = [1 for j in range(cur_n+1)]
    for j in range(2,cur_n + 1):
        sum_ = 0
        n = int(math.log2(j))
        k = 0
        while j - 2**k >=0:        
            sum_ += dp[j - 2**k]
            k += 1
        dp[j] = sum_
    print(dp[len(dp)-1])
```



