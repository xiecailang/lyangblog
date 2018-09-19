---
layout:     post
title:      "编辑距离（Edit distance）"
subtitle:   " \"Leetcode\""
date:       2018-09-19 10:00:00
author:     "lang"
header-img: "http://lyang-blog-pics.oss-cn-shanghai.aliyuncs.com/post-bg-2017/0330/170330.jpg"

catalog: true
tags:
    - Tech
---

# 问题

给定两个单词 word1 和 word2，计算出将 word1 转换成 word2 所使用的最少操作数 。  
你可以对一个单词进行如下三种操作：  

1. 插入一个字符
2. 删除一个字符
3. 替换一个字符
   
**示例 1:**  
    输入: word1 = "horse", word2 = "ros"
    输出: 3
    解释: 
    horse -> rorse (将 'h' 替换为 'r')
    rorse -> rose (删除 'r')
    rose -> ros (删除 'e')

# 应用

通常作为一种相似度计算函数，在自然语言处理（NLP）中应用较多，比如**拼写纠错**，**DNA分析**（局部序列比对），命名实体抽取（缩写与全称），实体共指，机器翻译等等，更详细参考[斯坦福大学自然语言处理第三课“最小编辑距离（Minimum Edit Distance）](http://52opencourse.com/96/斯坦福大学自然语言处理第三课-最小编辑距离%EF%BC%88minimum-edit-distance%EF%BC%89)

# 结题思路

**动态规划**求解此问题，此问题可以分为3个子问题，假设$$D(i, j)$$表示$$word1$$前$$[0, 1, \cdot, i]$$个字符串转换成$$word2$$前$$[0, 1, \cdot, j]$$个字符串所需要的最小操作数，那么$$D(i, j)$$可以由以下3个子问题得到

1. $$word1$$的前$$i-1$$个字符串与$$word2$$的前$$j$$个字符串已经操作完成，最小操作数为$$D(i-1, j)$$，那么删除$$word1$$的第$$i$$个字母有可能使得$$word1$$前$$i$$个字符串与$$word2$$前$$j$$个字符串相同，操作数为1
2. $$word1$$的前$$i$$个字符串与$$word2$$的前$$j-1$$个字符串已经操作完成，最小操作数为$$D(i, j-1)$$，那么在$$word1$$的第$$i$$插入一个字母有可能使得$$word1$$前$$i$$个字符串与$$word2$$前$$j$$个字符串相同，操作数为1
3. $$word1$$的前$$i-1$$个字符串与$$word2$$的前$$j-1$$个字符串已经操作完成，最小操作数为$$D(i-1, j-1)$$，如果$$word1[i] \neq word2[j]$$那么将$$word1$$的第$$i$$个字母替换成$$word2$$的第$$j$$个字母，有可能使得$$word1$$前$$i$$个字符串与$$word2$$前$$j$$个字符串相同，操作数为1; 如果$$word1[i] = word2[j]$$那么不需要操作$$word1$$前$$i$$个字符串与$$word2$$前$$j$$个字符串已经相同，操作数为0

以上可以写出递归公式：  
<center>$$D(i, j) = \min\left\{\begin{array}{ll} D(i - 1, j) + 1 \\ D(i, j - 1) \\ D(i - 1, j - 1) + \& \left\{ \begin{array}{ll} 2 \& \textrm{if $word1[i] \neq word2[j]$} \\ 0 \& \textrm{if $word1[i] = word2[j]}}\end{array}\right \end{array}\right$$</center>

# 代码

```python
class Solution:
    def minDistance(self, word1, word2):
        """
        :type word1: str
        :type word2: str
        :rtype: int
        """
        m, n = len(word1), len(word2)
        dp = [[0 for i in range(n+1)] for j in range(m+1)]
        for i in range(m+1):
            for j in range(n+1):
                if i == 0:
                    dp[i][j] = j
                elif j == 0:
                    dp[i][j] = i
                else:
                    dp[i][j] = min(dp[i-1][j-1] + (0 if word1[i-1] == word2[j-1] else 1), dp[i-1][j] + 1, dp[i][j -1] + 1)
        return dp[m][n]
word1, word2 = input(),  input()
so = Solution()
print(so.minDistance(word1, word2))
```