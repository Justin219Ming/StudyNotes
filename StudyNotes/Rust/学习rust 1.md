# 目录
- cargo
- 用一个猜数字游戏的程序来切入rust
- 常见编程概念
- 所有权
- 结构题

### 操作rust软件？
- 成功安装？  在命令行中输入
> rustc --version
- 更新rust
> rustup update
- 卸载
> rustup self uninstall

### 学会使用cargo
**cargo是rust的构建系统和包管理器。**
- 成功安装cargo？ 命令行输入
> cargo --version

#### 使用cargo创建项目
在你指定想存放代码的位置，利用命令行或者终端的方式来创建一个新项目
> cargo new hello_cargo
> cd hello_cargo

第一行命令新建了名为 hello_cargo 的目录和项目。我们将项目命名为 hello_cargo，同时 Cargo 在一个同名目录中创建项目文件。

#### 构建并运行cargo项目
在对应的hello_cargon目录下，执行下面命令来构建项目
> cargo build

然后使用下面命令来编译和运行生成的可执行文件
> cargo run

*当然也可以直接执行cargo run他会自动执行cargo build的命令*

另外下面这个命令可以快速检查代码来确保其可以编译，但不会产生可执行文件
> cargo check

**使用 Cargo 的一个优点是，不管你使用什么操作系统，其命令都是一样的。**

#### 优化编译项目
当项目最终准备好发布时，可以使用 ` cargo build  --release ` 来优化编译项目。

这会在 target/release 而不是 target/debug 下生成可执行文件。这些优化可以让 Rust 代码运行的更快，不过启用这些优化也需要消耗更长的编译时间。

如果是在测试代码的运行时间，确保运行 `cargo build --release` 并使用target/release 下的可执行文件进行测试

****

**未理解**
要在任何已存在的项目上工作时，可以使用如下命令通过 Git 检出代码，移动到该项目目录并构建：
> git clone example.org/someproject
> cd someproject
> cargo build

****

## 用一个猜数字游戏的程序来切入rust

### 导入库
rust有自动导入的**标准库内容**，如果你需要的类型不在与导入的内容中，就必须使用 `use` 语句显示的将其引入作用域

#### 标准库，无需导入
- **String**  一个标准库提供的字符串类型，他是UTF-8编码的可增长文本块

#### 非标准库，需要自行导入

```rust
use std::io;
// 功能：接收用户输入
use rand::Rng;
// 和随机数有关的一个库
use std::cmp::Ordering;
// Ordering 也是一个枚举，不过它的成员是 Less、Greater 和 Equal。这是比较两个值时可能出现的三种结果。
```    

#### hello world
```rust
fn main(){
    println!("hello world!");
}
```

#### 打印输出的宏 println!
```rust
println!("111");  //打印出111并换行

let x = 5;
let y = 10;

println!("x = {x} and y + 2 = {}", y + 2);
//这行代码会打印出 x = 5 and y + 2 = 12。
```
1. 在大括号中的变量会输出他的值。
2. 在大括号中留空会按照后面用逗号分割的表达式顺序输出。

### 变量
在rust中变量是默认不可变的，这点和c相反
变量的声明使用关键字 `let`
如果想要使得变量可以修改值的大小，在声明的时候需要加上 `mut` 

```rust
let apples = 5;  //声明一个不可变的变量
let mut bananas = 10;　／／声明一个可变的变量
let mut guess = String::new(); //创建一个变量来存储用户的输入，但是这里省略了变量的类型
```

### 静态方法(关联函数)
`String::new()` 其中 `::` 语法表明 `new` 是 `String` 类型的一个静态方法

#### 接受用户输入
```rust
io::stdin()
    .read_line(&mut guess)
```

如果程序的开头没有使用 `use std::io`; 引入 `io` 库，我们仍可以通过把函数调用写成 `std::io::stdin` 来使用函数。`stdin` 函数返回一个 `std::io::Stdin` 的实例，这代表终端标准输入句柄的类型。

