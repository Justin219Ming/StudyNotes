## 序章



### Go应用场景

- 网络编程，这一块目前应用最广，包括Web应用、API应用、下载应用、内存数据库等
- 云平台开发，目前国外很多云平台在采用Go开
- 服务器编程, 以前你如果使用Java或者C++做的那些事情，都可以用Go来做
- 分布式系统，数据库代理器等
- 它可以做从底层到前端的任何工作

### 与C代码对比
- C语言中通过文件来管理代码
想使用某一个函数时,只需要include导入对应的.h文件即可
- Go语言中通过包来管理代码
Go语言没有.h文件的概念, 在Go中想使用某一个函数时, 只需要import导入对应的包即可

- C语言中函数、变量公私有管理
通过extern和static实现是否公开函数和变量
- Go语言中函数、变量公私有管理
通过函数名称首字母大小写实现是否公开函数
通过变量名称首字母大小写实现是否公开变量
- Go语言原生支持Unicode，它可以处理全世界任何语言的文本。

### GO SDK
什么是SDK
- 软件开发工具包（外语首字母缩写：SDK、外语全称：**SoftwareDevelopmentKit）**一般都是一些软件工程师为特定的软件包、软件框架、硬件平台、操作系统等建立应用软件时的开发工具的集合
- 如果不安装SDK, 你可以编写Go语言代码, 但是你不能编译执行编写好的Go语言代码

### 关于go生成文件

Go是一门编译型语言，Go语言的工具链将源代码及其依赖转换成计算机的机器指令（译注：静态编译）。Go语言提供的工具都通过一个单独的命令`go`调用，`go`命令有一系列子命令。最简单的一个子命令就是run。这个命令编译一个或多个以.go结尾的源文件，链接库文件，并运行最终生成的可执行文件。

### 初始化一个GO应用

打开终端运行命令,完成初始化

> $ go mod init sample-app

#### 运行编写好的文件

```go
package main

import "fmt"

func main(){
	fmt.Println("hello,世界")
}
```

**使用$表示命令行提示符**

> $  go run main.go 

通过这种方式来运行编写好的.go文件

#### 生成可执行文件

如果不只是一次性实验，你肯定希望能够编译这个程序，保存编译结果以备将来之用。可以用build子命令：

> $ go build helloworld.go

这个命令生成一个名为 helloworld 的可执行的二进制文件（译注：Windows系统下生成的可执行文件是 helloworld.exe ，增加了.exe后缀名），之后你可以随时运行它（译注：在Windows系统下在命令行直接输入helloworld.exe命令运行），不需任何处理（译注：因为静态编译，所以不用担心在系统库更新的时候冲突）。

在终端或者命令行调用即可

> $ ./helloworld.exe

### 关于go代码

Go语言的代码通过**包**（package）组织，包类似于其它语言里的库（libraries）或者模块（modules）。一个包由位于单个目录下的一个或多个.go源代码文件组成，目录定义包的作用。每个源文件都以一条`package`声明语句开始，这个例子里就是`package main`，表示该文件属于哪个包，紧跟着一系列导入（import）的包，之后是存储在这个文件里的程序语句。

`import`声明必须跟在文件的`package`声明之后。随后，则是组成程序的函数、变量、常量、类型的声明语句（分别由关键字`func`、`var`、`const`、`type`定义）。

Go语言不需要在语句或者声明的末尾添加分号，除非一行上有多条语句。实际上，编译器会主动把特定符号后的换行符转换为分号，因此换行符添加的位置会影响Go代码的正确解析。

举个例子，函数的左括号`{`必须和`func`函数声明在同一行上，且位于末尾，不能独占一行，而在表达式`x + y`中，可在`+`后换行，不能在`+`前换行（译注：以+结尾的话不会被插入分号分隔符，但是以x结尾的话则会被分号分隔符，从而导致编译错误）。

Go语言在代码格式上采取了很强硬的态度。`gofmt`工具把代码格式化为标准格式



### 强/弱类型语言 静态语言/动态语言

- 静态类型语言  Java Go C   明确定义好数据类型 int string 等

- 动态类型语言 php JavaScript   没有明确定义好数据类型 计算机会自行猜测数据类型

静态语言为我们提供了一个更加严格的编程环境，并且通常会生成更加健壮的代码。动态语言为编写代码带来了灵活性，同时也提高了编写代码的效率，但是如果你不注意数据的类型，可能就会导致代码更容易出错。

- 强类型语言 不会处理与类型定义明显矛盾的错误
- 弱类型语言 会猜测程序员的意图 对一些值的类型进行转换 从而让程序继续执行

Go是强类型静态语言

### Scanln 和 Printf

```go
package main

import "fmt"

func main() {
	var name, age string
	fmt.Printf("inout your name:\n")
	fmt.Scanln(&name)
	fmt.Printf("input your age:\n")
	fmt.Scanln(&age)
	fmt.Printf("your name is %v,your age are %v", name, age)

}
```

`import "fmt"`表示导入内置包fmt，实现数据的标准化输出

`fmt.Printf（）`

`fmt.Scanln()`

暂时就举例用法，需要再查询

### 小结

Go语言是基于编译、垃圾手机和并发的编程语言，专门针对多处理器系统应用程序的编程进行了优化，使用Go有媲美C的运行速度，而且安全，支持并行进程。

#### 基本注意事项

- 源码文件以.go作为文件扩展名
- 程序的主入口以main（）方法表示，并且不支持任何返回值和参数传入
- 区分大小写
- 一行代码代表一个语句，没有分号断开
- 定义的变量和导入的包没有被使用，程序会提示错误

## 第二章 基础语法

### 关键字与标识符

关键字（保留字） 

- break 中断循环
- case 选择结构
- const 定义常量
- continue 跳过本次循环
- else 判断语句
- for 循环
- goto 跳转语句
- if 条件语句
- return 设计函数返回值
- select 选择结构语句
- struct 结构体
- switch 选择结构
- type 定义自定义的数据类型
- var 定义变量
- range 切片或者集合等元素
- package 包的名字
- map 定义集合
- interface 定义接口
- import 导入包
- go 启动并发执行
- func 定义函数或者方法
- fallthrough 在switch..case中，加入fallthrough，当case匹配成功，继续强制匹配其他case语句
- default 设置默认值
- chan 定义通道

标识符（你设置的变量名字）

- 第一个字符不能是数字
- 特殊字符不能成为标识符

### 空白表示符

空白标识符由（_）表示，作为匿名占位符

用于3种情况

- 导入包，但不调用里面的任何变量或函数，只执行包的初始化函数init()
- 函数有多个返回值，但程序只用一个返回值，其他用_代替
- 类型断言，判断某个类型是否实现了接口，否则编译出错

