---
share: true
title: database system concepts --  query
date: 2024-08-04T14:41:00+08:00
tags:
  - lab
  - database
  - "#database_system_concepts"
  - database_system_concepts
dir: posts/lab/database_system_concepts
summary: 数据库系统概览--查询处理和优化部分，笔记 + 课后题
---



# C15 query processing

> 教材内容：charpter 15, 22

![](/blog/images/Pasted%20image%2020240804142753.png)
![](/blog/images/Pasted%20image%2020240804142821.png)


## 笔记

> 概述

用户输入SQL语句，翻译为关系代数/语法树，经过优化器生成最终的语法树，最后执行输出
![](/blog/images/Pasted%20image%2020240806141322.png)

在本章中，我们研究如何评估查询计划中的单个操作以及如何估算其成本；我们将在第 16 章回到查询优化的问题。第 15.2 节概述了如何测量查询的成本。第 15.3 节到第 15.6 节讨论了单个关系代数操作的评估。多个操作可以组合成一个流水线，在流水线中，每个操作在处理输入元组时，输入元组可能正由其他操作生成。在第 15.7 节，我们探讨了如何协调查询评估计划中多个操作的执行，特别是如何使用流水线操作来避免将中间结果写入磁盘。

> 测量查询的成本

开销（执行计划 + 单个算子的开销）
- 磁盘IO
- CPU
- 分布式通信

为什么不最小化响应时间？
- SQL > buffer > disk，因为buffer内容随机，所以buffer占比更多的查询会更快
- 对于并行IO，如RAID；T1需要访问更多的磁盘块但是可以并行读取，而T2访问很少的磁盘块但是只能串行读取
- 所以响应时间并不可靠，可以最小化资源开销

> select

假设$t_s$为磁盘的寻道以及旋转时间，而$t_T$为传输时间，$b_r$是一个文件中块的个数
![450*450](/blog/images/Pasted%20image%2020240806161438.png)
- A1：第一种情况是全表线性扫描；第二种情况是只输出对应的key，所以遍历到合适的就终止，平均即$b_r/2$
- A2：b+树，先查询索引，再查询一条记录（b+树每个节点放在一个块中）
- A3：b+树，先查询索引，再查询该记录的非关键字属性（b是该记录占的块数，假设块连续存放）
- A4：第一种，同A2，使用二级索引；第二种，先找索引，再找n次记录（因为二级索引不连续）
- A5：范围查找，同A3
- A6：二级索引的范围查找，同A4

> sorting

排序的原因
- 输出需要排序
- 如join使用排序的数据会更快

外部归并排序


> join

针对等值连接，可以分类为
- nested-join
	- naive：每个R元组都遍历一次S
	- block：R每个块对应一次S
	- index：对于R每个元组用索引去S里找
- sort：排序 + 合并
- hash：RS同时哈希

> other

duplicate elimination：排序，哈希
projection
set operations：集合操作，哈希
outer join
aggregation

> 完整表达式的计算

两种
- Materialization：每个算子存储中间结果，最后输出
- pipeline：不存储中间结果，每个算子的结果直接传给下一个


## 15.1

> 假设（为了简化这个练习）每个块只能容纳一个元组，并且内存最多只能容纳三个块。请展示在每次排序合并算法的过程中创建的运行（runs），当对以下元组进行排序时，按第一个属性排序：(kangaroo, 17), (wallaby, 21), (emu, 1), (wombat, 13), (platypus, 3), (lion, 8), (warthog, 4), (zebra, 11), (meerkat, 6), (hyena, 9), (hornbill, 2), (baboon, 12).
> ---

![](/blog/images/Pasted%20image%2020240808135521.png)
- 这里我感觉有问题，因为内存最多3个block，所以因该是用2-路归并（有一个块作为输出块）

## 15.2

