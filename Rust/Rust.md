# 简介

Rust有很多独有的概念，与大多数主流语言不同。 Rust是预先编译(ahead-of-time)的语言。即经过Rust编译形成的程序无须rust环境即可运行。

# 历史

Rust 最初是Mozilla公司的一个研究性项目，用于构建Firfox。为了解决内存问题，Rust被期望是安全的。一直以来，Mozilla都在使用Rust编写一个名为Servo的实验性浏览器引擎，其所有内容都是并行执行的。这个引擎的CSS渲染引擎曾用于Firefox的量子版。

之后Rust因其优秀的特性获得各大厂商和开发者的青睐。Google的新操作系统Fuschia就有一部分(30%)是由Rust编写的；System76使用纯Rust开发了下一代安全操作系统Redox；微软也使用Rust重写了Windows系统的一部分低级组件。

## 优点

- 执行高效
    - 性能和C/C++差不多。
- 内存安全
    - 有着严格的生命周期管控，在编译期就能消除BUG。无GC
- 多处理器
    - 语言设计就考虑到了多处理器。并发性能很好。

## 缺点

- 难学

## 擅长的领域

- 高性能 Web Service
- WebAssembly
- 命令行工具
- 网络编程
- 嵌入式设备
- 系统编程

# 环境

## 安装

### Windows