```go
package main

// 场合一
// 只使用net/http/pprof的初始化函数init()
import (
	"fmt"
	_ "net/http/pprof"
)

func myfunc() (int, string) {
	// 自定义函数，设置两个返回值
	a := 10
	b := "golang"
	return a, b
}

type Foo interface {
	// 定义接口
	Say()
}
type Dog struct {
	// 定义结构体
	name string
}

func (d Dog) Say() {
	// 结构体实现接口Foo的使用
	fmt.Println(d.name + " say hi")
}

func main() {
	// 场合二
	// 调用函数myfunc()并只获取第一个返回值
	a, _ := myfunc()
	fmt.Printf("只获取函数myfunc的第一个返回值%d：\n", a)

	// 场合三
	// 判断结构体Dog是否实现接口Foo的使用
	// 等同于判定有没有定义func (d Dog) Say(){}
	// 用作类型断言，如果Dog没有实现Foo，则会报编译错误
	var _ Foo = Dog{"black dog"}
}

```

### 预声明标识符

- int
- bool等类型
- new
- len
- make等内置函数方法

### 导出标识符

程序导入某个包，并允许调用包中的已经定义好的变量和函数、结构体等。**导出标识符首字母必须是大写格式，否则无法调用。**

```go
package aa

var A_string = "Hello go!"

var numbers int = 10

type A_struct struct {
	Name string
}

```



```go
package main

import (
	// 导入自定义包aa
	"fmt"

	"./aa"
)

func main() {
	// 调用自定义包的A_string
	fmt.Printf("调用自定义包的A_string：%s", aa.A_string)
}

```

在上述的例子中，需要用别的包的变量、函数等，需要是首字母大写。

### 唯一标识符

每一个标识符是唯一的。

### 变量与常量

在Go中，变量用`var`关键字定义，有一下定义方式：

```go
package main

import (
	"fmt"
)

func main()  {
	// 定义变量，不设初始值
	var a int
	a = 10
	// 定义变量并设置初始值
	var b int = 10
	// 批量定义变量，可根据情况是否设置初始值
	var (
		//c int
		//d string
		_ string
		e int = 10
	)
	// 多个变量同一数据类型
	var f, g int
	// 批量赋值
	f, g = 10, 10
	f = f+g
	// 定义变量并赋值，通过数值设置变量的数据类型
	h := 10
	fmt.Printf("定义变量，不设初始值：%d\n", a)
	fmt.Printf("定义变量并设置初始值：%d\n", b)
	fmt.Printf("批量定义变量，可根据情况是否设置初始值：%d\n", e)
	fmt.Printf("多个变量同一数据类型：%d\n", f)
	fmt.Printf("定义变量并赋值，通过数值设置变量的数据类型：%d\n", h)
}
```

### 常量与iota

用`const`来定义常量

```go
package main

import (
	"fmt"
)

func main()  {
	// 单个常量定义方式一
	const a int = 10
	// 单个常量定义方式二
	const b = 20
	// 多个常量定义方式一
	const (
		c int = 10
		d = "golang"
	)
	// 多个常量定义方式二
	const e, f = true, 20
	fmt.Printf("单个常量定义方式一：%d\n", a)
	fmt.Printf("单个常量定义方式二：%d\n", b)
	fmt.Printf("多个常量定义方式一：%d\n", c)
	fmt.Printf("多个常量定义方式二：%d\n", f)
}


```

关键字iota是一个特殊的常量，在const出现时将被重置为0，如果const定义多个常量，iota会进行计数累加，如下：（不太明白有什么用）

**典型案例是实现数据存储转换**

```go
package main

import "fmt"

func main(){

	const (
		_  = iota                      // 忽略iota第一个值
		KB float64 = 1 << (10 * iota)  // 1 << (10*1)
		MB                             // 1 << (10*2)
		GB                             // 1 << (10*3)
		TB                             // 1 << (10*4)
	)
	fmt.Printf("B转KB的进制为：%.0f\n", KB)
	fmt.Printf("B转MB的进制为：%.0f\n", MB)
	fmt.Printf("B转GB的进制为：%.0f\n", GB)
	fmt.Printf("B转TB的进制为：%.0f\n", TB)
}
```

### 算术运算符

```go
package main

import "fmt"

func main(){
	var x, y = 8, 5
	fmt.Printf("加法运算符：%d\n", x+y)
	fmt.Printf("减法运算符：%d\n", x-y)
	fmt.Printf("乘法运算符：%d\n", x*y)
	fmt.Printf("除法运算符：%d\n", x/y)
	fmt.Printf("求余运算符：%d\n", x%y)
	x++
	fmt.Printf("++运算符：%d\n", x)
	y--
	fmt.Printf("--整运算符：%d\n", y)
}

```

### 比较运算符

比较的两个变量类型必须相同，结果以true和false返回

```go
package main

import "fmt"

func main(){
	var x, y = 8, 5
	fmt.Printf("x大于y：%v\n", x > y)
	fmt.Printf("x大于或等于y：%v\n", x >= y)
	fmt.Printf("x小于y：%v\n", x < y)
	fmt.Printf("x小于或等于y：%v\n", x <= y)
	fmt.Printf("x等于y：%v\n", x == y)
	fmt.Printf("x不等于y：%v\n", x != y)
}

```

### 赋值运算符

```go
package main

import "fmt"

func main() {
	var a, c int = 21, 0
	c = a
	fmt.Printf("=运算符实例，c值为 = %d\n", c)
	c, a = 1, 20
	c +=  a
	fmt.Printf("+=运算符实例，c值为 = %d\n", c)
	c, a = 1, 20
	c -=  a
	fmt.Printf("-=运算符实例，c值为 = %d\n", c)
	c, a = 1, 20
	c *=  a
	fmt.Printf("*=运算符实例，c值为 = %d\n", c)
	c, a = 1, 20
	c /=  a
	fmt.Printf("/=运算符实例，c值为 = %d\n", c)
	c, a = 1, 20
	c %=  a
	fmt.Printf("求余运算符实例，c值为 = %d\n", c)
	c = 200
	c <<=  2
	fmt.Printf("<<=运算符实例，c值为 = %d\n", c)
	c = 200
	c >>=  2
	fmt.Printf(">>=运算符实例，c值为 = %d\n", c)
	c = 200
	c &=  2
	fmt.Printf("&=运算符实例，c值为 = %d\n", c)
	c = 200
	c ^=  2
	fmt.Printf("^=运算符实例，c值为 = %d\n", c)
	c = 200
	c |=  2
	fmt.Printf("|=运算符实例，c值为 = %d\n", c)
}
```

### 逻辑运算符

与或非，只能对两个布尔类型进行判定 	&& 	||	 ！

### 位运算符

将数值自动转换为二进制进行计算。

```go
package main

import "fmt"

func main() {

	var a = 60      // 60 = 0011 1100
	var b = 13      // 13 = 0000 1101
	var c = 0

	c = a & b       // 12 = 0000 1100
	fmt.Printf("c的十进制值为 %d\n", c )
	fmt.Printf("c的二进制值为 %b\n", c )

	c = a | b       // 61 = 0011 1101
	fmt.Printf("c的十进制值为 %d\n", c )
	fmt.Printf("c的二进制值为 %b\n", c )

	c = a ^ b       // 49 = 0011 0001
	fmt.Printf("c的十进制值为 %d\n", c )
	fmt.Printf("c的二进制值为 %b\n", c )

	c = a << 2     // 240 = 1111 0000
	fmt.Printf("c的十进制值为 %d\n", c )
	fmt.Printf("c的二进制值为 %b\n", c )

	c = a >> 2     // 15 = 0000 1111
	fmt.Printf("c的十进制值为 %d\n", c )
	fmt.Printf("c的二进制值为 %b\n", c )
}
```

