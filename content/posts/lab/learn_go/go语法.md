---
share: true
title: go语法
date: 2024-09-26T14:41:00+08:00
tags:
  - lab
  - learn_go
dir: posts/lab/learn_go/
summary: go语法
draft: true
---
# go tour

## 基础

### 包、函数、变量

第一个程序：调用`go run main.go`
```go
package main  // Go会从main包的main函数开始执行

import (  // 导入包
	"fmt"  // 包的名字
	"math/rand"
)

func main() {
	fmt.Println("我最喜欢的数字是 ", rand.Intn(10))
}

```

包：`package`
```go
import(
	bieming "xxx"  // 别名
	"yyy"  // yyy里面大写的变量/函数才能使用
	_ "zzz"  // 匿名调用init
)
```
- 获取不同包的函数：先声明全局变量，再调用init；通过import链式调用其他包的init；不允许循环引用
```go
// main
package main

import (
	"fmt"
	"gomall/pack"
)

func main() {
	ret := pack.Add(10, 20)
	fmt.Println(ret)
	fmt.Println("x value: ", pack.X)
}
// pack
package pack

import "fmt"

func Add(x, y int) int {
	return x + y
}

var X = 10 // 先声明，再init

func init() {
	fmt.Println("hello")
}


```

导出名
```go
func main() {
	fmt.Println(math.Pi)  // math.Pi大写：已导出，可以理解为公共属性
	// math.pi会报错
}
```

函数
```go
func add(x int, y int) int {
	return x + y
}

func add(x, y int) int {  // 同样的数据类型可以省略
	return x + y
}

func swap(x, y string) (string, string) {  // 多返回值
	return y, x
}

func split(sum int) (x, y int) {  // 支持带名字的返回值
	x = sum * 4 / 9
	y = sum - x
	return
}
```

变量
```go
// 没有初始化的变量，赋值0，false, ""
var c, python, java bool

func main() {
	var i int
	fmt.Println(i, c, python, java)
}

// 如果提供初始值，自动推断数据类型
var i, j int = 1, 2

func main() {
	var c, python, java = true, false, "no!"
	fmt.Println(i, j, c, python, java)
}

// :=赋值
// :=用于新变量的声明(只能用于函数体内部)，而=用于已有变量的赋值
k := 3
```

数据的基本类型
```
bool  // 不可以用0代替false

string  // 不能改变，是常量，如str[0] = 'a'；通常用""或则``表示字符串

int  int8  int16  int32  int64
uint uint8 uint16 uint32 uint64 uintptr

// 64位机器，go默认int64, 而c++默认int32
// float默认float64

byte // uint8 的别名，即字符本质是整数

rune // int32 的别名
     // 表示一个 Unicode 码位：(utf-8)不定长字符编码，1-4字节
     // 如下面代码，输出c1 = 99
	var c1 = 'c'  // 默认rune类型, 区分var c1 byte
	fmt.Println("c1 = ", c1)

float32 float64

complex64 complex128
```
打印类型：
```go
package main

import (
	"fmt"
	"math/cmplx"
)

var (
	ToBe   bool       = false
	MaxInt uint64     = 1<<64 - 1
	z      complex128 = cmplx.Sqrt(-5 + 12i)
)

func main() {
	fmt.Printf("类型：%T 值：%v\n", ToBe, ToBe)
	fmt.Printf("类型：%T 值：%v\n", MaxInt, MaxInt)
	fmt.Printf("类型：%T 值：%v\n", z, z)
}

```
类型转换：需要显式转换；`:=`会自动进行类型推断
```go
func main() {
	var x, y int = 3, 4
	var f float64 = math.Sqrt(float64(x*x + y*y))
	var z uint = uint(f)
	fmt.Println(x, y, z)
}

// 转换后，类型不变
func main() {
	var a uint64 = 1e5
	var b uint8 = uint8(a)
	fmt.Printf("a = %T, %d, %b; b = %T, %d", a, a, a, b, b)
	// a = uint64, 100000, 11000011010100000; b = uint8, 160
}

// 其他类型转换为string
func main() {
	// int -> string
	var a int = 9
	fmt.Printf("a = %T, %d\n", a, a)
	str := fmt.Sprintf("%d", a)  // 也可以用strconv.FormatInt
	fmt.Printf("str = %T, %d", str, str)
	// a = int, 9
	//str = string, %!d(string=9)
}

// string > 其他类型
	var str string = "99"
	a, _ := strconv.ParseInt(str, 10, 64)
	fmt.Printf("a = %T, %d", a, a)


```

常量：不能用`:=`赋值
```go
const Pi = 3.14

func main() {
	const World = "世界"
	fmt.Println("Hello", World)
	fmt.Println("Happy", Pi, "Day")

	const Truth = true
	fmt.Println("Go rules?", Truth)
}
```

运算符
- `i++`：只能独立一行使用；没有`++i`，没有`i = i++`
```go
// 常见面试题：交换a, b
a = a + b
b = a - b
a = a - b
```

输入
```go
func main() {
	var a int
	fmt.Scanln(&a)
	fmt.Println(a)
	var str string
	fmt.Scanf("%s", &str)
	fmt.Println(str)
}
```


### 控制语句

for：没有小括号
```go
func main() {
	sum := 0
	for i := 0; i < 10; i++ {
		sum += i
	}
	fmt.Println(sum)
}

// for也是while
func main() {
	sum := 1
	for sum < 1000 {
		sum += sum
	}
	fmt.Println(sum)
}

// 无限循环
for{}

// 下标
for id, val := range str{
	
}
```


if：也不需要小括号
```go
// 同for, if 语句可以在条件表达式前执行一个简短语句
// 该语句声明的变量作用域仅在 if 之内。
func pow(x, n, lim float64) float64 {
	if v := math.Pow(x, n); v < lim {
		return v
	}
	return lim
}
```

switch：两点不同
- case自动添加了break
- case后无需为常量，且取值不限于整数
```go
package main

import (
	"fmt"
	"runtime"
)

func main() {
	fmt.Print("Go 运行的系统环境：")
	switch os := runtime.GOOS; os {
	case "darwin":
		fmt.Println("macOS.")
	case "linux":
		fmt.Println("Linux.")
	default:
		// freebsd, openbsd,
		// plan9, windows...
		fmt.Printf("%s.\n", os)
	}
}

```
可以等同于`if-then-else`
```go
package main

import (
	"fmt"
	"time"
)

func main() {
	t := time.Now()
	switch {
	case t.Hour() < 12:
		fmt.Println("早上好！")
	case t.Hour() < 17:
		fmt.Println("下午好！")
	default:
		fmt.Println("晚上好！")
	}
}

```

