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

根据文氏图来推导  
<center>![img1][ab]</center>  
从图中可以看出，在已知事件B发生的情况下，事件A发生的概率   
<center>$$P(A \mid B) = \frac{P(A \cap B)}{P(B)}$$</center>  
因此，$$P(A \cap B) = P(A \mid B)P(B)$$  
同理可得，$$P(A \cap B) = P(B \mid A)P(A)$$  
联合可得条件概率计算公式$$ P(A \mid B) = \frac{P(B \mid A)P(A)}{P(B)}$$  

考虑整个样本空间S  
<center>![img2][abs]</center>  
除图中标注事件外，我们记$$A'$$为事件A没发生的概率，我们将事件B划分为两个部分$$P(B) = P(A \cap B) + P(A' \cap B)$$，上面已经推导了$$P(A \cap B) = P(B \mid A)P(A)$$，带入可得全概率$$P(B) = P(B \mid A)P(A) + P(B \mid A')P(A')$$，这就可以得到条件概率的另一种写法：  
<center>$$P(A \mid B) = \frac{P(B \mid A)P(A)}{P(B \mid A)P(A) + P(B \mid A')P(A')}$$</center>  
推广到$$n$$个事件于是得到了开始给出的贝叶斯公式


# 贝叶斯分类问题

朴素贝叶斯分类估计的原理是 **最大化后验概率** $$P(Y \mid X)$$，计算后验概率需要已知 **条件概率** $$P(X | Y)$$ 和 **先验概率** $$P(Y)$$，其中X为样本特征空间，Y为类标记空间。在已知这两个参数的条件下我们可以比较容易给目标分类。  
例如：
*已知好评跟差评的句子各40句，现给出一个句子，判断它是好评还是差评*  
计算步骤：  

1. 计算先验概率及条件概率：
    1.  假设$$G=$$好评，$$B=$$差评，则先验概率为$$P(G), P(B)$$，可以假设都为50\%，即0.5
    2.  假设输入语句中出现单词$$W_i$$，则计算条件概率需要统计好评中$$W_i$$出现的语句次数，比如在好评中有20句包含单词“good”，则条件概率$$P(W='good' \mid G) = 20/40 = 0.5$$