代码的下一部分，`.read_line(&mut guess)`，调用 `read_line` 方法从标准输入句柄获取用户输入。我们还将 `&mut guess`作为参数。`read_line` 的工作是，无论用户在标准输入中键入什么内容，都将其追加（不会覆盖其原有内容）到一个字符串中，因此它需要字符串作为参数。

### 书写的格式
```rust
io::stdin().read_line(&mut guess).expect("Failed to read line");
```
过长的代码行难以阅读，所以最好拆开来写。通常来说，当使用 .method_name() 语法调用方法时引入换行符和空格将长的代码行拆开是明智的。

```rust
io::stdin()
    .read_line(&mut guess)
    .expect("Failed to read line");
```

### 错误处理
```rust
io::stdin()
    .read_line(&mut guess)
    .expect("Failed to read line");
```

之前提到了 `read_line` 会将用户输入附加到传递给它的字符串中，不过它也会返回一个类型为 `Result` 的值。 `Result` 是一种枚举类型，通常也写作 `enum`。枚举类型变量的值可以是多种可能状态中的一个。我们把每种可能的状态称为一种 **枚举成员**。

`Result` 的成员是` Ok` 和 `Err`，`Ok `成员表示操作成功，内部包含成功时产生的值。`Err` 成员则意味着操作失败，并且包含失败的前因后果

这些 `Result` 类型的作用是编码错误处理信息。`Result` 类型的值，像其他类型一样，拥有定义于其上的方法。`Result` 的实例拥有 `expect` 方法。如果 `io::Result` 实例的值是 `Err`，`expect` 会导致程序崩溃，并显示当做参数传递给 `expect` 的信息。如果 `Result` 实例的值是 `Ok`，`expect` 会获取 `Ok` 中的值并原样返回。在本例中，这个值是用户输入到标准输入中的字节数。

### 使用 crate 来增加更多的功能
比如我想要使用一个能生成随机数的函数，但是本身的标准库中没有，那么我需要添加 **依赖** 到 `Cargo.toml` 文件。

在文件中找到 `[dependencies]` 标题，在 Cargo.toml 文件中，标题以及之后的内容属同一个片段，直到遇到下一个标题才开始新的片段。

**[dependencies] 片段告诉 Cargo 本项目依赖了哪些外部 crate 及其版本。**

```rust
[dependencies]
rand = "0.8.5"
```

本例中，我们使用语义化版本 `0.8.5` 来指定 `rand crate`。`0.8.5` 事实上是 `^0.8.5` 的简写，它表示任何至少是 `0.8.5` 但小于 `0.9.0 `的版本。

添加完依赖之后需要重新在终端中执行 `cargo build` 来确保依赖进行下载和编译，此时会发现项目中多了一个文件 `Cargo.lock`

### Cargo.lock 文件确保构建是可重现的
Cargo 有一个机制来确保任何人在任何时候重新构建代码，都会产生相同的结果：Cargo 只会使用你指定的依赖版本。

*也就是说在.lock文件中存储了你使用的所有外部依赖和你现在使用的标准库的版本以及他们来源的网络地址，只要拿到了这个文件，可以重新构建作者当初代码运行出来的效果，不需要担心版本迭代更新的问题*

### 更新crate到一个新版本
当你 确实 需要升级 crate 时，Cargo 提供了这样一个命令，update，它会忽略 Cargo.lock 文件，并计算出所有符合 Cargo.toml 声明的最新版本。Cargo 接下来会把这些版本写入 Cargo.lock 文件。不过，Cargo 默认只会寻找大于 0.8.5 而小于 0.9.0 的版本。如果 rand crate 发布了两个新版本，0.8.6 和 0.9.0，在运行 cargo update 时会出现如下内容：

> $ cargo update
    Updating crates.io index
    Updating rand v0.8.5 -> v0.8.6