defer
- 推迟调用的函数其参数会立即求值，但直到外层函数返回前该函数都不会被调用
- 将函数推迟到外层函数返回之后执行
```go
func main() {
	defer fmt.Println("world")

	fmt.Println("hello")
}

// 推迟调用的函数调用会被压入一个栈中。 当外层函数返回时，被推迟的调用会按照后进先出的顺
// 序调用。
func main() {
	fmt.Println("counting")

	for i := 0; i < 10; i++ {
		defer fmt.Println(i)
	}

	fmt.Println("done")
}
```

### 结构体、切片、映射

类型别名
自定义类型：转换时，如`var y int = (int)x`需要强制转换
```go
package main

import "fmt"

type MyInt int // 类型定义

type YourInt = int // 类型别名
// 常用：byte = uint8, rune = int32

func main() {
	var a MyInt
	a = 10
	fmt.Printf("%T, %v\n", a, a) // main.MyInt, 10

	var b YourInt
	b = 100
	fmt.Printf("%T, %v\n", b, b) // int, 100

}

```


指针
- 空值是：`nil`
- 没有指针运算
- 用前要先分配空间(new)，同map（make，用于slice, map, chan）
```go
func main() {
	i, j := 42, 2701

	p := &i         // 指向 i
	fmt.Println(*p) // 通过指针读取 i 的值
	*p = 21         // 通过指针设置 i 的值
	fmt.Println(i)  // 查看 i 的值

	p = &j         // 指向 j
	*p = *p / 37   // 通过指针对 j 进行除法运算
	fmt.Println(j) // 查看 j 的值
}
```

结构体
- 如果函数返回结构体，常返回指针，可以节省传递开销
```go
type Vertex struct {  // type定义类型
	X int
	Y int
}

func main() {
	fmt.Println(Vertex{1, 2})  // 会输出：{1 2}

	v := Vertex{1, 2}
	v.X = 4  // 访问
	fmt.Println(v.X)

	v := Vertex{1, 2}
	p := &v
	p.X = 100 // 等同于(*p).x，吟诗解引用
	fmt.Println(v)
}
// 第二段
type Vertex struct {
	X, Y int
}

var (
	v1 = Vertex{1, 2}  // 创建一个 Vertex 类型的结构体
	v2 = Vertex{X: 1}  // Y:0 被隐式地赋予零值
	v3 = Vertex{}      // X:0 Y:0
	p  = &Vertex{1, 2} // 创建一个 *Vertex 类型的结构体（指针）
)

func main() {
	fmt.Println(v1, p, v2, v3)  // p会打印：&{1 2}
}
```
- 内存对齐
```go
package main

import (
	"fmt"
	"unsafe"
)

type first struct {
	x, y int
}

// why内存对齐：减少CPU访问内存次数
type second struct {
	a int32 // 这个元素按照4对齐
	b int64 // 这个元素按照8对齐
	// 一般从小到大写元素
}

func main() {
	fmt.Println(unsafe.Sizeof(first{}))
	fmt.Println(unsafe.Sizeof(second{}))
}

```
- 嵌套结构体：继承
```go
package main

import "fmt"

// animal and dog
type animal struct {
	name string
}

func (a *animal) move() {
	fmt.Println(a.name, ": move")
}

type dog struct {
	feet int
	*animal
}

func (d *dog) wang() {
	fmt.Println(d.name, "wang", d.feet)
}

func main() {
	d := &dog{
		feet: 4,
		animal: &animal{
			name: "haha",
		},
	}
	d.wang()
	d.move()
}

```


值类型和引用类型
- 值：int, float, struct, 数组，string
- 引用：指针，slice（其实是值传递，本质是struct，但是array指向底层数组）, map, channel, interface（本质也是struct）

数组
```go
package main

import "fmt"

func main() {
	var a [2]string  // 声明
	a[0] = "Hello"
	a[1] = "World"
	fmt.Println(a[0], a[1])
	fmt.Println(a)  // 会输出；[Hello World]

	primes := [6]int{2, 3, 5, 7, 11, 13}
	fmt.Println(primes)
}

```

