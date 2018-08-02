---
layout:     post
title:      "回归之最小二乘法"
subtitle:   " \"Machine Learning\""
date:       2018-08-02 10:00:00
author:     "lang"
header-img: "http://lyang-blog-pics.oss-cn-shanghai.aliyuncs.com/post-bg-2017/0330/170330.jpg"

catalog: true
tags:
    - Tech
---

> 最小二乘法是回归里面最基本的算法，通过最小化误差的平方和来寻找数据最佳的匹配函数

# 推导

样本回归模型：  
$$Y_i = \hat{\beta}_0+\hat{\beta}_1X_i+e_i$$  
则$$e_i = Y_i - \hat{\beta}_0 - \hat{\beta}_1X_i$$  

平方损失函数：  
$$\mathbf{Q} = \sum_{i=1}^{n}e_i^2 = \sum_{i=1}^{n}(Y_i - \hat{\beta}_0 - \hat{\beta}_1X_i)^2$$  
通过求最小的Q来确定这条直线，也就是确定$$\hat{\beta}_0, \hat{\beta}_1$$，以$$\hat{\beta}_0, \hat{\beta}_1$$为Q函数的变量，就变成了求极值的问题，通过对两个变量求偏导数：  
$$\frac{\partial{}Q}{\partial{}\hat{\beta}_0} = -2\sum_{i=1}^{n}(Y_i - \hat{\beta}_0 - \hat{\beta}_1X_i) = 0$$  
$$\frac{\partial{}Q}{\partial{}\hat{\beta}_1} = -2X_i\sum_{i=1}^{n}(Y_i - \hat{\beta}_0 - \hat{\beta}_1X_i) = 0$$  
求解方程组即可得出$$\hat{\beta}_0, \hat{\beta}_1$$:  
$$\hat{\beta}_0 = \frac{n\sum{}X_iY_i - \sum{}X_i\sum{}Y_i}{n\sum{}X_i^2 - (\sum{}X_i)^2}$$  
$$\hat{\beta}_1 = \frac{\sum{}X_i^2\sum{}Y_i - \sum{}X_i\sum{}X_iY_i}{n\sum{}X_i^2 - (\sum{}X_i)^2}$$  

# c++ 实现代码

[代码来源](https://www.cnblogs.com/armysheng/p/3422923.html)  
```cpp
/*
最小二乘法C++实现
参数1为输入文件
输入 ： x
输出： 预测的y  
*/
#include <iostream>
#include <fstream>
#include <vector>
using namespace std;

class LeastSquare{
    double a, b;
public:
    LeastSquare(const vector<double>& x, const vector<double>& y)
    {
        double t1=0, t2=0, t3=0, t4=0;
        for(int i=0; i<x.size(); ++i)
        {
            t1 += x[i]*x[i];
            t2 += x[i];
            t3 += x[i]*y[i];
            t4 += y[i];
        }
        a = (t3*x.size() - t2*t4) / (t1*x.size() - t2*t2);  // 求得β1 
        b = (t1*t4 - t2*t3) / (t1*x.size() - t2*t2);        // 求得β2
    }

    double getY(const double x) const
    {
        return a*x + b;
    }

    void print() const
    {
        cout<<"y = "<<a<<"x + "<<b<<"\n";
    }

};

int main(int argc, char *argv[])
{
    if(argc != 2)
    {
        cout<<"Usage: DataFile.txt"<<endl;
        return -1;
    }
    else
    {
        vector<double> x;
        ifstream in(argv[1]);
        for(double d; in>>d; )
            x.push_back(d);
        int sz = x.size();
        vector<double> y(x.begin()+sz/2, x.end());
        x.resize(sz/2);
        LeastSquare ls(x, y);
        ls.print();
        
        cout<<"Input x:\n";
        double x0;
        while(cin>>x0)
        {
            cout<<"y = "<<ls.getY(x0)<<endl;
            cout<<"Input x:\n";
        }
    }
}
```