Cargo 忽略了 0.9.0 版本。这时，你也会注意到的 Cargo.lock 文件中的变化无外乎现在使用的 rand crate 版本是0.8.6 。如果想要使用 0.9.0 版本的 rand 或是任何 0.9.x 系列的版本，必须像这样更新 Cargo.toml 文件：

> [dependencies]
rand = "0.9.0"

下一次运行 `cargo build` 时，Cargo 会从 registry 更新可用的 crate，并根据你指定的新版本重新计算。

#### 随机数生成
```rust
use rand::Rng;

fn main(){
    let secret_number = rand::thread_rng().gen_range(1..=100);
}
```
`thread_rng` 是随机数生成器，从操作系统中获取seed

`gen_range `方法获取一个范围表达式 这类范围表达式使用了 `start..=end` 这样的形式，也就是说包含了上下端点，所以需要指定 `1..=100` 来请求一个 1 和 100 之间的数。

#### match
```rust

match guess.cmp(&secret_number) {
    Ordering::Less => println!("Too small!"),
    Ordering::Greater => println!("Too big!"),
    Ordering::Equal => println!("You win!"),
}
```

#### 将字符串类型转换成为数字类型
```rust
let guess: u32 = guess.trim().parse().expect("Please type a number!");
```
Rust 允许用一个新值来 **隐藏**  guess 之前的值。这个功能常用在需要转换值类型之类的场景。它**允许我们复用 guess 变量的名字**，而不是被迫创建两个不同变量，诸如 guess_str 和 guess 之类。

表达式中的 `guess` 指的是包含输入的字符串类型 `guess` 变量。
String 实例的 `trim` 方法会去除字符串开头和结尾的空白字符。
字符串的 `parse` 方法 将字符串转换成其他类型。这里用它来把字符串转换为数值。我们需要告诉 Rust 具体的数字类型，这里通过 `let guess: u32` 指定。`guess` 后面的冒号（:）告诉 Rust 我们指定了变量的类型。
如果 `parse` 不能从字符串生成一个数字，`expect` 会使游戏崩溃并打印附带的信息。

#### lool循环
```rust
loop {
        println!("Please input your guess.");

        // --snip--

        match guess.cmp(&secret_number) {
            Ordering::Less => println!("Too small!"),
            Ordering::Greater => println!("Too big!"),
            Ordering::Equal => {
                println!("You win!");
                break;
            }
        }
}
```
*用户总能使用 ctrl-c 终止程序*

#### 处理无效输入
```rust
let guess: u32 = match guess.trim().parse() {
    Ok(num) => num,
    Err(_) => continue,
};
```
我们将 `expect` 调用换成 `match` 语句，以从遇到错误就崩溃转换为处理错误。须知 `parse` 返回一个 Result 类型，而 Result 是一个拥有 Ok 或 Err 成员的枚举。这里使用的 `match` 表达式，和之前处理 `cmp `方法返回 `Ordering` 时用的一样。

如果 `parse` 能够成功的将字符串转换为一个数字，它会返回一个包含结果数字的 Ok。这个 Ok 值与 match 第一个分支的模式相匹配，该分支对应的动作返回 Ok 值中的数字 num，最后如愿变成新创建的 `guess` 变量。

如果 `parse` 不能将字符串转换为一个数字，它会返回一个包含更多错误信息的 Err。Err 值不能匹配第一个 `match` 分支的 `Ok(num)` 模式，但是会匹配第二个分支的 `Err(_)` 模式：`_` 是一个通配符值，本例中用来匹配所有 Err 值，不管其中有何种信息。所以程序会执行第二个分支的动作，`continue` 意味着进入 `loop` 的下一次循环，请求另一个猜测。这样程序就有效的忽略了 `parse` 可能遇到的所有错误！

