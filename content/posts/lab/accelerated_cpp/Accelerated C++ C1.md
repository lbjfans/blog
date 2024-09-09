---
share: true
title: Accelerated C++ charpter1
date: 2024-08-10T14:41:00+08:00
tags:
  - lab
  - Accelerated_Cpp
dir: posts/lab/accelerated_cpp/
summary: Accelerated C++ 第1章 字符串的使用
draft: true
---

# 1-1

![](/blog/images/Pasted%20image%2020240810155859.png)

有效；`+`左结合，用于连接两个字符串(string + string)，或一个字符串和一个字符串字面量(string + char[])，注意这是重载了string的+，而char[]是基本类型，没有重载

# 1-2

![](/blog/images/Pasted%20image%2020240810160248.png)

无效；理由同上

# 1-3

![](/blog/images/Pasted%20image%2020240810160956.png)

有效；`{}`区分了不同作用域
# 1-4

![](/blog/images/Pasted%20image%2020240810161040.png)
都有效

# 1-5
![](/blog/images/Pasted%20image%2020240810161206.png)
无效，因为x已经超过了作用域

# 1-6

![](/blog/images/Pasted%20image%2020240810161326.png)
直接输出，跳过了第二次输入，因为cin遇到空格停止，而缓冲区的数据直接给到了第二次的cin
可以用`getline(cin, name);`输入一行的数据


