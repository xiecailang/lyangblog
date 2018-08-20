---
layout:     post
title:      "括号生成"
subtitle:   " \"Leetcode\""
date:       2018-08-20 10:00:00
author:     "lang"
header-img: "http://lyang-blog-pics.oss-cn-shanghai.aliyuncs.com/post-bg-2017/0330/170330.jpg"

catalog: true
tags:
    - Tech
---

# Problem

给出 n 代表生成括号的对数，请你写出一个函数，使其能够生成所有可能的并且有效的括号组合。  
例如，给出 n = 3，生成结果为：

    [
    "((()))",
    "(()())",
    "(())()",
    "()(())",
    "()()()"
    ] 

# 思路

问题等价于从左往右，左括号总要大于等于右括号，所以，用递归来做

# code

```python
class Solution:
    def dfs(self, left, right, temp, ans):
        if left < right:
            return
        if left == 0 and right == 0:
            ans.append(temp)
        else:
            if left > 0:                
                self.dfs(left - 1, right, temp + ')', ans)
            if right > 0:                
                self.dfs(left, right - 1, temp + '(', ans)

    def generateParenthesis(self, n):
        """
        :type n: int
        :rtype: List[str]
        """
        ans = []
        temp = ''
        self.dfs(n, n, temp, ans)
        return ans
        
    
#print(3//2)
sol = Solution()
n = int(input())
print(sol.generateParenthesis(n))
```