### 猜数字代码
```rust
use rand::Rng;
use std::cmp::Ordering;
use std::io;

fn main() {
    println!("Guess the number!");

    let secret_number = rand::thread_rng().gen_range(1..=100);

    loop {
        println!("Please input your guess.");

        let mut guess = String::new();

        io::stdin()
            .read_line(&mut guess)
            .expect("Failed to read line");

        let guess: u32 = match guess.trim().parse() {
            Ok(num) => num,
            Err(_) => continue,
        };

        println!("You guessed: {guess}");

        match guess.cmp(&secret_number) {
            Ordering::Less => println!("Too small!"),
            Ordering::Greater => println!("Too big!"),
            Ordering::Equal => {
                println!("You win!");
                break;
            }
        }
    }
}
```

## 一些编程中常见的概念
- 变量与可变性
- 数据类型
- 函数
- 注释
- 控制流
  
### 变量与可变性
- 变量
    **在Rust中变量是默认不可变的，如果需要变量拥有可变性需要在声明中加上 `mut`**
- 常量
    常量类似与不可变的变量，但是不可以用`mut`修饰，另外在声明时**使用关键字**`const`，并且**必须注明值的类型**。

    Rust 对常量的命名约定是在单词之间使用**全大写加下划线**。

    常量只能被设置为常量表达式，而**不可以是其他任何只能在运行时计算出的值**。

    举例
```rust
const THREE_HOURS_IN_SECONDS: u32 = 60 * 60 * 3;
```

#### 隐藏
可以定义一个与之前变量名相同名字的新变量，在Rust中这种方式称为 第二个变量 **隐藏** 了第一个变量，无论对这个新变量如何操作，都不会影响对第一个变量的值
另外，隐藏可以**嵌套**
举例
```rust
fn main() {
    let x = 5;

    let x = x + 1;

    {
        let x = x * 2;
        println!("The value of x in the inner scope is: {x}");
    }

    println!("The value of x is: {x}");
}
```
输出为：
> The value of x in the  inner scope is: 12
> The value of x is: 6

*实际上也可以直接对原来的变量用mut修饰之后直接使用但是唯一的区别在于在**作用域结束之后隐藏会恢复到没隐藏前的值**，而我们用mut修饰的话此时的变量已经不在作用域返回内了*

举例
```rust
fn main() {
    println!("Hello, world!");
    let mut x = 5;
    x = x + 1;
    println!("x = {x}");
    println!("x + 1 = {}", x + 1);
}
```
输出为：
> Hello, world!
x = 6
x + 1 = 7

`mut` 与隐藏的另一个区别是，当再次使用 `let` 时，实际上创建了一个新变量，我们可以改变值的类型，并且复用这个名字。例如，假设程序请求用户输入空格字符来说明希望在文本之间显示多少个空格，接下来我们想将输入存储成数字（多少个空格）：
举例
```rust 
fn main() {
    let spaces = "   ";
    let spaces = spaces.len();
}
```

### 数据类型
- 标量类型---整型、浮点型、布尔类型和字符类型
- 复合类型
  
Rust是 **静态类型语言**， 静态语言是在编译时变量的数据类型即可确定的语言，要求在使用变量之前必须声明数据类型。

#### 整型

长度|有符号|无符号
:---:|:---:|:---:|
8b|i8|u8
32b|i32|u32
64b|i64|u64
128b|i128|u128
arch|isize|usize

每一个有符号的变体可以储存包含从 -(2^n-1^) 到 2^n-1^ - 1 在内的数字，这里 n 是变体使用的位数。所以 `i8` 可以储存从 -(2^7^) 到 2^7^ - 1 在内的数字，也就是从 -128 到 127。无符号的变体可以储存从 0 到 2^n^ - 1 的数字，所以 `u8` 可以储存从 0 到 2^8^ - 1 的数字，也就是从 0 到 255。

另外，`isize` 和 `usize` 类型依赖运行程序的计算机架构：64 位架构上它们是 64 位的，32 位架构上它们是 32 位的。

数字类型默认 `i32`

****

**未理解**

可以使用表格中的任何一种形式编写数字字面值。请注意可以是多种数字类型的数字字面值允许使用类型后缀，例如 `57u8` 来指定类型，同时也允许使用` _ `做为分隔符以方便读数，例如`1_000`，它的值与你指定的 `1000` 相同。

