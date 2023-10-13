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

## 数据类型

### 浮点型

在Go中不同数据类型进行运算，必须要强制转换数据类型，才可以正常开始运算

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





