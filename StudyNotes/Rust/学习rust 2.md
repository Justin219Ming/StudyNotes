## 所有权
#### 栈
后进先出，速度快
#### 堆
需要先请求一定大小的空间，速度比栈慢，但是可以调用的空间大于栈

### 所有权规则
> 1. Rust 中的每一个值都有一个 所有者（owner）。
> 2. 值在任一时刻有且只有一个所有者。
> 3. 当所有者（变量）离开作用域，这个值将被丢弃。

```rust
fn main() {
    {                      // s 在这里无效，它尚未声明
        let s = String::from("hello");   // 从此处起，s 是有效的

        // 使用 s
    }                      // 此作用域已结束，s 不再有效
}
```

### string
对于 String 类型，为了支持一个可变，可增长的文本片段，需要在堆上分配一块在编译时未知大小的内存来存放内容。

### 所有权--移动
这段代码不能运行
```rust
fn main() {
    let s1 = String::from("hello");
    let s2 = s1;

    println!("{}, world!", s1);
}
```
**这就是rust关于所有权的其中一个重点**
当你的变量数据赋值给另一个的时候，原来的变量将会被rust自动抛弃，不能再使用，这个**移动**既不是浅拷贝也不是深拷贝

### 所有权--克隆
当确实需要复制string中堆上的数据时，使用`clone`函数来完成， **等于深拷贝**
```rust
fn main() {
    let s1 = String::from("hello");
    let s2 = s1.clone();

    println!("s1 = {}, s2 = {}", s1, s2);
}
```

### 所有权--栈--拷贝
```rust
fn main() {
    let x = 5;
    let y = x;

    println!("x = {}, y = {}", x, y);
}
```
如果变量是 **已知大小** 的类型， 同时是储存在 **栈** 上的，那么这个变量就会直接拷贝

**满足上面条件的变量有**

- 所有整数类型，比如 u32。
- 布尔类型，bool，它的值是 true 和 false。
- 所有浮点数类型，比如 f64。
- 字符类型，char。
- 元组，当且仅当其包含的类型也都实现 Copy 的时候。比如，(i32, i32) 实现了 Copy，但 (i32, String) 就没有。

### 所有权--函数
```rust
fn main() {
    let s = String::from("hello");  // s 进入作用域

    takes_ownership(s);             // s 的值移动到函数里 ...
                                    // ... 所以到这里不再有效

    let x = 5;                      // x 进入作用域

    makes_copy(x);                  // x 应该移动函数里，
                                    // 但 i32 是 Copy 的，
                                    // 所以在后面可继续使用 x

} // 这里，x 先移出了作用域，然后是 s。但因为 s 的值已被移走，
  // 没有特殊之处

fn takes_ownership(some_string: String) { // some_string 进入作用域
    println!("{}", some_string);
} // 这里，some_string 移出作用域并调用 `drop` 方法。
  // 占用的内存被释放

fn makes_copy(some_integer: i32) { // some_integer 进入作用域
    println!("{}", some_integer);
} // 这里，some_integer 移出作用域。没有特殊之处
```
当尝试在调用 takes_ownership 后使用 s 时，Rust 会抛出一个编译时错误。

### 所有权--返回值
对于函数的返回值来说也满足所有权的内容
```rust
fn main() {
    let s1 = gives_ownership();         // gives_ownership 将返回值
                                        // 转移给 s1

    let s2 = String::from("hello");     // s2 进入作用域

    let s3 = takes_and_gives_back(s2);  // s2 被移动到
                                        // takes_and_gives_back 中，
                                        // 它也将返回值移给 s3
} // 这里，s3 移出作用域并被丢弃。s2 也移出作用域，但已被移走，
  // 所以什么也不会发生。s1 离开作用域并被丢弃

fn gives_ownership() -> String {             // gives_ownership 会将
                                             // 返回值移动给
                                             // 调用它的函数

    let some_string = String::from("yours"); // some_string 进入作用域。

    some_string                              // 返回 some_string 
                                             // 并移出给调用的函数
                                             // 
}

// takes_and_gives_back 将传入字符串并返回该值
fn takes_and_gives_back(a_string: String) -> String { // a_string 进入作用域
                                                      // 

    a_string  // 返回 a_string 并移出给调用的函数
}
```

