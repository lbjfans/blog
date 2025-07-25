---
share: true
title: 机器学习--KNN
date: 2024-09-19T14:41:00+08:00
tags:
  - "#exam"
  - "#machine_learning"
  - exam
  - machine_learning
dir: posts/exam/machine_learning/
summary: KNN部分
draft: true
---
# 任务

目标：非线性分类，回归
无监督
教材只讲分类


# 概述

在一个含未知样本的空间，可以根据样本最近的k个样本的数据类型来确定未知样本的数据类型

直观：某个点属于哪个类，可以选择该点与距离它最近的其他K个点（要素1：如何计算距离 > 最近），K个点（要素2：如何选择K）中哪个类占比大（要素3：多数决定类别），该点就属于哪个类

[K近邻算法详解_k-近邻算法训练时间普遍偏长-CSDN博客](https://blog.csdn.net/wangmumu321/article/details/78576916)

kd树的建立和搜索例题

# 题目

3.以下关于k-近邻算法的说法中正确的是（ B ）
A.k-近邻算法不可以用来解决回归问题
- 在回归问题中，k-近邻会根据k个最近邻的点的均值来预测连续变量的值。
B.随着k值的增大，决策边界会越来越光滑
- [(*´∇｀*) 欢迎回来！ (cnblogs.com)](https://www.cnblogs.com/CNLayton/p/12189500.html)
C.k-近邻算法适合解决高维稀疏数据上的问题
- 维数灾难
- [K近邻算法所面临的维数灾难问题_最近邻如何避免维度灾难-CSDN博客](https://blog.csdn.net/wcysghww/article/details/82589975)
D.相对3近邻模型而言，1近邻模型的bias更大，variance更小
- [通俗讲解机器学习中的偏差 (Bias) 和方差 (Variance) - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/600015111)
- 偏差bias：对训练集的拟合程度
- 方差variance：数据改变对输出的影响程度
- K小，曲线不平滑，容易过拟合，方差大，偏差小

k-近邻算法训练时间普遍偏长：错误，因为KNN是懒惰训练，非参数训练，训练只是保存数据，预测时需要计算距离花费时间长

# 疑问

KNN和KMEANS

有监督和无监督

课后题没写

听：简答例题
[分类问题：分类算法+KNN算法详解+考试例题讲解_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1CD4y177op/?vd_source=773a63398bea4e166f99c44cae6bee92)