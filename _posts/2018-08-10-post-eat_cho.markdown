---
layout:     post
title:      "贪吃的小Q"
subtitle:   " \"笔试真题\""
date:       2018-08-10 10:00:00
author:     "lang"
header-img: "http://lyang-blog-pics.oss-cn-shanghai.aliyuncs.com/post-bg-2017/0330/170330.jpg"

catalog: true
tags:
    - Tech
---

# 题目

[链接](https://www.nowcoder.com/questionTerminal/d732267e73ce4918b61d9e3d0ddd9182?orderByHotValue=1&page=1&onlyReference=false)

小Q的父母要出差N天，走之前给小Q留下了M块巧克力。小Q决定每天吃的巧克力数量不少于前一天吃的一半，但是他又不想在父母回来之前的某一天没有巧克力吃，请问他第一天最多能吃多少块巧克力  

输入描述:  
每个输入包含一个测试用例。每个测试用例的第一行包含两个正整数，表示父母出差的天数N(N<=50000)和巧克力的数量M(N<=M<=100000)。

输出描述:  
输出一个数表示小Q第一天最多能吃多少块巧克力。

示例1

    输入
    3 7
    输出
    4

# 思路

二分查找的变形，详见[算法题16 贪吃的小Q 牛客网 腾讯笔试题](http://www.cnblogs.com/yanmk/p/9313840.html)

# 代码

```python
#贪吃的小Q
n, m =[int(i) for i in input().split()]
#第一天吃掉s，总共需要多少巧克力
def cal_sum(s):
    t_sum = 0
    for i in range(n):
        t_sum += s
        s = (s + 1)//2 #向上取整
    return t_sum

low, high = 1, m
while low <= high:
    mid = (low + high)//2
    if cal_sum(mid) == m:
        print(mid)
        break
    elif cal_sum(mid) < m:
        low = mid + 1
    else:
        high = mid - 1
if low > high:
    print(low - 1)
```