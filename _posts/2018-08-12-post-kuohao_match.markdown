---
layout:     post
title:      "括号匹配"
subtitle:   " \"笔试真题\""
date:       2018-08-12 10:00:00
author:     "lang"
header-img: "http://lyang-blog-pics.oss-cn-shanghai.aliyuncs.com/post-bg-2017/0330/170330.jpg"

catalog: true
tags:
    - Tech
---

# 问题

如果每个左括号都有一个右括号与之匹配，则这个序列就是括号匹配序列  
牛牛有一系列括号序列，要从这里面任意选两个位置进行交换操作，使之成为括号匹配序列

    输入示例：
    ())(
    )))(((
    输出：
    Yes
    No

# 思路

要使得交换一次，序列就变成括号匹配序列，说明序列中只有一对左右括号不匹配。按照匹配括号算法（栈），左括号入栈，遇到右括号出栈，如果遍历完字符串，只有1个或0个不匹配（栈空）的右括号，则说明只要交换一次即可

# 代码

```python
#括号匹配
str_ = list(input())
stack = []
cnt = 0
over_cnt = 0
for c in str_:
    if c == '(':
        stack.append(c)
    elif c == ')':
        if len(stack) > 0:
            stack.pop(-1)
        else:
            over_cnt += 1
    cnt += 1
print('Yes' if cnt == len(str_) and over_cnt <= 1 else 'No')
```