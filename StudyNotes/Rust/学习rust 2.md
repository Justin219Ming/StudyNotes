## ����Ȩ
#### ջ
����ȳ����ٶȿ�
#### ��
��Ҫ������һ����С�Ŀռ䣬�ٶȱ�ջ�������ǿ��Ե��õĿռ����ջ

### ����Ȩ����
> 1. Rust �е�ÿһ��ֵ����һ�� �����ߣ�owner����
> 2. ֵ����һʱ������ֻ��һ�������ߡ�
> 3. �������ߣ��������뿪���������ֵ����������

```rust
fn main() {
    {                      // s ��������Ч������δ����
        let s = String::from("hello");   // �Ӵ˴���s ����Ч��

        // ʹ�� s
    }                      // ���������ѽ�����s ������Ч
}
```

### string
���� String ���ͣ�Ϊ��֧��һ���ɱ䣬���������ı�Ƭ�Σ���Ҫ�ڶ��Ϸ���һ���ڱ���ʱδ֪��С���ڴ���������ݡ�

### ����Ȩ--�ƶ�
��δ��벻������
```rust
fn main() {
    let s1 = String::from("hello");
    let s2 = s1;

    println!("{}, world!", s1);
}
```
**�����rust��������Ȩ������һ���ص�**
����ı������ݸ�ֵ����һ����ʱ��ԭ���ı������ᱻrust�Զ�������������ʹ�ã����**�ƶ�**�Ȳ���ǳ����Ҳ�������

### ����Ȩ--��¡
��ȷʵ��Ҫ����string�ж��ϵ�����ʱ��ʹ��`clone`��������ɣ� **�������**
```rust
fn main() {
    let s1 = String::from("hello");
    let s2 = s1.clone();

    println!("s1 = {}, s2 = {}", s1, s2);
}
```

### ����Ȩ--ջ--����
```rust
fn main() {
    let x = 5;
    let y = x;

    println!("x = {}, y = {}", x, y);
}
```
��������� **��֪��С** �����ͣ� ͬʱ�Ǵ����� **ջ** �ϵģ���ô��������ͻ�ֱ�ӿ���

**�������������ı�����**

- �����������ͣ����� u32��
- �������ͣ�bool������ֵ�� true �� false��
- ���и��������ͣ����� f64��
- �ַ����ͣ�char��
- Ԫ�飬���ҽ��������������Ҳ��ʵ�� Copy ��ʱ�򡣱��磬(i32, i32) ʵ���� Copy���� (i32, String) ��û�С�

### ����Ȩ--����
```rust
fn main() {
    let s = String::from("hello");  // s ����������

    takes_ownership(s);             // s ��ֵ�ƶ��������� ...
                                    // ... ���Ե����ﲻ����Ч

    let x = 5;                      // x ����������

    makes_copy(x);                  // x Ӧ���ƶ������
                                    // �� i32 �� Copy �ģ�
                                    // �����ں���ɼ���ʹ�� x

} // ���x ���Ƴ���������Ȼ���� s������Ϊ s ��ֵ�ѱ����ߣ�
  // û������֮��

fn takes_ownership(some_string: String) { // some_string ����������
    println!("{}", some_string);
} // ���some_string �Ƴ������򲢵��� `drop` ������
  // ռ�õ��ڴ汻�ͷ�

fn makes_copy(some_integer: i32) { // some_integer ����������
    println!("{}", some_integer);
} // ���some_integer �Ƴ�������û������֮��
```
�������ڵ��� takes_ownership ��ʹ�� s ʱ��Rust ���׳�һ������ʱ����

### ����Ȩ--����ֵ
���ں����ķ���ֵ��˵Ҳ��������Ȩ������
```rust
fn main() {
    let s1 = gives_ownership();         // gives_ownership ������ֵ
                                        // ת�Ƹ� s1

    let s2 = String::from("hello");     // s2 ����������

    let s3 = takes_and_gives_back(s2);  // s2 ���ƶ���
                                        // takes_and_gives_back �У�
                                        // ��Ҳ������ֵ�Ƹ� s3
} // ���s3 �Ƴ������򲢱�������s2 Ҳ�Ƴ������򣬵��ѱ����ߣ�
  // ����ʲôҲ���ᷢ����s1 �뿪�����򲢱�����

fn gives_ownership() -> String {             // gives_ownership �Ὣ
                                             // ����ֵ�ƶ���
                                             // �������ĺ���

    let some_string = String::from("yours"); // some_string ����������

    some_string                              // ���� some_string 
                                             // ���Ƴ������õĺ���
                                             // 
}

// takes_and_gives_back �������ַ��������ظ�ֵ
fn takes_and_gives_back(a_string: String) -> String { // a_string ����������
                                                      // 

    a_string  // ���� a_string ���Ƴ������õĺ���
}
```