变量的所有权总是遵循相同的模式：将值赋给另一个变量时**移动**它。当持有**堆**中数据值的变量**离开作用域**时，其值将通过 drop 被清理掉，除非数据被移动为另一个变量所有。



### 引用
如果想不移动所有权而使用数据就必须引用,正如变量默认是不可变的，引用也一样。**默认不允许修改**引用的值
```rust
fn main() {
    let s1 = String::from("hello");

    let len = calculate_length(&s1);

    println!("The length of '{}' is {}.", s1, len);
}

fn calculate_length(s: &String) -> usize {
    s.len()
}
```

### 可变引用
限制：
- 如果你有一个对该变量的可变引用，你就不能再创建对该变量的引用。

- 不能对一个变量同时拥有可变引用和不可变引用


```rust
fn main() {
    let mut s = String::from("hello");

    change(&mut s);
}

fn change(some_string: &mut String) {
    some_string.push_str(", world");
}
```

## 作用域
- rust中的作用域有点特别，可以没函数的时候直接用`{}`来划分一块作用域出来
```rust
fn main() {
    let mut s = String::from("hello");

    {
        let r1 = &mut s;
    } // r1 在这里离开了作用域，所以我们完全可以创建一个新的引用

    let r2 = &mut s;
}
```

- 对于引用的作用域也很特别，他在你最后使用完引用之后默认作用域结束，所以可以这样
```rust
fn main() {
    let mut s = String::from("hello");

    let r1 = &s; // 没问题
    let r2 = &s; // 没问题
    println!("{} and {}", r1, r2);
    // 此位置之后 r1 和 r2 不再使用

    let r3 = &mut s; // 没问题
    println!("{}", r3);
}
```

- 被删除了引用对应的内容的数据，如果再使用引用本身，会报错，对于C而言没有这个检查功能

### 引用总结
- 在任意给定时间，要么 只能有一个可变引用，要么 只能有多个不可变引用。
- 引用必须总是有效的。

### Slice
slice 允许你引用集合中一段连续的元素序列，而不用引用整个集合。slice 是一类引用，所以它没有所有权。

```rust
fn first_word(s: &String) -> usize {
    // 将字符串转换成字节数组
    let bytes = s.as_bytes();

    // 使用 iter 方法在字节数组上创建一个迭代器
    for (i, &item) in bytes.iter().enumerate() {
        // 在 for 循环中，我们通过字节的字面值语法来寻找代表空格的字节。
        // 如果找到了一个空格，返回它的位置。
        // 否则，使用 s.len() 返回字符串的长度
        if item == b' ' {
            return i;
        }
    }

    s.len()
}

fn main() {}
```
slice 兼容性挺高，可以和string，或者数组类型，或者其他类型都可以

例如
```rust
fn first_word(s: &str) -> &str {
    let bytes = s.as_bytes();

    for (i, &item) in bytes.iter().enumerate() {
        if item == b' ' {
            return &s[0..i];
        }
    }

    &s[..]
}

fn main() {
    let my_string = String::from("hello world");

    // `first_word` 适用于 `String`（的 slice），部分或全部
    let word = first_word(&my_string[0..6]);
    let word = first_word(&my_string[..]);
    // `first_word` 也适用于 `String` 的引用，
    // 这等价于整个 `String` 的 slice
    let word = first_word(&my_string);

    let my_string_literal = "hello world";

    // `first_word` 适用于字符串字面值，部分或全部
    let word = first_word(&my_string_literal[0..6]);
    let word = first_word(&my_string_literal[..]);

    // 因为字符串字面值已经 **是** 字符串 slice 了，
    // 这也是适用的，无需 slice 语法！
    let word = first_word(my_string_literal)；
}
```

### ..语法
- 对于 `let world = &s[6..11];` 的情况，`world` 将是一个包含指向 `s` 索引 6 的指针和长度值 5 的 `slice`。
- 可以舍弃最后的数字代表一直到最后
- 也可以舍弃开头的数字代表从开头开始

### 结构体
定义结构体，需要使用 `struct` 关键字并为整个结构体提供一个名字。结构体的名字需要描述它所组合的数据的意义。接着，在大括号中，定义每一部分数据的名字和类型，我们称为 字段

