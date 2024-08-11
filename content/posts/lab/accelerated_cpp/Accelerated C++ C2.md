---
share: true
title: Accelerated C++ charpter2
date: 2024-08-11T14:41:00+08:00
tags:
  - lab
  - Accelerated_Cpp
dir: posts/lab/accelerated_cpp/
summary: Accelerated C++ 第2章 循环与计数
---

# 2-0

编译运行即可

```cpp
#include<iostream>
#include<string>

using namespace std;

int main(int argc, char const *argv[])
{
    cout << "Please enter your first name:";
    string name;
    cin >> name;

    const string greeting = "Hello, " + name + "!";

    const int pad = 1;  // greeting上下左右的填充边界字符个数
    const int rows = pad * 2 + 3;  // 行数
    const string::size_type cols = greeting.size() + pad * 2 + 2;

    cout << endl;
    for (int r = 0; r != rows; ++r)
    {
        string::size_type  c = 0;
        while(c != cols){
            if(r == pad + 1 && c == pad + 1){
                cout << greeting;
                c += greeting.size();
            }else{
                if(r == 0 || r == rows - 1 ||
                   c == 0 || c == cols - 1) //第0行和最后一行以及第0列和最后一列
                    cout << "*";
                else
                    cout << " ";
                ++c;
            }
        }
        cout << endl;
    }
    return 0;
}

```

几个知识点
- `string::size_type`：用于string或vector，相较于`size_t`是一个容器概念，定义和`size_t`相同，但是可能容器大小大于`size_t`最大值的情况也可以修改
- `size_t`：为了可移植性使用，如`memcpy(char* s1, const char* s2, unsigned n)`，一些机器需要拷贝更大的数据，可以改为`unsigned long`，但是对于位数更少的机器，使用`unsigned long`可能有更大的开销；为了统一函数声明，使用`size_t`，不同机器可以使用不同的类型，如`typedef size_t unsigned int`
	- 补充：`sizeof()`返回类型也是`size_t`，是一次分配内存的最大值
	- [(1 条消息) size_t 这个类型的意义是什么？ - 知乎 (zhihu.com)](https://www.zhihu.com/question/24773728)
- `unsigned int`, `unsigned long`：32位机器都是4个字节，64位机器分别是4/8字节
- `string::npos`：常用于搜索失败，如`if(s.find(xx) == string::npos)`，设置为-1，对于64位机器是`2^64 - 1`

如下段代码（linux64）
```cpp
string s = "abcd";  
int len = 2;  
cout << len - s.size() << endl; // 18446744073709551614  

cout << sizeof(unsigned long) << endl; // 8  
cout << sizeof(size_t) << endl; // 8  
cout << sizeof(unsigned int) << endl; // 4  
cout << sizeof(s.size()) << endl; // 8

cout << string::npos << endl; // 18446744073709551615
```

# 2-10

![](/blog/images/Pasted%20image%2020240811224648.png)

`using`作用域：对于`while{...}`里的，`cout`可以替代`std::cout`使用