切片
- 区间是左闭右开，上下界默认0/length，如`[:5], [1:]`
- 是数组的一个视角，更改切片会更改原数组
- 理解为元素是指向原数组的指针
```go
// 地址
func main() {
	arr := [5]int{1, 2, 3, 4, 5}
	fmt.Printf("%v\n", &arr[1])
	slice := arr[1:3]
	fmt.Printf("%v", &slice[0])
}
// 类似这种定义
type slice struct {
	p   *[2]int
	len int
	cap int
}

package main

import "fmt"

func main() {
	primes := [6]int{2, 3, 5, 7, 11, 13}

	var s []int = primes[1:4]
	fmt.Println(s)
}

// 切片字面量：先创建数组，再用切片
package main

import "fmt"

func main() {
	q := []int{2, 3, 5, 7, 11, 13}
	fmt.Println(q)

	r := []bool{true, false, true, true, false, true}
	fmt.Println(r)

	s := []struct {
		i int
		b bool
	}{
		{2, true},
		{3, false},
		{5, true},
		{7, true},
		{11, false},
		{13, true},
	}
	fmt.Println(s)
}


// 默认行为
package main

import "fmt"

func main() {
	s := []int{2, 3, 5, 7, 11, 13}

	s = s[1:4]
	fmt.Println(s)

	s = s[:2]
	fmt.Println(s)

	s = s[1:]
	fmt.Println(s)
}
// [3 5 7]
// [3 5]
// [5]


// 扩展和舍弃，查看长度（len，切片里元素的数目）和容量（切片里第一个元素到最后一个元素的数目）
package main

import "fmt"

func main() {
	s := []int{2, 3, 5, 7, 11, 13}
	printSlice(s)

	// 截取切片使其长度为 0
	s = s[:0]
	printSlice(s)

	// 扩展其长度
	s = s[:4]
	printSlice(s)

	// 舍弃前两个值
	s = s[2:]
	printSlice(s)
}

func printSlice(s []int) {
	fmt.Printf("len=%d cap=%d %v\n", len(s), cap(s), s)
}
/*
len=6 cap=6 [2 3 5 7 11 13]
len=0 cap=6 []
len=4 cap=6 [2 3 5 7]
len=2 cap=4 [5 7]
*/

// nil切片：nil 切片的长度和容量为 0 且没有底层数组
func main() {
	var s []int
	fmt.Println(s, len(s), cap(s))
	if s == nil {
		fmt.Println("nil!")
	}
}

// make创建切片，在底层维护，不能像arr那样维护
func main() {
	a := make([]int, 5)
	printSlice("a", a)

	b := make([]int, 0, 5)
	printSlice("b", b)

	c := b[:2]
	printSlice("c", c)

	d := c[2:5]
	printSlice("d", d)
}

func printSlice(s string, x []int) {
	fmt.Printf("%s len=%d cap=%d %v\n",
		s, len(x), cap(x), x)
}
/*
a len=5 cap=5 [0 0 0 0 0]
b len=0 cap=5 []
c len=2 cap=5 [0 0]  // 这里b指向的数组是全0，所以相当于c做了一个切片扩展
d len=3 cap=3 [0 0 0]
*/

// 切片的切片
	board := [][]string{
		[]string{"_", "_", "_"},
		[]string{"_", "_", "_"},
		[]string{"_", "_", "_"},
	}

// append
package main

import "fmt"

func main() {
	var s []int
	printSlice(s)

	// 可在空切片上追加
	s = append(s, 0)
	printSlice(s)

	// 这个切片会按需增长
	s = append(s, 1)
	printSlice(s)

	// 可以一次性添加多个元素
	s = append(s, 2, 3, 4)
	printSlice(s)
}

func printSlice(s []int) {
	fmt.Printf("len=%d cap=%d %v\n", len(s), cap(s), s)
}

// for的range遍历：返回两个值，下标，值
var pow = []int{1, 2, 4, 8, 16, 32, 64, 128}

func main() {
	for i, v := range pow {
		fmt.Printf("2**%d = %d\n", i, v)
	}
	for i := range pow {  // 省略值
		pow[i] = 1 << uint(i) // == 2**i
	}
	for _, value := range pow {  // _表示下标
		fmt.Printf("%d\n", value)
	}
}

// string可以理解为不可以改的切片，修改时需要先转换
func main() {
	a := "abc"
	fmt.Println(a)
	arr := []byte(a)  // []rune
	arr[0] = 'd'
	a = string(arr)
	fmt.Println(a)
}
```

map
- make：初始化m为nil，make先在内存初始化变量，然后用m指向这个变量
- 无序，需要先放到slice中变有序
```go
package main

import "fmt"

type Vertex struct {
	Lat, Long float64
}

var m map[string]Vertex

func main() {
	m = make(map[string]Vertex)
	m["Bell Labs"] = Vertex{
		40.68433, -74.39967,
	}
	fmt.Println(m["Bell Labs"])
}

// 也可以这样初始化
var m = map[string]Vertex{
	"Bell Labs": Vertex{
		40.68433, -74.39967,
	},
	"Google": Vertex{
		37.42202, -122.08408,
	},
}
// 甚至
var m = map[string]Vertex{
	"Bell Labs": {40.68433, -74.39967},
	"Google":    {37.42202, -122.08408},
}

// 其他操作
func main() {
	m := make(map[string]int)

	m["答案"] = 42  // 赋值
	fmt.Println("值：", m["答案"])

	m["答案"] = 48  // 修改
	fmt.Println("值：", m["答案"])

	delete(m, "答案")  // 删除
	fmt.Println("值：", m["答案"])

	v, ok := m["答案"]  // 查看是否存在
	fmt.Println("值：", v, "是否存在？", ok)
}

```


函数：函数也是值
- 小写：私有
- 大写：其他文件也可以访问
- 不支持重载
- 变长参数：如`(args... int)`，此时args是切片类型
```go
package main

import (
	"fmt"
	"math"
)

// fn是函数，接收两个float64，返回float64
func compute(fn func(float64, float64) float64) float64 {  
	return fn(3, 4)
}

func main() {
	hypot := func(x, y float64) float64 {
		return math.Sqrt(x*x + y*y)
	}
	fmt.Println(hypot(5, 12))

	fmt.Println(compute(hypot))
	fmt.Println(compute(math.Pow))
}

```

init函数
- 每个文件都有，在main前调用
- 先执行变量定义，再执行init，最后执行main
- 常用于引入别的包的变量，别的包会先用init函数
```go
package main

import "fmt"

var temp = first()

func first() int {
	fmt.Println("first")
	return 10
}

func init() {
	fmt.Printf("hello\n")
}

func main() {
	fmt.Println("world")
	fmt.Printf("%d\n", temp)
}

```

匿名函数：只使用一次
- 直接使用
- 变量
```go
func main() {
	ret := func(a, b int) int {
		return a + b
	}(10, 20)
	fmt.Println(ret)
}
// or
func main() {
	ret := func(a, b int) int {
		return a + b
	}
	fmt.Println(ret(10, 20))
}
```

函数闭包
- 返回函数 + 外层变量的引用；类似：类的变量+方法
- 可以“记住”并访问其外部作用域的变量
```go
package main

import "fmt"

func adder() func(int) int {  // 返回类型是函数
	sum := 0  // 变量被记住了
	return func(x int) int {
		sum += x
		return sum
	}
}

func main() {
	pos, neg := adder(), adder()
	for i := 0; i < 10; i++ {
		fmt.Println(
			pos(i),
			neg(-2*i),
		)
	}
}

// fib
func fibonacci() func() int {
	a, b := 0, 1 // 初始化前两个斐波那契数
	return func() int {
		ret := a
		a, b = b, a+b 
		return ret      // 返回当前的斐波那契数
	}
}

func main() {
	f := fibonacci()
	for i := 0; i < 10; i++ {
		fmt.Println(f())
	}
}

```

### 错误处理

