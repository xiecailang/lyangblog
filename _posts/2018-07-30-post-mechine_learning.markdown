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
监督式学习：y=f(x),可分为回归问题和分类问题  
分类和回归问题比较:  

| 属性 | 分类 | 回归 |
| - | :-: | :-: |
| 输出类型 | 离散 | 连续 |
| 目标 | 找出决策边界 | 找出最好的fit line |
| 评估 | 查准率accuracy | 误差 |

非监督式学习：只拥有x没有相关的输出变量，可分为聚类问题和关联问题  
半监督式学习：拥有大部分x，只有少部分数据拥有y，现实中如照片分类、数据打标签等等  

# 朴素贝叶斯 Naive bayes - 监督性 - 分类

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
$$P(B_i\mid{}A) = \frac{P(B_i)P(A\mid{}B_i)}{\sum_{j=1}^{n}P(B_j)P(A\mid{}B_j)}$$  
先验概率：$$P(C) = 0.01, P(\neg{}C)=0.99$$  
测试：$$P(Pos.\mid{}C) = 0.9, P(Neg.\mid{}\neg{}C)=0.9, P(Pos.\mid{}\neg{}C)=0.1$$  
联合概率：$$P(C,Pos.) = P(C)P(Pos.\mid{}C) = 0.009, P(\neg{}C,Pos.)=P(\neg{}C)P(Pos.\mid{}\neg{}C) = 0.099.$$  
归一化集合：$$P(Pos.) = P(C,Pos.)+P(\neg{}C,Pos.) = 0.108$$  
归一化后验概率：$$P(C\mid{}Pos.) = 0.009/0.108 = 0.0833,  P(\neg{}C\mid{}Pos.) = 0.099/0.108 = 0.9167$$  
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

# SVM - 监督性 - 分类

寻找两类数据的分割线，或者超平面  
关键词： **Margin**，与分割线最近的点与分割线的距离  
SVM的任务就是最大化 **Margin**  
SVM总是优先考虑数据是否被正确分类（实际中会忽略某些异常数据），再考虑是否最大化 **Margin**  
对于线性不可分的两类数据，通常通过添加新特征来进一步区分，例如，数据A集中在坐标轴原点附近，数据B距离原点较远，则可以添加新特征$$z=x^2+y^2$$, 建立新的坐标轴x-z，发现数据集A、B线性可分了 
**核函数**用于将特征从低维空间映射到高维空间，使得线性不可分的数据变得线性可分，常用的有线性linear、多项式poly，径向基函数rbf，s函数sigmoid等等

**SVM优缺点**
对于明显边界的数据可以有很好的效果，但是对于海量的复杂数据，他们表现不够好，**训练时间与数据量的三次方成正比**。对于噪声过大的数据同样表现不佳，噪声有可能会导致算法太慢和过拟合

```python
from sklearn import svm
clf = svm.SVC()
clf.fit(x, y)
clf.predict(z)
```

# 决策树 Decision trees - 监督性 - 分类

DT也可以使用核函数技巧将数据进行线性和非线性之间的转换  
DT参数：min_samples_split，可分割的样本数下限，决定了样本数减小到某一个值后是否还需要继续进行分割  
min_samples_leaf, 叶节点最小样本数  
max_depth，最大深度
调节这些参数可以缓和过拟合现象

**熵 Entropy**：一系列样本中纯度（单一性）的度量，所有样本都在一个分类，则Entropy=0，所有样本都不在对应的分类，则Entropy=1    
DT的目的就是递归重复，找到能使Entropy尽量小（纯度高）的分割点  
$$\mathbf{E}=\sum_{i}-p_i\mathrm(log)_2(p_i)$$  

**信息增益**=Entropy(parent) - [加权平均]Entropy(children)  
DT算法最大化信息增益

code  
```python
from sklearn import tree
clf = tree.DecisionTreeClassifier()
clf = clf.fit(x, y)
```

# 回归

线性回归方程 y=kx+b  
k: 斜率（slope），b：截距 （intercept）  
更一般性方程（多元回归）：  
$$\hat{y}(w, x) = w_0 + w_1x_1+...+w_px_p$$  
$$w = (w_1, ..., w_p)$$是系数coef_  
$$w_0$$为截距intercept_  

线性回归误差error = 实际 - 预测  
回归中需要平衡*误差*和*过拟合*  
回归中要尽量最小化误差平方和(SSE)：$$min(\sum_{all samples}(actual - predicted)^2)$$  
回归训练评估指标：$$0<R^2<1$$ 和 均方误差根(RMSE)

code  
```python
from sklearn import linear_model
clf = linear_model.LinearRegression()
clf.fit(x, y)
```

# K-均值 k-means - 非监督式 - 聚类

最基本的聚类算法  
k-means算法（橡皮筋），给出初始的聚类中心，迭代改变聚类中心位置使得points到聚类中心的二次距离最小，可以把point和中心点之间的连接线看成橡皮筋，我们要找出使得橡皮筋能量最小的状态  
当迭代完成后，中心点之间连接线的垂直平分线将数据分为不同的两类  
可以在[k-means在线尝试](http://www.naftaliharris.com/blog/visualizing-k-means-clustering/)进行模拟尝试

# 特征缩放

假如，已知chris的身高体重，给他选择合适尺码的T恤，给出cameron和sarah身高体重，他们分别穿L和S码： 

names|weight(lbs)|height(ft)|
|-|:-:|:-:|
|chris|140|6.1|
|cameron|175|5.9|
|sarah|115|5.2|

如果没有对特征进行缩放，直接用weight+height公式比较三个人，会发现sarah的数据更接近chris，然而实际上应该是cameron穿的T恤更适合chris。  
特征缩放公式：  
$$x' = \frac{x - x_{min}}{x_{max} - x_{min}}$$  
因此，对于体重[115, 140, 175]都要通过上面的公式缩放到相应的大小，身高也是如此。  

# 文本学习

文本学习中的一个基本问题，就是文本数据的长度不一致，在机器学习中，通常有一个功能：bag of words，即词袋。通过比较词袋和输入的文本，计算出词袋中包含的单词出现的次数，这样就可以得到一个向量  
比如  
输入： nice day  
词袋：nice very day he she love  
向量: 1  0  1  0  0  0  
在进行文本分析前有两个步骤需要做：**去除低信息单词（stopword），比如the, in, for...** 和 **词干化,同一个意思不同词性的单词，比如unresponsive, response, responsivity...它们的词干是respon**  

自然语言工具 **NLTK** [下载](http://stackoverflow.com/questions/5843817/programmatically-install-nltk-corpora-models-i-e-without-the-gui-downloader)  
下载步骤
    import nltk
    nltk.download()

# 主成分分析 PCA

**数据维度** 坐标位移和旋转来确定数据维度  
PCA第一主成分，新坐标中心  
PCA第二主成分，新坐标x向量，它在旧坐标轴上的分量为$$\Delta{}x, \Delta{}y$$，同样y向量也有这两个分量，将新的x，y向量归一化后他们分量也会发生变化  
不管对于什么数据，PCA总是会给出一个确定的主轴  
对于呈直线分布的数据，长轴占主导地位，沿着长轴可以捕获大部分的数据  

PCA也可用于给数据降维，将大量的特征进行压缩，提取出主要成分，方法是找出高维数据的主轴，将数据投影到主轴上形成一维数据