### 指针和解引用

```go
package main

import "fmt"

func main() {
	var a = 4
	var ptr *int
	fmt.Printf("a变量的内存地址：%v\n", &a)
	// 将变量a的内存地址赋给指针ptr
	ptr = &a
	fmt.Printf("指针ptr的内存地址为：%v\n", ptr)
	fmt.Printf("指针ptr的值为：%v\n", *ptr)
}
```

### 注释

在go中采用和C 一样的注释方式 

>  // 或者   /*    */

## 第三章 数据类型

### 浮点型

在Go中不同数据类型进行运算，必须要强制转换数据类型，才可以正常开始运算

其中`fmt.Sprintf`是四舍五入的处理结果

```go
package main

import (
	"fmt"
)

func main() {
	//fmt.Println(math.MaxFloat32)
	//fmt.Println(math.MaxFloat64)

	var f32 float32 = 1.1
	var f64 float64 = 2.2
	// 将float32的浮点数转为float64，再执行相加运算
	r := float64(f32) + f64
	fmt.Printf("运行结果为：%v\n", r)
	// 将整型转为float32的浮点数，再执行相加运算
	var i int = 10
	rd := float32(i) + f32
	fmt.Printf("运行结果为：%v\n", rd)

	k := fmt.Sprintf("%.1f", r)
	fmt.Printf("进度丢失处理结果为：%v", k)
}
```

### 复数的计算

- complex128(64位实数和虚数)
- complex64(32位)
- compelx(x,y)是预声明标识符进行赋值
- x，y分别是实部和虚部

语法格式如下

`var comp complex128 = complex(x,y)`

类型转换用`complex128() complex64()`实现

```go
package main

import "fmt"

func main() {
	var comp1 complex128 = complex(1, 3)
	var comp2 complex128 = complex(2, 4)
	var comp3 complex64 = complex(3,6)
	adds := comp1 + comp2
	reduce := comp1 - comp2
	mult := comp1 * comp2
	// 不同的复数类型不能直接运算
	// 将complex64转为complex128类型，再执行运算
	div := complex128(comp3) / comp1
	fmt.Printf("复数相加：%v\n", adds)
	fmt.Printf("复数相减：%v\n", reduce)
	fmt.Printf("复数相乘：%v\n", mult)
	fmt.Printf("复数相除：%v\n", div)
}
```

### 布尔型

```go
package main

import "fmt"

func main() {
	var n int = 10
	// 关系运算符
	r := n == 20
	fmt.Printf("10是否等于20：%v\n", r)
	// if语句
	if n > 0 {
		fmt.Printf("if的判断条件为true\n")
	}
	// for语句
	for n > 0 {
		fmt.Printf("for的判断条件为true\n")
		break
	}
}
```

### 字符类型

- uint8 ：代表ASCII码的一个字符 等于byte类型大小
- rune： 代表UTF-8字符 等于int32类型大小

字符的赋值必须使用单引号，且只有一个字符

```go
package main

import "fmt"

func main() {
	var a byte
	var b rune
	a = 'u'
	b = 'a'
	c := 'c'
	// 格式化%T是输出变量的数据类型
	fmt.Printf("变量a的数据类型为：%T\n", a)
	fmt.Printf("变量b的数据类型为：%T\n", b)
	fmt.Printf("变量c的数据类型为：%T", c)
}
```

### 字符串操作

字符串的格式化操作

```go
package main

import "fmt"

func main() {
	// 输出字符a的ASCII
	fmt.Printf("格式化符号v：%v\n", 'a')
	// 输出整型的数据类型
	fmt.Printf("格式化符号T：%T\n", 123)
	// 输出带双引号的字符串
	fmt.Printf("格式化符号q：%q\n", "Hello go")
	// 输出的字符串
	fmt.Printf("格式化符号s：%s\n", "Hello go")
	// 输出保留小数点两位的浮点数，.2是小数点后保留位数
	fmt.Printf("格式化符号f：%.2f\n", 123.321)
	// 输出十进制的整型
	fmt.Printf("格式化符号d：%d\n", 3121)
	//// 输出十进制的整型
	//fmt.Printf("格式化符号d：%d\n", 12.12)
}
```

### 字符串拼接

官方建议使用Builder

```go
package main

import (
	"bytes"
	"fmt"
	"strings"
)

func main() {
	n := "hello world"
	m := "I am Tom"
	// 使用"+"拼接字符串
	j := n + "," + m
	fmt.Println(j)
	// 使用fmt.Sprintf()拼接字符串
	k := fmt.Sprintf("%s,%s", n, m)
	fmt.Println(k)
	// 使用strings.Join()拼接字符串
	g := strings.Join([]string{n, m}, ",")
	fmt.Println(g)
	// 使用builder.WriteString()连接字符串
	var builder strings.Builder
	builder.WriteString(n)
	builder.WriteString(",")
	builder.WriteString(m)
	fmt.Println(builder.String())
	// 使用buffer.WriteString()连接字符串
	var buffer bytes.Buffer
	buffer.WriteString(n)
	buffer.WriteString(",")
	buffer.WriteString(m)
	fmt.Println(buffer.String())
}

```

### 获取字符串长度

正常情况下，使用len()会因为UTF-8是3位字符而导致中文字符一个等于3个

所以如果有多字节的字符 可以使用RunCountInString或者[]rune()实现

```go
package main

import (
	"fmt"
	"unicode/utf8"
)

func main() {
	n := "Hello,golang"
	m := "你好,Go语言"
	//fmt.Println("字符串n的长度：", len(n))
	//fmt.Println("字符串m的长度：", len(m))
	// 使用utf8.RuneCountInString()获取字符串长度
	fmt.Println("utf8获取n的长度：", utf8.RuneCountInString(n))
	fmt.Println("utf8获取m的长度：", utf8.RuneCountInString(m))
	// 使用[]rune()获取字符串长度
	fmt.Println("[]rune()获取n的长度：", len([]rune(n)))
	fmt.Println("[]rune()获取m的长度：", len([]rune(m)))
}

```

### 遍历字符串

遍历中文字符串的时候，需要注意使用 range 或者 rune

```go
package main

import "fmt"

func main() {
	

	//n := "Hi,Go语言"
	//m := []rune(n)
	//for i:=0; i<len(m); i++{
	//	// %c输出每个字符
	//	// %d输出字符对应的十进制数
	//	fmt.Printf("%c——%d\n", m[i], m[i])
	//}


	n := "Hi,Go语言"
	for _, s := range n{
		// %c输出每个字符
		// %d输出字符对应的十进制数
		fmt.Printf("%c——%d\n", s, s)
	}

}

```

### 字符串的位置与截取

