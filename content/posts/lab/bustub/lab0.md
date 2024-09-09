---
share: true
title: cmu15445--lab0
date: 2024-08-02T14:41:00+08:00
tags:
  - lab
  - database
  - "#bustub"
  - bustub
dir: posts/lab/bustub/
summary: cmu15445 环境配置和lab0
draft: true
---


# 环境搭配

## gradescope

[FAQ | CMU 15-445/645 :: Intro to Database Systems (Spring 2023)](https://15445.courses.cs.cmu.edu/spring2023/faq.html)
gradescope code: `2KJRB5`

[15-445/645 (Non-CMU) Dashboard | Gradescope](https://www.gradescope.com/courses/500628)

## c++相关

[C++11 Tutorial - thisPointer](https://thispointer.com/c11-tutorial/)

## 步骤

仓库地址：[cmu-db/bustub: The BusTub Relational Database Management System (Educational) (github.com)](https://github.com/cmu-db/bustub)

我的环境：windows + wsl2 + clion

仓库初始化
```git

// 初始化
git init
git add remote github https://github.com/lbjfans/bustub.git

// 克隆bustub
git clone --bare https://github.com/cmu-db/bustub.git bustub-public
cd bustub-public

// 自己的仓库
git push https://github.com/lbjfans/bustub.git master
cd .. 
rm -rf bustub-public
git clone https://github.com/lbjfans/bustub.git

// 版本回溯，建立自己的分支
cd bustub
git checkout e0163edd49d2ff086f4106d1e3cb05a0b865be23
git checkout -b base

// 配置，编译和测试
sudo build_support/packages.sh
mkdir build 
cd build 
cmake .. 
make
make check-tests

```



# lab0

官方：[Project #0 - C++ Primer | CMU 15-445/645 :: Intro to Database Systems (Spring 2023)](https://15445.courses.cs.cmu.edu/spring2023/project0/)

常用git
```git
// 创建写的分支
git checkout -b trie_test
// 写完一部分后提交
Git add .
Git commit -m 'lab0'
git checkout trie
git merge trie_test
Git push github trie:trie
```

