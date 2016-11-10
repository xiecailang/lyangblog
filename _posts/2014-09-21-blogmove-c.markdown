---
layout:     post
title:      "[博客搬家]华为oj挑战赛八皇后问题思考"
subtitle:   " \"from csdn\""
date:       2014-09-21 12:00:00
author:     "cailang.X"
header-img: "http://lyang-blog-pics.oss-cn-shanghai.aliyuncs.com/post-bg-2015/move-post-bg.jpg?x-oss-process=image"
catalog: true
tags:
    - move
---

# 题目大致描述

8×8棋盘放置8个皇后，不能直接被吃掉，一共有92中放置方式，输入1~92中任意若干数字，输出92种布局中对应（由小到大排序好的92种布局）的布局。比如输入1 2；输出05726314 06357142；
上午刚看题目的时候有点晕，又是这种棋盘的题目，之前在网易腾讯也遇到过类似的题目，上午华为OJ分值最高的也是一个棋盘，在上午各种绞尽脑汁依然无果后，下午决心攻破这一类图相关的遍历方法。

# 算法分析

在网上搜了很多资料，有用回溯的，递归的，还有高端的位运算。综合之后选择了比较有一般性的递归方法，研究了网上一些代码之后总结了一下，解题思路大致如下：
step 1：放置第1行，判断第二行可以放置的位置
![这里写图片描述](http://img.blog.csdn.net/20160415113613746)
step 2：放置第二行，判断第三行可以放置的位置
2.1：不能和第一个皇后同行，同列，同斜线
![这里写图片描述](http://img.blog.csdn.net/20160415113748321)
2.2：不能和第二个皇后同行，同列，同斜线
![这里写图片描述](http://img.blog.csdn.net/20160415113922884)
step 3：。。。依次类推

源代码如下：

```java
import java.util.ArrayList;
import java.util.Scanner;
class Queen{
	static final int QueenMax = 8;

	static int oktimes = 0;

	static int chess[] = new int[QueenMax];//每一个Queen的放置位置 ,chess[index] = 列数，index = 行数

	static ArrayList<String> queenList = new ArrayList<String>();//记录数据

	public static void main(String[] args) { 	

		for (int i=0;i<QueenMax;i++)

			chess[i]=-1;

		//放置第一个Queen		

		placequeen(0);

		Scanner sc = new Scanner(System.in);

		String str = sc.nextLine();

		if(str.length() == 0)

			return;

		String[] strlist = str.split(" ");

		//输出结果

		for(int i = 0 ; i < strlist.length ; i++)

		{

			System.out.println(queenList.get(Integer.parseInt(strlist[i])));

		}
	}

	public static void placequeen(int num){ //num 为现在要放置的行数

		int i=0;

		//安全数组，记录第num+1行第i列是否可以放queen

		boolean qsave[] = new boolean[QueenMax];

		for(;i<QueenMax;i++)

			qsave[i]=true;

		//下面先把安全位数组完成 ，与已经放置好的皇后同一行，同一列，同一斜线都不能放置下一个queen

		i=0;

		while (i<num){

			qsave[chess[i]]=false;

			int k=num-i;

			if ( (chess[i]+k >= 0) && (chess[i]+k < QueenMax) )

				qsave[chess[i]+k]=false;

			if ( (chess[i]-k >= 0) && (chess[i]-k < QueenMax) )

				qsave[chess[i]-k]=false;

			i++;

		}

		//遍历安全位，在可以放置的位置放置下一行

		for(i=0;i<QueenMax;i++){

			if (qsave[i]==false)

				continue;

			if (num<QueenMax-1){

				chess[num]=i;

				placequeen(num+1);

			}

			else{ //第八个皇后已经放置好，找到正确布局

				chess[num]=i;

				String temp = "";

				for (i=0;i<QueenMax;i++){

					temp += (chess[i]+"");

				}

				queenList.add(temp);

			}

		}  

	}

}
```

挑战赛最难的一题也可以用这个思路来解，题意大致如下
M*M的方格，每个放置面值0～9的金币，输入起点坐标和终点坐标，找出能收集到最多金币的路径，路径规则如图

![这里写图片描述](http://img.blog.csdn.net/20160415114018556)


——cailang.X 2016-11