- index()第一个参数是被查找的主字符串，第二个参数是要查找的截断字符串
- lastindex是第一次出现的字符的最后位置
- 截取方式 [起始位置：终止位置]

```go
package main

import (
	"fmt"
	"strings"
)

func main() {
	n := "hello-world-world-你好呀"
	// 获取字符串的子串world的最开始位置
	m := strings.Index(n, "world")
	fmt.Println("获取子串world的最开始位置：", m)
	// 获取字符串的子串world在最末端的位置
	l := strings.LastIndex(n, "world")
	fmt.Println("获取world在最末端的位置：", l)
	// 截取m往后的字符串
	k := n[m:]
	fmt.Println("截取m往后的字符串：", k)
	// 截取m位置往后的3位字符串
	p := n[m:m+3]
	fmt.Println("截取m位置往后的3位字符串：", p)
}

```

### 字符串分割

> string.Split(s,sep)

- s代表被分割的字符串
- sep代表用什么分割

```go
package main

import (
	"fmt"
	"strings"
)

func main() {
	n := "hello@-world@-I@-am@-Tom"
	// 对字符串的空格进行分割
	m := strings.Split(n, "@-")
	fmt.Printf("分割后的数据类型：%T\n", m)
	for _, i:= range m{
		// 输出分割后的每个字符串
		fmt.Println(i)
	}
}

```

### 字符串替换

> string.Replace(s,old,new,n)

- s是被代替的字符串
- old是要替代前的字符串
- new是替代后的字符串
- n是替换次数 -1是代表全部替换

```go
package main

import (
	"fmt"
	"strings"
)

func main() {
	s := "hello world I am Tom"
	// 参数n代表替换的次数，从左边开始计算，-1代表替换全部
	m := strings.Replace(s, " ", "-", -1)
	fmt.Println(m)
	// 参数n等于1代表只替换一次
	k := strings.Replace(s, " ", "-", 1)
	fmt.Println(k)
}

```

### 转换数据类型

整型与浮点型

```go
package main
import "fmt"
func main(){
    n := 123
    f := 234
    m := int(f) + n
    k := float64(n) + f
    fmt.Printf("转整型再运算: %T:%v\n",m,m);
    fmt.Printf("转浮点型再运算： %T:%v\n",k,k);
}
```

### 整型与字符串

> strconv,Itoa() 的参数传递整型数据，返回值是转换后的字符串
>
> strconv.Atoi()的参数传递字符串数据，返回值是整型
>
> i, err := strconv.ParseInt (s, base, bitSize)
>
> - s是需要被转换的字符串
> - base是字符串的进制数
> - bitSize是整型类型
> - i是转换后的结果
> - err 返回转换失败的异常信息 

```GO
package main

import (
	"fmt"
	"strconv"
)

func main() {
	// 整型转换字符串
	s := strconv.Itoa(100)
	fmt.Printf("整型转换字符串：%T：%v\n", s, s)
	// 字符串转换整型
	i, _ := strconv.Atoi("110")
	fmt.Printf("字符串转换整型：%T：%v", i, i)

	// 字符串转换整型int8
	k, _ :=strconv.ParseInt("120", 10, 8)
	fmt.Printf("字符串转换整型int8：%T：%v\n", k, k)
	// 字符串转换整型int16
	l, _ :=strconv.ParseInt("120", 10, 16)
	fmt.Printf("字符串转换整型int16：%T：%v\n", l, l)
	// 字符串转换整型int32
	m, _ :=strconv.ParseInt("120", 10, 32)
	fmt.Printf("字符串转换整型int32：%T：%v\n", m, m)
	// 字符串转换整型int64
	n, _ :=strconv.ParseInt("120", 10, 64)
	fmt.Printf("字符串转换整型int64：%T：%v", n, n)
}

```

```go
package main

import (
	"fmt"
	"strconv"
)

func main() {
	// 浮点型转换字符串
	f := 100.12345678901234567890123456789
	fmt.Println(strconv.FormatFloat(f, 'b', 5, 32))
	fmt.Println(strconv.FormatFloat(f, 'b', 5, 32))
	fmt.Println(strconv.FormatFloat(f, 'e', 5, 32))
	fmt.Println(strconv.FormatFloat(f, 'E', 5, 32))
	fmt.Println(strconv.FormatFloat(f, 'f', 5, 32))
	fmt.Println(strconv.FormatFloat(f, 'g', 5, 32))
	fmt.Println(strconv.FormatFloat(f, 'G', 5, 32))

	// 字符串转换浮点型，类型为float32
	s := "0.1234"
	k, _ := strconv.ParseFloat(s, 32)
	fmt.Printf("字符串转换浮点型，类型为float32：%T——%v\n", k, k)
	// 字符串转换浮点型，类型为float64
	l, _ := strconv.ParseFloat(s, 64)
	fmt.Printf("字符串转换浮点型，类型为float64：%T——%v\n", l, l)
}

```

以上是浮点数和字符串的转换

主要涉及两个函数

> strconv.FormatFloat()
>
> strconv.ParseFloat()

 具体细节需要用的时候自行查询 大部分转换方法都在内置包strconv中有定义

## 第四章 流程控制

### if

if可以定义在if作用范围内的变量

```go
// 随机数
	rand.Seed(time.Now().Unix())
	// num := rand.Intn(100)从100中随机生成整数
	if num := rand.Intn(100); num < 20 {
		fmt.Printf("随机数为%v\n", num)
	}else if num > 20{
		fmt.Printf("随机数为%v\n", num)
	}

```

### switch多条件分支

```go
package main

import "fmt"

func main() {
	//// 使用方法1
	//finger := 3
	//switch finger {
	//	// 当变量finger=1的时候
	//	case 1:
	//		fmt.Println("大拇指")
	//	// 当变量finger=2的时候
	//	case 2:
	//		fmt.Println("食指")
	//	// 当变量finger=3的时候
	//	case 3:
	//		fmt.Println("中指")
	//	case 4:
	//		fmt.Println("无名指")
	//	case 5:
	//		fmt.Println("小拇指")
	//	default:
	//		fmt.Println("无效的输入！")
	//}
	//
	//// 使用方法2
	//rand.Seed(time.Now().Unix())
	//num := rand.Intn(100)
	//switch {
	//	// 判断num是否大于20
	//	case num < 20:
	//		fmt.Printf("变量num的值为：%v，小于20\n", num)
	//	default:
	//		fmt.Printf("变量num的值为：%v，大于20\n", num)
	//}
	//
	//// 使用方法3
	//rand.Seed(time.Now().Unix())
	//switch n := rand.Intn(9); n {
	//	// 变量n在（1, 3, 5, 7, 9）区间内
	//	case 1, 3, 5, 7, 9:
	//		fmt.Printf("奇数，值为：%v，小于20\n", n)
	//	// 变量n在（2, 4, 6, 8）区间内
	//	case 2, 4, 6, 8:
	//		fmt.Printf("偶数，值为：%v，小于20\n", n)
	//	default:
	//		fmt.Printf("啥也不是")
	//}
	finger := 1
	switch finger {
	// 当变量finger=1的时候
	case 1:
		fmt.Println("大拇指")
		fallthrough
	// 当变量finger=2的时候
	case 2:
		fmt.Println("食指")
		fallthrough
	// 当变量finger=3的时候
	case 3:
		fmt.Println("中指")
	case 4:
		fmt.Println("无名指")
	case 5:
		fmt.Println("小拇指")
	default:
		fmt.Println("无效的输入！")
	}
}
```