> 考虑图15.14中的银行数据库，其中主键已下划线标出，并给出如下SQL查询：
```sql
SELECT T.branch_name
FROM branch T, branch S
WHERE T.assets > S.assets AND S.branch_city = "Brooklyn"
```
> 编写一个与此查询等效的高效关系代数表达式，并说明选择的理由。

- ![](/blog/images/Pasted%20image%2020240808135845.png)
---

![](/blog/images/Pasted%20image%2020240808140054.png)
这个表达式在可能的最小数据量上执行了θ连接。它通过将连接的右侧操作数限制为仅包含布鲁克林的分支，并且还从两个操作数中删除不必要的属性来实现这一点。

## 15.3

> 假设关系 \( r1(A, B, C) \) 和 \( r2(C, D, E) \) 具有以下属性：\( r1 \) 有 20,000 个元组，\( r2 \) 有 45,000 个元组，\( r1 \) 的 25 个元组可以放在一个块中，\( r2 \) 的 30 个元组可以放在一个块中。使用以下每种连接策略连接 \( r1 \) 和 \( r2 \) 时，估算所需的块传输和查找次数：
	a. 嵌套循环连接（Nested-loop join）。  
	b. 块嵌套循环连接（Block nested-loop join）。  
	c. 归并连接（Merge join）。  
	d. 哈希连接（Hash join）。
---


关系 `r1` 需要 800 个块，关系 `r2` 需要 1500 个块。假设内存中有 `M` 页。如果 `M > 800`，即使使用简单的嵌套循环连接，也可以轻松完成连接操作，仅需 `1500 + 800` 次磁盘访问。因此，我们只考虑 `M <= 800` 页的情况。

a. 
使用 r1 作为外部关系时，需要 20,000 × 1,500 + 800 = 30,000,800 次磁盘访问。
如果 r2 是外部关系，则需要 45,000 × 800 + 1,500 = 36,001,500 次磁盘访问。
- r1需要20000/25 = 800块，r2需要45000 / 30 = 1500块，正常会把小表放外层循环（即r1）

b.
如果 r1 是外部关系，则需要 $\lceil \frac{800}{M - 1} \rceil \times 1500 + 800$ 次磁盘访问。如果 r2 是外部关系，则需要 $\lceil \frac{1500}{M - 1} \rceil \times 800 + 1500$ 次磁盘访问。

c.
假设 $r_1$ 和 $r_2$ 在连接键上没有初始排序，并且 $b_b = 1$，总的排序成本（包括输出）为：

$$
B_s = 1500 \left(2 \lceil \log_{M - 1} \left(\frac{1500}{M}\right) \rceil + 2\right) + 800 \left(2 \lceil \log_{M - 1} \left(\frac{800}{M}\right) \rceil + 2\right)
$$

磁盘访问次数。假设具有相同连接属性值的所有元组都可以放入内存中，总成本为 $B_s + 1500 + 800$ 磁盘访问次数。

d.
我们假设不会发生溢出。由于 $r_1$ 较小，我们将其作为构建关系（build relation），而将 $r_2$ 作为探测关系（probe relation）。如果 $M > 800$，即不需要递归划分，则成本为 $3 \times (1500 + 800) = 6900$ 磁盘访问次数；否则，成本为

$$
2 \times (1500 + 800) \lceil \log_{M-1}(800) - 1 \rceil + 1500 + 800
$$

磁盘访问次数。

## 15.4

> 如果索引是二级索引且在连接属性上有多个具有相同值的元组，则第15.5.3节中描述的索引嵌套循环连接算法可能效率低下。这是为什么呢？请描述一种利用排序来减少检索内部关系元组成本的方法。在什么情况下，这种算法会比混合归并连接算法更高效？
---

- 不懂

## 15.5

> 设$r$和$s$为没有索引的关系，且假设这些关系未排序。假设内存无限，计算$r \bowtie s$的最低成本（以I/O操作为单位）是什么？这种算法需要多少内存？
---

