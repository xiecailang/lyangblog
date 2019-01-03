---
layout:     post
title:      "快速排序"
subtitle:   " \"算法\""
date:       2019-01-03 10:00:00
author:     "lang"
header-img: "http://lyang-blog-pics.oss-cn-shanghai.aliyuncs.com/post-bg-2017/0330/170330.jpg"

catalog: true
tags:
    - Tech
---

# 快速排序

平均复杂度O(nlogn),最差时间复杂度$$n^2$$  
排序思想：选定key，通常为第一个元素，通过交换将小于key的元素放到key的左边，大于key的放到右边，再分别对左边和右边的子数组排序，是一个迭代的过程。  

1. 如果数组只有一个元素，结束排序
2. 确定key,left和right指向数组的开始和结尾
3. 比较left处元素与key大小并向后移动，直到与right相遇或者遇到大于key的元素
4. 比较right处元素与key大小并向前移动，直到与left相遇或者遇到小于key的元素
5. 如果left与right没有相遇则交换他们的元素，跳转step3，直到left与right相遇
6. 将key放在分开的数组中间
7. 将key左边的子数组排序，跳转到step1
8. 将key右边的子数组排序，跳转到step1

# 代码

```python
def qsort(a, low, high):
    if low >= high:
        return
    l, r = low, high
    key = a[high]
    while l < r:
        while l < r and a[l] <= key:
            l += 1
        while l < r and a[r] >= key:
            r -= 1
        if l < r:
            a[r], a[l] = a[l], a[r]
    a[r], a[high] = key, a[r]
    qsort(a, 0, l-1)
    qsort(a, r+1, high)

a = [3, 5, 8, 1, 2, 9, 4, 7, 6]
qsort(a, 0, len(a) - 1)
print(a)
```

# 快排与堆排

都属于**分治法**思想的排序算法  
**堆排序比较和交换次数比快速排序多**，所以平均而言比快速排序慢，也就是常数因子比快速排序大，如果你需要的是“排序”，那么绝大多数场合都应该用快速排序而不是其它的O(nlogn)算法。  
但有时候你要的不是“排序”，而是另外一些与排序相关的东西，比如最大/小的元素，topK之类，这时候堆排序的优势就出来了。用堆排序可以在N个元素中找到top K，时间复杂度是O(N log K)，空间复杂的是O(K)，而快速排序的空间复杂度是O(N)，也就是说，如果你要在很多元素中找很少几个top K的元素，或者在一个巨大的数据流里找到top K，快速排序是不合适的，堆排序更省地方。  
另外一个适合用heap的场合是优先队列，需要在一组不停更新的数据中不停地找最大/小元素，快速排序也不合适