```rust

struct User {
    active: bool,
    username: String,
    email: String,
    sign_in_count: u64,
}

fn main() {
    // 这里你想要结构体可变就必须是整个结构体变量可变
    let mut user1 = User {
        active: true,
        username: String::from("someusername123"),
        email: String::from("someone@example.com"),
        sign_in_count: 1,
    };

    user1.email = String::from("anotheremail@example.com");
}

// 类似一个手写的构造函数，但返回值是结构体的例子
fn build_user(email: String, username: String) -> User {
    User {
        active: true,
        // 这里可以简写成 username,
        username: username, 
        // 简写email,
        email: email,
        sign_in_count: 1,
    }
}

fn main() {
    let user1 = build_user(
        String::from("someone@example.com"),
        String::from("someusername123"),
    );
}

// 可以用旧的结构体创建一个新的结构体，其中..表示其他内容和原结构体变量中的一样
    let user2 = User {
        email: String::from("another@example.com"),
        ..user1
    };
// 或者全部明示
    let user2 = User {
        active: user1.active,
        username: user1.username,
        email: String::from("another@example.com"),
        sign_in_count: user1.sign_in_count,
    };
// 经典用法 提高程序可读性
struct Rectangle {
    width: u32,
    height: u32,
}

fn main() {
    let rect1 = Rectangle {
        width: 30,
        height: 50,
    };

    println!(
        "The area of the rectangle is {} square pixels.",
        area(&rect1)
    );
}

fn area(rectangle: &Rectangle) -> u32 {
    rectangle.width * rectangle.height
}

```

需要注意的是，如果我们利用旧结构体变量创建一个新结构体变量，那么如果其中不属于可以copy类型的变量（比如string）被用`=`赋值给了新结构体的某一部分，那么旧结构体就属于 **所有权移动** 了。

### 元组结构体
没有具体的字段名,可以用解构的方式来访问
```rust
struct Color(i32, i32, i32);
struct Point(i32, i32, i32);

fn main() {
    let black = Color(0, 0, 0);
    let origin = Point(0, 0, 0);
}
```

定义一个没有任何字段的结构体！它们被称为 **类单元结构体**  *暂时不知道用来干嘛*
```rust
struct AlwaysEqual;

fn main() {
    let subject = AlwaysEqual;
}
```

### 打印结构体
```rust
#[derive(Debug)]  
// 这个外部属性必须要加在结构体的声明前面，
// 这样才能使用println！ 来打印结构体
struct Rectangle {
    width: u32,
    height: u32,
}

fn main() {
    let rect1 = Rectangle {
        width: 30,
        height: 50,
    };

    println!("rect1 is {:?}", rect1);
    // 打印rect1 is Rectangle { width: 30, height: 50 }

    println!("rect1 is{:#?}",rect1);
    // 打印 rect1 is Rectangle {
    //width: 30,
    //height: 50,
//}
}

// 关于debug使用，dbg！会得到所有权，
// 所以如果你需要使用他，通常来说需要加上&
#[derive(Debug)]
struct Rectangle {
    width: u32,
    height: u32,
}

fn main() {
    let scale = 2;
    let rect1 = Rectangle {
        // 这里就是需要这个所有权
        width: dbg!(30 * scale),
        height: 50,
    };

    dbg!(&rect1);
}

```
输出结果：
```
30 * scale = 60
&rect1 = Rectangle {
    width: 60,
    height: 50,
}
```

### 方法（类似成员函数）
```rust
#[derive(Debug)]
struct Rectangle {
    width: u32,
    height: u32,
}

// 使用impl关键字，这是一个大块，同时需要记得使用&self,来引用结构体中的数据
impl Rectangle {
    fn area(&self) -> u32 {
        self.width * self.height
    }

    // 这里同时引用了其他的结构体
    fn can_hold(&self, other: &Rectangle) -> bool {
        self.width > other.width && self.height > other.height
    }
}


fn main() {
    let rect1 = Rectangle {
        width: 30,
        height: 50,
    };

    println!(
        "The area of the rectangle is {} square pixels.",
        rect1.area()
    );
}
```
### 关联函数（类似构造函数）
```rust
#[derive(Debug)]
struct Rectangle {
    width: u32,
    height: u32,
}

// 关键字 Self 在函数的返回类型中代指在 impl 关键字后出现的类型，在这里是 Rectangle
// 当然通常大家喜欢命名这个函数为new
impl Rectangle {
    fn square(size: u32) -> Self {
        Self {
            width: size,
            height: size,
        }
    }
}

fn main() {
    let sq = Rectangle::square(3);
}
```