---
layout:     post
title:      "不同的子序列"
subtitle:   " \"Leetcode\""
date:       2018-12-18 10:00:00
author:     "lang"
header-img: "http://lyang-blog-pics.oss-cn-shanghai.aliyuncs.com/post-bg-2017/0330/170330.jpg"

catalog: true
tags:
    - Tech
---

# 问题

给定一个字符串 S 和一个字符串 T，计算在 S 的子序列中 T 出现的个数。  
一个字符串的一个子序列是指，通过删除一些（也可以不删除）字符且不干扰剩余字符相对位置所组成的新字符串。（例如，"ACE" 是 "ABCDE" 的一个子序列，而 "AEC" 不是）

示例 1:

      输入: S = "rabbbit", T = "rabbit"
      输出: 3
      解释:
      如下图所示, 有 3 种可以从 S 中得到 "rabbit" 的方案。
      (上箭头符号 ^ 表示选取的字母)
      rabbbit
      ^^^^ ^^
      rabbbit
      ^^ ^^^^
      rabbbit
      ^^^ ^^^

# 分析

**动态规划**问题，对于上面的实例可以分为下面几步分析

1. 如果S和T长度相等，那么只有S==T一种方法
2. 如果S和T长度不相等，又可以分为以下几种情况：
  * 比如S='rab'，T='ra'，那么对于S删除或者加上‘b’都不会影响匹配数，而匹配数就是S='ra'和T='ra'的匹配，只有1种方法
  * 假设S='rabb'，T='rab'，那么对于S的最后一个'b'的处理就会影响匹配数了，利用**动态规划**的思想，有两种方法可以匹配：第一，删除最后一个'b'，那么匹配数就等于S='rab'与T='rab'的匹配数。第二，保留S的最后一个'b'，那么匹配数就等于S='ra'与T='ra'的匹配数。两者相加就是整体的匹配数

对于S='rabbb'和T='rab'的处理，类似于爬楼梯的问题，要得到'rab','rabbb'首先爬完'ra',然后他可以选择爬到第一个'b'到终点，也可以通过爬过第二个'b'或者第三个'b'。

# 算法

1. dp[][] = [len(s)+1][len(t)+1]
2. 初始化dp[0][] = 1，即对于T=''，无论S是什么都可以通过全部删除来的到T这一个种方法
3. for i, j in S, T
  * 对于i==j，如果S[:i] = T[:j],那么dp[i][j] = 1，否则dp[i][j] = 0
  * 否则，
    * 如果S[i-1] != T[i - 1], dp[i][j] = dp[i-1][j]
    * 否则，dp[i][j] = dp[i-1][j] + dp[i-1][j]
4. 返回dp[len(s)][len(t)]

# 代码

```python
class Solution:
    def numDistinct(self, s, t):
        """
        :type s: str
        :type t: str
        :rtype: int
        """
        dp = [[0 for _ in range(len(t) + 1)] for k in range(len(s)+1)]
        for i in range(len(s)+1):
            dp[i][0] = 1
        for j in range(1, len(t) + 1, 1):
            for i in range(1, len(s) + 1, 1):
                if i == j:
                    dp[i][j] = (1 if s[:i] == t[:j] else 0)
                else:
                    if s[i - 1] == t[j - 1]:
                        dp[i][j] = dp[i - 1][j - 1] + dp[i - 1][j]
                    else:

                        dp[i][j] = dp[i - 1][j]
        print(dp)
        return dp[len(s)][len(t)]
```

还有一种更高效方法，通过字典来实现，思路大致通过代码可以看出，唯一关键的是第二层for对访问顺序进行了反转(reversed)

```python
class Solution:
    def numDistinct(self, s, t):        
        """
        :type s: str
        :type t: str
        :rtype: int
        """
        char2idx = {}
        for idx, char in enumerate(t):
            if char not in char2idx:
                char2idx[char] = []
            char2idx[char].append(idx)

        dp = [0] * len(t)
        for char in s:          
            for idx in reversed(char2idx.get(char, [])):
                dp[idx] += dp[idx-1] if idx-1 >= 0 else 1

        return dp[-1]
```