defer
panic，error.new("xxx")
recover
- [Panic & Recover | Go 面试宝典 (ryansu.tech)](https://goguide.ryansu.tech/guide/concepts/golang/10-panic-recover.html#panic-%E7%9A%84%E6%89%A7%E8%A1%8C%E6%B5%81%E7%A8%8B)
```go
package main

import "fmt"

func test() {
	defer func() { // 运行完函数 or 遇到错误
		err := recover() // 捕获异常
		if err != nil {
			fmt.Println("divide 0 wrong!!!")
		}
	}()
	a := 10
	b := 0
	c := a / b
	fmt.Println(c)
}

func main() {
	test()
	fmt.Printf("over")
}

```

### time库

```go
package main

import (
	"fmt"
	"time"
)

func main() {
	now := time.Now()
	fmt.Println(now)
	// 日期 > 时间戳：从1970.1.1到现在的秒数
	timeStamp := now.Unix()
	fmt.Println(timeStamp)
	t := time.Unix(1730803941, 0)
	fmt.Println(t)  // 时间戳 > 具体日期
	// 定时器，通道
	for tmp := range time.Tick(time.Second) {
		fmt.Println(tmp)
	}
	// 时间格式化
	// y m d h m s
	// 2016 01 02 15 04 05
	fmt.Println(now.Format("2006-01-02 15:04:05 PM"))
}

```

### 文件操作

os.open
file.close
bufio.newreader：带缓冲区
```go
package main

import (
	"bufio"
	"fmt"
	"io"
	"os"
)

func main() {
	file, err := os.Open("test.txt")
	if err != nil {
		fmt.Println("open file error: ", err)
		return
	}

	defer file.Close()

	reader := bufio.NewReader(file)  
	for {
		readString, err := reader.ReadString('\n')
		fmt.Println(readString)
		if err == io.EOF {
			break
		}
	}
// 还可以
	//// 读取
	//var tmp = make([]byte, 128)
	//n, err := file.Read(tmp)
	//fmt.Println("read bytes: ", n)
	//fmt.Println(string(tmp)) // byte > string
}
```

写入
bufio.newwriter
writer.flush
ioutil

判断路径存在
os.stat(path)

复制
io.copy

### json

应用场景
![](/blog/images/Pasted%20image%2020241009233121.png)

序列化
`json.Marshal`
`tag`
```go
package main

import (
	"encoding/json"
	"fmt"
)

type ser struct {
	Num  int    `json:"number"`  // 注意大写，不然marshal访问不到
	Name string `json:"Name"`
}

func test() {
	x := ser{Num: 10, Name: "tim"}
	data, err := json.Marshal(&x)  // 需要跨包使用struct，所以都要变量大写
	if err != nil {
		return
	}
	fmt.Printf("%v", string(data))
}

func main() {
	test()
}

```

### go routine

主线程，协程
MPG（主线程，上下文，协程）

### 管道

sync同步
mutex锁

channel：线程安全，本质是队列
- 只读，只写
- select
```go
package main

import "fmt"

func main() {
	var ch chan int
	ch = make(chan int, 3)
	fmt.Printf("%v", ch) // 指针，引用类型
	ch <- 10             // 入队
	fmt.Println("%v, %v, %v", len(ch), cap(ch))
	var a int
	a = <-ch // 出队
	fmt.Println(a)
	ch <- 10
	ch <- 20
	ch <- 30
	// 遍历前要先关闭管道
	close(ch)
	// 遍历
	for v := range ch {
		fmt.Println(v)
	}
}

```


### 反射

如interface，只有在程序运行时才能知道变量的类型
如orm和struct的赋值
```go
package main

import (
	"fmt"
	"reflect"
)

type cat struct {
}

func reflectType(x interface{}) {
	// 1. 通过类型断言：eg. x, ok := x.(string)
	// 2. 通过反射获取类型和值
	obj := reflect.TypeOf(x)
	fmt.Println(obj, obj.Name(), obj.Kind())
	fmt.Printf("type is : %T\n", obj)
	objValue := reflect.ValueOf(x)
	fmt.Println(objValue)
	fmt.Printf("type is : %T\n", objValue)
	// 后续可以通过如v.Float()将reflect.Value > float
	// 也可以通过v.SetInt(100)修改，注意函数需要传入指针
}

func main() {
	var a float64 = 1.23
	reflectType(a)
	var b int32 = 22
	reflectType(b)
	var c cat
	reflectType(c)
}

```

结构体反射
```go
package main

import (
	"fmt"
	"reflect"
)

type student struct {
	Name  string `json:"name"` // 注意大写
	Score int    `json:"score"`
}

func main() {
	s := student{
		Name:  "tim",
		Score: 90,
	}
	// reflect
	t := reflect.TypeOf(s)
	fmt.Println(t, t.Name(), t.Kind())
	for i := 0; i < t.NumField(); i++ {
		// i是索引
		fmt.Println(t.Field(i)) // struct, 包含name, type, tag等
	}
}

```

### redis

0-15号
key-value
```redis
root@LAPTOP-P7DDEO1F:~# redis-cli
127.0.0.1:6379> set key1 hello
OK
127.0.0.1:6379> get key1
"hello"
127.0.0.1:6379> select 1
OK
127.0.0.1:6379[1]> select 0
OK
127.0.0.1:6379> dbsize
(integer) 1
127.0.0.1:6379> get key2
(nil)
127.0.0.1:6379> del key1
(integer) 1
127.0.0.1:6379> get key1
(nil)
127.0.0.1:6379> mset key1 hello key2 world
OK
127.0.0.1:6379> mget key1 key2
1) "hello"
2) "world"
127.0.0.1:6379>
```

数据类型
- string
- hash：`hget, hset`
- list：`lpush, lrange, rpush, lpop`
- set：`sadd, sismember, srem`
- zset：有序集合

```go
package main

import (
	"fmt"
	"github.com/garyburd/redigo/redis"
)

func main() {
	conn, err := redis.Dial("tcp", "localhost:6379")
	if err != nil {
		return
	}
	defer conn.Close()
	ret, _ := redis.String(conn.Do("Get", "key1"))
	fmt.Println(ret)
}

```

## 方法和接口

go没有类，但是可以定义方法：一类带特殊的 接收者 参数的函数
```go
package main

import (
	"fmt"
	"math"
)

type Vertex struct {
	X, Y float64
}

func (v Vertex) Abs() float64 {
	return math.Sqrt(v.X*v.X + v.Y*v.Y)
}

func main() {
	v := Vertex{3, 4}
	fmt.Println(v.Abs())
}


// 与函数的区别
func Abs(v Vertex) float64 {
	return math.Sqrt(v.X*v.X + v.Y*v.Y)
}

func main() {
	v := Vertex{3, 4}
	fmt.Println(Abs(v))
}

// 非结构体也可以定义方法
// 接收者的类型定义和方法声明必须在同一包内
type MyFloat float64

func (f MyFloat) Abs() float64 {
	if f < 0 {
		return float64(-f)
	}
	return float64(f)
}

func main() {
	f := MyFloat(-math.Sqrt2)
	fmt.Println(f.Abs())
}

// 指针
type Vertex struct {
	X, Y float64
}

func (v Vertex) Abs() float64 {
	return math.Sqrt(v.X*v.X + v.Y*v.Y)
}

func (v *Vertex) Scale(f float64) {  // 如果去掉*，则对副本进行scale；通常用指针接收者
	v.X = v.X * f
	v.Y = v.Y * f
}

func main() {
	v := Vertex{3, 4}
	// 由于 Scale 方法有一个指针接收者，为方便起见，Go 会将语句 v.Scale(5) 解释为 (&v).Scale(5)
	v.Scale(10)
	fmt.Println(v.Abs())
}

```


接口
- 方法的集合，是一种类型
- 接口类型的变量可以持有任何实现了这些方法的值
- 类似多态；隐式 [Java与Go：方法和接口_go接口与java接口-CSDN博客](https://blog.csdn.net/weixin_45700531/article/details/136893410)
```go
package main

import (
	"fmt"
)

// 接口是类型，方法的集合
type dog struct {
}

func (d dog) say() {
	fmt.Println("wang~")
}

type cat struct {
}

func (c cat) say() {
	fmt.Println("miao~")
}

// 只要有say方法的类型，都可以看作sayer类型
type sayer interface {
	say()
}

func do(arg sayer) {
	arg.say()
}

func main() {
	c := cat{}
	c.say()
	do(c)
	d := dog{}
	do(d)

	var s sayer
	c2 := cat{}
	s = c2
	fmt.Println(s)
}
```
- 指针接收者的接口实现，只能用指针调用：[Go 指针与接口那些事 - 晨鹤部落格 (chenhe.me)](https://chenhe.me/post/pointer-and-interface-in-go)
- 本质是struct，存类型 + 动态值（指针，指向值）

  
类型断言：用空接口可以代表任意类型
- 可以用于多态数组，即接口可以对应任何类型
- `map[string]interface{}`
```go
package main

import "fmt"

func main() {
	var i interface{} = "hello"  // 现在i就是string: "hello"
	fmt.Println(i)
	s := i.(string)
	fmt.Println(s)

	s, ok := i.(string)
	fmt.Println(s, ok)

	f, ok := i.(float64)
	fmt.Println(f, ok)

	f = i.(float64) // panic
	fmt.Println(f)
}


// eg.
package main

import "fmt"

func do(i interface{}) {
	switch v := i.(type) {
	case int:
		fmt.Printf("二倍的 %v 是 %v\n", v, v*2)
	case string:
		fmt.Printf("%q 长度为 %v 字节\n", v, len(v))
	default:
		fmt.Printf("我不知道类型 %T!\n", v)
	}
}

func main() {
	do(21)
	do("hello")
	do(true)
}

```

stringer：fmt默认输出字符串的格式
```go
type Stringer interface {
    String() string
}
// 修改
package main

import "fmt"

type Person struct {
	Name string
	Age  int
}

func (p Person) String() string {
	return fmt.Sprintf("%v (%v years)", p.Name, p.Age)
}

func main() {
	a := Person{"Arthur Dent", 42}
	z := Person{"Zaphod Beeblebrox", 9001}
	fmt.Println(a, z)
}

```

error: fmt会自动调用err
```go
type error interface {
    Error() string
}

i, err := strconv.Atoi("42")
if err != nil {
    fmt.Printf("couldn't convert number: %v\n", err)
    return
}
fmt.Println("Converted integer:", i)

// 修改
package main

import (
	"fmt"
	"time"
)

type MyError struct {
	When time.Time
	What string
}

func (e *MyError) Error() string {
	return fmt.Sprintf("at %v, %s",
		e.When, e.What)
}

func run() error {
	return &MyError{
		time.Now(),
		"it didn't work",
	}
}

func main() {
	if err := run(); err != nil {
		fmt.Println(err)
	}
}

```


reader
```go
package main

import (
	"fmt"
	"io"
	"strings"
)

func main() {
	r := strings.NewReader("Hello, Reader!")

	b := make([]byte, 8)
	for {
		n, err := r.Read(b)
		fmt.Printf("n = %v err = %v b = %v\n", n, err, b)
		fmt.Printf("b[:n] = %q\n", b[:n])
		if err == io.EOF {
			break
		}
	}
}

```

图像
```go
package image

type Image interface {
    ColorModel() color.Model
    Bounds() Rectangle
    At(x, y int) color.Color
}
```

### 泛型

参数可以是任意类型
```go
package main

import "fmt"

// Index 返回 x 在 s 中的下标，未找到则返回 -1。
func Index[T comparable](s []T, x T) int {
	for i, v := range s {
		// v 和 x 的类型为 T，它拥有 comparable 可比较的约束，
		// 因此我们可以使用 ==。
		if v == x {
			return i
		}
	}
	return -1
}

func main() {
	// Index 可以在整数切片上使用
	si := []int{10, 20, 15, -10}
	fmt.Println(Index(si, 15))

	// Index 也可以在字符串切片上使用
	ss := []string{"foo", "bar", "baz"}
	fmt.Println(Index(ss, "hello"))
}

```
### 并发

并行：如多个CPU运行两个任务
并发：一个CPU模拟同一时间段内运行两个任务

协程：用户态线程
```go
package main

import (
	"fmt"
	"sync"
)

var wg sync.WaitGroup

func say() {
	fmt.Println("hello")
	wg.Done() // 通知wg把计数器-1
}

func main() {

	wg.Add(1)
	go say()
	fmt.Println("main") // 可能只打印main，say协程没有运行就退出了
	// time.Sleep(1 * time.Second) // 默认纳秒
	wg.Wait() // 等go routine运行完才结束
}

```

channel
- 并发模型：CSP，通过通信实现共享内存
- 两个gorountine通信
- FIFO
- 引用类型
```go
package main

import "fmt"

func main() {
	var ch chan int        // 引用类型，需要初始化
	ch = make(chan int, 1) // 缓冲区大小
	// make(chan int) 无缓冲区通道：同步，必须手把手交换数据
	ch <- 10
	x := <-ch
	fmt.Println(x)
	// len: 个数
	// cap: 容量
	close(ch)
}
```
例子：注意单向通道
```go
package main

import "fmt"

func f1(ch chan<- int) {
	for i := 0; i < 100; i++ {
		ch <- i
	}
	close(ch)
}

func f2(ch1 <-chan int, ch2 chan<- int) {
	for {
		tmp, ok := <-ch1
		if !ok {
			break
		}
		ch2 <- tmp * tmp
	}
	close(ch2)
}

func main() {
	// 两个goroutine
	// 1. 生成0-100数字，发送到ch1
	// 2. 接收，计算平方，发送到ch2
	ch1 := make(chan int, 100)
	ch2 := make(chan int, 200)

	go f1(ch1)
	go f2(ch1, ch2)
	// 遍历方式2
	for ret := range ch2 {
		fmt.Println(ret)
	}
}

```

worker pool，goroutine池：控制数量

select：多路复用，即从不同chan取值，类似switch
```go
package main

import "fmt"

func main() {
	ch := make(chan int, 1)  // 假如是10，那么select会随机选一种执行
	for i := 0; i < 10; i++ {
		select {
		case x := <-ch:
			fmt.Println(x)
		case ch <- i:
		default:
			fmt.Println("nothing to do")
		}
	}
}

```


mutex
```go
package main

import (
	"fmt"
	"sync"
)

// 多个goroutine并发操作全局变量
var (
	x     int64
	wg    sync.WaitGroup
	mutex sync.Mutex // 互斥锁
)

func add() {
	for i := 0; i < 500000; i++ {
		mutex.Lock()
		x = x + 1 // 获取x，加1，给x赋值；多个进程会出现竞争的情况
		mutex.Unlock()
	}
	wg.Done()
}

func main() {
	wg.Add(2)
	go add()
	go add()
	wg.Wait()
	fmt.Println(x)
}

```
rwlock
```go
package main

import (
	"fmt"
	"sync"
	"time"
)

var (
	x      int64
	wg     sync.WaitGroup
	mutex  sync.Mutex
	rwlock sync.RWMutex // 适用于读多，写少
)

func read() {
	rwlock.RLock()  // 写锁
	time.Sleep(time.Millisecond)
	rwlock.RUnlock()
	wg.Done()
}

func write() {
	rwlock.Lock()
	x = x + 1
	time.Sleep(time.Millisecond * 10)
	rwlock.Unlock()
	wg.Done()
}

func main() {
	start := time.Now()

	for i := 0; i < 1000; i++ {
		wg.Add(1)
		go read()
	}

	for i := 0; i < 10; i++ {
		wg.Add(1)
		go write()
	}

	wg.Wait()
	fmt.Println(x)
	fmt.Println(time.Now().Sub(start))
}

```

map并发不安全，可以使用Sync中的map
```go
package main

import (
	"fmt"
	"sync"
)

var (
	wg sync.WaitGroup
)

var m = make(map[int]int) // 并发不安全
var m2 = sync.Map{}

func get(key int) int {
	return m[key]
}

func set(key int, value int) {
	m[key] = value
}

func main() {
	for i := 0; i < 20; i++ {
		wg.Add(1)
		go func(i int) {
			//set(i, i+100)
			//fmt.Println("key, value: ", i, get(i))
			m2.Store(i, i+100)
			v, _ := m2.Load(i)
			fmt.Println("key, value: ", i, v)
			wg.Done()
		}(i)
	}
	wg.Wait()
}

```

### socket

![](/blog/images/Pasted%20image%2020241108012157.png)
- 通过socket与TCP交互
```go
// server
package main

import (
	"bufio"
	"fmt"
	"net"
)

func process(conn net.Conn) {
	defer conn.Close()
	for {
		reader := bufio.NewReader(conn)
		var buf [128]byte
		n, err := reader.Read(buf[:])
		if err != nil {
			fmt.Println("read from conn fail", err)
			return
		}
		fmt.Println(string(buf[:n]))
		conn.Write([]byte("ok")) // 返回ok
	}
}

func main() {
	// server
	listen, err := net.Listen("tcp", "127.0.0.1:9999")
	if err != nil {
		fmt.Println("server fail")
		return
	}
	for {
		conn, err := listen.Accept()
		if err != nil {
			fmt.Println("accept fail")
			continue
		}
		go process(conn)
	}
}

// client
package main

import (
	"bufio"
	"fmt"
	"net"
	"os"
	"strings"
)

func main() {
	conn, err := net.Dial("tcp", "127.0.0.1:9999")
	if err != nil {
		fmt.Println("dial fail", err)
		return
	}
	// 发送和接收
	input := bufio.NewReader(os.Stdin)
	for {
		str, err := input.ReadString('\n')
		if err != nil {
			fmt.Println("input fail", err)
			return
		}
		str = strings.TrimSpace(str)
		if strings.ToUpper(str) == "Q" {
			break
		}
		_, err = conn.Write([]byte(str))
		if err != nil {
			fmt.Println("send fail", err)
			return
		}
		// 读
		var buf [1024]byte
		n, err := conn.Read(buf[:])
		if err != nil {
			fmt.Println("read ok fail", err)
			return
		}
		fmt.Println(string(buf[:n]))
	}
}

```

粘包：多个包合起来再通过TCP发送
- 可以自己写协议，定义自己包的长度




# learn go with tests

[Go 基础 - Hello, World - 《通过测试学习 Go 语言（Learn Go with tests 中文版）2019》 - 书栈网 · BookStack](https://www.bookstack.cn/read/learn-go-with-tests-zh/hello-world.md)

![](/blog/images/Pasted%20image%2020241009234404.png)

## hello world

hello.go
```go
package main

import "fmt"

func Hello() string {
	return "Hello, world"
}

func main() {
	fmt.Println(Hello())
}

```

hello_test.go：运行`go test`
```go
package main

import "testing"

func TestHello(t *testing.T) {
	t.Run("saying hello to people", func(t *testing.T) {
		got := Hello("Chris")
		want := "Hello, Chris"
		if got != want {
			t.Errorf("got '%s' want '%s'", got, want)
		}
	})
	t.Run("say hello world when an empty string is supplied", func(t *testing.T) {
		got := Hello("")
		want := "Hello, World"
		if got != want {
			t.Errorf("got '%s' want '%s'", got, want)
		}
	})
}


```

go mod
- [linux - 错误消息“go：在当前目录或任何父目录中找不到 go.mod 文件；请参阅 'go help modules'” - SegmentFault 思否](https://segmentfault.com/q/1010000042783749)

hello.go重构代码：常量
```go
const helloPrefix = "Hello, "

func Hello(name string) string {
	return helloPrefix + name
}
```

test重构：函数代替同样的代码段
```go
func TestHello(t *testing.T) {
    assertCorrectMessage := func(t *testing.T, got, want string) {
        t.Helper()  // 报告行号
        if got != want {
            t.Errorf("got '%s' want '%s'", got, want)
        }
    }
    t.Run("saying hello to people", func(t *testing.T) {
        got := Hello("Chris")
        want := "Hello, Chris"
        assertCorrectMessage(t, got, want)
    })
    t.Run("empty string defaults to 'world'", func(t *testing.T) {
        got := Hello("")
        want := "Hello, World"
        assertCorrectMessage(t, got, want)
    })
}
```

解决hello
```go
func Hello(name string) string {
	if name == "" {
		name = "World"
	}
	return helloPrefix + name
}
```

TDD：多种语言
test
```go
	t.Run("in Spanish", func(t *testing.T) { // TDD(test driven development)
		got := Hello("Elodie", "Spanish")
		want := "Hola, Elodie"
		assertCorrectMessage(t, got, want)
	})
```

hello.go：支持多种语言，且重构（常量，私有函数）
```go
const french = "French"
const spanish = "Spanish"
const englishPrefix = "Hello, "
const spanishHelloPrefix = "Hola, "
const frenchHelloPrefix = "bj, "

func Hello(name string, language string) string {
	if name == "" {
		name = "World"
	}
	return greetingPrefix(language) + name
}
func greetingPrefix(language string) (prefix string) { // 重构，私有函数
	switch language {
	case french:
		prefix = frenchHelloPrefix
	case spanish:
		prefix = spanishHelloPrefix
	default:
		prefix = englishPrefix
	}
	return
}
```


总结
- 编写一个失败的测试，并查看失败信息，可以看到我们已经为需求写了一个 相关 的测试，并且看到它产生了一个 易于理解的失败描述
- 编写最少量的代码以使其通过，因此我们知道我们有可工作软件
- 然后 重构，支持我们测试的安全性，以确保我们拥有易于使用的精心制作的代码

## 整数

函数前可以加入注释
官方文档：godoc
- [Go语言标准库文档中文版 | Go语言中文网 | Golang中文社区 | Golang中国 (studygolang.com)](https://studygolang.com/pkgdoc)
```cmd
go install golang.org/x/tools/cmd/godoc
godoc -http :8000
```


示例：也是test；`go test -v`查看
```go
func ExampleAdd() {
	sum := Add(1, 5)
	fmt.Println(sum)
	// Output: 6
}
```

## 迭代

基准测试：`go test -bench=.`
```go
func BenchmarkRepeat(b *testing.B) {
    for i := 0; i < b.N; i++ {
        Repeat("a")
    }
}
```
testing.B 可使你访问隐性命名（cryptically named）b.N。
基准测试运行时，代码会运行 b.N 次，并测量需要多长时间

## 数组与切片

数组
```go
func Sum(numbers [5]int) int {  // [5]int：数组
	var ret int
	//for i := 0; i < 5; i++ {
	//	ret += numbers[i]
	//}
	for _, number := range numbers {  // range遍历
		ret += number
	}
	return ret
}
```

切片
```go
func Sum(numbers []int) int { // []int: 切片
	sum := 0
	for _, number := range numbers {
		sum += number
	}
	return sum
}
```

test.go
- 切片不可以用等号比较：切片可以包含不同的数据类型，逐个比较的开销大
- 可以自己写函数比较
- 可以用`reflect.DeepEqual比较
```go
func TestSumAll(t *testing.T) {
	got := SumAll([]int{1, 2}, []int{0, 9})
	want := []int{3, 9}
	//if got != want { // 在 Go 中不能对切片使用等号运算符
	//	t.Errorf("got %v want %v", got, want)
	//}
	if !reflect.DeepEqual(got, want) {
		t.Errorf("got %v want %v", got, want)
	}
}
```
可变参数
```go
func SumAll(numbersToSum ...[]int) (sums []int) { // 可变参数
	lengthOfNumbers := len(numbersToSum)
	sums = make([]int, lengthOfNumbers) // 创建一个长度为len的切片
	for i, numbers := range numbersToSum {
		sums[i] = Sum(numbers)
	}
	return
}
// 重构
func SumAll(numbersToSum ...[]int) []int {
    var sums []int
    for _, numbers := range numbersToSum {
        sums = append(sums, Sum(numbers))  // 切片的容量是固定的，但是你可以使用 `append` 从原来的切片中创建一个新切片
    }
    return sums
}
```

在线编译器：[Go Playground - The Go Programming Language](https://go.dev/play/)

## 结构体，方法和接口

结构体
```go
type Rectangle struct {
	Width  float64
	Height float64
}

type Circle struct {
	Radius float64
}
```
求矩形、圆形的周长和面积
```go
func Area(circle Circle) float64 { ... }
func Area(rectangle Rectangle) float64 { ... }
```
报错：'Area' redeclared 

方法
```go
func (r Rectangle) Area() float64 {
	return r.Width * r.Height
}
```

接口：interface，隐式
```go
type Shape interface {
	Area() float64
}
```

表格驱动测试
```go
func TestArea(t *testing.T) {
    areaTests := []struct {
        shape Shape
        want  float64
    }{
        {Rectangle{12, 6}, 72.0},
        {Circle{10}, 314.1592653589793},
    }
    for _, tt := range areaTests {
        got := tt.shape.Area()
        if got != tt.want {
            t.Errorf("got %.2f want %.2f", got, tt.want)
        }
    }
}
```
运行指定的测试：`go test -run TestArea/Rectangle`
```go
func TestArea(t *testing.T) {
	areaTests := []struct {
		name    string
		shape   Shape
		hasArea float64
	}{
		{name: "Rectangle", shape: Rectangle{Width: 12, Height: 6}, hasArea: 72.0},
		{name: "Circle", shape: Circle{Radius: 10}, hasArea: 314.1592653589793},
		{name: "Triangle", shape: Triangle{Width: 12, Height: 6}, hasArea: 36.0},
	}
	for _, tt := range areaTests {
		// using tt.name from the case to use it as the `t.Run` test name
		t.Run(tt.name, func(t *testing.T) { // 运行指定的shape: go test -run TestArea/Rectangle -v
			got := tt.shape.Area()
			if got != tt.hasArea {
				t.Errorf("%#v got %.2f want %.2f", tt.shape, got, tt.hasArea)
			}
		})
	}
}
```

## 指针和错误

stringer
```go
type Stringer interface {
	String() string
}

func (b Bitcoin) String() string {
	return fmt.Sprintf("%d BTC", b)
}

func TestWallet(t *testing.T) {
	wallet := Wallet{}
	wallet.Deposit(Bitcoin(10))
	got := wallet.Balance()
	want := Bitcoin(20)
	if got != want {
		t.Errorf("got %s want %s", got, want) // 原来Bitcoin类型用%d输出，现在转换为上方的String()输出
	}
}
```

error
test.go
```go
package pointer

import (
	"fmt"
	"testing"
)

type Stringer interface {
	String() string
}

func (b Bitcoin) String() string {
	return fmt.Sprintf("%d BTC", b)
}

func TestWallet(t *testing.T) {
	t.Run("Deposit", func(t *testing.T) {
		wallet := Wallet{}
		wallet.Deposit(Bitcoin(10))
		assertBalance(t, wallet, Bitcoin(10))
	})
	t.Run("Withdraw with funds", func(t *testing.T) {
		wallet := Wallet{Bitcoin(20)}
		wallet.Withdraw(Bitcoin(10))
		assertBalance(t, wallet, Bitcoin(10))
	})
	t.Run("Withdraw insufficient funds", func(t *testing.T) {
		wallet := Wallet{Bitcoin(20)}
		err := wallet.Withdraw(Bitcoin(100))
		assertBalance(t, wallet, Bitcoin(20))
		assertError(t, err, InsufficientFundsError)  // 希望有error
	})
}
func assertBalance(t *testing.T, wallet Wallet, want Bitcoin) {
	got := wallet.Balance()
	if got != want {
		t.Errorf("got '%s' want '%s'", got, want)
	}
}
func assertError(t *testing.T, got error, want error) {
	if got == nil {
		t.Fatal("didn't get an error but wanted one")
	}
	if got != want {
		t.Errorf("got '%s', want '%s'", got, want)
	}
}
```
go
```go
var InsufficientFundsError = errors.New("cannot withdraw, insufficient funds")

func (w *Wallet) Withdraw(amount Bitcoin) error {
	if amount > w.balance {
		return InsufficientFundsError // 返回一个error
	}
	w.balance -= amount
	return nil
}
```

linters：代码检测
- [kisielk/errcheck: errcheck checks that you checked errors. (github.com)](https://github.com/kisielk/errcheck)
```cmd
errcheck .
```

## Maps

map: 不使用指针传递你就可以修改它们。这是因为 map 是引用类型。这意味着它拥有对底层数据结构的引用，就像指针一样
初始化
```go
var m map[string]string  // nil指针异常，m = nil，不分配内存，此时用m就会内存泄漏

dictionary = map[string]string{}  // 初始化内存
// OR
dictionary = make(map[string]string)
```
- 还有增删改查

自定义error
```go
type DictionaryErr string

func (e DictionaryErr) Error() string {
	return string(e)
}

const (
	ErrNotFound         = DictionaryErr("could not find the word you were looking for")
	ErrWordExists       = DictionaryErr("cannot add word because it already exists")
	ErrWordDoesNotExist = DictionaryErr("cannot update word because it does not exist")
)

func (d Dictionary) Update(word, definition string) error {
	_, err := d.Search(word)
	switch err {
	case ErrNotFound:
		return ErrWordDoesNotExist
	case nil:
		d[word] = definition
	default:
		return err
	}
	return nil
}
```

## 依赖注入

dependency injection

io.Writer

## Mocking

和依赖注入结合，不懂！！！

# 网课

[【尚硅谷】Golang入门到实战教程丨一套精通GO语言_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1ME411Y71o/?spm_id_from=333.337.search-card.all.click&vd_source=773a63398bea4e166f99c44cae6bee92)

[最新Go语言急速入门视频教程（七米出品）_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1ZJ411W7jG/?spm_id_from=333.999.0.0&vd_source=773a63398bea4e166f99c44cae6bee92)
## 概念

`go build`：先编译后运行，这样生成的`.exe`可以不在go环境中也能运行；编译整个项目
`go run`：直接运行；通常用于单个文件，自动删除生成可执行文件
`go get`：下载代码包

`readelf -h main`可以查看二进制文件的类型

`sdk`：包含`go build`, `go doc`等工具，以及如`fmt`等的库

安装redis
- [在 wsl2 中安装redis - googlegis - 博客园 (cnblogs.com)](https://www.cnblogs.com/googlegis/p/18070804#:~:text=%E5%9C%A8%20wsl2)

获取命令行参数：`os.Args` / flag包
- args是字符串切片



# go语言圣经

七米
[【置顶】Go语言学习之路/Go语言教程 | 李文周的博客 (liwenzhou.com)](https://www.liwenzhou.com/posts/Go/golang-menu/)

[Go语言圣经 - Go语言圣经 (gopl-zh.github.io)](https://gopl-zh.github.io/index.html)

[基础 | Go 面试宝典 (ryansu.tech)](https://goguide.ryansu.tech/guide/interview/golang/basic/1-basic.html)



# 疑惑

interface{}和泛型有什么区别呢？

test如何运行的
基准测试和普通测试的区别
`go test -cover`：测试覆盖率
go mock呢？

`reflect.DeepEqual` 不是「类型安全」的：会出现什么诡异行为？

make的作用: 返回一个指针？

依赖注入和解耦：设计模式
为什么用io.Writer

什么时候用空结构体，空结构体的内存对齐
- [Go struct 内存对齐 | Go 语言高性能编程 | 极客兔兔 (geektutu.com)](https://geektutu.com/post/hpg-struct-alignment.html)
- [Go结构体的内存布局 | 李文周的博客 (liwenzhou.com)](https://www.liwenzhou.com/posts/Go/struct-memory-layout/)