我们可以将较小的关系完全存储在内存中，逐块读取较大的关系，并使用较大的关系作为外层关系来执行嵌套循环连接。I/O操作的数量等于 $b_r + b_s$，内存需求为 $\min(b_r, b_s) + 2$ 页。


## 15.6

> 考虑数据库图 15.14，其中主键下划线。假设关系 `branch` 上有一个关于 `branch_city` 的 $B^+$-树索引，且没有其他索引。列出处理涉及否定的选择操作的不同方法：

- ![](/blog/images/Pasted%20image%2020240808145415.png)
---

a. $\sigma_{\neg (branch\_city < "Brooklyn")}(branch)$

使用索引定位 `branch_city` 字段值为 "Brooklyn" 的第一个元组。从这个元组开始，沿着指针链访问所有后续的元组，直到检索到所有相关的元组。

b. $\sigma_{\neg (branch\_city = "Brooklyn")}(branch)$

对于这个查询，索引没有作用。我们可以顺序扫描文件，并选择所有 `branch_city` 字段不是 "Brooklyn" 的元组。

c. $\sigma_{\neg (branch\_city < "Brooklyn" \lor assets < 5000)}(branch)$

这个查询等价于以下查询：

$$
\sigma_{(branch\_city \geq "Brooklyn" \land assets \geq 5000)}(branch)
$$

使用 `branch_city` 索引，我们可以通过从第一个 "Brooklyn" 元组开始，沿着指针链检索所有 `branch_city` 值大于或等于 "Brooklyn" 的元组。同时，对每个元组应用额外的条件 $assets \geq 5000$。

## 15.7

> 

# C16 query optimization

> 教材内容：chapter 16

## 笔记

> 概述

可以通过优化SQL表达式，以及针对不同算子使用不同的实现方式，使用不同的索引来减少开销

三个步骤
- 生成等价的表达式
- 注释，获取备选方案
- 评估并选取开销最小的方案

> 关系表达式转换

等价规则
- 联合选择可以分成多次选择
- 选择可以交换
![](/blog/images/Pasted%20image%2020240807134312.png)
- 投影只需保留最后一个
![](/blog/images/Pasted%20image%2020240807134412.png)
- 笛卡尔积+选择 等价于 theta-join
- theta-join可以交换
![](/blog/images/Pasted%20image%2020240807140100.png)
- 自然连接和theta-join的结合性
![](/blog/images/Pasted%20image%2020240807144458.png)
- theta-join和选择
![](/blog/images/Pasted%20image%2020240807144550.png)
- theta-join和投影
![](/blog/images/Pasted%20image%2020240807144618.png)
- 略


步骤
```Pseudocode
过程 genAllEquivalent(E)
  EQ = {E}  // 初始化等价表达式集合EQ，最初只包含表达式E
  重复执行以下步骤:
    将集合EQ中的每个表达式Ei与每个等价规则Rj进行匹配
    如果Ei的任何子表达式ei与Rj的一侧匹配
      创建一个新表达式E'，它与Ei相同，除了将ei转换为匹配Rj的另一侧
      如果E'不在EQ中，则将E'添加到EQ中
  直到无法再向EQ中添加新表达式为止

```


> Estimating Statistics of Expression Results（成本估算统计）


直方图：统计某个属性的频率，如当某个属性值相同的个数较多，优化器可以使用全表扫描效率而不是索引
- [【认知篇】Oracle的直方图是个嘛，能干啥？_oracle智能地生成直方图有什么用-CSDN博客](https://blog.csdn.net/db_murphy/article/details/103988333)
- [Oracle直方图的详细解析 - 孙愚 - 博客园 (cnblogs.com)](https://www.cnblogs.com/51linux/p/4128498.html)

选择
连接
其他操作的估计

> Choice of Evaluation Plans

基于成本（上一节以及15章）
启发式
- 尽早执行选择操作
- 尽早执行投影操作
- left-deep join
![](/blog/images/Pasted%20image%2020240807153106.png)
- 嵌套子查询