如果你想要在执行了一条case之后继续运行下一个case 使用`fallthrough`

`default`的作用是每一个case都不满足的时候执行

### for

GO只支持`for`语法不支持while 和 do while语法

> for 变量初始值; 判断条件; 变量控制｛
>
> ​	执行语句
>
> ｝

```go
package main

import "fmt"

func main() {
	// 组合方式一
	for i := 1; i < 10; i++ {
		fmt.Printf("本次循环：%v\n", i)
	}
	// 组合方式二
	for i := 1; i < 10; {
		fmt.Printf("本次循环：%v\n", i)
		i++
	}
	// 组合方式三
	var i int = 1
	for i < 5 {
		fmt.Printf("本次循环：%v\n", i)
		i++
	}
	// 组合方式四
	for {
		fmt.Printf("本次循环：%v\n", i)
		break
	}
}

```

有多种组合方式 

### for-range获取键值

 

```go
package main

import "fmt"

func main() {
	myStr := []string{"Jack", "Mark"}
	// 使用for循环输出切片myStr的元素
	for i := 0; i < len(myStr); i++ {
		fmt.Printf("本次循环的次数为：%v\n", i)
		fmt.Printf("切片myStr的元素为：%v\n", myStr[i])
	}
	// 使用for-range输出切片myStr的元素
	for i, v := range myStr {
		fmt.Printf("本次循环的次数为：%v\n", i)
		fmt.Printf("切片myStr的元素为：%v\n", v)
	}
}

```

### break终止代码

- 可以越级终止代码

```go
package main

import "fmt"

func main() {
	// 在最外层循环中设置标签，标签名自行修改
	for1:
	for i := 0; i < 3; i++ {
		for k := 1; k < 10; k++ {
			fmt.Printf("%v:%v\n", i, k)
			// break后加上标签名，直接终止最外层循环
			break for1
		}
	}
}


```

### continue

```go
package main

import "fmt"

func main() {
	for i := 1; i < 5; i++ {
		if i == 2 {
			continue
		}
		fmt.Printf("本次循环次数为：%v\n", i)
	}
}

```

### goto 无条件跳转

- 注意需要表明程序的名字

```go
package main

import "fmt"

func main() {
	for i := 1; i < 5; i++ {
		if i == 2 {
			goto gofunc
		}
		fmt.Printf("本次循环次数为：%v\n", i)
	}
	gofunc:
		fmt.Printf("使用goto跳转\n")
	fmt.Printf("程序结束了")
}

```



## 第五章 指针

  GO中的指针有两种类型

- 类型指针
- 切片指针

### 定义和空指针

> var name *type

- name 自行命名
- type是数据类型

```go
package main

import "fmt"

func main() {
	// 定义int类型的指针变
	var pint *int
	var a int = 10
	pint = &a
	fmt.Printf("指针值为：%v，空间地址：%v\n", pint, &pint)
	// 定义float64类型的指针变量
	var pfloat *float64
	fmt.Printf("指针值为：%v，空间地址：%v\n", pfloat, &pfloat)
	// 定义string类型的指针变量
	var pstr *string
	fmt.Printf("指针值为：%v，空间地址：%v\n", pstr, &pstr)
	// 定义bool类型的指针变量
	var pbool *bool
	fmt.Printf("指针值为：%v，空间地址：%v\n", pbool, &pbool)
	// 定义byte类型的指针变量
	var pbyte *byte
	fmt.Printf("指针值为：%v，空间地址：%v\n", pbyte, &pbyte)

	ptr := new(int)
	fmt.Printf("ptr指向的变量值为：%v，空间地址：%v\n", *ptr, &ptr)
}

```

### 指针赋值与取值

```go
package main

import "fmt"

func main() {
	//var a int = 200
	//fmt.Printf("变量a的空间地址：%v\n", &a)
	//// 定义int类型的指针变量
	//var pint *int
	//fmt.Printf("指针值为：%v，空间地址：%v\n", pint, &pint)
	//pint = &a
	//fmt.Printf("指针值为：%v，空间地址：%v\n", pint, &pint)
	//fmt.Printf("指针值的值为：%v，空间地址：%v\n", *pint, &pint)

	var b int = 100
	var pint *int
	fmt.Printf("指针值的值为：%v，空间地址：%v\n", pint, &pint)
	// 将变量b的内存地址赋值给指针pint
	pint = &b
	fmt.Printf("指针值的值为：%v，空间地址：%v\n", *pint, &pint)
	// 通过取值操作符“*”修改变量b的值
	*pint = 666
	fmt.Printf("指针值的值为：%v，空间地址：%v\n", *pint, &pint)
}
```

### 切片指针

定义方式

>  var name []*type
>
>  name := []*type{}

 ```go
 package main
 
 import "fmt"
 
 func main() {
 	// 定义一个空的字符串类型的切片指针
 	var pslice []*string
 	fmt.Printf("切片指针的元素：%v，内存地址：%v\n", pslice, &pslice)
 	// 定义变量a、b、c并赋值
 	var a, b string
 	a, b = "a", "b"
 	fmt.Printf("变量a、b的内存地址：%v、%v\n", &a, &b)
 	// 使用内置函数append()将变量a、b、c的内存地址添加到切片指针
 	pslice = append(pslice, &a)
 	pslice = append(pslice, &b)
 	fmt.Printf("切片指针的元素：%v\n", pslice)
 	// 输出切片指针的元素所对应的数值
 	// 使用取值操作符“*”从内存地址取值
 	for _, k := range pslice {
 		fmt.Printf("切片指针的元素所对应值：%v\n", *k)
 	}
 	// 从切片指针修改变量a的值，输出变量a
 	*pslice[0] = "hello"
 	fmt.Printf("修改后的变量值为：%v\n", a)
 	// 修改变量b的值，输出切片指针的变量b的值
 	b = "Golang"
 	fmt.Printf("修改后的变量值为：%v\n", *pslice[1])
 
 }
 
 ```

- 切片指针定义之后 如果没有设置初始值，默认为空，并且不会有分配内存地址，因此无法通过&来取得地址

### 指针的指针

> var name **type

获取值

> v := **name  

### 小结 综合应用