����������Ȩ������ѭ��ͬ��ģʽ����ֵ������һ������ʱ**�ƶ�**����������**��**������ֵ�ı���**�뿪������**ʱ����ֵ��ͨ�� drop ����������������ݱ��ƶ�Ϊ��һ���������С�



### ����
����벻�ƶ�����Ȩ��ʹ�����ݾͱ�������,�������Ĭ���ǲ��ɱ�ģ�����Ҳһ����**Ĭ�ϲ������޸�**���õ�ֵ
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

### �ɱ�����
���ƣ�
- �������һ���Ըñ����Ŀɱ����ã���Ͳ����ٴ����Ըñ��������á�

- ���ܶ�һ������ͬʱӵ�пɱ����úͲ��ɱ�����


```rust
fn main() {
    let mut s = String::from("hello");

    change(&mut s);
}

fn change(some_string: &mut String) {
    some_string.push_str(", world");
}
```

## ������
- rust�е��������е��ر𣬿���û������ʱ��ֱ����`{}`������һ�����������
```rust
fn main() {
    let mut s = String::from("hello");

    {
        let r1 = &mut s;
    } // r1 �������뿪������������������ȫ���Դ���һ���µ�����

    let r2 = &mut s;
}
```

- �������õ�������Ҳ���ر����������ʹ��������֮��Ĭ����������������Կ�������
```rust
fn main() {
    let mut s = String::from("hello");

    let r1 = &s; // û����
    let r2 = &s; // û����
    println!("{} and {}", r1, r2);
    // ��λ��֮�� r1 �� r2 ����ʹ��

    let r3 = &mut s; // û����
    println!("{}", r3);
}
```

- ��ɾ�������ö�Ӧ�����ݵ����ݣ������ʹ�����ñ����ᱨ������C����û�������鹦��

### �����ܽ�
- ���������ʱ�䣬Ҫô ֻ����һ���ɱ����ã�Ҫô ֻ���ж�����ɱ����á�
- ���ñ���������Ч�ġ�

### Slice
slice ���������ü�����һ��������Ԫ�����У������������������ϡ�slice ��һ�����ã�������û������Ȩ��

```rust
fn first_word(s: &String) -> usize {
    // ���ַ���ת�����ֽ�����
    let bytes = s.as_bytes();

    // ʹ�� iter �������ֽ������ϴ���һ��������
    for (i, &item) in bytes.iter().enumerate() {
        // �� for ѭ���У�����ͨ���ֽڵ�����ֵ�﷨��Ѱ�Ҵ���ո���ֽڡ�
        // ����ҵ���һ���ո񣬷�������λ�á�
        // ����ʹ�� s.len() �����ַ����ĳ���
        if item == b' ' {
            return i;
        }
    }

    s.len()
}

fn main() {}
```
slice ������ͦ�ߣ����Ժ�string�������������ͣ������������Ͷ�����

����
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

    // `first_word` ������ `String`���� slice�������ֻ�ȫ��
    let word = first_word(&my_string[0..6]);
    let word = first_word(&my_string[..]);
    // `first_word` Ҳ������ `String` �����ã�
    // ��ȼ������� `String` �� slice
    let word = first_word(&my_string);

    let my_string_literal = "hello world";

    // `first_word` �������ַ�������ֵ�����ֻ�ȫ��
    let word = first_word(&my_string_literal[0..6]);
    let word = first_word(&my_string_literal[..]);

    // ��Ϊ�ַ�������ֵ�Ѿ� **��** �ַ��� slice �ˣ�
    // ��Ҳ�����õģ����� slice �﷨��
    let word = first_word(my_string_literal)��
}
```

### ..�﷨
- ���� `let world = &s[6..11];` �������`world` ����һ������ָ�� `s` ���� 6 ��ָ��ͳ���ֵ 5 �� `slice`��
- ���������������ִ���һֱ�����
- Ҳ����������ͷ�����ִ���ӿ�ͷ��ʼ

### �ṹ��
����ṹ�壬��Ҫʹ�� `struct` �ؼ��ֲ�Ϊ�����ṹ���ṩһ�����֡��ṹ���������Ҫ����������ϵ����ݵ����塣���ţ��ڴ������У�����ÿһ�������ݵ����ֺ����ͣ����ǳ�Ϊ �ֶ�

```rust