2. 对于给定的实例$$W = (w_1, w_2, ..., w_n)$$，计算后验概率
<center>$$P(G \mid W) = \frac{P(G)\prod_{i=1}^{n}P(W_i \mid G)}{\sum_{k = 1}^{n}P(G)\prod_{j=1}^{n}P(W_j \mid G)$$</center>  
对于所有的单词$$W_i$$，上式的分母都是一样的，所以只需要对比分子的大小  

**代码**

```python
good_ = [strs.lower() for strs in ["high cost performance","Great place","Have a good time","quite special","good place","easy of access","Very worth a visit", "tickets are cheap", "convenient traffic", "overall feels good", "worth to see", "very convenient traffic", "very fun", "a good place", "The ticket is cost-effective", "the overall feeling is good", "the place worth going", "the view is good", "the overall is not bad", "feel good", "the scenery is very good", "the scenery is not bad", "I like it very much", "Really good", "It's worth seeing", "The ticket is not expensive", "The ticket is very cheap", "The scenery is not bad", "The price is very good", "It's still very good", "The scenery is very good", "The environment is great", "The scenery is OK", "The air is very fresh", "Very worth seeing", "Very worth going", "Good view", "Good traffic"," Value this price", "good value for money"]]
bad_ = [strs.lower() for strs in ["low cost performance", "I don't prefer it", "Nothing fun", "Nothing special", "Nothing good-looking", "Traffic is not very convenient", "Nothing special", "Tickets are too expensive", "Traffic inconvenient", "The overall feeling is bad", "Nothing to play", "Never come again", "Nothing to see", "Feeling bad", "Tickets are a bit expensive", "very bad", "The scenery is bad", "There is nothing to watch", "I don't like it", "The price is a bit expensive", "cost performance is too low", "I feel not good", "not high cost performance", "Not very fun", "Tickets are not cheap ", "traffic is not convenient", "the scenery is not good", "nothing to play", "too commercial", "tickets are expensive", "fare is a bit expensive", "nothing to see", "price not cheap", "price is very high", "ticket too expensive", "The traffic is not good", "The ticket is too expensive", "The scenery is very poor", "Not worth the price", "The traffic is bad"]]
new_words = [strs.lower() for strs in input().split()]
good_prs = []
bad_prs = []
#计算条件概率
for w in new_words:
    good_pr = sum([1 if w in strs else 0 for strs in good_])/len(good_)
    if good_pr == 0:
        good_pr = 0.01 
    good_prs.append(good_pr)
    bad_pr = sum([1 if w in strs else 0 for strs in bad_])/len(bad_)
    if bad_pr == 0:
        bad_pr = 0.01 
    bad_prs.append(bad_pr)
p_G, p_B = 0.5, 0.5 #好评差评的先验概率
from functools import reduce
#计算后验概率分子部分
ans_p_good = reduce(lambda x,y:x*y, good_prs) * p_G
ans_p_bad = reduce(lambda x,y:x*y, bad_prs) * p_B
print('good' if ans_p_good > ans_p_bad else 'bad')
```

# 贝叶斯估计

在实际问题中，先验概率和条件概率往往不是能够直接获得的，解决的办法就是把位置的概率分布（先验概率和条件概率）转化为参数估计，而 **极大似然估计** 就是一种参数估计方法，极大似然估计的目的是利用已知的样本结果，反推最有可能导致这种结果的参数值。极大似然估计的过程是估计给定模型的参数，通过若干次实验结果得到的参数，能够使样本出现的概率最大。**其前提是样本集中所有的样本都是独立同分布的**  

对于样本集$$D = {x_1, x_2, ..., x_n}$$，估计参数向量$$\theta$$，联合概率密度函数$$P(D \mid \theta)$$称为相对于样本集$$D$$的$$\theta$$似然函数，写作：  
<center>$$l(\theta) = P(D \mid \theta) = P(x_1, x_2, ..., x_n \mid \theta) = \prod_{i = 1}^{N}P(x_i \mid \theta)$$</center>  
如果参数向量$$\hat{\theta}$$是参数空间中能够使得$$l(\theta)$$最大的$$\theta$$值，则$$\hat{\theta}$$就是$$\theta$$的极大似然估计量，他是关于样本集$${x_1, x_2, ..., x_n}$$的函数，记作$$\hat{\theta}(x_1, x_2, ..., x_n)$$.  
极大似然估计的步骤一般为  

1. 写出似然函数
2. 对似然函数取对数，即$$H(\theta) = \mathrm{ln}(l(\theta))$$，并整理
3. 求导数$$\frac{dH(\theta)}{d(\theta)} = 0$$
4. 解似然方程，求出参数向量$$\theta$$,

**但是**，极大似然估计可能会出现所要估计的概率值为0的情况，解决这一问题的方法是采用贝叶斯估计，有贝叶斯公式可以推导。这里直接给出先验概率和条件概率的贝叶斯估计方法

1. 先验概率  
<center>$$P_\lambda(Y = c_k) = \frac{\sum_{i = 1}^{N}I(y_i = c_k) + \lambda}{N+K\lambda}$$</center>

2. 条件概率  
<center>$$P_{\lambda}(X^{(J)} = a_{jl} \mid Y = c_k) = \frac{\sum_{i = 1}^{N}I(x_i^{(j)} = a_{jl}, y_i = c_k) + \lambda}{\sum_{i = 1}^{N}I(y_i = c_k)} + S_j\lambda$$</center>

上式中，$$\lambda \geq 0$$，当$$\lambda = 0$$时，就是极大似然估计，当$$\lambda = 1$$时，称为拉普拉斯平滑。$$I()$$是求满足条件的样本出现次数。

[ab]:data:image/png;base64,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

[abs]:data:image/png;base64,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





