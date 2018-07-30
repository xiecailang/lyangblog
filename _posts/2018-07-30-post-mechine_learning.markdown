---
layout:     post
title:      "机器学习复习"
subtitle:   " \"Machine Learning\""
date:       2018-07-30 10:00:00
author:     "lang"
header-img: "http://lyang-blog-pics.oss-cn-shanghai.aliyuncs.com/post-bg-2017/0330/170330.jpg"

catalog: true
tags:
    - Tech
---

# 朴素贝叶斯 Naive bayes

* Example
患癌症的概率为P(C) = 0.01  
测试集中：  
1. 如果患有癌症 ，检查结果90% 呈阳性 （敏感型）
2. 如果没有患癌症，检查结果90%呈阴性 （特异型）  
问：如果检查呈阳性，那么患癌症的概率为多少  
假设测试集为1，根据 **条件1** 可知阳性且癌症的人数为$$0.01\times{}0.9$$，根据 **条件2** 可知阳性不患癌症的人数为$$0.1\times0.99$$,则检查出阳性的患癌概率为$$\frac{0.009}{0.009+0.099}=0.0833333.$$  
贝叶斯法则：  
先验概率 $$\times$$ 测试证据 = 后验概率  
贝叶斯公式：  
$$P(B_i\|A) = \frac{P(B_i)P(A\|B_i)}{\sum_{j=1}^{n}P(B_j)P(A\|B_j)}$$  
先验概率：$$P(C) = 0.01, P(\neg{}C)=0.99$$  
测试：$$P(Pos.\|C) = 0.9, P(Neg.\|\neg{}C)=0.9, P(Pos.\|\neg{}C)=0.1$$  
联合概率：$$P(C,Pos.) = P(C)P(Pos.\|C) = 0.009, P(\neg{}C,Pos.)=P(\neg{}C)P(Pos.\|\neg{}C) = 0.099.$$  
归一化集合：$$P(Pos.) = P(C,Pos.)+P(\neg{}C,Pos.) = 0.108$$  
归一化后验概率：$$P(C\|Pos.) = 0.009/0.108 = 0.0833,  P(\neg{}C\|Pos.) = 0.099/0.108 = 0.9167$$  
Naive bayes可用在Text learning中，例如，已知（通过训练）CHRIS和SARA使用某些单词的概率，给出一封邮件，通过贝叶斯方法可以判断出邮件是某一个人写的概率  
优点：模型简单，训练速度快  
缺点：Naive bayes对词序不敏感，交换词序通常不会对预测结果有影响  
using sklearn Code: 
 
```python
X = np.array([[-1, -1], [-2, -1], [-3, -2], [1, 1], [2, 1], [3, 2]])
Y = np.array([1, 1, 1, 2, 2, 2])
from sklearn.naive_bayes import GaussianNB
clf = GaussianNB()
clf.fit(x, y)
print(clf.predict([[-0.8, -1]])) 
```

# SVM

寻找两类数据的分割线，或者超平面  
关键词： **Margin**，与分割线最近的点与分割线的距离  
SVM的任务就是最大化 **Margin**  
SVM总是优先考虑数据是否被正确分类（实际中会忽略某些异常数据），再考虑是否最大化 **Margin**  
对于线性不可分的两类数据，通常通过添加新特征来进一步区分，例如，数据A集中在坐标轴原点附近，数据B距离原点较远，则可以添加新特征$$z=x^2+y^2$$, 建立新的坐标轴x-z，发现数据集A、B线性可分了  

```python
from sklearn import svm
clf = svm.SVC()
clf.fit(x, y)
clf.predict(z)
```