1. 进入[Rust官网](https://www.rust-lang.org/)下载对应版本的安装程序，
2. 运行安装程序，选择自定义安装。填写安装位置。之后它会自动下载

### Linux

- 使用sh运行脚本
    - `curl https://sh.rustup.rs -sSf | sh `

> 如果是WIndwos的子系统WSL
>
> ```
>  curl –proto '=https' –tlsv1.2 -sSf https://sh.rustup.rs | sh
> ```

### MacOs

- 同样使用sh脚本
    - `curl https://sh.rustup.rs -sSf | sh `

## 更新

在命令行执行如下命令

```sh
rustup update
```

## 卸载

在命令行执行如下命令

```sh
rustup self uninstall
```

## 使用

### 命令 `rustc`

#### 查看安装信息

```
rustc --version
```

输出信息格式为 `rustc <版本号> <commit-hash> <commit日期>` 。

#### 编译文件

```sh
rustc <源文件>
```

将自动编译出一个可执行文件。 而在windows操作系统上还会生成一个 `.pdb` 文件，里面包含了调试的信息。

### 命令 `rustup`

#### 查看文档

```sh
rustup doc
```

会在本地浏览器打开安装版本的文档。



# 包与项目管理

官方安装包自带了 Cargo，Cargo 是 Rust 的构建系统和包管理器。

Rust把代码所需要的库叫做 依赖（dependencies）。

而在 Rust 中，代码包被称为 *crates*。

## Cargo使用

### 创建项目

```sh
cargo new <项目名>
```



Cargo 新建了名为指定名的目录，并在其下生成了两个文件和一个目录

- Cargo.toml 文件

    使用 [*TOML*](https://github.com/toml-lang/toml) (*Tom's Obvious, Minimal Language*) 格式，是 Cargo 配置文件。

- src 目录

    - main.rs 文件

同时Cargo也在项目目录下初始化了一个 git 仓库，以及一个 .gitignore 文件。

> 可以通过 `--vcs` 参数使 `cargo new` 切换到其它版本控制系统（VCS），或者不使用 VCS。运行 `cargo new --help` 参看可用的选项。

>  [*TOML*](https://github.com/toml-lang/toml) (*Tom's Obvious, Minimal Language*) 格式
>
> - 片段（section）
>
>     标识其下的语句是用来配置什么的。
>
>     - [package]
>
>         配置包
>
>     - [dependencies]
>
>         罗列项目依赖的片段
>
> - 语句（segment）
>
>     



Cargo 期望源文件存放在 *src* 目录中。项目根目录只存放 README、license 信息、配置文件和其他跟代码无关的文件。





### 构建并运行项目

```sh
cargo build
```

这个命令会创建一个与项目同名的可执行文件，放在`target/debug/`。

首次运行 `cargo build` 时，也会使 Cargo 在项目根目录创建一个新文件：*Cargo.lock*。这个文件记录项目依赖的实际版本，其是自动更新的不需要人进行修改。

可以使用 `cargo build --release` 来优化编译项目。这会在 `target/release` 而不是 `target/debug` 下生成可执行文件。启用这些优化也需要消耗更长的编译时间，其往往用在构建最终程序时。





```sh
cargo run
```

同时编译并运行生成的可执行文件。

如果 Cargo 发现文件并没有被改变，就直接运行了二进制文件。如果修改了源文件的话，Cargo 会在运行之前重新构建项目。



```sh
cargo check
```

该命令快速检查代码确保其可以编译，但并不产生可执行文件。其比build 快得多。





### 安装新的包

```sh
Cargo install <库名> 
```

与写入Cargo.toml不同，通过该命令添加依赖包只会下载二进制包。





### 更新项目依赖

```sh
cargo update
```

在项目根目录下运行该命令，Cargo会忽略 *Cargo.lock* 文件指定的依赖版本，并计算出所有符合 *Cargo.toml* 声明的最新版本。如果成功了，Cargo 会把这些版本写入 *Cargo.lock* 文件。

其遵循小版本号，例如*Cargo.toml* 声明的版本是`0.8.3`，而 crate 发布了两个新版本，`0.8.4` 和 `0.9.0`。运行命令只会更新到`0.8.4`



# 库与模块





## 导入库

需要配置`Cargo.toml`文件，添加要依赖的包的名称和版本号。

在运行时，将自动下载对应的库(源文件)以及一系列的依赖库。然后编译。

> Cargo 从 *registry* 上获取所有包的最新版本信息，这是一份来自 [Crates.io](https://crates.io/) 的数据拷贝。
>
> Crates.io 是 Rust 生态环境中的开发者们向他人贡献 Rust 开源项目的地方。

例如要添加随机数库则有

```toml
[dependencies]
rand = "0.8.3"
```

> Cargo 理解[语义化版本（Semantic Versioning）](http://semver.org/)（有时也称为 *SemVer*），这是一种定义版本号的标准。`0.8.3` 事实上是 `^0.8.3` 的简写，它表示 “任何与 0.8.3 版本公有 API 相兼容的版本”。



当第一次构建项目时，Cargo 计算出所有符合要求的依赖版本并写入 *Cargo.lock* 文件。当将来构建项目时，Cargo 会发现 *Cargo.lock* 已存在并使用其中指定的版本，而不是再次计算所有的版本。

当你 **确实** 需要升级 crate 时，Cargo 提供了另一个命令，`update`它会忽略 *Cargo.lock* 文件，并计算出所有符合 *Cargo.toml* 声明的最新版本。如果成功了，Cargo 会把这些版本写入 *Cargo.lock* 文件。









## 使用

默认情况下，Rust 将 [*prelude*](https://doc.rust-lang.org/std/prelude/index.html) 模块中少量的类型引入到每个程序的作用域中。如果需要的类型不在 prelude 中，你必须使用 `use` 语句显式地将其引入作用域。

```rust
use std::io;
```



冒号（`::`）是运算符，允许将特定的函数置于类型的命名空间（namespace）下。



## 常用库

### 标准库 `std`

#### 输入输出库 `io`

`std::io` 库提供很多有用的功能，包括接收用户输入的功能。

##### 输入

使用`stdin` 函数返回一个 `std::io::Stdin`的实例，代表终端标准输入句柄的类型（如果程序的开头没有 `use std::io` ，可以把函数调用写成 `std::io::stdin`）。

```rust
use std::io;
io::stdin();
```

`std::io::Stdin`的实例有许多读入的函数。

- `.read_line(&mut var)`

    需要可变字符串作为参数，从标准输入句柄获取用户输入的一行，将其存入一个字符串中。需要注意，按下 enter 键时，会在字符串中增加一个换行（newline）符，即字符串最后一个字符为`\n`。

    



# Rust设计

## 文件

Rust源文件后缀名为 `.rs`

## 规范

- 文件名命名使用下划线分割单词，如 `hello_world.rs` 。
- 文件缩减使用4个空格，而非tab键。
- Rust 常量的命名规范是使用下划线分隔的大写字母单词，并且可以在数字字面值中插入下划线来提升可读性。
- Rust 代码中的函数和变量名使用 *snake case* 规范风格。在 snake case 中，所有字母都是小写并使用下划线分隔单词。



## 所有权

Rust 的核心功能（之一）是 **所有权**（*ownership*）

所有运行的程序都必须管理其使用计算机内存的方式。一些语言中具有垃圾回收机制，在程序运行时不断地寻找不再使用的内存；在另一些语言中，程序员必须亲自分配和释放内存。Rust 则选择了第三种方式：通过所有权系统管理内存，编译器在编译时会根据一系列的规则进行检查。在运行时，所有权系统的任何功能都不会减慢程序。

### 栈（Stack）与堆（Heap）

栈和堆都是代码在运行时可供使用的内存，但是它们的结构不同。

> **栈**
>
> 栈以放入值的顺序存储值并以相反顺序取出值。这也被称作 **后进先出**（*last in, first out*）。增加数据叫做 **进栈**（*pushing onto the stack*），而移出数据叫做 **出栈**（*popping off the stack*）。
>
> 栈中的所有数据都必须占用已知且固定的大小。

> **堆**
>
> 相较于栈，堆是缺乏组织的。堆是划分的用来存储数据的内存块。当向堆放入数据时，你要请求一定大小的空间。操作系统在堆的某处找到一块足够大的空位，把它标记为已使用，并返回一个表示该位置地址的 **指针**（*pointer*）。这个过程称作 **在堆上分配内存**（*allocating on the heap*），有时简称为 “分配”（allocating）。



将数据推入栈中并不被认为是分配。因为指针的大小是已知并且固定的，你可以将指针存储在栈上，不过当需要实际数据时，必须访问指针。

入栈比在堆上分配内存要快，因为（入栈时）操作系统无需为存储新数据去搜索内存空间；其位置总是在栈顶。相比之下，在堆上分配内存则需要更多的工作，这是因为操作系统必须首先找到一块足够存放数据的内存空间，并接着做一些记录为下一次分配做准备。

同时，访问堆上的数据比访问栈上的数据慢，因为必须通过指针来访问。现代处理器在内存中跳转越少就越快（缓存）。在堆上分配大量的空间也可能消耗时间。

当代码调用一个函数时，传递给函数的值（包括可能指向堆上数据的指针）和函数的局部变量被压入栈中。当函数结束时，这些值被移出栈。

跟踪哪部分代码正在使用堆上的哪些数据，最大限度的减少堆上的重复数据的数量，以及清理堆上不再使用的数据确保不会耗尽空间，这些问题正是所有权系统要处理的。



### 所有权规则

1. Rust 中的每一个值都有一个被称为其 **所有者**（*owner*）的变量。
2. 值在任一时刻有且只有一个所有者。
3. 当所有者（变量）离开作用域，这个值将被丢弃。



### 变量作用域

作用域是一个项（item）在程序中有效的范围。

```rust
{                      // s 在这里无效, 它尚未声明
    let s = "hello";
    // 使用 s
}                      // 此作用域已结束，s 不再有效
```

变量 `s` 绑定到了一个字符串字面值，这个字符串值是硬编码进程序代码中的。这个变量从声明的点开始直到当前作用域结束时都是有效的。

- 当 `s` 进入作用域 时，它就是有效的。
- 这一直持续到它 离开作用域 为止。

上述变量s实际上使用的是分配在栈上的直接硬编码进最终的可执行文件中的字符串字面值，这使得字符串字面值快速且高效。不过这些特性都只得益于字符串字面值的不可变性。不幸的是，我们不能为了每一个在编译时大小未知的文本而将一块内存放入二进制文件中，并且它的大小还可能随着程序运行而改变。

当然，变量也可以绑定一个在编译时大小未知的文本，只需在堆上分配空间。

```rust
{                      	  // s 在这里无效, 它尚未声明
    let s = String::from("hello"); 
   	// 使用 s
}        				// 此作用域已结束，s 不再有效
```

在堆上创建一个变量，需要在运行时向操作系统请求内存，第一部分由我们完成。

而对于将内存返回给操作系统的方法，则是Rust语言和其他不同之处。在有 **垃圾回收**（*garbage collector*，*GC*）的语言中， GC 记录并清除不再使用的内存，而我们并不需要关心它。没有 GC 的话，识别出不再使用的内存并调用代码显式释放就是我们的责任了，跟请求内存的时候一样。从历史的角度上说正确处理内存回收曾经是一个困难的编程问题。如果忘记回收了会浪费内存。如果过早回收了，将会出现无效变量。如果重复回收，这也是个 bug。我们需要精确的为一个 `allocate` 配对一个 `free`。

Rust 采取了一个不同的策略：内存在拥有它的变量离开作用域后就被自动释放。当变量离开作用域，Rust 为我们调用一个特殊的函数。这个函数叫做 `drop`，在这里 `String` 的作者可以放置释放内存的代码。Rust 在结尾的 `}` 处自动调用 `drop`。

> 在 C++ 中，这种 item 在生命周期结束时释放资源的模式有时被称作 **资源获取即初始化**（*Resource Acquisition Is Initialization (RAII)*）。如果你使用过 RAII 模式的话应该对 Rust 的 `drop` 函数并不陌生。



现在它看起来很简单，不过在更复杂的场景下代码的行为可能是不可预测的，比如当有多个变量使用在堆上分配的内存时。下面探索一些这样的场景。

#### 数据在变量间移动

Rust 中的多个变量可以采用一种独特的方式与同一数据交互。

```rust
let x = 5;
let y = x;
```

将 `5` 绑定到 `x`；接着生成一个值 `x` 的拷贝并绑定到 `y`，因为整数是有已知固定大小的简单值，所以这两个 `5` 被放入了栈中。

```rust
let s1 = String::from("hello");
let s2 = s1;
```

`String` 由三部分组成，如图左侧所示：一个指向存放字符串内容内存的指针，一个长度，和一个容量。这一组数据存储在栈上。右侧则是堆上存放内容的内存部分。

![String in memory](https://kaisery.github.io/trpl-zh-cn/img/trpl04-01.svg)

当我们将 `s1` 赋值给 `s2`，`String` 的数据被复制了，这意味着我们从栈上拷贝了它的指针、长度和容量。我们并没有复制指针指向的堆上数据。

![s1 and s2 pointing to the same value](https://kaisery.github.io/trpl-zh-cn/img/trpl04-02.svg)

为了确保内存安全，这种场景下 Rust 认为 `s1` 不再有效，因此 Rust 不需要在 `s1` 离开作用域后清理任何东西。那么，在 `s2` 被创建之后尝试使用 `s1`会报告错误：使用无效的引用。

不同于其他语言 **浅拷贝**（*shallow copy*）和 **深拷贝**（*deep copy*）概念， Rust 同时使第一个变量无效了，这个操作被称为 **移动**（*move*），而不是浅拷贝。

Rust 永远也不会自动创建数据的 “深拷贝”。因此，任何 **自动** 的复制可以被认为对运行时性能影响较小。



#### 从变量里克隆数据

如果我们确实需要深度复制 `String` 中堆上的数据，而不仅仅是栈上的数据，可以使用一个叫做 `clone` 的通用函数。

```rust
let s1 = String::from("hello");
let s2 = s1.clone();

println!("s1 = {}, s2 = {}", s1, s2);
```



> 像整型这样的在编译时已知大小的类型被整个存储在栈上，拷贝其实际的值是快速的，调用 `clone` 并不会与通常的浅拷贝有什么不同。
>
> Rust 有一个叫做 `Copy` trait 的特殊注解，可以用在类似整型这样的存储在栈上的类型上。如果一个类型拥有 `Copy` trait，一个旧的变量在将其赋值给其他变量后仍然可用。
>
> 同时，Rust对其也进行了限制，不允许自身或其任何部分实现了 `Drop` trait 的类型使用 `Copy` trait。

> 什么类型是 `Copy` 的呢？可以查看给定类型的文档来确认，不过作为一个通用的规则，任何简单标量值的组合可以是 `Copy` 的，不需要分配内存或某种形式资源的类型是 `Copy` 的。如下是一些 `Copy` 的类型：
>
> - 所有整数类型，比如 `u32`。
> - 布尔类型，`bool`，它的值是 `true` 和 `false`。
> - 所有浮点数类型，比如 `f64`。
> - 字符类型，`char`。
> - 元组，当且仅当其包含的类型也都是 `Copy` 的时候。比如，`(i32, i32)` 是 `Copy` 的，但 `(i32, String)` 就不是。



### 所有权与函数

将值传递给函数在语义上与给变量赋值相似。向函数传递值可能会移动或者复制，就像赋值语句一样。

```rust
fn main() {
    let s = String::from("hello");  // s 进入作用域

    takes_ownership(s);             // s 的值移动到函数里 ...
                                    // ... 所以到这里不再有效

    let x = 5;                      // x 进入作用域

    makes_copy(x);                  // x 应该移动函数里，
                                    // 但 i32 是 Copy 的，所以在后面可继续使用 x

} // 这里, x 先移出了作用域，然后是 s。但因为 s 的值已被移走，
  // 所以不会有特殊操作

fn takes_ownership(some_string: String) { // some_string 进入作用域
    println!("{}", some_string);
} // 这里，some_string 移出作用域并调用 `drop` 方法。占用的内存被释放

fn makes_copy(some_integer: i32) { // some_integer 进入作用域
    println!("{}", some_integer);
} // 这里，some_integer 移出作用域。不会有特殊操作

```

当尝试在调用 `takes_ownership` 后使用 `s` 时，Rust 会抛出一个编译时错误。这些静态检查使我们免于犯错。



#### 返回值与作用域

返回值也可以转移所有权。

```rust
fn main() {
    let s1 = gives_ownership();         // gives_ownership 将返回值
                                        // 移给 s1

    let s2 = String::from("hello");     // s2 进入作用域

    let s3 = takes_and_gives_back(s2);  // s2 被移动到
                                        // takes_and_gives_back 中,
                                        // 它也将返回值移给 s3
} // 这里, s3 移出作用域并被丢弃。s2 也移出作用域，但已被移走，
  // 所以什么也不会发生。s1 移出作用域并被丢弃

fn gives_ownership() -> String {             // gives_ownership 将返回值移动给
                                             // 调用它的函数

    let some_string = String::from("hello"); // some_string 进入作用域.

    some_string                              // 返回 some_string 并移出给调用的函数
}

// takes_and_gives_back 将传入字符串并返回该值
fn takes_and_gives_back(a_string: String) -> String { // a_string 进入作用域

    a_string  // 返回 a_string 并移出给调用的函数
}

```

变量的所有权总是遵循相同的模式：将值赋给另一个变量时移动它。当持有堆中数据值的变量离开作用域时，其值将通过 `drop` 被清理掉，除非数据被移动为另一个变量所有。





#### 引用与借用

在函数定义中，我们可以在参数前面加上 `&` 来表示以一个对象的**引用**（*references*）作为参数而不是获取值的所有权。

```rust
fn main() {
    let s1 = String::from("hello");

    let len = calculate_length(&s1);

    println!("The length of '{}' is {}.", s1, len);
}

fn calculate_length(s: &String) -> usize {// s 是对 String 的引用
    s.len()
}// 这里，s 离开了作用域。但因为它并不拥有引用值的所有权，
  // 所以什么也不会发生
```

`&s1` 语法让我们创建一个 **指向** 值 `s1` 的引用，但是并不拥有它。因为并不拥有这个值，当引用离开作用域时其指向的值也不会被丢弃。变量 `s` 有效的作用域与函数参数的作用域一样，不过当引用离开作用域后并不丢弃它指向的数据，因为没有所有权。当函数使用引用而不是实际值作为参数，无需返回值来交还所有权，因为就不曾拥有所有权。

![&String s pointing at String s1](https://kaisery.github.io/trpl-zh-cn/img/trpl04-05.svg)

将获取引用作为函数参数称为 **借用**（*borrowing*）。

正如变量默认是不可变的，引用也一样。（默认）不允许修改引用的值。如果想要以课可变的引用，就必须显式声明：必须将 `s` 改为 `mut`。然后必须创建一个可变引用 `&mut s` 和接受一个可变引用 `some_string: &mut String`。

```rust
fn main() {
    let mut s = String::from("hello");

    change(&mut s);
}

fn change(some_string: &mut String) {
    some_string.push_str(", world");
}
```

可变引用有一个很大的限制：在特定作用域中的特定数据只能有一个可变引用。

即以下代码会失败

```rust
let mut s = String::from("hello");

let r1 = &mut s;
let r2 = &mut s;

println!("{}, {}", r1, r2);  // 试图同时使用引用了同一个值的两个可变引用
```

这个限制允许可变性，不过是以一种受限制的方式允许，其好处是 Rust 可以在编译时就避免数据竞争。

> **数据竞争**（*data race*）
>
> 类似于竞态条件，它可由这三个行为造成：
>
> - 两个或更多指针同时访问同一数据。
> - 至少有一个指针被用来写入数据。
> - 没有同步数据访问的机制。
>
> 数据竞争会导致未定义行为，难以在运行时追踪，并且难以诊断和修复；Rust 避免了这种情况的发生，因为它甚至不会编译存在数据竞争的代码！

可以使用大括号来创建一个新的作用域，以允许拥有多个可变引用，只是不能 **同时** 拥有：

```rust
#![allow(unused)]
fn main() {
    let mut s = String::from("hello");

    {
        let r1 = &mut s;

    } // r1 在这里离开了作用域，所以我们完全可以创建一个新的引用

    let r2 = &mut s;
}
```

类似的规则也存在于同时使用可变与不可变引用中。如下

```rust
let mut s = String::from("hello");

let r1 = &s; // 没问题
let r2 = &s; // 没问题
let r3 = &mut s; // 大问题

println!("{}, {}, and {}", r1, r2, r3); // 试图同时使用引用了同一个值的可变引用和不可变引用
```

但是，多个不可变引用是可以的。

一个引用的作用域从声明的地方开始一直持续到最后一次使用为止。如果最后一次使用不可变引用在声明可变引用之前，那也是被允许的。

```rust
{
    let mut s = String::from("hello");

    let r1 = &s; // 没问题
    let r2 = &s; // 没问题
    println!("{} and {}", r1, r2);
    // 此位置之后 r1 和 r2 不再使用

    let r3 = &mut s; // 没问题
    println!("{}", r3);
}
```





#### 悬垂引用

在具有指针的语言中，很容易通过释放内存时保留指向它的指针而错误地生成一个 **悬垂指针**（*dangling pointer*），所谓悬垂指针是其指向的内存可能已经被分配给其它持有者。同样的，类比到引用，也存在着**悬垂引用**（Dangling References）的概念。

相比之下，在 Rust 中编译器确保引用永远也不会变成悬垂状态：当你拥有一些数据的引用，编译器确保数据不会在其引用之前离开作用域。

比如，下列用法是错误的

```rust
fn main() {
    let reference_to_nothing = dangle();
}

fn dangle() -> &String {
    let s = String::from("hello");

    &s		// 错误
}
```









使用 `&` 引用相反的操作是 **解引用**（*dereferencing*），它使用解引用运算符，`*`







# 变量

## 声明

使用`let` 关键字声明**变量**（*variable*），默认是不可变变量（immutable）。

```rust
let foo=bar;
```

新建了一个叫做 `foo` 的变量并把它绑定到值 `bar` 上



如果需要声明可变变量，需要加上 `mut` 关键字

```rust
let mut guess = String::new()
```

这一行创建了一个可变变量，当前它绑定到一个新的 `String` 空实例上。



> 用大型数据结构时，适当地使用可变变量，可能比复制和返回新分配的实例更快。对于较小的数据结构，总是创建新实例，采用更偏向函数式的编程风格，可能会使代码更易理解，为可读性而牺牲性能或许是值得的。





## 变量隐藏

Rust 允许用一个新值来 **隐藏** （*shadow*）变量之前的值。这个功能常用在需要转换值类型之类的场景。它允许我们复用变量的名字，而不是被迫创建两个不同变量，

```rust
let x = 5;
let x = x + 1;
let x = x * 2;
```



隐藏与将变量标记为 `mut` 是有区别的。当不小心尝试对变量重新赋值时，如果没有使用 `let` 关键字，就会导致编译时错误。通过使用 `let`，我们可以用这个值进行一些计算，不过计算完之后变量仍然是不变的。

`mut` 与隐藏的另一个区别是，当再次使用 `let` 时，实际上创建了一个新变量，我们可以改变值的类型，但复用这个名字。







# 常量

Rust使用 `const` 关键字，并且 必须显式注明值的类型。

```rust
const MAX_POINTS: u32 = 100_000;
```

在声明它的作用域之中，常量在整个程序生命周期中都有效，这使得常量可以作为多处代码使用的全局范围的值。





> 变量和常量的区别
>
> 不允许对常量使用 `mut`。常量不光默认不能变，它总是不能变。
>
> 常量可以在任何作用域中声明，包括全局作用域。变量不能。
>
> 常量只能被设置为常量表达式，而不能是函数调用的结果，或任何其他只能在运行时计算出的值。







# 数据结构

Rust 是 **静态类型**（*statically typed*）语言，即在编译时就必须知道所有变量的类型。当多种类型均有可能时，必须增加类型注解。

在 Rust 中，每一个值都属于某一个 **数据类型**（*data type*），这告诉 Rust 它被指定为何种数据，以便明确数据处理方式。

我们将看到两类数据类型子集：标量（scalar）和复合（compound）。



## 标量类型

**标量**（*scalar*）类型代表一个单独的值。Rust 有四种基本的标量类型：整型、浮点型、布尔类型和字符类型。

### 整型

**整数** 是一个没有小数部分的数字。Rust的整形如下

| 长度    | 有符号  | 无符号  |
| ------- | ------- | ------- |
| 8-bit   | `i8`    | `u8`    |
| 16-bit  | `i16`   | `u16`   |
| 32-bit  | `i32`   | `u32`   |
| 64-bit  | `i64`   | `u64`   |
| 128-bit | `i128`  | `u128`  |
| arch    | `isize` | `usize` |

Rust 默认使用 `i32`，它通常是最快的，甚至在 64 位系统上也是。

每一个有符号的变体可以储存包含从 -(2^n-1^) 到 2^n-1^ - 1 在内的数字，这里 *n* 是变体使用的位数。当需要考虑符号的时候，数字以加号或减号作为前缀（可以安全地假设为正数时，加号前缀通常省略）。有符号数在计算机内部一补码形式存储。

对于`isize` 和 `usize` 类型，它们依赖运行程序的计算机架构：64 位架构上它们是 64 位的， 32 位架构上它们是 32 位的。`isize` 或 `usize` 主要作为某些集合的索引。



#### 字面量

数字字面值可以用以下形式编写，除 byte 以外的所有数字字面值允许使用类型后缀，同时也允许使用 `_` 做为分隔符以方便读数。

| 数字字面值                    | 例子          |
| ----------------------------- | ------------- |
| Decimal (十进制)              | `98_222`      |
| Hex (十六进制)                | `0xff`        |
| Octal (八进制)                | `0o77`        |
| Binary (二进制)               | `0b1111_0000` |
| Byte (单字节字符)(仅限于`u8`) | `b'A'`        |



> 对于[整型溢出](https://kaisery.github.io/trpl-zh-cn/ch03-02-data-types.html#整型溢出)，当在 debug 模式编译时，Rust 检查这类问题并使程序 *panic*。
>
> 而在 release 构建中，Rust 不检测溢出，相反会进行一种被称为二进制补码包装（*two’s complement wrapping*）的操作。简而言之，`256` 变成 `0`，`257` 变成 `1`，依此类推。





### 浮点型

Rust 也有两个原生的 **浮点数**（*floating-point numbers*）类型，它们是带小数点的数字。Rust 的浮点数类型是 `f32` 和 `f64`，分别占 32 位和 64 位。默认类型是 `f64`，其在现代 CPU 中，它与 `f32` 速度几乎一样，不过精度更高。

浮点数采用 IEEE-754 标准表示。`f32` 是单精度浮点数，`f64` 是双精度浮点数。

```rust
let x = 2.0; // f64
let y: f32 = 3.0; // f32
```



### 布尔型

Rust 中的布尔类型使用 `bool` 表示，有两个可能的值：`true` 和 `false`。

```rust
let t = true;
let f: bool = false; // 显式指定类型注解
```



### 字符类型

Rust 的 `char` 类型是语言中最原生的字母类型，由单引号指定。

```rust
let c = 'z';
let z = 'ℤ';
let heart_eyed_cat = '😻';
```

Rust 的 `char` 类型的大小为四个字节(four bytes)，并代表了一个 Unicode 标量值（Unicode Scalar Value）。

Unicode 标量值包含从 `U+0000` 到 `U+D7FF` 和 `U+E000` 到 `U+10FFFF` 在内的值。





### 数值运算

Rust 中的所有数字类型都支持基本数学运算：加法、减法、乘法、除法和取余。

```rust
// 加法
let sum = 5 + 10;

// 减法
let difference = 95.5 - 4.3;

// 乘法
let product = 4 * 30;

// 除法
let quotient = 56.7 / 32.2;

// 取余
let remainder = 43 % 5;
```






## 符合类型

**复合类型**（*Compound types*）可以将多个值组合成一个类型。Rust 有两个原生的复合类型：元组（tuple）和数组（array）。



### 元组类型

元组是一个将**多个其他类型**的值组合进一个复合类型的主要方式。元组中的每一个位置都有一个类型，而且这些不同值的类型也不必是相同的。元组长度固定，一旦声明，其长度不会增大或缩小。

Rust使用包含在圆括号中的逗号分隔的值列表来创建一个元组。

```rust
let tup: (i32, f64, u8) = (500, 6.4, 1);
```

`tup` 变量绑定到整个元组上，因为元组是一个单独的复合元素。



为了从元组中获取单个值，可以使用模式匹配（pattern matching）来解构（destructure）元组值

```rust
let tup = (500, 6.4, 1);
let (x, y, z) = tup;
```

使用了 `let` 和一个模式将 `tup` 分成了三个不同的变量，`x`、`y` 和 `z`。这叫做 **解构**（*destructuring*），因为它将一个元组拆成了三个部分。



也可以使用点号（`.`）后跟值的索引来直接访问它们

```rust
let x: (i32, f64, u8) = (500, 6.4, 1);
let five_hundred = x.0;
let six_point_four = x.1;
let one = x.2;
```



### 数组类型

Rust数组中的每个元素的类型必须相同，同样是固定长度的：一旦声明，它们的长度不能增长或缩小。

Rust 中，数组中的值位于中括号内的逗号分隔的列表中

```rust
let a = [1, 2, 3, 4, 5];
```

也可以显示声明数组的类型：在方括号中包含每个元素的类型，后跟分号，再后跟数组元素的数量。

```rust
let a: [i32; 5] = [1, 2, 3, 4, 5];
```

如果要为每个元素创建包含相同值的数组，可以指定初始值，后跟分号，然后在方括号中指定数组的长度，例如声明包含5个3的数组。

```rust
let a = [3; 5];
// 等同于
let a = [3, 3, 3, 3, 3];
```

数组是一整块分配在栈上的内存。当想要在栈（stack）而不是在堆（heap）上为数据分配空间，或者是想要确保总是有固定数量的元素时，数组非常有用。



#### 访问

索引从0开始

```rust
let a = [1, 2, 3, 4, 5];
let first = a[0];
let second = a[1];
```



> 对于越界访问，编译并不会产生任何错误，不过程序会出现一个 **运行时**（*runtime*）错误并且不会成功退出，当尝试用索引访问一个元素时，Rust 会检查指定的索引是否小于数组的长度。如果索引超出了数组长度，Rust 会 *panic*。





## 枚举

枚举类型持有固定集合的值，这些值被称为枚举的 **成员**（*variants*）。





## 字符串

### 字符串字面值

字符串字面值是被硬编码进程序里的字符串值，使用 `"` 进行分割。

```rust
"123454321"
```

它们是不可变的。

### 字符串类型

字符串类型是 `String` 由标准库提供，被分配到堆上，能够存储在编译时未知大小的文本。内部使用utf-8格式的编码。

一个新的空字符串由**关联函数**（*associated function*）`new`创建。

也可以使用 `from`函数基于字符串字面量来创建。

```rust
let s = String::from("hello");
```



对于 `String` 类型，为了支持一个可变，可增长的文本片段，需要在堆上分配一块在编译时未知大小的内存来存放内容。这意味着：

- 必须在运行时向操作系统请求内存。

    由我们完成：当调用 `String::from` 时，它的实现 (*implementation*) 请求其所需的内存。

- 需要一个当我们处理完 `String` 时将内存返回给操作系统的方法。





#### 类型转换

字符串的 `parse` 方法将字符串解析成数字，这个方法可以解析多种数字类型，因此需要告诉 Rust 具体的数字类型，使用Rust的自带的推断

```rust
let guess: u32  = guess.trim().parse().expect("Please type a number!");
```

如果 `parse` 不能从字符串生成一个数字，返回一个 `Result` 的 `Err` 成员时，`expect` 会使程序崩溃并打印附带的信息。如果 `parse` 成功地将字符串转换为一个数字，它会返回 `Result` 的 `Ok` 成员，然后 `expect` 会返回 `Ok` 值中的数字。





#### 方法

`as_bytes` 方法可以将 `String` 转化为字节数组，返回的是队员数组的引用。

```rust
let bytes = s.as_bytes(); // bytes是&[u8]类型的
```



`len`方法可以获取`String`类的长度，返回的是整数，其与原本的字符串是分离，也就是说，在之后改变字符串后使用整数也不会报错。

```rust
let l = s.len();
```







## Slice类型

slice 类型允许你引用集合中一段连续的元素序列，而不用引用整个集合。它是一个没有所有权的数据类型。

```rust
let s = String::from("hello world");

let hello = &s[0..5];
let world = &s[6..11];
```

上述引用由中括号中的 `[starting_index..ending_index]` 指定的 range 创建。其类似于引用整个 `String` 不过带有额外的 `[0..5]` 部分。它不是对整个 `String` 的引用，而是对部分 `String` 的引用。其中 `starting_index` 是 slice 的第一个位置，`ending_index` 则是 slice 最后一个位置的后一个值。

实际上，slice的存储是由指针和长度构成的。如下

![world containing a pointer to the 6th byte of String s and a length 5](https://kaisery.github.io/trpl-zh-cn/img/trpl04-06.svg)



> `..` range 语法
>
> ```rust
> let sub_str = &x[0..2];
> ```
>
> 
>
> 从第一个索引（0）开始，可以不写两个点号之前的值。
>
> 包含到的最后一个元素，也可以舍弃尾部的数字。
>
> 也可以同时舍弃这两个值来获取整个数组的 slice。
>
> > 对于字符串 slice range 的索引必须位于有效的 UTF-8 字符边界内，如果尝试从一个多字节字符的中间位置创建字符串 slice，则程序将会因错误而退出。









# 语法

## 语句

Rust 是一门基于表达式（expression-based）的语言，有着语句和表达式两个概念。

**语句**（*Statements*）是执行一些操作但不返回值的指令，通常以分号 `;` 结束。其包含了赋值语句，分支语句等。函数定义也是语句。

表达式（*Expressions*）计算并产生一个值，表达式可以是语句的一部分。大部分 Rust 代码是由表达式组成的：宏调用是一个表达式。我们用来创建新作用域的大括号（代码块），`{}`，也是一个表达式。表达式的结尾没有分号，如果在表达式的结尾加上分号，它就变成了语句。

比如

```rust
let y = {
    let x = 3;
    x + 1
};
// 大括号即内部就是一个表达式，其值为 x+1 的值。注意 x+1 后面没有;
```





### 赋值语句

使用 `let` 关键字创建变量并绑定一个值是一个赋值语句。

```rust
let y = 6;
```

由于语句不返回值。因此，不能把 `let` 语句赋值给另一个变量（不像C能够 `x=y=6`）。



### 注释语句

#### 单行注释

`//` 开始一个注释，持续到行尾。

```rust
let x = 1; // 将x初始化为 1
```



#### 文档注释







### 分支语句

#### `if` 表达式

所有的 `if` 表达式都以 `if` 关键字开头，其后跟一个条件。 `if` 表达式中与条件关联的代码块有时被叫做 *arms*。也可以包含一个可选的 `else` 表达式来提供一个在条件为假时应当执行的代码块。

```rust
if number < 5 {
    println!("condition was true");
} else {
    println!("condition was false");
}
```

Rust 并不会尝试自动地将非布尔值转换为布尔值，即代码中的条件必须是 `bool` 值（这与其他语言不太相同）。

当然，也可以使用 `else if` 表达式处理多重条件。

```rust
    if number % 4 == 0 {
        println!("number is divisible by 4");
    } else if number % 3 == 0 {
        println!("number is divisible by 3");
    } else if number % 2 == 0 {
        println!("number is divisible by 2");
    } else {
        println!("number is not divisible by 4, 3, or 2");
    }
```



因为 `if` 是一个表达式，我们可以在 `let` 语句的右侧使用它，这意味着 `if` 的每个分支的可能的返回值都必须是相同类型；

```rust
let number = if condition {
    5
} else {
    6
};
```





#### `match` 表达式

一个 `match` 表达式由 **分支（arms）** 构成。一个分支包含一个 **模式**（*pattern*）和表达式开头的值与分支模式相匹配时应该执行的代码。Rust 获取提供给 `match` 的值并挨个检查每个分支的模式。

```rust
match guess.cmp(&secret_number) {
    Ordering::Less => println!("Too small!"),
    Ordering::Greater => println!("Too big!"),
    Ordering::Equal => println!("You win!"),
}
```

注意，单独的`match`语句块结尾不带`;`





### 循环语句

Rust 提供了多种 **循环**（*loops*）：`loop`、`while` 和 `for`。一个循环执行循环体中的代码直到结尾并紧接着回到开头继续执行。

#### `loop` 表达式

`loop` 关键字创建了一个无限循环。注意，单独的`loop`语句块结尾不带`;`

```rust
loop{
    
}
```

使用`break` 关键字，可以跳出循环

```rust
loop{
    // 完成了某件事
    break;
}
```

`loop` 的一个用例是重试可能会失败的操作，比如检查线程是否完成了任务，可能会需要将操作的结果传递给其它的代码。

将返回值加入用来停止循环的 `break` 表达式可以传递参数

```rust
let result = loop {
    counter += 1;

    if counter == 10 {
        break counter * 2;
    }
};
```



如果想要跳过循环剩余部分，使用`continue`关键字

```rust
loop{
    // 达到了某个条件
    continue;
}
```



#### `while`条件循环

```rust
let mut number = 3;

while number != 0 {
    println!("{}!", number);

    number = number - 1;
}
```

可以使用 `while` 结构来遍历集合中的元素

```rust
let a = [10, 20, 30, 40, 50];
let mut index = 0;

while index < 5 {
    println!("the value is: {}", a[index]);

    index = index + 1;
}
```



#### `for` 遍历循环

使用`while`遍历集合的过程很容易出错，也使程序更慢，因为编译器增加了运行时代码来对每次循环的每个元素进行条件检查。

作为更简洁的替代方案，可以使用 `for` 循环来对一个集合的每个元素执行一些代码。

```rust
let a = [10, 20, 30, 40, 50];

for element in a.iter() {
    println!("the value is: {}", element);
}
```

使用 `for` 循环的话，不需要惦记着在改变数组元素个数时修改其他的代码了。

`for` 循环的安全性和简洁性使得它成为 Rust 中使用最多的循环结构。它也可以循环执行代码特定次数，需要借助标准库提供的类型 `Range`。

```rust
for number in (1..4).rev() {
    println!("{}!", number);
}
```

> `rev`方法用来反转 range







## 函数

### 定义

Rust 中的函数定义如下，函数名后跟一对圆括号，大括号告诉编译器哪里是函数体的开始和结尾。

```rust
fu main(){

}
```

Rust 不关心函数定义于何处，即调用可在定义之前也可在定义之后。

> `main` 函数是每个Rust程序最先执行的代码。



#### 参数

函数也可以被定义为拥有 **参数**（*parameters*），参数是特殊变量，是函数签名的一部分。当函数拥有参数（形参）时，可以为这些参数提供具体的值（实参）。技术上讲，这些具体值被称为参数（*arguments*），但是在日常交流中，人们倾向于不区分使用 *parameter* 和 *argument* 来表示函数定义中的变量或调用函数时传入的具体值。

```rust
fn another_function(x: i32, y: i32) {
    println!("The value of x is: {}", x);
    println!("The value of y is: {}", y);
}
```

在函数签名中，必须声明每个参数的类型。当一个函数有多个参数时，使用逗号分隔。



参数默认是不可变的。



在前面使用 `&` 声明参数传递是按引用（*reference*）传递。引用在Rust中默认也是不可变的，如果想要在函数中修改引用的变量同样需要加上 `mut` 关键字。





#### 返回值

函数可以向调用它的代码返回值。需要在箭头（`->`）后声明它的类型。

```rust
fun max(a:i32, b:i32) -> bool{
    true
}
```

在 Rust 中，函数的返回值等同于函数体最后一个表达式的值，也可以使用 `return` 关键字和指定值，显式地返回。



可以使用元组来返回多个值。

```rust
fn main() {
    let s1 = String::from("hello");

    let (s2, len) = calculate_length(s1);

    println!("The length of '{}' is {}.", s2, len);
}

fn calculate_length(s: String) -> (String, usize) {
    let length = s.len(); // len() 返回字符串的长度

    (s, length)
}
```









Rust 标准库中有很多叫做 `Result` 的类型，很多标准库函数返回值类型是 `Result`的变体。`Result`是枚举类型，不同类型的数据实现了各自的`Result`。



`Result` 有两个成员 `Ok` 和 `Err` ：如果是`Ok` 则代表执行成功，可以从`Ok` 中取出返回值；如果是`Err`则表示执行失败，可以从 `Err`中取出错误信息。

`Result` 类型的作用是编码错误处理信息。`Result` 类型的值，像其他类型一样，拥有定义于其上的方法。比如

`io::Result<usize>` 定义了一些方法：

- `expect`方法

    如果`Result`的值是 `Err`，则执行该方法会中断当前程序，并输出传入的参数。而如果值为 `Ok` `expect`就会提取`Ok`中的值返回。









## 关联函数

关联函数是针对类型实现的，他不属于特定实例，一些语言中把它称为 **静态方法**（*static method*）。



# 宏

最后一个字母为 `!`



## 常用的宏

### `println!`

输出字符串到控制台。

可使用占位符`{}`。可以有多个，将会按顺序填充。

```rust
let lang = "Rust";
print!("{} is Best", lang);
```