数字字面值|例子
:--:|:--:|
十进制|98_200
十六进制|0xff
八进制|0o77
二进制|0b1100_0011
单字节字符（仅限`u8`）|b'A'

#### 整型溢出
比方说有一个 `u8` ，它可以存放从零到 255 的值。那么当你将其修改为 256 时会发生什么呢？这被称为 “整型溢出”，这会导致以下两种行为之一的发生。

1. 当在 debug 模式编译时，Rust 检查这类问题并使程序 panic，这个术语被 Rust 用来表明程序因错误而退出。

2. 使用 `--release` flag 在 release 模式中构建时，Rust 不会检测会导致 panic 的整型溢出。相反发生整型溢出时，Rust 会进行一种被称为二进制补码 wrapping 的操作。简而言之，比此类型能容纳最大值还大的值会回绕到最小值，值 256 变成 0，值 257 变成 1，依此类推。

为了显式地处理溢出的可能性，可以使用这几类**标准库**提供的原始数字类型方法：

- 所有模式下都可以使用 `wrapping_*` 方法进行 wrapping，如 `wrapping_add`
- 如果 `checked_* `方法出现溢出，则返回 None值
- 用 `overflowing_*` 方法返回值和一个布尔值，表示是否出现溢出
- 用 `saturating_*` 方法在值的最小值或最大值处进行饱和处理

****

#### 浮点型
Rust 的浮点数类型是 `f32` 和 `f64`，分别占 32 位和 64 位。默认类型是 `f64` ， 所有的浮点型都是有符号的。
```rust
fn main() {
    let x = 2.0; // f64

    let y: f32 = 3.0; // f32
}
```

#### 数值运算
同C一样

#### bool型
同C一样

#### 字符类型
**在Rust中字符类型 `char` 是4B，也就是32位**, 这意味着它可以比 ASCII 表示更多内容。
在 Rust 中，带变音符号的字母（Accented letters），中文、日文、韩文等字符，emoji（绘文字）以及零长度的空白字符都是有效的 char 值。

### 复合类型
复合类型（Compound types）可以将多个值组合成一个类型。Rust 有两个原生的复合类型：元组（tuple）和数组（array）。

#### 元组
元组是一个将多个其他类型的值**组合**进一个复合类型的主要方式。元组**长度固定**：一旦声明，其长度不会增大或缩小。

我们使用包含在圆括号中的逗号分隔的值列表来创建一个元组。元组中的每一个位置都有一个类型。
例子：
```rust
fn main() {
    let tup: (i32, f64, u8) = (500, 6.4, 1);
}
```

##### 解构元组
```rust
fn main() {
    let tup = (500, 6.4, 1);

    let (x, y, z) = tup;

    println!("The value of y is: {y}");
}
```
程序首先创建了一个元组并绑定到 `tup` 变量上。接着使用了 `let` 和一个模式将 `tup` 分成了三个不同的变量，`x`、`y` 和 `z`。这叫做 **解构**（destructuring），因为它将一个元组拆成了三个部分。最后，程序打印出了 `y` 的值，也就是 `6.4`。

我们也可以使用点号（.）后跟值的索引来直接访问它们。例如：

```rust
fn main() {
    let x: (i32, f64, u8) = (500, 6.4, 1);

    let five_hundred = x.0;

    let six_point_four = x.1;

    let one = x.2;
}
```

**元组的第一个索引值是 0**

不带任何值的元组有个特殊的名称，叫做 单元（unit） 元组。这种值以及对应的类型都写作 ()，表示空值或空的返回类型。如果表达式不返回任何其他值，则会隐式返回单元值

#### 数组类型
与元组不同，数组中的每个元素的**类型必须相同**。Rust 中的数组与一些其他语言中的数组不同，Rust 中的数组**长度是固定**的。

但是数组并不如 `vector` 类型灵活。`vector` 类型是标准库提供的一个 **允许** 增长和缩小长度的类似数组的集合类型。

