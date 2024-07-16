---
share: true
title: Accelerated C++ charpter0
date: 2024-07-16T14:41:00+08:00
tags:
  - lab
  - Accelerated_Cpp
dir: posts/lab/
summary: Accelerated C++ 第0章
---
# 环境

> windows + wsl2 + clion

## cmake

[cmake使用详细教程（日常使用这一篇就足够了）_cmake教程-CSDN博客](https://blog.csdn.net/iuu77/article/details/129229361)


# 练习

## 0-1

![](/blog/images/Pasted%20image%2020240716145100.png)

计算，但是结果被丢弃

## 0-2

![](/blog/images/Pasted%20image%2020240716145234.png)

注意添加转义符号`\`
复制到clion中会自动帮助添加转移符号
```cpp
int main(){  
	cout << "This (\") is a quote, and this (\\) is a backlash." << endl;  
return 0;  
}
```

## 0-5/6

![](/blog/images/Pasted%20image%2020240716145500.png)

0-5：不是，因为没有中括号

0-6：是，每个中括号都是一个作用域


## 0-7/8

![](/blog/images/Pasted%20image%2020240716145618.png)

考察注释
0-7：无效，因为第一个`/*`和第一个`*/`匹配，后面的文字无法被编译器解释

0-8：有效，因为`//`是单行注释

## 0-9

![](/blog/images/Pasted%20image%2020240716145946.png)

```cpp
int main()  { }
```
c++会自动补上`return 0;`
