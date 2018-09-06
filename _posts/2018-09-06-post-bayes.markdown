---
layout:     post
title:      "朴素贝叶斯"
subtitle:   " \"机器学习-自然语言处理\""
date:       2018-09-06 10:00:00
author:     "lang"
header-img: "http://lyang-blog-pics.oss-cn-shanghai.aliyuncs.com/post-bg-2017/0330/170330.jpg"

catalog: true
tags:
    - Tech
---

参考文章：  
[条件概率，全概率，贝叶斯公式理解](https://www.jianshu.com/p/c59851b1c0f3)  
[极大似然估计详解](https://blog.csdn.net/zengxiantao1994/article/details/72787849)

# 引子

夏天，某公园男性穿拖鞋的概率为 **1/2**，女性穿拖鞋的概率为 **2/3**，公园的男女比例为 **2:1**  
问题：假如在公园随机碰到一个穿拖鞋的路人，请问路人的性别为男性或女性的概率分别多少  

假设，$$w_1=$$男性，$$w_2=$$女性，$$x=$$穿拖鞋，则问题转化成求后验概率$$P(w_1 \mid x)$$和$$P(w_2 \mid x)$$。根据题意可知  
先验概率：是男性的概率$$P(w_1) = \frac{2}{3}$$，是女性的概率$$P(w_2) = \frac{1}{3}$$  
条件概率：男性穿拖鞋的概率$$P(x\mid w_1) = \frac{1}{2}$$，女性穿拖鞋的概率$$P(x\mid w_2) = 2/3$$  
男女穿拖鞋的事件相互独立，于是公园里穿拖鞋的概率$$P(x) = P(x\mid w_1)P(w_1) + P(x\mid w_2)P(w_2) = \frac{5}{9}$$  
根据 **条件概率计算公式**  
<center>$$P(Y \mid X) = \frac{P(X \mid Y)P(Y)}{P(X)}$$</center>  
可知  
$$P(w_1 \mid x) = \frac{P(x\mid w_1)P(w_1)}{P(x)} = \frac{3}{5}$$  
$$P(w_2 \mid x) = \frac{P(x\mid w_2)P(w_2)}{P(x)} = \frac{2}{5}$$  

# 贝叶斯公式及其推导

贝叶斯公式：  
<center>$$P(B_i \mid A) = \frac{P(B_i)P(A \mid B_i)}{\sum_{k=1}^{n}P(B_k)P(A \mid B_k)}$$</center>  



# 贝叶斯分类问题

朴素贝叶斯分类估计的原理是 **最大化后验概率** $$P(Y \mid X)$$，计算后验概率需要已知 **条件概率** $$P(X | Y)$$ 和 **先验概率** $$P(Y)$$，其中X为样本特征空间，Y为类标记空间。