```go
package main

import (
	"fmt"
	"math/rand"
	"time"
)

func main() {
	// 购买号码
	// 定义变量myNum，存放用户当前输入数据
	var myNum int
	// 定义变量myNums，存放用户所有输入数据
	var myNums []int
	// 循环7次，给用户输入7个数据
	for i := 0; i < 7; i++ {
		// 输出操作提示
		fmt.Printf("请输入第%v位号码：\n", i+1)
		// 存储用户输入数据
		fmt.Scanln(&myNum)
		// 将当前数据存放在切片myNums
		myNums = append(myNums, myNum)
	}
	fmt.Printf("你选到号码分别为：%v\n", myNums)

	// 公布开奖号码
	// 定义变量s，切片类型，切片元素为指针类型
	var result []*int
	// 定义变量status，数据类型为布尔型
	var status bool
	// 设置随机数的随机种子
	rand.Seed(time.Now().UnixNano())
	// 设置死循环
	for {
		// 定义变量num，数据类型为整型
		var num int
		// 设置变量status的值
		status = false
		// 创建随机数
		num = rand.Intn(36) + 1
		// 遍历切片result的每个元素
		// 如果随机数num已存在切片result，将变量status等于true
		for _, k := range result {
			if *k == num {
				status = true
			}
		}
		// 变量status等于false
		// 说明随机数num不在切片result里面，将随机数num加入切片result
		if status == false {
			result = append(result, &num)
		}
		// 切片长度等于7，终止死循环
		if len(result) == 7 {
			break
		}
	}
	// 遍历输出切片所有元素
	for i, k := range result {
		fmt.Printf("第%v位号码为：%v\n", i+1, *k)
	}

	// 兑奖
	// 遍历切片result和myNums，将两个切片元素一一对比
	for _, k := range result {
		for _, j := range myNums {
			if *k == j {
				fmt.Printf("号码%v选中了\n", j)
			}
		}
	}
}

```

## 第六章 内置容器



### 数组

> var name [number]type

- number 可以是表达式 但是最终结果必须是整数

- 默认数组里面装填为0
- String类型里面为空字符串

```go
package main

import "fmt"

func main() {
	// 定义长度为2的数组并设置每个元素值
	var s = [2]int{100, 200}
	// 输出数组元素
	for i := 0; i < len(s); i++ {
		fmt.Printf("数组s第%v个元素是：%v\n", i+1, s[i])
	}

	// 定义数组并设置每个元素值，数值长度根据元素个数自动设置
	var ss = [...]int{300, 400}
	// 输出数组元素
	for i := 0; i < len(ss); i++ {
		fmt.Printf("数组ss第%v个元素是：%v\n", i+1, ss[i])
	}

	// 定义数组并设置第一个和第四个元素值
	var sss = [...]int{0: 300, 3: 500}
	// 输出数组元素
	for i := 0; i < len(sss); i++ {
		fmt.Printf("数组sss第%v个元素是：%v\n", i+1, sss[i])
	}
}

```

 ```go
 package main
 
 import (
 	"fmt"
 	"math"
 )
 
 func main() {
 	var result int = 0
 	// 定义3行2列长度的二维数组
 	var s [3][2]int
 	// 为二维数组赋值
 	s = [3][2]int{{10, 20}, {30, 40}, {50, 60}}
 	for i := 0; i < len(s); i++ {
 		// 循环每一行数据
 		for k := 0; k < len(s[i]); k++ {
 			// 循环每一列数据
 			result = result + s[i][k]
 			fmt.Printf("当前元素值为：%v\n", s[i][k])
 		}
 	}
 	fmt.Printf("二维数组的总行数为：%v\n", len(s))
 	fmt.Printf("二维数组的总列数为：%v\n", len(s[0]))
 	fmt.Printf("二维数组的总值为：%v\n", result)
 
 	// 定义2*1*3长度的三维数组
 	var point [2][1][3]int
 	point = [2][1][3]int{{{3, 5, 7}}, {{5, 3, 2}}}
 	// 获取坐标点
 	pointA := point[0][0]
 	pointB := point[1][0]
 	fmt.Printf("坐标点A：%v\n", pointA)
 	fmt.Printf("坐标点B：%v\n", pointB)
 	// 计算两个坐标点距离
 	// 计算两坐标的x坐标之差的平方
 	x := (pointA[0] - pointB[0]) * (pointA[0] - pointB[0])
 	// 计算两坐标的y坐标之差的平方
 	y := (pointA[1] - pointB[1]) * (pointA[1] - pointB[1])
 	// 计算两坐标的z坐标之差的平方
 	z := (pointA[2] - pointB[2]) * (pointA[2] - pointB[2])
 	result := math.Sqrt(float64(x + y + z))
 	fmt.Printf("两坐标点距离为：%v\n", result)
 }
 
 ```

### 切片

- 可以理解为动态数组

> 定义 ： var name []type
>
> 定义并赋值 ： var name = []{value1, value2}
>
> 使用make :  var name []type = make ([type, len])

```go
package main

import "fmt"

func main() {
	var s []int
	var ss = []int{1, 2}
	var sss []int = make([]int, 3)
	fmt.Printf("只定义：%v，内存地址为：%v\n", s, &s)
	fmt.Printf("定义并赋值：%v，内存地址为：%v\n", ss, &ss)
	fmt.Printf("使用make()函数定义：%v，内存地址为：%v\n", sss, &sss)
	
	//var ss = []int{1, 2}
	//fmt.Printf("切片变量ss的元素值为：%v\n", ss)
	
	
}
```

### 新增切片元素

> ss := append(slice , elems)

- slice是代表待新增的元素切片
- elems代表新增元素的元素值 可以是切片 这样就等同于切片连接

```go
package main

import "fmt"

func main() {
	var ss = []int{1, 2}
	fmt.Printf("新增元素前的切片ss：%v\n", ss)
	// 新增元素不覆盖原有切片
	sss := append(ss, 3)
	fmt.Printf("新增元素后的切片ss：%v\n", ss)
	fmt.Printf("新切片sss：%v\n", sss)
	// 新增元素并覆盖原有切片
	ss = append(ss, 4)
	fmt.Printf("新增元素后的切片ss：%v\n", ss)
	// 添加多个元素
	ss = append(ss, 5, 6, 7, 8)
	fmt.Printf("新增元素后的切片ss：%v\n", ss)
}

```

```go
package main

import "fmt"

func main() {
	var ss = []int{1, 2, 3, 4, 5, 6, 7}
	// 截取第二个到第五个元素
	s1 := ss[1:4]
	fmt.Printf("截取第二个到第五个元素：%v\n", s1)
	// 截取第三个元素之后的所有元素
	s2 := ss[2:]
	fmt.Printf("截取第三个元素之后的所有元素：%v\n", s2)
	// 截取第三个元素之前的所有元素
	s3 := ss[:2]
	fmt.Printf("截取第三个元素之前的所有元素：%v\n", s3)
	// 如果切片ss没被覆盖，经过截取后不改变原有的切片数据
	fmt.Printf("切片变量ss的值：%v\n", ss)


	//var ss = []int{1, 2, 3, 4, 5, 6, 7}
	//fmt.Printf("切片ss的元素：%v\n", ss)
	//// 删除元素4、5、6
	//ss = append(ss[:2], ss[6:]...)
	//fmt.Printf("切片ss的元素：%v\n", ss)

}

```

 ### 复制切片

> i := copy(slice1 , slice2)

- slice1代表待复制的切片
- slice2代表被复制的切片