```rust
// 我们将数组的值写成在方括号内，用逗号分隔
let a = [1, 2, 3, 4, 5];

// 可以像这样编写数组的类型：在方括号中包含每个元素的类型，后跟分号，再后跟数组元素的数量。
let a: [i32; 5] = [1, 2, 3, 4, 5];

// 当你确定元素个数不会改变时，数组会更有用
let months = ["January", "February", "March", "April", "May", "June", "July",
    "August", "September", "October", "November", "December"];

// 你还可以通过在方括号中指定初始值加分号再加元素个数的方式来创建一个每个元素都为相同值的数组：
// 这种写法与 let a = [3, 3, 3, 3, 3]; 效果相同，但更简洁。
let a = [3; 5];
```

##### 访问数组元素
```rust
// 可以使用索引来访问数组的元素，像这样：
let a = [1, 2, 3, 4, 5];

let first = a[0];
let second = a[1];
```

当尝试用索引访问一个元素时，Rust 会检查指定的索引是否小于数组的长度。如果索引超出了数组长度，Rust 会 panic，这是 Rust 术语，它用于程序因为错误而退出的情况。

### 函数
```rust
fn main() {
    another_function(5);
}

fn another_function(x: i32) {
    println!("The value of x is: {x}");
}
```
> The value of x is: 5

源码中 another_function 定义在 main 函数 **之后**；也可以定义在之前。

在函数签名中，**必须** 声明每个参数的类型。

```rust
fn main() {
    print_labeled_measurement(5, 'h');
}

fn print_labeled_measurement(value: i32, unit_label: char) {
    println!("The measurement is: {value}{unit_label}");
}
```
> The measurement is: 5h

### 语句和表达式
 Rust 是一门**基于表达式**（expression-based）的语言，这是一个需要理解的（不同于其他语言）重要区别。

 **语句**（Statements）是执行一些操作但不返回值的指令。 **表达式**（Expressions）计算并产生一个值。让我们看一些例子。

 语句不返回值。因此，不能把 let 语句赋值给另一个变量，比如下面的例子尝试做的，会产生一个**错误**：

 ```rust
 fn main() {
    let x = (let y = 6);
}
 ```

 `let y = 6 `语句并不返回值，所以没有可以绑定到` x `上的值。这与其他语言不同，例如 C ，它的赋值语句会返回所赋的值。在这些语言中，可以这么写 `x = y = 6`，这样 `x `和 `y` 的值都是 `6`；Rust 中不能这样写。

表达式会返回值，表达式的结尾 **没有分号**。 下面是表达式的一个例子

```rust
fn main() {
    let y = {
        let x = 3;
        x + 1
    };

    println!("The value of y is: {y}");
}
```

**注意 x+1 这一行在结尾没有分号**

### 具有返回值的函数
函数可以向调用它的代码返回值。我们并不对返回值命名，但要在箭头（->）后声明它的类型。

在 Rust 中，函数的返回值等同于函数体最后一个表达式的值。使用 `return `关键字和指定值，可从函数中提前返回；但大部分函数隐式的返回最后的表达式。这是一个有返回值的函数的例子：

```rust
fn main() {
    let x = plus_one(5);

    println!("The value of x is: {x}");
}

fn plus_one(x: i32) -> i32 {
    // 这是一个表达式，结尾没有分号
    x + 1  
}
```
> The value of x is: 6

### 注释
用 `//` 进行注释
一般注释在语句的上面一行

### if表达式


*不同与c语言 if表达式中的判断条件必须是bool类型，不可以是数值类型，rust没有自动转换的功能*

```rust
fn main(){
    let number = 3;
    if number < 5{
        println!("true");
    }
    // else if ...
    else {
        println!("false!");
    }
}
```

**大部分终端都支持一个快捷键，ctrl-c，来终止一个陷入无限循环的程序。**

### 循环
- loop
    用break来打破循环
- while
- for

```rust
for number in (1..4).rev(){
    println!("{number}!");
}
println!(GO!);
```