struct User {
    active: bool,
    username: String,
    email: String,
    sign_in_count: u64,
}

fn main() {
    // ��������Ҫ�ṹ��ɱ�ͱ����������ṹ������ɱ�
    let mut user1 = User {
        active: true,
        username: String::from("someusername123"),
        email: String::from("someone@example.com"),
        sign_in_count: 1,
    };

    user1.email = String::from("anotheremail@example.com");
}

// ����һ����д�Ĺ��캯����������ֵ�ǽṹ�������
fn build_user(email: String, username: String) -> User {
    User {
        active: true,
        // ������Լ�д�� username,
        username: username, 
        // ��дemail,
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

// �����þɵĽṹ�崴��һ���µĽṹ�壬����..��ʾ�������ݺ�ԭ�ṹ������е�һ��
    let user2 = User {
        email: String::from("another@example.com"),
        ..user1
    };
// ����ȫ����ʾ
    let user2 = User {
        active: user1.active,
        username: user1.username,
        email: String::from("another@example.com"),
        sign_in_count: user1.sign_in_count,
    };
// �����÷� ��߳���ɶ���
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

��Ҫע����ǣ�����������þɽṹ���������һ���½ṹ���������ô������в����ڿ���copy���͵ı���������string������`=`��ֵ�����½ṹ���ĳһ���֣���ô�ɽṹ������� **����Ȩ�ƶ�** �ˡ�

### Ԫ��ṹ��
û�о�����ֶ���,�����ý⹹�ķ�ʽ������
```rust
struct Color(i32, i32, i32);
struct Point(i32, i32, i32);

fn main() {
    let black = Color(0, 0, 0);
    let origin = Point(0, 0, 0);
}
```

����һ��û���κ��ֶεĽṹ�壡���Ǳ���Ϊ **�൥Ԫ�ṹ��**  *��ʱ��֪����������*
```rust
struct AlwaysEqual;

fn main() {
    let subject = AlwaysEqual;
}
```

### ��ӡ�ṹ��
```rust
#[derive(Debug)]  
// ����ⲿ���Ա���Ҫ���ڽṹ�������ǰ�棬
// ��������ʹ��println�� ����ӡ�ṹ��
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
    // ��ӡrect1 is Rectangle { width: 30, height: 50 }

    println!("rect1 is{:#?}",rect1);
    // ��ӡ rect1 is Rectangle {
    //width: 30,
    //height: 50,
//}
}

// ����debugʹ�ã�dbg����õ�����Ȩ��
// �����������Ҫʹ������ͨ����˵��Ҫ����&
#[derive(Debug)]
struct Rectangle {
    width: u32,
    height: u32,
}

fn main() {
    let scale = 2;
    let rect1 = Rectangle {
        // ���������Ҫ�������Ȩ
        width: dbg!(30 * scale),
        height: 50,
    };

    dbg!(&rect1);
}

```
��������
```
30 * scale = 60
&rect1 = Rectangle {
    width: 60,
    height: 50,
}
```

### ���������Ƴ�Ա������
```rust
#[derive(Debug)]
struct Rectangle {
    width: u32,
    height: u32,
}

// ʹ��impl�ؼ��֣�����һ����飬ͬʱ��Ҫ�ǵ�ʹ��&self,�����ýṹ���е�����
impl Rectangle {
    fn area(&self) -> u32 {
        self.width * self.height
    }

    // ����ͬʱ�����������Ľṹ��
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
### �������������ƹ��캯����
```rust
#[derive(Debug)]
struct Rectangle {
    width: u32,
    height: u32,
}

// �ؼ��� Self �ں����ķ��������д�ָ�� impl �ؼ��ֺ���ֵ����ͣ��������� Rectangle
// ��Ȼͨ�����ϲ�������������Ϊnew
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