---
layout:     post
title:      "与所有单词相关联的字串"
subtitle:   " \"Leetcode\""
date:       2018-09-14 10:00:00
author:     "lang"
header-img: "http://lyang-blog-pics.oss-cn-shanghai.aliyuncs.com/post-bg-2017/0330/170330.jpg"

catalog: true
tags:
    - Tech
---

# problem

给定一个字符串 s 和一些长度相同的单词 words。在 s 中找出可以恰好串联 words 中所有单词的子串的起始位置。  
注意子串要与 words 中的单词完全匹配，中间不能有其他字符，但不需要考虑 words 中单词串联的顺序。  
示例 1:  

    输入:
    s = "barfoothefoobarman",
    words = ["foo","bar"]
    输出: [0,9]

解释: 从索引 0 和 9 开始的子串分别是 "barfoor" 和 "foobar" 。  
输出的顺序不重要, [9,0] 也是有效答案。  
示例 2:  

    输入:
    s = "wordgoodstudentgoodword",
    words = ["word","student"]
    输出: []

# 思路

1. 判断*words*中单词是否长度相同，每个字符长度为*len*，不相同返回空，相同则将*word*保存到字典*dic1*中，*key*为单词，*value*为出现次数
2. 从 *j = 0* 开始，把字符串*s*分割成*m*个子串，每个子串长度为*len*。从*i = j*开始遍历子串，保存左边界*left*，取出分割后的单词*t = s[i:i+len]*做以下判断：
    1. 如果*t*在*dic1*中出现，将*t*保存在*dic2*中，如果*dic2[t]*已经存在，则*dic2[t] += 1*。
    2. 如果*dic2[t] <= dic1[t]*，则*cnt += 1*，否则将*left*右移到第一次出现单词*t*后一个单词的开始位置，并且*dic2[t] -= 1*
    3. 如果*t*没有在*dic1*中出现，则*cnt*清零，*dic2*清空，*left += len*
    4. 当*cnt = len(words)*说明匹配子串存在，*left*即子串开始位置
3. 循环执行步骤2，*j*取值为*[0, 1, 2, ..., len]*

# 代码

```python
class Solution:
    def findSubstring(self, s, words):
        """
        :type s: str
        :type words: List[str]
        :rtype: List[int]
        """
        if len(s) == 0 or len(words) == 0:
            return []
        dic_words = {}
        tar_words = {}
        len_ = len(words[0])
        ans = []
        for w in words:
            if w in dic_words:
                dic_words[w] += 1
            else:
                dic_words[w] = 1
            if len(w) != len_:
                return []
        for j in range(len_):
            cnt = 0
            left = j
            tar_words.clear()
            for i in range(j, len(s), len_):
                t = s[i:i+len_]
                
                if t in dic_words:
                    tar_words[t] = 1 if t not in tar_words else tar_words[t] + 1
                    if tar_words[t] <= dic_words[t]:
                        cnt += 1
                    else:
                        #左移边界
                        for k in range(left, i, len_):
                            k_w = s[k:k+len_]
                            if k_w == t:
                                left = k + len_
                                tar_words[k_w] -= 1
                                break
                            tar_words[k_w] -= 1
                            if tar_words[k_w] == 0:
                                tar_words.pop(k_w)
                            cnt -= 1
                        
                else:
                    cnt = 0
                    left = i+len_
                    tar_words.clear()
                if cnt == len(words):
                    ans.append(left)
                    s_word = s[left:left + len_]
                    tar_words[s_word] -= 1
                    if tar_words[s_word] == 0:
                        tar_words.pop(s_word)
                    cnt -= 1
                    left += len_
        return ans
s = input()
words = [s for s in input().split()]
so = Solution()
print(so.findSubstring(s, words))
```