```go
slice1 := []int{1, 2, 3, 8}
	slice2 := []int{4, 5, 6}
	// 将slice1的元素复制到slice2
	//copy(slice2, slice1)
	// 将slice2的元素复制到slice1
	copy(slice1, slice2)
	fmt.Printf("将slice2的元素复制到slice1：%v\n", slice1)
```

- 如果元素个数不同， 以参数slice1的个数为主。

### 切片的长度与容量

当切片容量不足时，go会自动将容量翻倍

```go
package main

import "fmt"

func main() {
	// 内置函数cap()获取切片容量
	// 内置函数len()获取切片长度
	s1 := make([]int, 3, 4)
	fmt.Printf("切片变量s1的值：%v\n", s1)
	fmt.Printf("切片变量s1的长度：%v\n", len(s1))
	fmt.Printf("切片变量s1的容量：%v\n", cap(s1))
	// 第一次添加元素
	s1 = append(s1, 10)
	fmt.Printf("切片变量s1的值：%v\n", s1)
	fmt.Printf("切片变量s1的长度：%v\n", len(s1))
	fmt.Printf("切片变量s1的容量：%v\n", cap(s1))
	// 第二次添加元素
	s1 = append(s1, 10)
	fmt.Printf("切片变量s1的值：%v\n", s1)
	fmt.Printf("切片变量s1的长度：%v\n", len(s1))
	fmt.Printf("切片变量s1的容量：%v\n", cap(s1))
}

```

### 集合

又称为映射（map）是一种无序的键值对（key-value）

在其他语言中成为字典（python)

一个集合中的键值对是一样的数据类型

```go
package main

import (
	"fmt"
)

func main() {
	// 只定义
	var m1 = map[string]string{}
	m1["name"] = "Tom"
	fmt.Printf("集合m1：%v\n", m1)
	// 定义并赋值
	var m2 = map[string]string{"name": "Lily"}
	fmt.Printf("集合m2：%v\n", m2)
	// 使用make()函数定义
	m3 := make(map[string]string)
	m3["name"] = "Tim"
	fmt.Printf("集合m3：%v\n", m3)
}

```

### 删除集合元素

使用`m[key]=value`的方式来新增或者修改

如果集合中没有这个key 那就新增

如果集合中有这key 那就修改

删除使用delete的方式实现

```go
package main

import "fmt"

func main() {
	var m1 = map[string]string{}
	m1["name"] = "Tom"
	m1["age"] = "20"
	m1["addr"] = "GZ"
	fmt.Printf("集合m1的数据：%v\n", m1)
	// 删除key=addr的数据
	delete(m1, "addr")
	fmt.Printf("集合m1的数据：%v\n", m1)
}

```

### 集合和JSON的互换

```go
package main

import (
	"encoding/json"
	"fmt"
)

func main() {
	// 定义字符串，用于记录JSON数据
	var j string
	j = `{"infos": [{"name": "Tom", "age": 15}, {"name": "Lily", "age": 20}]}`
	// 定义集合，value的数据类型为接口interface类型
	var m1 = map[string]interface{}{}
	// 将JSON字符串转换为集合
	json.Unmarshal([]byte(j), &m1)
	// 遍历输出JSON
	for k, v := range m1 {
		fmt.Printf("集合m1的键为：%v\n", k)
		fmt.Printf("集合m1的值为：%v\n", v)
		// 解析JSON里面的数组
		vv := v.([]interface{})
		for i := 0; i < len(vv); i++ {
			fmt.Printf("数组vv的值为：%v\n", vv[i])
			// 解析数组里面的集合
			vvv := vv[i].(map[string]interface{})
			name := vvv["name"]
			age := vvv["age"]
			fmt.Printf("键为name的数据为：%v\n", name)
			fmt.Printf("键为age的数据为：%v\n", age)
		}
	}
}

```



```go
package main

import (
	"encoding/json"
	"fmt"
)

func main() {
	// 定义集合
	var m1 = map[string]interface{}{}
	m1["name"] = "Tom"
	m1["age"] = 10
	fmt.Printf("m1的数据为：%v\n", m1)
	var m2 = map[string]interface{}{}
	m2["name"] = "Lily"
	m2["age"] = 20
	fmt.Printf("m2的数据为：%v\n", m2)
	// 定义切片
	var s1 = []map[string]interface{}{m1, m2}
	fmt.Printf("s1的数据为：%v\n", s1)
	// 定义集合，键为字符串类型，值为接口类型
	var m3 = map[string]interface{}{}
	m3["infos"] = s1
	data, _ := json.Marshal(&m3)
	fmt.Printf("JSON数据为：%v\n", string(data))
}

```

### 列表

`container/list`包

> var name list.List
>
> name := list.New()
>
> 

**是一种链表类型 但是具体的实现方式比较难理解 **

P112





## 函数

  基本格式

> func name (参数) (返回值) {
>
> ​	代码块
>
> ​	return 返回值
>
> }

```go
package main

import (
	"fmt"
	"strconv"
)

func myfun(name string, age int) (string, bool) {
	// 参数name和age
	// (string, bool)是返回值的数据类型
	var n string
	var b bool
	if name != "" {
		// 字符串拼接
		n = name + " is existence, age is " + strconv.Itoa(age)
		b = true
	} else {
		n = "name is not existence"
		b = false
	}
	// 返回值
	return n, b
}

func main() {
	// 调用函数，并设置返回值
	s, _ := myfun("Tom", 15)
	fmt.Println(s)
	// 调用函数，虽然有返回值，但函数外不需要使用
	myfun("Tom", 15)
}

```

### 不固定参数数量

```go
package main

import (
	"fmt"
)

//func myfun(numbers ...int) {
//	for _, k := range numbers {
//		fmt.Printf("参数值为：%v\n", k)
//	}
//}
func myfun(numbers ...interface{}) {
	for _, k := range numbers {
		fmt.Printf("参数值为：%v\n", k)
	}
}


func main() {
	var s = []string{"Mary", "Tim"}
	var m = map[string]interface{}{"name": "Mary", "age": 10}
	// 调用函数
	myfun(45, "Tom", s, m)
}

```

### 匿名函数

```go
package main
import (
	"fmt"
)
func main(){

	//res := func(n1 int, n2 int) int {
	//	return n1 + n2
	//}(10, 30)
	//
	//fmt.Printf("函数执行结果为：%v\n", res)
	//


	// 将匿名函数赋给函数变量myfun
	myfun := func (n1 int, n2 int) int {
		return n1 - n2
	}

	// 变量myfun的数据类型是函数类型，可以由该变量完成函数调用
	res2 := myfun(10, 30)
	res3 := myfun(50, 30)
	fmt.Printf("匿名函数调用第一次：%v\n", res2)
	fmt.Printf("匿名函数调用第二次：%v\n", res3)
	fmt.Printf("函数变量myfun的数据类型：%T\n", myfun)
}
```

 ### 闭包函数

不理解

