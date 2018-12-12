---
layout:     post
title:      "匹配字符串和字符串数组"
subtitle:   " \"动态规划\""
date:       2018-12-12 10:00:00
author:     "lang"
header-img: "http://lyang-blog-pics.oss-cn-shanghai.aliyuncs.com/post-bg-2017/0330/170330.jpg"

catalog: true
tags:
    - Tech
---

# 问题

	输入：一个字符串S，一个字符串数组LIST，LIST已经去重。判断S是否可以由LIST的元素组成，LIST元素可以重复使用  
	比如：S="abc", LIST=["a", "bc", "c"]，S可以由"a", "bc"组成
	
# 思路

这是一个简单的动态规划问题，可在时间复杂度$$O(n^2)$$得到解。在match的过程中需要回溯，比如

	s = 'abcdef'
	lst = ['a', 'ab', 'c', 'abcde','f']
	
如果按照常规查找：

1. 'a'存在，后移
2. 'b'不存在，记录'b',后移
3. 'bc'存在，后移
4. 'd'不存在，记录'd'，后移
5. 'de'不存在，记录'de',后移
6. 'def'不存在，到达字符串尾，判断不能匹配

但是，很明显'abcdef'可以由'abcde'和'f'组成，这里面的逻辑错误在于**当前字符不存在list中并不能代表它和它前面的字符组成的子串也不存在**，因此解决问题的关键在于当前字符不存在时往前回溯，如果组成的子串存在且子串前面的子串也存在，则可以匹配  
用dp[]来记录从0到当前子串是否存在LIST中，初始化dp[0] = True。递推式可以写成  
<center>$$\text{dp}[i] = \text{dp}[i-1] \text{and} s[i] in \text{LIST}$$</center>  
当dp[i]==False时，从i往前寻找子串s[j:i]，如果s[j:i]存在且dp[j] == True，说明子串s[:i]是匹配的

# 代码

```python
def match(s, lst):
    dp = [False for _ in range(len(s) + 1)]
    dp[0] = True
    for i in range(1, len(s) + 1):
        dp[i] = dp[i - 1] and (s[i - 1] in lst)
        if not dp[i]:
            for j in range(i-2, -1, -1):
                if s[j:i] in lst and dp[j]:
                    dp[i] = True
                    break
    return dp[len(s)]

s = 'abcdef'
lst = ['a', 'ab', 'c', 'abcde','ef']
print(match(s, lst))
```
