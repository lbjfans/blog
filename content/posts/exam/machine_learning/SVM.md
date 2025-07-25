---
share: true
title: 机器学习--SVM
date: 2024-09-14T14:41:00+08:00
tags:
  - "#exam"
  - "#machine_learning"
  - exam
  - machine_learning
dir: posts/exam/machine_learning/
summary: SVM部分
draft: true
---

任务：二分类，也可以用于多分类

类型：监督学习

基本思想：对于空间中的样本点集合，可用一个超平面将样本点分成两部分，一部分属于正类，一部分属于负类。在保证超平面能够正确将样本进行分类的同时，使得距离超平面最近的点到超平面的距离尽可能的大（这些点称为支持向量）

支持向量：决定分类面可以平移的范围的数据点

软间隔：允许噪声数据，有利于获取更大的分类间隔和消除过拟合

对于线性不可分的情况，先将数据映射到高维空间，用超平面划分

# 线性可分SVM

超平面以及分类决策函数
![](/blog/images/Pasted%20image%2020240916231904.png)

函数间隔：没有除向量的模
几何间隔：点到平面的距离
![](/blog/images/Pasted%20image%2020240916232405.png)
- 目标是让距离最大化
- w, b翻倍，分母的w也翻倍；可以令wx+b = 1，变为y = yi / ||w||

目标：等价为最小化$||w||^2  / 2$
![](/blog/images/Pasted%20image%2020240916235505.png)

算法
![](/blog/images/Pasted%20image%2020240916235941.png)


例题
![](/blog/images/Pasted%20image%2020240917000607.png)
![](/blog/images/Pasted%20image%2020240917000616.png)




对偶问题：让求解更简单
![](/blog/images/Pasted%20image%2020240917000709.png)
- 几个疑惑：为什么原始问题是极小极大，为什么可以转为极大极小
![](/blog/images/Pasted%20image%2020240917171222.png)

例题
![](/blog/images/Pasted%20image%2020240917171745.png)
![](/blog/images/Pasted%20image%2020240917173659.png)


# 软间隔最大化

```
什么情况下需要使用线性支持向量机来求解？
答：生产环境中，我们获取到的数据往往存在噪声（正类中混入少量的负类样本，负类中混入少量的正类样本），从而使得数据变得线性不可分。在这种情况下，需要用到该向量机来进行求解。
```

松弛变量
支持向量
合页损失函数

# 重点

例题
- 基本
- 对偶形式