```go
package main

import "fmt"

// 闭包 = 函数 + 引用环境
func adder() func(int) int {
	// 定义函数adder()，返回值为匿名函数func(int) int
	var x int = 10
	// 匿名函数作为函数返回值
	return func(y int) int {
		x += y
		return x
	}
}

func main() {
	// 函数adder()是一个闭包:
	// 函数adder()内部有变量x（引用环境）和匿名函数
	// 匿名函数引用了其外部作用域中的变量x
	// 在函数adder()的生命周期内 变量x一直有效
	f := adder()
	fmt.Println(f(10))
	fmt.Println(f(20))
	f1 := adder()
	fmt.Println(f1(2000))
	fmt.Println(f1(5000))
}

```



### 函数的自身调用



```go
package main

import "fmt"

func fibonacci(n int) int {
	// 定义递归函数
	if n < 2 {
		return n
	}
	// 调用自身，传入不同参数值
	return fibonacci(n-2) + fibonacci(n-1)
}

func main() {
	var i int
	// 调用函数fibonacci()
	for i = 0; i < 10; i++ {
		fmt.Printf("%d ", fibonacci(i))
	}
}

```



## 结构体

```go
package main

import "fmt"

// 定义结构体person
type person struct {
	name string
	age  int
}

func main() {
	// 实例化方法1
	// 实例化结构体person，生成实例化对象p
	p := person{name: "Tom", age: 18}
	// 由实例化对象p访问成员
	fmt.Printf("结构体成员name的值：%v\n", p.name)
	fmt.Printf("结构体成员age的值：%v\n", p.age)

	// 实例化方法2
	// 实例化结构体
	var p1 person
	// 对结构体成员进行赋值操作
	p1.name = "Tim"
	p1.age = 22
	// 由实例化对象p1访问成员
	fmt.Printf("结构体成员name的值：%v\n", p1.name)
	fmt.Printf("结构体成员age的值：%v\n", p1.age)

	// 实例化方法3
	// 使用new()实例化结构体
	p3 := new(person)
	// 对结构体成员进行赋值操作
	p3.name = "LiLy"
	p3.age = 28
	// 由实例化对象p3访问成员
	fmt.Printf("结构体成员name的值：%v\n", p3.name)
	fmt.Printf("结构体成员age的值：%v\n", p3.age)

	// 实例化方法4
	// 取结构体的地址实例化
	p4 := &person{}
	// 对结构体成员进行赋值操作
	p4.name = "Mary"
	p4.age = 16
	// 由实例化对象p4访问成员
	fmt.Printf("结构体成员name的值：%v\n", p4.name)
	fmt.Printf("结构体成员age的值：%v\n", p4.age)
}

```

### 使用指针方式的实例化

```go
package main

import "fmt"

// 定义结构体person
type person struct {
	name string
	age  int
}

func main() {
	// 实例化方法3
	// 使用new()实例化结构体
	var p3 *person = new(person)
	// 对结构体成员进行赋值操作
	(*p3).name = "LiLy"
	(*p3).age = 28
	// 由实例化对象p3访问成员
	fmt.Printf("结构体成员name的值：%v\n", p3.name)
	fmt.Printf("结构体成员age的值：%v\n", p3.age)

	// 实例化方法4
	// 取结构体的地址实例化
	var p4 *person = &person{}
	// 对结构体成员进行赋值操作
	(*p4).name = "Mary"
	(*p4).age = 16
	// 由实例化对象p4访问成员
	fmt.Printf("结构体成员name的值：%v\n", p4.name)
	fmt.Printf("结构体成员age的值：%v\n", p4.age)
}

```

使用指针访问的时候可以直接使用实心点访问

会自动转换成为对应的操作

> (*p3).name = 'LiLy'
>
> p3.name = xxx

### 结构体标签





### 命令行参数（其他）

#### 字符串

```go
// Echo1 prints its command-line arguments.
package main

import (
    "fmt"
    "os"
)

func main() {
    var s, sep string
    for i := 1; i < len(os.Args); i++ {
        s += sep + os.Args[i]
        sep = " "
    }
    fmt.Println(s)
}
```

var声明定义了两个string类型的变量s和sep。变量会在声明时直接初始化。如果变量没有显式初始化，则被隐式地赋予其类型的*零值*（zero value），数值类型是0，字符串类型是空字符串""。这个例子里，声明把s和sep隐式地初始化成空字符串。

表示连接字符串sep和os.Args。程序中使用的语句：

```go
s += sep + os.Args[i]
```

是一条*赋值语句*，将s的旧值跟sep与os.Args[i]连接后赋值回s，等价于：

```go
s = s + sep + os.Args[i]
```

#### for循环

Go语言只有for循环这一种循环语句。for循环有多种形式，其中一种如下所示：

```go
for initialization; condition; post {
    // zero or more statements
}
```

for循环三个部分不需括号包围。大括号强制要求，左大括号必须和*post*语句在同一行。

*initialization*语句是可选的，在循环开始前执行。*initalization*如果存在，必须是一条*简单语句*（simple statement），即，短变量声明、自增语句、赋值语句或函数调用。`condition`是一个布尔表达式（boolean expression），其值在每次循环迭代开始时计算。如果为`true`则执行循环体语句。`post`语句在循环体执行结束后执行，之后再次对`condition`求值。`condition`值为`false`时，循环结束。

or循环的这三个部分每个都可以省略，如果省略`initialization`和`post`，分号也可以省略：

```go
// a traditional "while" loop
for condition {
    // ...
}
```

如果连`condition`也省略了，像下面这样：

```go
// a traditional infinite loop
for {
    // ...
}
```

这就变成一个无限循环，尽管如此，还可以用其他方式终止循环，如一条`break`或`return`语句。

`for`循环的另一种形式，在某种数据类型的区间（range）上遍历，如字符串或切片。`echo`的第二版本展示了这种形式：

```go
// Echo2 prints its command-line arguments.
package main

import (
    "fmt"
    "os"
)

func main() {
    s, sep := "", ""
    for _, arg := range os.Args[1:] {
        s += sep + arg
        sep = " "
    }
    fmt.Println(s)
}
```

每次循环迭代，`range`产生一对值；索引以及在该索引处的元素值。这个例子不需要索引，但`range`的语法要求，要处理元素，必须处理索引。一种思路是把索引赋值给一个临时变量（如`temp`）然后忽略它的值，但Go语言不允许使用无用的局部变量（local variables），因为这会导致编译错误。

Go语言中这种情况的解决方法是用`空标识符`（blank identifier），即`_`（也就是下划线）。空标识符可用于在任何语法需要变量名但程序逻辑不需要的时候（如：在循环里）丢弃不需要的循环索引，并保留元素值。大多数的Go程序员都会像上面这样使用`range`和`_`写`echo`程序，因为隐式地而非显式地索引os.Args，容易写对。

`echo`的这个版本使用一条短变量声明来声明并初始化`s`和`seps`，也可以将这两个变量分开声明，声明一个变量有好几种方式，下面这些都等价：

```go
s := ""
var s string
var s = ""
var s string = ""
```

实践中一般使用前两种形式中的某个，初始值重要的话就显式地指定变量的类型，否则使用隐式初始化。





