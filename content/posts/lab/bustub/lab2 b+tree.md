---
share: true
title: cmu15445--lab2 b+ tree
date: 2024-08-15T14:41:00+08:00
tags:
  - lab
  - database
  - "#bustub"
  - bustub
dir: posts/lab/bustub/
summary: cmu15445 lab2 b+ tree
---
# 复习理论

教材663
[hw2-sols.pdf (cmu.edu)](https://15445.courses.cs.cmu.edu/spring2023/files/hw2-sols.pdf)

[B+树,B-link树,LSM树...一个视频带你了解常用存储引擎数据结构（合集）_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1se4y1U7Dn/?spm_id_from=333.337.search-card.all.click&vd_source=773a63398bea4e166f99c44cae6bee92)
[数据库底层数据结构 B树B+树LSM树 详解对比与总结-腾讯云开发者社区-腾讯云 (tencent.com)](https://cloud.tencent.com/developer/article/1835924)


出现
存储的数据结构需要两个特性：适合磁盘（顺序IO，减少IO次数），并发
为了减少磁盘随机访问的次数，保证增删改查时间复杂度O(logn)

数据结构
二叉查找树：对于每个节点，左子树数值小，右子树数值大
平衡二叉树、红黑树：通过旋转避免二叉查找树的左/右斜树情况
B树：允许每个节点有更多的子节点
B+树：数据只在叶子节点
B+树并发：[数据库内核月报 (taobao.org)](http://mysql.taobao.org/monthly/2018/09/01/)
- 悲观锁：分支加写锁
- 乐观锁：加读锁，需要修改再重新加写锁
- 对于多核心，一个核心加锁会导致其他核心缓存失效；因此，如果存在一种自底向上加锁的策略，只有在树节点分裂或者合并或者删除的情况下向上加锁，只对被修改的树节点加锁，就可以在很大程度上减少加锁的范围和频率，从而提高B+树的多线程扩展性
- Blink树
LSM：适用于多写入场景



# 面经

-  B+树比B树好在哪里？哪个层数更多？
	- 遍历问题：B树全部查找需要中序遍历，而B+遍历叶子节点即可，可以利用空间局部性，即利用磁盘预读原理提前将这些数据读入内存，减少了磁盘 IO 的次数
	- 同样的大小的磁盘页，使用B+树可以容纳更多索引节点元素，在相同的数据量下，Ｂ＋树更加“矮胖”，IO操作更少
	- Ｂ树的查找只需找到匹配元素即可，最好情况下查找到根节点，最坏情况下查找到叶子结点，所说性能很不稳定，而Ｂ＋树每次必须查找到叶子结点，性能稳定
-  B+树乐观锁怎么实现？
- B树的应用场景有哪些？
	- [(1 条消息) AVL树，红黑树，B树，B+树，Trie树都分别应用在哪些现实场景中？ - 知乎 (zhihu.com)](https://www.zhihu.com/question/30527705)
- B+树和LSM-Tree的使用场景？LSM-Tree存在的问题有哪些？针对这些问题有什么解决思路？
- B+树并发处理有哪些优化思路？Blink树有什么特点？
	- [B-link Tree：一种B+Tree的并发优化 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/372830975#:~:text=%E6%9C%80%E7%AE%80%E5%8D%95%E7%9A%84%E6%96%B9%E6%A1%88%E6%98%AF%E5%9C%A8%E6%A0%91%E7%BB%93%E6%9E%84%E8%B0%83%E6%95%B4%E6%97%B6%E4%BD%BF%E7%94%A8%E5%85%A8%E5%B1%80%E9%94%81%E4%BD%8F%E6%95%B4%E6%A3%B5B%2B%20Tree%EF%BC%8C%E9%98%BB%E6%AD%A2%E4%B8%80%E5%88%87%E5%B9%B6%E5%8F%91%E8%AE%BF%E9%97%AE%E7%9B%B4%E5%88%B0%E6%A0%91%E7%BB%93%E6%9E%84%E8%B0%83%E6%95%B4%E5%AE%8C%E6%AF%95%EF%BC%8CInnoDB%E6%97%A9%E6%9C%9F%E4%B9%9F%E6%98%AF%E9%87%87%E5%8F%96%E4%BA%86%E8%AF%A5%E5%81%9A%E6%B3%95%EF%BC%8C%E5%BD%93%E7%84%B6%E8%BF%99%E4%BC%9A%E5%AF%BC%E8%87%B4%E6%80%A7%E8%83%BD%E7%89%B9%E5%88%AB%E5%B7%AE%E3%80%82,%E8%80%8CB-link%20Tree%E5%88%99%E6%8B%92%E7%BB%9D%E5%85%A8%E5%B1%80%E9%94%81%E8%BF%99%E4%B8%80%E5%81%9A%E6%B3%95%E3%80%82%20%E5%AE%83%E6%89%A7%E8%A1%8C%E4%B8%80%E7%A7%8D%E8%87%AA%E5%BA%95%E5%90%91%E4%B8%8A%E7%9A%84%E8%B0%83%E6%95%B4%E6%96%B9%E6%B3%95%EF%BC%8C%E6%AF%8F%E6%AC%A1%E5%8F%AA%E5%AF%B9%E5%BD%93%E5%89%8D%E8%B0%83%E6%95%B4%E8%8A%82%E7%82%B9%E5%8A%A0%E9%94%81%EF%BC%8C%E5%BD%93%E5%AD%90%E8%8A%82%E7%82%B9%E8%B0%83%E6%95%B4%E5%AE%8C%E6%AF%95%E5%90%8E%E5%86%8D%E5%90%91%E4%B8%8A%E5%9B%9E%E6%BA%AF%E8%B0%83%E6%95%B4%E7%88%B6%E8%8A%82%E7%82%B9%EF%BC%8C%E7%9B%B4%E5%88%B0%E6%89%80%E6%9C%89%E8%B0%83%E6%95%B4%E5%AE%8C%E6%AF%95%E3%80%82)
- 为什么不用哈希表
	- 不适用范围查询

# 开始lab2

[Project #2 - B+Tree | CMU 15-445/645 :: Intro to Database Systems (Spring 2023)](https://15445.courses.cs.cmu.edu/spring2023/project2/#b+tree-page)

# Task #1 - B+Tree Pages

跟随b_plus_tree_insert_test和b_plus_tree_sequential_scale_test
- simple insert
- simple search
- simple split
- multiple split




# 疑问

header_page如何帮助并发？

context有啥用


# visualize

可以使用`tools/b_plus_tree_printer`
```git
$ # To build the tool
$ mkdir build
$ cd build
$ make b_plus_tree_printer -j$(nproc)
$ ./bin/b_plus_tree_printer
>> ... USAGE ...
>> 5 5 // set leaf node and internal node max size to be 5
>> f input.txt // Insert into the tree with some inserts 
>> g my-tree.dot // output the tree to dot format 
>> q // Quit the test (Or use another terminal) 

// 生成图片的2种方式：
http://dreampuf.github.io/GraphvizOnline/
dot -Tpng -O my-tree.dot

// 和官方实现对比
https://15445.courses.cs.cmu.edu/spring2023/bpt-printer/
```
[Graphviz Online (dreampuf.github.io)](https://dreampuf.github.io/GraphvizOnline/#digraph%20G%20%7B%0A%0A%20%20subgraph%20cluster_0%20%7B%0A%20%20%20%20style%3Dfilled%3B%0A%20%20%20%20color%3Dlightgrey%3B%0A%20%20%20%20node%20%5Bstyle%3Dfilled%2Ccolor%3Dwhite%5D%3B%0A%20%20%20%20a0%20-%3E%20a1%20-%3E%20a2%20-%3E%20a3%3B%0A%20%20%20%20label%20%3D%20%22process%20%231%22%3B%0A%20%20%7D%0A%0A%20%20subgraph%20cluster_1%20%7B%0A%20%20%20%20node%20%5Bstyle%3Dfilled%5D%3B%0A%20%20%20%20b0%20-%3E%20b1%20-%3E%20b2%20-%3E%20b3%3B%0A%20%20%20%20label%20%3D%20%22process%20%232%22%3B%0A%20%20%20%20color%3Dblue%0A%20%20%7D%0A%20%20start%20-%3E%20a0%3B%0A%20%20start%20-%3E%20b0%3B%0A%20%20a1%20-%3E%20b3%3B%0A%20%20b2%20-%3E%20a3%3B%0A%20%20a3%20-%3E%20a0%3B%0A%20%20a3%20-%3E%20end%3B%0A%20%20b3%20-%3E%20end%3B%0A%0A%20%20start%20%5Bshape%3DMdiamond%5D%3B%0A%20%20end%20%5Bshape%3DMsquare%5D%3B%0A%7D)


