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
  - `curl https://sh.rustup.rs -sSf | sh`

> 如果是WIndwos的子系统WSL
>
> ```
>  curl –proto '=https' –tlsv1.2 -sSf https://sh.rustup.rs | sh
> ```

### MacOs

- 同样使用sh脚本
  - `curl https://sh.rustup.rs -sSf | sh`

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

```shell
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

> [*TOML*](https://github.com/toml-lang/toml) (*Tom's Obvious, Minimal Language*) 格式
>
> - 片段（section）
>
>     标识其下的语句是用来配置什么的。
>
>   - [package]
>
>         配置包
>
>   - [dependencies]
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

# 包、库与模块

Rust 有许多功能可以让你管理代码的组织，包括哪些内容可以被公开，哪些内容作为私有部分，以及程序每个作用域中的名字。这些功能。这有时被称为 “模块系统（the module system）

- **包**（*Packages*）： Cargo 的一个功能，它允许你构建、测试和分享 crate。
- **Crates** ：一个模块的树形结构，它形成了库或二进制项目。
- **模块**（*Modules*）和 **use**： 允许你控制作用域和路径的私有性。
- **路径**（*path*）：一个命名例如结构体、函数或模块等项的方式

Rust把代码所需要的库叫做 依赖（dependencies）。

## 包与crate

在 Rust 中，代码包被称为 *crates*。crate 是一个二进制项或者库。

*包*（*package*） 是提供一系列功能的一个或者多个 crate。一个包会包含有一个 *Cargo.toml* 文件，阐述如何去构建这些 crate。

包中所包含的内容由几条规则来确立。

- 一个包中至多 **只能** 包含一个库 crate(library crate)；
- 包中可以包含任意多个二进制 crate(binary crate)；
- 包中至少包含一个 crate，无论是库的还是二进制的。

*crate root* 是一个源文件，Rust 编译器以它为起始点，并构成 crate 的根模块

当输入命令 `cargo new` 时，Cargo 会给我们的包创建一个 *Cargo.toml* 文件，同时创建了 *src/main.rs* 源文件。 Cargo 遵循一个约定：*src/main.rs* 就是一个与包同名的二进制 crate 的 crate 根。同样的，如果包目录中包含 *src/lib.rs*，则包带有与其同名的库 crate，且 *src/lib.rs* 是 crate 根。crate 根文件将由 Cargo 传递给 `rustc` 来实际构建库或者二进制项目。

如果一个包只包含 *src/main.rs* ，意味着它将只含有一个名为 `my-project` 的二进制 crate。如果一个包同时含有 *src/main.rs* 和 *src/lib.rs*，则它有两个 crate：一个库和一个二进制项，且名字都与包相同。通过将文件放在 *src/bin* 目录下，一个包可以拥有多个二进制 crate：每个 *src/bin* 下的文件都会被编译成一个独立的二进制 crate。

一个 crate 会将一个作用域内的相关功能分组到一起，使得该功能可以很方便地在多个项目之间共享。如`rand` crate 提供了生成随机数的功能，通过将 `rand` crate 加入到我们项目的作用域中，我们就可以在自己的项目中使用该功能。`rand` crate 提供的所有功能都可以通过该 crate 的名字：`rand` 进行访问。

## 模块

*模块* 可以将一个 crate 中的代码进行分组，以提高可读性与重用性。模块还可以控制项的 *私有性*，即项是可以被外部代码使用的（*public*），还是作为一个内部实现的内容，不能被外部代码使用（*private*）。

### 定义

一个模块，是以 `mod` 关键字为起始，然后指定模块的名字，并且用花括号包围模块的主体。模块内可以保存一些定义的其他项，比如结构体、枚举、常量、特性、或者函数。

```rust
mod front_of_house{
    mod hosting{
        // 
        fn add_to_waitlist(){}
        // 
        fn seat_at_table(){}
    }
    mod serving{
        // 
        fn take_order(){}
        //
        fn server_order(){}
        //
        fn take_payment(){}
    }
}
```

在模块内，我们还可以定义其他的模块

通过使用模块，我们可以将相关的定义分组到一起，并指出他们为什么相关。

```shell
crate
 └── front_of_house
     ├── hosting
     │   ├── add_to_waitlist
     │   └── seat_at_table
     └── serving
         ├── take_order
         ├── serve_order
         └── take_payment
```

`src/main.rs` 和 `src/lib.rs` 叫做 crate 根，是因为这两个文件的内容都分别在 crate 模块结构的根组成了一个名为 `crate` 的模块，该结构被称为 *模块树*（*module tree*）。

在模块树中，把定义在同一模块中的一些模块称为互为 *兄弟*（*siblings*） ，而对于包含在模块 B 中的模块 A ，称模块 A 为模块 B 的 *子*（*child*），模块 B 则是模块 A 的 *父*（*parent*）。

整个模块树都植根于名为 `crate` 的隐式模块下。

### 可见性

模块不仅对于组织代码很有用。它们还定义了 Rust 的 *私有性边界*（*privacy boundary*），这条界线不允许外部代码了解、调用和依赖被封装的实现细节。如果希望创建一个私有函数或结构体，你可以将其放入模块。

Rust 中默认所有项（函数、方法、结构体、枚举、模块和常量）都是私有的。父模块中的项不能使用子模块中的私有项，但是子模块中的项可以使用他们父模块中的项。这是因为子模块封装并隐藏了他们的实现详情，但是子模块可以看到他们定义的上下文。

可以通过使用 `pub` 关键字来创建公共项，使子模块的内部部分暴露给上级模块。

```rust
mod front_of_house {
    pub mod hosting {
        fn add_to_waitlist() {}
    }
}
```

需要注意的是，对于公开的子模块，也只能访问它的公开项。如，在 `mod hosting` 前添加了 `pub` 关键字，使其变成公有的。如果我们可以访问 `front_of_house`，那我们也可以访问 `hosting`。但是 `hosting` 的 *内容*（*contents*） 仍然是私有的；这表明使模块公有并不使其内容也是公有的。要想访问`hosting`模块中的`add_to_waitlist`，也需要为其加上 `pub` 关键字。

对于定义在同一父模块中的兄弟模块相互之间是可见，即使没有用 `pub` 修饰。

可以使用 `pub` 来设计公有的结构体和枚举。但要注意，如果在一个结构体定义的前面使用了 `pub` ，这个结构体会变成公有的，但是这个结构体的字段仍然是私有的。可以根据情况决定每个字段是否公有。

```rust
#![allow(unused)]
fn main() {
    mod back_of_house {
        // 公开结构体
        pub struct Breakfast {
            pub toast: String,            // 公开字段
            seasonal_fruit: String,        // 私有字段
        }

        impl Breakfast {
            pub fn summer(toast: &str) -> Breakfast {
                Breakfast {
                    toast: String::from(toast),
                    seasonal_fruit: String::from("peaches"), // 兄弟可见
                }
            }
        }
    }

    pub fn eat_at_restaurant() {
        // Order a breakfast in the summer with Rye toast
        let mut meal = back_of_house::Breakfast::summer("Rye");
        // Change our mind about what bread we'd like
        meal.toast = String::from("Wheat");
        println!("I'd like {} toast please", meal.toast);

        // 下一行代码会造成编译错误。
        // meal.seasonal_fruit = String::from("blueberries");
    }
}
```

不能在 `eat_at_restaurant` 中使用 `seasonal_fruit` 字段，因为 `seasonal_fruit` 是私有的。

为 `back_of_house::Breakfast` 具有私有字段，所以这个结构体需要提供一个公共的关联函数来构造 `Breakfast` 的实例(这里我们命名为 `summer`)。如果 `Breakfast` 没有这样的函数，我们将无法在 `eat_at_restaurant` 中创建 `Breakfast` 实例，因为我们不能在 `eat_at_restaurant` 中设置私有字段 `seasonal_fruit` 的值。

与结构体不同，如果将枚举设为公有，则它的所有成员都将变为公有。

```rust
#![allow(unused)]
fn main() {
    mod back_of_house {
        pub enum Appetizer {
            Soup,
            Salad,
        }
    }

    pub fn eat_at_restaurant() {
        let order1 = back_of_house::Appetizer::Soup;
        let order2 = back_of_house::Appetizer::Salad;
    }
}
```

### 模块分割

当模块变得更大时，需要将它们的定义移动到单独的文件中，从而使代码更容易阅读。

```rust
// src/lib.rs
mod front_of_house;

pub use crate::front_of_house::hosting;

pub fn eat_at_restaurant() {
    hosting::add_to_waitlist();
    hosting::add_to_waitlist();
    hosting::add_to_waitlist();
}
```

将 `front_of_house` 模块移动到属于它自己的文件 *src/front_of_house.rs* 中，通过改变 crate 根文件，使其导入 `front_of_house` 模块。在 `mod front_of_house` 后使用分号，而不是代码块，这将告诉 Rust 在另一个与模块同名的文件中加载模块的内容。

*src/front_of_house.rs* 会获取 `front_of_house` 模块的定义内容。

```rust
//  src/front_of_house.rs
pub mod hosting {
    pub fn add_to_waitlist() {}
}
```

进一步拆解，还可以将`hosting` 放置在单独的文件中。

把 *src/front_of_house.rs* 修改为

```rust
// src/front_of_house.rs
pub mod hosting;
```

然后创建一个 *src/front_of_house* 目录和一个包含 `hosting` 模块定义的 *src/front_of_house/hosting.rs* 文件

```rust
// src/front_of_house/hosting.rs
pub fn add_to_waitlist() {}
```

## 路径

使用路径的方式，Rust 可以在模块树中找到一个项的位置。如果我们想要调用一个函数，我们需要知道它的路径。

像在文件系统使用路径一样，路径有两种形式：

- **绝对路径**（*absolute path*）从 crate 根开始，以 crate 名或者字面值 `crate` 开头。
- **相对路径**（*relative path*）从当前模块开始，以 `self`、`super` 或当前模块的标识符开头。

绝对路径和相对路径都后跟一个或多个由双冒号（`::`）分割的标识符。

```rust
mod front_of_house {
    pub mod hosting {
        pub fn add_to_waitlist() {}
    }
}

pub fn eat_at_restaurant() {
    // 绝对路径
    crate::front_of_house::hosting::add_to_waitlist();

    // 相对路径
    front_of_house::hosting::add_to_waitlist();
}
```

还可以使用 `super` 开头来构建从父模块开始的相对路径。这么做类似于文件系统中以 `..` 开头的语法。

```rust
fn serve_order() {}

mod back_of_house {
    fn fix_incorrect_order() {
        cook_order();
        super::serve_order();
    }

    fn cook_order() {}
}
```

### 导入路径

可以使用 `use` 关键字将路径一次性引入作用域，然后调用该路径中的项，就如同它们是本地项一样。

```rust
mod front_of_house {
    pub mod hosting {
        pub fn add_to_waitlist() {}
    }
}

use crate::front_of_house::hosting;

pub fn eat_at_restaurant() {
    hosting::add_to_waitlist();
    hosting::add_to_waitlist();
    hosting::add_to_waitlist();
}
```

将 `crate::front_of_house::hosting` 模块引入了 `eat_at_restaurant` 函数的作用域，只需要指定 `hosting::add_to_waitlist` 即可在 `eat_at_restaurant` 中调用 `add_to_waitlist` 函数。

在作用域中增加 `use` 和路径类似于在文件系统中创建软连接（符号连接，symbolic link）。通过在 crate 根增加 `use crate::front_of_house::hosting`，现在 `hosting` 在作用域中就是有效的名称了，如同 `hosting` 模块被定义于 crate 根一样。通过 `use` 引入作用域的路径也会检查私有性，同其它路径一样。

当然也可以使用 `use` 和相对路径来将一个项引入作用域。

```rust
mod front_of_house {
    pub mod hosting {
        pub fn add_to_waitlist() {}
    }
}

use front_of_house::hosting;

pub fn eat_at_restaurant() {
    hosting::add_to_waitlist();
    hosting::add_to_waitlist();
    hosting::add_to_waitlist();
}
```

使用`use`引入模块时，停止在父模块可以区分这两个不同模块内的同名类型。同时，Rust对于使用 `use` 将两个同名类型引入同一作用域这个问题还有另一个解决办法：在这个类型的路径后面，使用 `as` 指定一个新的本地名称或者别名。

```rust
#![allow(unused)]
fn main() {
    use std::fmt::Result;
    use std::io::Result as IoResult;

    fn function1() -> Result {
        // --snip--
        Ok(())
    }

    fn function2() -> IoResult<()> {
        // --snip--
        Ok(())
    }
}
```

### 重导出名称

当使用 `use` 关键字将名称导入作用域时，在新作用域中可用的名称是私有的。如果为了让调用你编写的代码的代码能够像在自己的作用域内引用这些类型，可以结合 `pub` 和 `use`。这个技术被称为 “*重导出*（*re-exporting*）”，因为这样做将项引入作用域并同时使其可供其他代码引入自己的作用域。

```rust
mod front_of_house {
    pub mod hosting {
        pub fn add_to_waitlist() {}
    }
}

pub use crate::front_of_house::hosting;

pub fn eat_at_restaurant() {
    hosting::add_to_waitlist();
    hosting::add_to_waitlist();
    hosting::add_to_waitlist();
}
```

通过 `pub use`，外部代码现在可以通过新路径 `hosting::add_to_waitlist` 来调用 `add_to_waitlist` 函数。如果没有指定 `pub use`，`eat_at_restaurant` 函数可以在其作用域中调用 `hosting::add_to_waitlist`，但外部代码则不允许使用这个新路径。

### 嵌套路径

当需要引入很多定义于相同包或相同模块的项时，为每一项单独列出一行会占用源码很大的空间。使用嵌套路径将相同的项在一行中引入作用域。这么做需要指定路径的相同部分，接着是两个冒号，接着是大括号中的各自不同的路径部分。如，对于如下代码

```rust
use std::cmp::Ordering;
use std::io;
```

可以简化为

```rust
use std::{cmp::Ordering, io};
```

可以在路径的任何层级使用嵌套路径，这在组合两个共享子路径的 `use` 语句时非常有用。

```rust
use std::io;
use std::io::Write;
```

两个路径的相同部分是 `std::io`，这正是第一个路径。为了在一行 `use` 语句中引入这两个路径，可以在嵌套路径中使用 `self`

```rust
use std::io::{self, Write};
```

### 全部引入

如果希望将一个路径下 **所有** 公有项引入作用域，可以指定路径后跟 `*`，glob 运算符：

```rust
use std::collections::*;
```

glob 运算符经常用于测试模块 `tests` 中，这时会将所有内容引入作用域；

glob 运算符有时也用于 prelude 模式；

### 使用外部包

为了在项目中使用外部包，需要配置`Cargo.toml`文件，添加要依赖的包的名称和版本号。

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

接着，为了将 `rand` 定义引入项目包的作用域，我们加入一行 `use` 起始的包名，它以 `rand` 包名开头并列出了需要引入作用域的项。

## 使用

默认情况下，Rust 将 [*prelude*](https://doc.rust-lang.org/std/prelude/index.html) 模块中少量的类型引入到每个程序的作用域中。如果需要的类型不在 prelude 中，你必须使用 `use` 语句显式地将其引入作用域。

```rust
use rand::Rng;

fn main() {
    let secret_number = rand::thread_rng().gen_range(1, 101);
}
```

冒号（`::`）是运算符，允许将特定的函数置于类型的命名空间（namespace）下。

> 注意标准库（`std`）对于包来说也是外部 crate。因为标准库随 Rust 语言一同分发，无需修改 *Cargo.toml* 来引入 `std`，不过需要通过 `use` 将标准库中定义的项引入项目包的作用域中来引用它们
>
> ```rust
> use std::collections::HashMap;
> ```
>
> 一个以标准库 crate 名 `std` 开头的绝对路径。

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
{                         // s 在这里无效, 它尚未声明
    let s = String::from("hello"); 
       // 使用 s
}            // 此作用域已结束，s 不再有效
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

![String in memory](Rust_img/trpl04-01.svg)

当我们将 `s1` 赋值给 `s2`，`String` 的数据被复制了，这意味着我们从栈上拷贝了它的指针、长度和容量。我们并没有复制指针指向的堆上数据。

![s1 and s2 pointing to the same value](Rust_img/trpl04-02.svg)

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

![&String s pointing at String s1](Rust_img/trpl04-05.svg)

将获取引用作为函数参数称为 **借用**（*borrowing*）。

正如变量默认是不可变的，引用也一样。（默认）不允许修改引用的值。如果想要可变的引用，就必须显式声明：被引用的变量一定是可变的，即必须将 `s` 改为 `mut`。然后必须创建一个可变引用 `&mut s` 和接受一个可变引用 `some_string: &mut String`。

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

这里对原数据的引用（`r1`,`r2`）的最后一次使用时在第7行的`println!`，那么在此之前的`r3`可变引用改变了原数据，这在rust中是不允许的，因为违背了引用的不可变性原则。

但是，多个不可变引用是可以共存使用的，并且只要在引用的原数据被改变之前使用完毕也是可以的。

准确来说， 一个引用的作用域从声明的地方开始一直持续到最后一次使用为止。所以，如果最后一次使用不可变引用在声明可变引用之前，是被允许的。

```rust
{
    let mut s = String::from("hello");

    let r1 = &s; // 没问题
    let r2 = &s; // 没问题
    println!("{} and {}", r1, r2);
    // 此位置之后 r1 和 r2 不再使用，所以下列改变了s的使用是可以的

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

    &s  // 错误
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
let z = '好';
let heart_eyed_cat = '😻';
```

Rust 的 `char` 类型的大小为32位四个字节(four bytes)，并代表了一个 Unicode 标量值（Unicode Scalar Value）。

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

## 复合类型

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

> 需要注意的是，数组的长度也是数组类型的一部分。
>
> 比如 对于函数
>
> ```rust
> printArry(a:[int;5]);
> ```
>
> 就不能传入其他长度的数组。

#### 访问

索引从0开始

```rust
let a = [1, 2, 3, 4, 5];
let first = a[0];
let second = a[1];
```

> 对于越界访问，编译并不会产生任何错误，不过程序会出现一个 **运行时**（*runtime*）错误并且不会成功退出，当尝试用索引访问一个元素时，Rust 会检查指定的索引是否小于数组的长度。如果索引超出了数组长度，Rust 会 *panic*。

## 枚举

 **枚举**（*enumerations*），也被称作 *enums*

枚举类型持有固定集合的值，这些值被称为枚举的 **成员**（*variants*）。

### 定义与使用

```rust
enum IpAddrKind {
    V4,
    V6,
}
```

创建 `IpAddrKind` 两个不同成员的实例：

```rust
#![allow(unused)]
fn main() {
    enum IpAddrKind {
        V4,
        V6,
    }

    let four = IpAddrKind::V4;
    let six = IpAddrKind::V6;
}
```

枚举的成员位于其标识符的命名空间中，并使用两个冒号分开。`IpAddrKind::V4` 和 `IpAddrKind::V6` 都是 `IpAddrKind` 类型的。

### 关联数据

Rust的枚举可以将数据直接放进每一个枚举成员，其需要在定义时指明可以存放的数据类型

```rust
enum IpAddr {
    V4(String),
    V6(String),
}
```

这样就可以直接使用

```rust
let home = IpAddr::V4(String::from("127.0.0.1"));
let loopback = IpAddr::V6(String::from("::1"));
```

可以将任意类型的数据放入枚举成员中：例如字符串、数字类型或者结构体，甚至可以包含另一个枚举。

枚举成员关联数据并不是整体性的，可以只为部分成员关联数据

```rust
enum Message {
    Quit,                        // 没有关联任何数据。
    Move { x: i32, y: i32 },      // 包含一个匿名结构体。
    Write(String),                 // 包含单独一个 String。
    ChangeColor(i32, i32, i32),      // 包含三个 i32。
}
```

### 方法

结构体和枚举还有另一个相似点：就像可以使用 `impl` 来为结构体定义方法那样，也可以在枚举上定义方法。

```rust
impl Message {
    fn call(&self) {
        // 在这里定义方法体
    }
}

let m = Message::Write(String::from("hello"));
m.call();
```

### 函数参数

定义一个函数来获取任何 `IpAddrKind`

```rust
fn route(ip_type: IpAddrKind) { }
```

### 标准库中的枚举

#### `IpAddr`

```rust
struct Ipv4Addr {
    // --snip--
}

struct Ipv6Addr {
    // --snip--
}

enum IpAddr {
    V4(Ipv4Addr),
    V6(Ipv6Addr),
}
```

#### `Option`

`Option` 是标准库定义的一个枚举。`Option` 类型应用广泛因为它编码了一个非常普遍的场景，即一个值要么有值要么没值。

Rust 并没有很多其他语言中有的空值功能。**空值**（*Null* ）是一个值，它代表没有值。在有空值的语言中，变量总是这两种状态之一：空值和非空值。

Rust 并没有空值，不过它确实拥有一个可以编码存在或不存在概念的枚举。这个枚举是 `Option<T>`，定义于标准库中。

```rust
enum Option<T> {
    Some(T),
    None,
}
```

`<T>` 意味着 `Option` 枚举的 `Some` 成员可以包含任意类型的数据。

`Option<T>`是如此有用以至于它甚至被包含在了 prelude 之中，即不需要将其显式引入作用域，它的成员也是如此，可以不需要 `Option::` 前缀来直接使用 `Some` 和 `None`。

```rust
let some_number = Some(5);
let some_string = Some("a string");

let absent_number: Option<i32> = None;
```

当有一个 `Some` 值时，我们就知道存在一个值，而这个值保存在 `Some` 中。当有个 `None` 值时，在某种意义上，它跟空值具有相同的意义：并没有一个有效的值。

如果直接使用 `None` 而不是 `Some`，需要告诉 Rust `Option<T>` 是什么类型的，因为编译器只通过 `None` 值无法推断出 `Some` 成员保存的值的类型。

> `Option<T>` 为什么就比空值要好？
>
> 因为 `Option<T>` 和 `T`（这里 `T` 可以是任何类型）是不同的类型，编译器不允许像一个肯定有效的值那样使用 `Option<T>`。
>
> ```rust
> let x: i8 = 5;
> let y: Option<i8> = Some(5);
> 
> let sum = x + y;        // 错误，Option<i8> 与 i8 是不同的类型，不能直接相加
> ```
>
> 当在 Rust 中拥有一个像 `i8` 这样类型的值时，编译器确保它总是有一个有效的值。我们可以自信使用而无需做空值检查。
>
> 只有当使用 `Option<i8>`（或者任何用到的类型）的时候需要担心可能没有值，而编译器会确保我们在使用值之前处理了为空的情况。
>
> 即在对 `Option<T>` 进行 `T` 的运算之前必须将其转换为 `T`。通常这能帮助我们捕获到空值最常见的问题之一：假设某值不为空但实际上为空的情况。

## 字符串

### 字符串字面值

字符串字面值是被硬编码进程序里的字符串值，使用 `"` 进行分割。

```rust
"123454321"
```

它们是不可变的。如果赋给变量，则类型为`&str` ，详见`Slice`类型

```rust
let x = "123454321";
```

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

slice 类型允许你**引用**集合中一段连续的元素序列，而不用引用整个集合。它是一个没有所有权的数据类型。

```rust
let s = String::from("hello world");

let hello = &s[0..5];
let world = &s[6..11];
```

上述引用由中括号中的 `[starting_index..ending_index]` 指定的 range 创建。其类似于引用整个 `String` 不过带有额外的 `[0..5]` 部分。它不是对整个 `String` 的引用，而是对部分 `String` 的引用。其中 `starting_index` 是 slice 的第一个位置，`ending_index` 则是 slice 最后一个位置的后一个值。

实际上，slice的存储是由指针和长度构成的。如下

![world containing a pointer to the 6th byte of String s and a length 5](Rust_img/trpl04-06.svg)

> `..` range 语法
>
> ```rust
> let sub_str = &x[0..2];
> ```
>
>
>
> 从第一个索引（0）开始，可以不写两个点号之前的值。表示从第一个开始
>
> ```rust
> let sub_str = &x[..2];
> ```
>
> 包含到的最后一个元素，也可以舍弃尾部的数字。
>
> ```rust
> let sub_str = &x[0..];
> ```
>
>
>
> 也可以同时舍弃这两个值来获取整个数组的 slice。
>
> > 对于字符串 slice range 的索引必须位于有效的 UTF-8 字符边界内，如果尝试从一个多字节字符的中间位置创建字符串 slice，则程序将会因错误而退出。
>
>
>
> 如果想要包含右边界值，使用等号
>
> ```rust
> let sub_str = &x[0..=2];
> ```
>
>

如果不使用 `String::from()` 方法在堆上新建一个字符串，而是直接赋值，那么将会把字符串分配在栈上，并使用slice对原字符串数据进行不可变引用。

事实上“字符串 slice” 的类型声明写作 `&str`，是一个不可变引用。在使用字符串字面值时，

```rust
let str1 = "Hello world"; //&str
```

实际是一个指向二进制程序特定位置的 slice。

也有其他类型的更通用的slice，对于数组

```rust
#![allow(unused)]
fn main() {
    let a = [1, 2, 3, 4, 5];
}
```

取其中一部分

```rust
let slice = &a[1..3];
```

这个 slice 的类型是 `&[i32]`。它跟字符串 slice 的工作方式一样，通过存储第一个集合元素的引用和一个集合总长度。也可以对其他所有集合使用这类 slice。

> 使用 `let x = s[1..3];` 获取到的是`str`类型，其与`String`、`&str`不同的是它是真正存储的字符串的数据，只有在运行时才能确定大小。

## struct结构体

和元组一样，结构体的每一部分可以是不同类型。但不同于元组，结构体需要命名各部分数据以便能清楚的表明其值的意义。有了这些名字，结构体比元组更灵活：不需要依赖顺序来指定或访问实例中的值。

### 基本结构体

#### 定义

定义结构体，需要使用 `struct` 关键字并为整个结构体提供一个名字。在大括号中，定义每一部分数据的名字和类型，我们称为 **字段**（*field*）。

```rust
struct User{
    username:String,
    email:String,
    sign_in_count:u36,
    active:bool,
}
```

#### 使用

##### 创建实例

创建一个实例需要以结构体的名字开头，在大括号中使用 `key: value` 键-值对的形式提供字段，其中 key 是字段的名字，value 是需要存储在字段中的数据值。

```rust
let user1 = User{
    email:String::from("example@email.com"),
    username:String::from("admin"),
    active:true,
    sign_in_count:1,
};
```

实例中字段的顺序不需要和它们在结构体中声明的顺序一致。换句话说，结构体的定义就像一个类型的通用模板，而实例则会在这个模板中放入特定数据来创建这个类型的值。

> `User` 结构体的定义中，使用了自身拥有所有权的 `String` 类型而不是 `&str` 字符串 slice 类型，所以只要整个结构体是有效的话其数据也是有效。
>
> 当然也可以使结构体存储被其他对象拥有的数据的引用，不过这么做的话需要用上 **生命周期**（*lifetimes*）。生命周期确保结构体引用的数据有效性跟结构体本身保持一致。如果你尝试在结构体中存储一个引用而不指定生命周期将是无效的。

需要注意同其他任何表达式一样，可以在函数体的最后一个表达式中构造一个结构体的新实例，来隐式地返回这个实例。

```rust
fn get_user(email:String, username:String)-> User{
    User{
        email:email,
        username:username,
        active:true,
        sign_in_count:1,
    }
}
```

对于函数参数与结构体字段相同的情况，Rust提供了更为简单的字段初始化简写语法**（*field init shorthand*）

```rust
fn build_user(email:Stirng, username:String)->User{
    User{
        email,
        username,
        active:true,
        sign_in_count:1,
    }
}
```

Rust也支持从其他实例更新部分数据后再创建实例，即**结构体更新语法**（*struct update syntax*）

```rust
#![allow(unused)]
fn main() {
    struct User {
        username: String,
        email: String,
        sign_in_count: u64,
        active: bool,
    }

    let user1 = User {
        email: String::from("someone@example.com"),
        username: String::from("someusername123"),
        active: true,
        sign_in_count: 1,
    };

    let user2 = User {
        email: String::from("another@example.com"),
        username: String::from("anotherusername567"),
        ..user1
    };
}

```

对于结构体定义的位置，Rust比C/C++灵活，可以将结构体定义在某个语句块里（如函数）里，这样结构体的可见性和生命周期就只在这个语句块里了

```rust
fn main(){
    #[derive(Debug)]
    struct Rect{
        width:u32,
        height:u32,
    }
    let x = Rect{width:100,height:300};
    println!("{:#?}",x);
}
```

##### 使用实例

从结构体中获取某个特定的值，可以使用点号。

```rust
let mut user2 = User{
    email: String::from("someone@example.com"),
    username: String::from("someusername123"),
    active: true,
    sign_in_count: 1,
};

user2.email =  String::from("new@email.com");
```

Rust 并不允许只将某个字段标记为可变。如何想要盖面某一个字段的值就需要将整个实例声明为可变的。

#### 方法

**方法** 与函数类似：它们使用 `fn` 关键字和名称声明，可以拥有参数和返回值，同时包含在某处调用该方法时会执行的代码。不过方法与函数是不同的，因为它们在结构体的上下文中被定义（或者是枚举或 trait 对象的上下文），并且它们第一个参数总是 `self`，它代表调用该方法的结构体实例。

```rust
#[derive(Debug)]
struct Rectangle{
    width:u32,
    height:u32,
}

impl Rectangle{
    fn area(&self) -> u32{
        self.width * self.height
    }
}
```

为了使函数定义于结构体 的上下文中，使用 `impl` 块（`impl` 是 *implementation* 的缩写）。与结构体相关的方法就定义在其中。

`self` 指代调用实例自身，方法可以选择获取 `self` 的所有权，可以不可变地借用 `self`，或者可变地借用 `self`，就跟其他参数一样。如果想要改变调用方法的实例，需要将第一个参数改为 `&mut self`。

使用时直接用实例进行使用`.`调用

```rust
let rect1 = Rectangle { width: 30, height: 50 };
let area = rect1.area();
```

> Rust 并没有一个与C/C++中 `->` 等效的运算符；相反，Rust 有一个叫 **自动引用和解引用**（*automatic referencing and dereferencing*）的功能。方法调用是 Rust 中少数几个拥有这种行为的地方。
>
> 当使用 `object.something()` 调用方法时，Rust 会自动为 `object` 添加 `&`、`&mut` 或 `*` 以便使 `object` 与方法签名匹配。下面代码是等价：
>
> ```rust
> p1.distance(&p2);
> (&p1).distance(&p2);
> ```
>
> 第一行看起来简洁的多。这种自动引用的行为之所以有效，是因为方法有一个明确的接收者———— `self` 的类型。
>
> 在给出接收者和方法名的前提下，Rust 可以明确地计算出方法是仅仅读取（`&self`），做出修改（`&mut self`）或者是获取所有权（`self`）。事实上，Rust 对方法接收者的隐式借用让所有权在实践中更友好。

#### 关联函数

`impl` 块的另一个有用的功能是：允许在 `impl` 块中定义 **不** 以 `self` 作为参数的函数。这被称为 **关联函数**（*associated functions*），因为它们与结构体相关联。它们仍是函数而不是方法，因为它们并不作用于一个结构体的实例。

```rust
#![allow(unused)]
fn main() {
    #[derive(Debug)]
    struct Rectangle {
        width: u32,
        height: u32,
    }

    impl Rectangle {
        // 返回一个正方形
        fn square(size: u32) -> Rectangle {
            Rectangle { width: size, height: size }
        }
    }

}
```

`::` 语法用于关联函数和模块创建的命名空间,调用时使用`::` 语法来调用这个关联函数，这个方法位于结构体的命名空间中：

```rust
let sq = Rectangle::square(3);
```

关联函数经常被用作返回一个结构体新实例的构造函数。

> 每个结构体都允许拥有多个 `impl` 块。一般没有理由将这些方法分散在多个 `impl` 块中，不过这是有效的语法。

### 元组结构体

Rust可以定义与元组类似的结构体，称为 **元组结构体**（*tuple structs*）。其有着结构体名称提供的含义，但没有具体的字段名，只有字段的类型。

```rust
struct Color(i32, i32, i32);
struct Point(i32, i32, i32);

let black = Color(0, 0, 0);
let origin = Point(0, 0, 0);
```

`black` 和 `origin` 值的类型不同，因为它们是不同的元组结构体的实例，使有着相同的结构的元组成为不同的类型时，元组结构体是很有用的。

元组结构体实例类似于元组：可以将其解构为单独的部分，也可以使用 `.` 后跟索引来访问单独的值。

```rust
```

### 类单元结构体

可以定义一个没有任何字段的结构体！它们被称为 **类单元结构体**（*unit-like structs*）因为它们类似于 `()`，即 unit 类型。类单元结构体常常在你想要在某个类型上实现 trait 但不需要在类型中存储数据的时候发挥作用。

### 类单元结构体

可以定义一个没有任何字段的结构体，它们被称为 **类单元结构体**（*unit-like structs*）因为它们类似于 `()`，即 unit 类型。类单元结构体常常在你想要在某个类型上实现 trait 但不需要在类型中存储数据的时候发挥作用。

```rust
struct Single();
```

### 结构体数据的所有权

当结构体的字段拥有所有权时，只要整个结构体是有效的话其数据也是有效的。

也可以使结构体存储被其他对象拥有的数据的引用，需要用上 **生命周期**（*lifetimes*）加以限制。生命周期确保结构体引用的数据有效性跟结构体本身保持一致，如果尝试在结构体中存储一个引用而不指定生命周期将是无效的。

```rust
struct User {
    username: &str,
    email: &str,
    sign_in_count: u64,
    active: bool,
}

fn main() {
    let user1 = User {
        // 以下两句将会报错
        email: "someone@example.com",
        username: "someusername123",
        active: true,
        sign_in_count: 1,
    };
}
```

## 常见集合

Rust 标准库中包含一系列被称为 **集合**（*collections*）的非常有用的数据结构。大部分其他数据类型都代表一个特定的值，不过集合可以包含多个值。

不同于内建的数组和元组类型，这些集合指向的数据是储存在堆上的，这意味着数据的数量不必在编译时就已知，并且还可以随着程序的运行增长或缩小。

### vector

vector 允许我们在一个单独的数据结构中储存多于一个的相同类型的值，它在内存中彼此相邻地排列所有的值。

 Rust中vector 是用泛型实现的，对应类型是`Vec<T>`，被称为 *vector*

#### 新建

使用 `new`关联函数

```rust
let v:Vec<i32> = Vec::new();
```

注意如果仅仅定义，那这里需要增加了一个类型注解。因为本语句即后文都没有向这个 vector 中插入任何值，Rust 并不知道储存什么类型的元素。当然上一句没有任何用途，因为其不可变。

在更实际的代码中，一旦插入值 Rust 就可以推断出想要存放的类型。更常见的做法是使用初始值来创建一个 `Vec`，因此为了方便 Rust 提供了 `vec!` 宏。

```rust
let v = vec![1, 2, 3];
```

默认情况下，编译器会把整数推断成`i32` 类型

Rust 在编译时就必须准确的知道 vector 中类型的原因在于它需要知道储存每个元素到底需要多少内存，并可以准确的知道这个 vector 中允许什么类型。

对于 vector 储存布同类型的值的情况，可以定义一个枚举，以便能在 vector 中存放不同类型的数据

```rust
enum SpreadsheetCell {
    Int(i32),
    Float(f64),
    Text(String),
}

let row = vec![
    SpreadsheetCell::Int(3),
    SpreadsheetCell::Text(String::from("blue")),
    SpreadsheetCell::Float(10.12),
];
```

#### 更新

对于可变的vector，使用 `push` 方法添加元素

```rust
let mut v = Vec::new();
v.push(5);
v.push(6);
```

放入其中的所有值都是 `i32` 类型的， Rust 就能根据数据做出如此判断，所以不需要 `Vec<i32>` 注解。

使用 `pop` 方法移除并返回 vector 的最后一个元素。

类似于任何其他的 `struct`，vector 在其离开作用域时会被释放。当 vector 被丢弃时，所有其内容也会被丢弃，这意味着这里它包含的整数将被清理。

#### 读取

有两种方法引用 vector 中储存的值。

```rust
let v = vec![1, 2, 3, 4, 5];

// 使用索引获取元素
let third: &i32 = &v[2];
println!("The third element is {}", third);

// 使用get方法
match v.get(2) {
    Some(third) => println!("The third element is {}", third),
    None => println!("There is no third element."),
}
```

使用 `&` 和 `[]` 返回一个引用，当引用一个不存在的元素时 Rust 会造成 panic。

使用 `get` 方法以索引作为参数来返回一个 `Option<&T>`，接着你的代码可以有处理 `Some(&element)` 或 `None` 的逻辑

一旦程序获取了一个有效的引用，借用检查器将会执行所有权和借用规则（第四章讲到）来确保 vector 内容的这个引用和任何其他引用保持有效。vector的所有权是对于其所有元素而言的，例如，当我们获取了 vector 的第一个元素的不可变引用并尝试在 vector 末尾增加一个元素的时候，是行不通的。这是因为vector 的工作方式：在 vector 的结尾增加新元素时，在没有足够空间将所有所有元素依次相邻存放的情况下，可能会要求分配新内存并将老的元素拷贝到新的空间中。这时，第一个元素的引用就指向了被释放的内存。借用规则阻止程序陷入这种状况。

对于想要依次访问 vector 中的每一个元素的情况，可以使用 `for` 循环进行遍历。

```rust
let v = vec![100, 32, 57];
// 获取每一个元素的不可变引用
// 不用引用的话所有权就转移了，遍历完之后vec就不能用了
for i in &v {
    println!("{}", i);
}
```

当然也可以遍历可变变量进行操作

```rust
let mut v = vec![100, 32, 57];
for i in &mut v {
    *i += 50;
}
```

为了修改可变引用所指向的值，在使用 `+=` 运算符之前必须使用解引用运算符（`*`）获取 `i` 中的值。

### 字符串

Rust 的**核心**语言中只有一种字符串类型：`str`，字符串 slice（是一些储存在别处的 UTF-8 编码字符串数据的引用），它通常以被借用的形式出现，即`&str`。

而称作 `String` 的类型是由**标准库**提供的，而没有写进核心语言部分，它是可增长的、可变的、有所有权的、UTF-8 编码的字符串类型。

> `String` 和字符串 slice 都是 UTF-8 编码的。

Rust 标准库中还包含一系列其他字符串类型，比如 `OsString`、`OsStr`、`CString` 和 `CStr`。相关库 crate 甚至会提供更多储存字符串数据的选择。这些 以`String` 或是 `Str` 结尾的名字对应着它们提供的所有权和可借用的字符串变体，这些字符串类型能够以不同的编码，或者内存表现形式上以不同的形式，来存储文本内容。

#### 字符串字面值

字符串字面值是被硬编码进程序里的字符串值，使用 `"` 进行分割。

```rust
"123454321"
```

它们是不可变的。在rust中如果将它们赋值给变量，变量的类型将是`&str`

```rust
let x:&str = "1234321";
```

#### 字符串类型

字符串类型是 `String` 由标准库提供，被分配到堆上，能够存储在编译时未知大小的文本。内部使用utf-8格式的编码。

很多 `Vec` 可用的操作在 `String` 中同样可用，一个新的空字符串由**关联函数**（*associated function*）`new`创建。其将新建了一个空的 `String`。

```rust
let mut s = String::new();
```

可以使用 `to_string` 方法获取新的 `String`，它能用于任何实现了 `Display` trait 的类型，字符串字面值也实现了它。

```rust
let data = "initial contents";

let s = data.to_string();

// 该方法也可直接用于字符串字面值：
let s = "initial contents".to_string();
```

也可以使用 `from`函数基于字符串字面量来创建。

```rust
let s = String::from("hello");
```

对于 `String` 类型，为了支持一个可变，可增长的文本片段，需要在堆上分配一块在编译时未知大小的内存来存放内容。这意味着：

- 必须在运行时向操作系统请求内存。
  
  由我们完成：当调用 `String::from` 时，它的实现 (*implementation*) 请求其所需的内存。

- 需要一个当我们处理完 `String` 时将内存返回给操作系统的方法。

##### 更新字符串

`String` 的大小可以增加，其内容也可以改变，就像可以放入更多数据来改变 `Vec` 的内容一样。

可以通过 `push_str` 方法来附加字符串 slice，从而使 `String` 变长，其采用字符串 slice，因为并不需要获取参数的所有权，而只是又新复制了一份数据。所以下列代码不会报错。

```rust
let mut s1 = String::from("foo");
let s2 = "bar";
s1.push_str(s2);
println!("s2 is {}", s2);
```

`push` 方法被定义为获取一个单独的字符作为参数，并附加到 `String` 中。

```rust
let mut s = String::from("lo");
s.push('l');
```

另外，可以方便的使用 `+` 运算符或 `format!` 宏来拼接 `String` 值。

```rust
let s1 = String::from("Hello, ");
let s2 = String::from("World!");
let s3 = s1 + &s2;        // s1 被移动了，不能继续使用
```

`+` 实际上调用的是`add`函数，会获取字符串的所有权。对于`s1`，参与运算后将不再拥有字符串的所有权，而`s2`使用引用也是因为`add`函数的签名要求。

```rust
fn add(self, s:&str)-> String{}
```

> 上述代码并不是标准库中实际的签名；标准库中的 `add` 使用泛型定义。
>
> 这里的 `add` 的签名使用具体类型`&str`代替了泛型，这也正是当使用 `String` 值调用这个方法会发生的。

这里`&s2`虽然是`&String`类型的，但是Rust中`&String` 可以被 **强转**（*coerced*）成 `&str`。因为 `add` 没有获取参数的所有权，所以 `s2` 在这个操作后仍然是有效的 `String`。这个语句会获取 `s1` 的所有权，附加上从 `s2` 中拷贝的内容，并返回结果的所有权。换句话说，它看起来好像生成了很多拷贝，不过实际上并没有：这个实现比拷贝要更高效。

对于连接多个字符串，`+` 的行为就显得笨重了，对于更为复杂的字符串链接，可以使用 `format!` 宏：

```rust
let s1 = String::from("tic");
let s2 = String::from("tac");
let s3 = String::from("toe");

let s = format!("{}-{}-{}",s1,s2,s3);
```

`format!` 与 `println!` 的工作原理相同，不过不同于将输出打印到屏幕上，它返回一个带有结果内容的 `String`，且不会获取任何参数的所有权。

##### 索引字符串

在很多语言中，通过索引来引用字符串中的单独字符是有效且常见的操作。然而在 Rust 中，如果你尝试使用索引语法访问 `String` 的一部分，会出现一个错误。

```rust
let s1 = String::from("hello");
let h = s1[0];        // 错误
```

`String` 是一个 `Vec<u8>` 的封装。对于`len()`方法，其实返回的是储存数据所需要的**字节数**。

```rust
let len = String::from("Здравствуйте").len();
```

`len`的值是24而不是12，这是使用 UTF-8 编码 “Здравствуйте” 所需要的字节数，这是因为每个 Unicode 标量值需要两个字节存储。因此一个字符串字节值的索引并不总是对应一个有效的 Unicode 标量值。当使用索引时用户通常不会想要一个字节值被返回，即便这个字符串只有拉丁字母： 即便 `&"hello"[0]` 是返回字节值的有效代码，它也应当返回 `104` 而不是 `h`。为了避免返回意外的值并造成不能立刻发现的 bug，Rust 根本不会编译这些代码，并在开发过程中及早杜绝了误会的发生。

Rust 提供了多种不同的方式来解释计算机储存的原始字符串数据，这样程序就可以选择它需要的表现方式，而无所谓是何种人类语言。

最后一个 Rust 不允许使用索引获取 `String` 字符的原因是，索引操作预期总是需要常数时间 (O(1))。但是对于 `String` 不可能保证这样的性能，因为 Rust 必须从开头到索引位置遍历来确定有多少有效的字符。

但是对于索引字符串 slice ，使用索引`[]`和一个 range 来获取数据是允许的，可是其返回的类型是不明确的，它只按照字节取值，例如可以使用

```rust
let hello = "Здравствуйте";

let s = &hello[0..4];
```

`s` 会是一个 `&str`，它包含字符串的头四个字节。由于这些字母都是两个字节长的，所以这意味着 `s` 将会是 “Зд”。

但如果获取 `&hello[0..1]` 会发生什么呢？答案是：Rust 在运行时会 panic，就跟访问 vector 中的无效索引时一样。

> 字节、标量、字符型
>
> 字节，是计算机最终储存的数据的单位，现代计算机一个字节一般是8位。对于ASCII编码下的英文字母，使用2个字节存储一个字母。但对于其他编码方式和其他语言，却不一定。

##### 遍历字符串

因为单独使用索引访问字符串的第几个字节是存在风险的，Rust提供了`chars`方法将字符串分成单独的 Unicode 标量值。

调用`chars`方法会返回对应的`char`类型(单个Unicode标量，占据四个字节)的数据，接着就可以遍历其结果来访问每一个元素了：

```rust
for c in "नमस्ते".chars() {
    println!("{}", c);
}
```

如果有需要直接操作字节的场景，可以使用 `bytes`方法返回每一个原始字节的值（u8类型）。

```rust
for b in "你好".bytes(){
    println!("{}", b);
}
// 228
// 189
// 160
// 229
// 165
// 189
```

##### 类型转换

 **Deref 强制转换**（*deref coercion*）

字符串的 `parse` 方法将字符串解析成数字，这个方法可以解析多种数字类型，因此需要告诉 Rust 具体的数字类型，使用Rust的自带的推断

```rust
let guess: u32  = guess.trim().parse().expect("Please type a number!");
```

如果 `parse` 不能从字符串生成一个数字，返回一个 `Result` 的 `Err` 成员时，`expect` 会使程序崩溃并打印附带的信息。如果 `parse` 成功地将字符串转换为一个数字，它会返回 `Result` 的 `Ok` 成员，然后 `expect` 会返回 `Ok` 值中的数字。

##### 方法

`as_bytes` 方法可以将 `String` 转化为字节数组，返回的是队员数组的引用。

```rust
let bytes = s.as_bytes(); // bytes是&[u8]类型的
```

`len`方法可以获取`String`类的长度，返回的是整数，其与原本的字符串是分离，也就是说，在之后改变字符串后使用整数也不会报错。

```rust
let l = s.len();
```

### 哈希map

`HashMap<K, V>`类型储存了一个键类型 `K` 对应一个值类型 `V` 的映射。它通过一个 **哈希函数**（*hashing function*）来实现映射，决定如何将键和值放入内存中。

> 很多编程语言支持这种数据结构，不过通常有不同的名字：哈希、map、对象、哈希表或者关联数组。

哈希 map 可以用于需要任何类型作为键来寻找数据的情况，而不是像 vector 那样通过索引。哈希 map 是同质的：所有的键必须是相同类型，值也必须都是相同类型。

要使用rust的哈希，必须先导入，使用 `use` 导入标准库中集合内的哈希数据结构

```rust
use std::collection::HashMap;
```

> 在这三个常用集合中，`HashMap` 是最不常用的，所以并没有被 prelude 自动引用。标准库中对 `HashMap` 的支持也相对较少，例如，并没有内建的构建宏。

像 vector 一样，哈希 map 将它们的数据储存在堆上。

#### 新建哈希

使用 `new` 创建一个空的 `HashMap`

```rust
#![allow(unused)]
fn main() {
    use std::collections::HashMap;

    let mut scores = HashMap::new();

    scores.insert(String::from("Blue"), 10);
    scores.insert(String::from("Yellow"), 50);
}
```

之后可使用 `insert` 插入元素。

另一个构建哈希 map 的方法是使用一个元组的 vector 的 `collect` 方法，其中每个元组包含一个键值对。`collect` 方法可以将数据收集进一系列的集合类型，包括 `HashMap`。

下面队伍的名字和初始分数分别在两个 vector 中，可以使用 `zip` 方法来创建一个元组的 vector，其中 “Blue” 与 10 是一对，依此类推。接着就可以使用 `collect` 方法将这个元组 vector 转换成一个 `HashMap`，

```rust
fn main(){
 use std::collection::HashMap;
 
    let teams = vec!["Blue".to_string(),"Yellow".to_string()];
    let initial_scores =vec![10,50];
    
    let score_table:HashMap<_, _> = teams.iter().zip(initial_scores.iter()).collect();
}
```

这里 `HashMap<_, _>` 类型注解是必要的，因为可能 `collect` 是模板方法，可以生成很多不同的数据结构，而除非显式指定否则 Rust 无从得知你需要的类型。但是对于键和值的类、型参数来说，可以使用下划线占位，而 Rust 能够根据 vector 中数据的类型推断出 `HashMap` 所包含的类型。

> 哈希 map 和所有权
>
> 对于像 `i32` 这样的实现了 `Copy` trait 的类型，其值可以拷贝进哈希 map。对于像 `String` 这样拥有所有权的值，其值将被移动而哈希 map 会成为这些值的所有者。

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

Rust 有一个叫做 `match` 的极为强大的控制流运算符，它允许我们将一个值与一系列的模式相比较，并根据相匹配的模式执行相应代码。

一个 `match` 表达式由 **分支（arms）** 构成。一个分支包含一个 **模式**（*pattern*）和表达式开头的值与分支模式相匹配时应该执行的代码。模式可由字面值、变量、通配符和许多其他内容构成；

Rust 获取提供给 `match` 的值并挨个检查每个分支的模式，并且在遇到第一个 “符合” 的模式时，值会进入相关联的代码块并在执行中被使用。

```rust
match guess.cmp(&secret_number) {
    Ordering::Less => println!("Too small!"),
    Ordering::Greater => println!("Too big!"),
    Ordering::Equal => println!("You win!"),
}
```

每个分支相关联的代码是一个表达式，而表达式的结果值将作为整个 `match` 表达式的返回值。

如果分支代码较短的话通常不使用大括号。如果想要在分支中运行多行代码，可以使用大括号。

匹配分支的另一个有用的功能是可以绑定匹配的模式的部分值。这也就是如何从枚举成员中提取值的。

```rust
#[derive(Debug)]
enum State {
    Frontground,
    Background,
}

enum Program {
    Beginning,
    Stop,
    Running(State),        // 运行时可能处于不同的状态
}

fn check_program(program: Program) -> u8 {
    match program {
        Program::Beginning => 1,
        Program::Stop => 10,
        Program::Running(state) => {        // 这里绑定了state变量
            println!("Running in {:?}!", state);
            20
        },
    }
}
```

在匹配 `Program::Running` 成员的分支的模式中增加了一个叫做 `state` 的变量。当匹配到 `Program::Running` 时，变量 `state` 将会绑定program中Running的值。接着在那个分支的代码中使用 `state`。

Rust 知道我们没有覆盖所有可能的情况甚至知道哪些模式被忘记了！Rust 中的匹配是 **穷尽的**（*exhaustive*）：必须穷举到最后的可能性来使代码有效。Rust 也提供了一个模式用于不想列举出所有可能值的场景。即`_`通配符。

`_` 模式会匹配所有的值。通过将其放置于其他分支之后，`_` 将会匹配所有之前没有指定的可能的值。

> `if let` 语法
>
> `if let` 语法让我们以一种不那么冗长的方式结合 `if` 和 `let`，来处理只匹配一个模式的值而忽略其他模式的情况。
>
> 对于下列情况
>
> ```rust
> let some_u8_value= Some(0u8);
> match some_u8_value{
>    Some(3) => println!("there"),
>    _ =>(),
> }
> ```
>
> `match` 只关心当值为 `Some(3)` 时执行代码，使用match就必须为了满足 `match` 表达式（穷尽性）的要求，即必须在处理完这唯一的成员后加上 `_ => ()`，而Rust提供了 `if let` 这种更短的方式。
>
> ```rust
> if let Some(3) = some_u8_value{
>    println!("there");
> }
> ```
>
> `if let` 获取通过等号分隔的一个模式和一个表达式。它的工作方式与 `match` 相同，这里的表达式对应 `match` 而模式则对应第一个分支。
>
> 可以认为 `if let` 是 `match` 的一个语法糖，它当值匹配某一模式时执行代码而忽略所有其他值，这编写更少代码，更少的缩进和更少的样板代码，但也会失去 `match` 强制要求的穷尽性检查。`match` 和 `if let` 之间的选择依赖特定的环境以及增加简洁度和失去穷尽性检查的权衡取舍。
>
> 可以在 `if let` 中包含一个 `else`。`else` 块中的代码与 `match` 表达式中的 `_` 分支块中的代码相同，这样的 `match` 表达式就等同于 `if let` 和 `else`。
>
> ```rust
> let mut count = 0;
> if let Coin::Quarter(state) = coin {
>    println!("State quarter from {:?}!", state);
> } else {
>    count += 1;
> }
> ```

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

# 派生

## trait

### 数据信息

对于简单的自定义类型。宏`println!`是无法像打印基本类型一样打印他们的数据信息的，原因是基本类型都默认实现了 `Display`。

`println!` 宏能处理很多类型的格式，不过，`{}` 默认告诉 `println!` 使用被称为 `Display` 的格式，它的完整定义是`std::fmt::Display`。

基本类型都默认实现了 `Display`，因为它就是向用户展示 `1` 或其他任何基本类型的唯一方式。

对于结构体，`println!` 应该用来输出的格式是不明确的，需要自主实现

```rust

```

### 调试信息

在 `{}` 中加入 `:?` 指示符告诉 `println!` 我们想要使用叫做 `Debug` 的输出格式。

`Debug` 是一个 trait，它允许我们以一种对开发者有帮助的方式打印结构体，以便当我们调试代码时能看到它的值。

可以选择自定义实现`Debug`trait，但Rust **确实** 包含了打印出调试信息的功能，只不过要想使用必须为显式选择这个功能。

```rust
#[derive(Debug)]
struct Rectangle {
    width: u32,
    height: u32,
}
fn main(){
    let rect1 = Rectangle{width:30,height:50};
    println!("{:?}", rect1);
}
```

上面代码使用增添注解的形式来派生 `Debug` trait，

默认情况下，其输出如下面格式，它在一行中显示这个实例的所有字段。

```textile
Rectangle {width:30, height:50}
```

如果想要格式化，则使用 `{:#?}`，它将输出如下

```rust
Rectangle {
    width: 30,
    height: 50
}
```

# 泛型

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

> 对于自定义类型需要实现 trait `std::fmt::Display`。

在 `{}` 中加入 `:?` 指示符告诉 `println!` 我们想要使用叫做 `Debug` 的输出格式。

`Debug` 是一个 trait，它允许我们以一种对开发者有帮助的方式打印结构体，以便当我们调试代码时能看到它的值。

> 对于自定义类型需要实现 trait `std::fmt::Debug`。
>
> 如果不想显式实现也可以使用注解  `#[derive(Debug)]` 来派生 `Debug` trait，这将显示所打印实例的所有字段。

```rust
#[dervice(Debug)]
struct Rect{
    width:u32,
    height:u32,
}

fn main(){
    let rect1 = Rect{width:32,heghit:50};
    println!("{:?}",rect1);
}
```

可以使用 `{:#?}` 替换的 `{:?}`，相较于`{:?}`其效果是在每个字段打印后进行换行。

### `format!`

`format!`宏和`println!`宏类似，其作用是借用变量，并返回格式化后的字符串。

```rust
let t1 = String::from("tic");
let t2 = String::from("tac");
let t3 = String::from("toe");

let t4 = format!("{}-{}-{}",t1, t2, t3);

// 之后原变量还能使用
println!("{}-{}-{}",t1, t2, t3);
```

# 智能指针

**指针** （*pointer*）是一个包含内存地址的变量的通用概念。这个地址引用，或 “指向”（points at）一些其他数据。Rust 中最常见的指针是第四章介绍的 **引用**（*reference*）。引用以 `&` 符号为标志并借用了他们所指向的值。除了引用数据没有任何其他特殊功能。它们也没有任何额外开销，所以应用得最多。

另一方面，**智能指针**（*smart pointers*）是一类数据结构，他们的表现类似指针，但是也拥有额外的元数据和功能。智能指针的概念并不为 Rust 所独有；其起源于 C++ 并存在于其他语言中。Rust 标准库中不同的智能指针提供了多于引用的额外功能。本章将会探索的一个例子便是 **引用计数** （*reference counting*）智能指针类型，其允许数据有多个所有者。引用计数智能指针记录总共有多少个所有者，并当没有任何所有者时负责清理数据。

在 Rust 中，普通引用和智能指针的一个额外的区别是引用是一类只借用数据的指针；相反，在大部分情况下，智能指针 **拥有** 他们指向的数据。

> Rust常见的自带智能指针类型有 `String`、`Vec<T>`

智能指针通常使用结构体实现。智能指针区别于常规结构体的显著特性在于其实现了 `Deref` 和 `Drop` trait。

- `Deref` trait 允许智能指针结构体实例表现的像引用一样，这样就可以编写既用于引用、又用于智能指针的代码。
- `Drop` trait 允许我们自定义当智能指针离开作用域时运行的代码。

标准库中最常用的一些：

- `Box<T>`，用于在堆上分配值
- `Rc<T>`，一个引用计数类型，其数据可以有多个所有者
- `Ref<T>` 和 `RefMut<T>`，通过 `RefCell<T>` 访问。（ `RefCell<T>` 是一个在运行时而不是在编译时执行借用规则的类型）。

**内部可变性**（*interior mutability*）模式，这是不可变类型暴露出改变其内部值的 API。我们也会讨论 **引用循环**（*reference cycles*）会如何泄漏内存，以及如何避免。

## `Box<T>`

最简单直接的智能指针是 *box*，其类型是 `Box<T>`。 box 允许你将一个值放在堆上而不是栈上。留在栈上的则是指向堆数据的指针。

除了数据被储存在堆上而不是栈上之外，box 没有性能损失。不过也没有很多额外的功能。它们多用于如下场景：

- 当有一个在编译时未知大小的类型，而又想要在需要确切大小的上下文中使用这个类型值的时候。
- 当有大量数据并希望在确保数据不被拷贝的情况下转移所有权的时候。转移大量数据的所有权可能会花费很长的时间，因为数据在栈上进行了拷贝。为了改善这种情况下的性能，可以通过 box 将这些数据储存在堆上。
- 当希望拥有一个值并只关心它的类型是否实现了特定 trait 而不是其具体类型的时候。只有少量的指针数据在栈上被拷贝。第三种情况被称为 **trait 对象**（*trait object*）

### `BOX<T>` 的使用

```rust
fn main(){
 let b = Box::new(5);
    println!("b = {}",b);
}
```

这里定义了变量 `b`，其值是一个指向被分配在堆上的 `i32`类型值 `5` 的 `Box`。

可以像数据是储存在栈上的那样访问 box 中的数据。正如任何拥有数据所有权的值那样，当像 `b` 这样的 box 在 `main` 的末尾离开作用域时，它将被释放。这个释放过程将作用于 box 本身（位于栈上）和它所指向的数据（位于堆上）。

### `Box` 创建递归类型

Rust 需要在编译时知道类型占用多少空间。一种无法在编译时知道大小的类型是 **递归类型**（*recursive type*），其值的一部分可以是相同类型的另一个值。这种值的嵌套理论上可以无限的进行下去，所以 Rust 不知道递归类型需要多少空间。不过 box 有一个已知的大小，所以通过在循环类型定义中插入 box，就可以创建递归类型了。

下面先以*cons list*，一个函数式编程语言中的常见类型，为例来展示这个（递归类型）概念。

*cons list* 是一个来源于 Lisp 编程语言及其方言的数据结构。在 Lisp 中，`cons` 函数（“construct function" 的缩写）利用两个参数来构造一个新的列表，他们通常是一个单独的值和另一个列表。

cons 函数的概念涉及到更常见的函数式编程术语；“将 *x* 与 *y* 连接” 通常意味着构建一个新的容器而将 *x* 的元素放在新容器的开头，其后则是容器 *y* 的元素。

cons list 的每一项都包含两个元素：当前项的值和下一项。其最后一项值包含一个叫做 `Nil` 的值且没有下一项。cons list 通过递归调用 `cons` 函数产生。代表递归的终止条件（base case）的规范名称是 `Nil`，它宣布列表的终止。注意这不同于第六章中的 “null” 或 “nil” 的概念，他们代表无效或缺失的值。（注意虽然函数式编程语言经常使用 cons list，但是它并不是一个 Rust 中常见的类型。大部分在 Rust 中需要列表的时候，`Vec<T>` 是一个更好的选择。）

下面试定义一个只存放 `i32` 值的 cons list 的枚举。

```rust
enum List{
    Cons(i32, List),
    Nil,
}
```

如果可以使用，则使用时形式如下：

```rust
use crate::List::{Cons,Nil};

fn main(){
    let list = Cons(1,Cons(2,Cons(3,Nil)));
}
```

但是在编译时会报错，因为定义的类型大小无法做出推断。

> Rust 如何决定需要多少空间来存放一个非递归类型。
>
> ```rust
> enum Message{
>     Quit,
>     Move { x:i32, y:i32},
>     Write(String),
>     ChangeColor(i32,i32,i32),
> }
> ```
>
> 如上枚举类型，当 Rust 需要知道要为 `Message` 值分配多少空间时，它可以检查每一个成员并发现 `Message::Quit` 并不需要任何空间，`Message::Move` 需要足够储存两个 `i32` 值的空间，依此类推。因此，`Message` 值所需的空间等于储存其最大成员的空间大小。

当 Rust 编译器检查 `List` 这样的递归类型时会发生什么呢。编译器尝试计算出储存一个 `List` 枚举需要多少内存，并开始检查 `Cons` 成员，那么 `Cons` 需要的空间等于 `i32` 的大小加上 `List` 的大小。为了计算 `List` 需要多少内存，它检查其成员，从 `Cons` 成员开始。`Cons`成员储存了一个 `i32` 值和一个`List`值，这样的计算将无限进行下去。

**使用Box**

Rust 无法计算出要为定义为递归的类型分配多少空间，所以不能直接储存一个值而是间接的储存一个指向值的指针。

`Box<T>` 是一个指针，总是知道它需要多少空间：指针的大小并不会根据其指向的数据量而改变。这意味着可以将 `Box` 放入 `Cons` 成员中而不是直接存放另一个 `List` 值。`Box` 会指向另一个位于堆上的 `List` 值，而不是存放在 `Cons` 成员中。从概念上讲，仍然有一个通过在其中 “存放” 其他列表创建的列表，不过现在实现这个概念的方式更像是一个项挨着另一项，而不是一项包含另一项。

那么更改`List`的定义如下

```rust
enum List{
 Cons(i32, Box<List>),
    Nil,
}
```

相应得，`List`的使用就变成了

```rust
use crate::List::{Cons, Nil};

fn main(){
    let list = Cons(1,
     Box::new(Cons(2,
      Box::new(Cons(3,
       Box::new(Nil))))));
}
```

`Cons` 成员将会需要一个 `i32` 的大小加上储存 box 指针数据的空间。`Nil` 成员不储存值，所以它比 `Cons` 成员需要更少的空间。任何 `List` 值最多需要一个 `i32` 加上 box 指针数据的大小。

`Box` 只提供了间接存储和堆分配；他们并没有任何其他特殊的功能，当然也没有这些特殊功能带来的性能损失，所以可以用于像 cons list 这样间接存储是唯一所需功能的场景。

`Box<T>` 类型是一个智能指针，因为它实现了 `Deref` trait，它允许 `Box<T>` 值被当作引用对待。当 `Box<T>` 值离开作用域时，由于 `Box<T>` 类型 `Drop` trait 的实现，box 所指向的堆数据也会被清除。

## 构建智能指针类型

实现 `Deref` trait 允许我们重载 **解引用运算符**（*dereference operator*）`*`（与乘法运算符或通配符相区别）。通过这种方式实现 `Deref` trait 的智能指针可以被当作常规引用来对待，可以编写操作引用的代码并用于智能指针。

### 解引用运算符

常规引用是一个指针类型，一种理解指针的方式是将其看成指向储存在其他某处值的箭头。

```rust
fn main(){
    let x = 5;
    let y = &x;
    assert_eq!(5,x);
    asswet_eq!(5,*x);
}
```

变量 `x` 存放了一个 `i32` 值 `5`。`y` 等于 `x` 的一个引用。可以断言 `x` 等于 `5`。然而，如果希望对 `y` 的值做出断言，必须使用 `*y` 来解出引用所指向的值（也就是 **解引用**）。一旦解引用了 `y`，就可以访问 `y` 所指向的整型值并可以与 `5` 做比较。

相反如果尝试编写 `assert_eq!(5, y);`，则会得到编译错误。因为不允许比较数字的引用与数字，因为它们是不同的类型。必须使用解引用运算符解出引用所指向的值。

使用解引用对`Box<T>`可以进行同样操作

```rust
fn main(){
    let x = 5;
    let y = Box::new(x);
    assert_eq!(5, x);
    assert_eq!(5,*y);
}
```

之所以可以这么做的原因是`Box<T>`重载了`Deref` trait。下面通过自定义一个`Box<T>`来说明。

从根本上说，`Box<T>` 被定义为包含一个元素的元组结构体，类似的

```rust
struct MyBox<T>(T);

impl<T> MyBox<T> {
    fn new(x:T) -> MyBox<T>{
        MyBox(x)
    }
}
```

这里定义了一个结构体 `MyBox` 并声明了一个泛型参数 `T`，因为我们希望其可以存放任何类型的值。`MyBox` 是一个包含 `T` 类型元素的元组结构体。`MyBox::new` 函数获取一个 `T` 类型的参数并返回一个存放传入值的 `MyBox` 实例。

### 通过实现 Deref trait 将某类型像引用一样处理

现在`MyBox<T>` 类型还不能解引用，因为我们尚未在该类型实现这个功能。为了启用 `*` 运算符的解引用功能，需要实现 `Deref` trait。

为了实现 trait，需要提供 trait 所需的方法实现。`Deref` trait，由标准库提供，要求实现名为 `deref` 的方法，其借用 `self` 并返回一个内部数据的引用：

```rust
use std::ops::Deref;

impl<T> Deref for MyBox<T>{
    type Tarfet = T;
    fn deref(&self)-> &T{
        &self.0
    }
}
```

> `type Target = T;` 语法定义了用于此 trait 的关联类型。关联类型是一个稍有不同的定义泛型参数的方式。

`deref` 方法体中写入了 `&self.0`，这样 `deref` 返回了希望通过 `*` 运算符访问的值的引用。没有 `Deref` trait 的话，编译器只会解引用 `&` 引用类型。`deref` 方法向编译器提供了获取任何实现了 `Deref` trait 的类型的值，并且调用这个类型的 `deref` 方法来获取一个它知道如何解引用的 `&` 引用的能力。

当解析到`*y`时，Rust实际上在底层运行了`*(y.deref())`，即将 `*` 运算符替换为先调用 `deref` 方法再进行普通解引用的操作，如此我们便不用担心是否还需手动调用 `deref` 方法了。Rust 的这个特性可以让我们写出行为一致的代码，无论是面对的是常规引用还是实现了 `Deref` 的类型。

`deref` 方法返回值的引用，以及 `*(y.deref())` 括号外边的普通解引用仍为必须的原因在于所有权。如果 `deref` 方法直接返回值而不是值的引用，其值（的所有权）将被移出 `self`。在这里以及大部分使用解引用运算符的情况下我们并不希望获取 `MyBox<T>` 内部值的所有权。

注意，每次当我们在代码中使用 `*` 时， `*` 运算符都被替换成了先调用 `deref` 方法再接着使用 `*` 解引用的操作，且只会发生一次，不会对 `*` 操作符无限递归替换，例如解引用出上面 `i32` 类型的值就停止了。

### 函数和方法的隐式解引用强制多态

**解引用强制多态**（*deref coercions*）是 Rust 在函数或方法传参上的一种便利。解引用强制多态将一种类型（A）隐式转换为另外一种类型（B）的引用，因为A类型实现了Deref trait，并且其关联类型是B类型。

解引用强制多态只能工作在实现了Deref trait的类型上。比如，解引用强制多态可以将&String转换为&str，因为类型String实现了Deref trait并且其关联类型是str。代码如下：

```rust
#[stable(feature="rust1", since= "1.0.0")]
impl ops::Dered for String{
    type Target = str;
    
    #[inline]
    fn deref(&self) -> &str{
        unsafe{
            str::from_utf8_unchecked(&self.vec)
        }
    }
}
```

当我们将特定类型的值的引用作为参数传递给函数或方法，但是被传递的值的引用与函数或方法中定义的参数类型不匹配时，会发生解引用强制多态。这时会有一系列的 `deref` 方法被调用，把我们提供的参数类型转换成函数或方法需要的参数类型。

可以使用字符串 slice 作为参数调用 `hello` 函数，比如 `hello("Rust");`。解引用强制多态使得用 `MyBox<String>` 类型值的引用调用 `hello` 成为可能：

```rust
fn hello(name:&str){
    println!("Hello, {}", name);
}

fn main(){
    let m = MyBox::new(String::from("Rust"));
    hello(&m); // 传入 &MyBox<String>类型
    hello(&*m);// 传入 &String类型
}
```

这里使用 `&m` 调用 `hello` 函数，其为 `MyBox<String>` 值的引用。因为`MyBox<T>` 上实现了 `Deref` trait，Rust 可以通过 `deref` 调用将 `&MyBox<String>` 变为 `&String`。标准库中提供了 `String` 上的 `Deref` 实现，其会返回字符串 slice，这可以在 `Deref` 的 API 文档中看到。Rust 再次调用 `deref` 将 `&String` 变为 `&str`，这就符合 `hello` 函数的定义了。

> 如果没有解引用强制多态，则必须编写如下代码
>
> ```rust
> fn main(){
>     let m = MyBox::new(String::from("Rust"));
>     hello(&(*m)[..])
> }
> ```
>
> `(*m)` 将 `MyBox<String>` 解引用为 `String`。接着 `&` 和 `[..]` 获取了整个 `String` 的字符串 slice 来匹配 `hello` 的签名。

这些解析都发生在编译时，所以利用解引用强制多态并没有运行时惩罚。解引用强制多态的加入使得 Rust 程序员编写函数和方法调用时无需增加过多显式使用 `&` 和 `*` 的引用和解引用，Rust 会分析这些类型并使用任意多次 `Deref::deref` 调用以获得匹配参数的类型。这个功能也使得我们可以编写更多同时作用于引用或智能指针的代码。

### 解引用强制多态如何与可变性交互

类似于使用 `Deref` trait 重载不可变引用的 `*` 运算符，Rust 提供了 `DerefMut` trait 用于重载可变引用的 `*` 运算符。

Rust 在发现类型和 trait 实现满足三种情况时会进行解引用强制多态：

- 当 `T: Deref<Target=U>` 时从 `&T` 到 `&U`。
- 当 `T: DerefMut<Target=U>` 时从 `&mut T` 到 `&mut U`。
- 当 `T: Deref<Target=U>` 时从 `&mut T` 到 `&U`。

头两个情况除了可变性之外是相同的，分别代表了解引用之后数据是否可变。而第三种情况Rust 会将可变引用强转为不可变引用，但是反之是 **不可能** 的。

## Drop Trait 负责清理

对于智能指针模式来说第二个重要的 trait 是 `Drop`，其允许我们在值要离开作用域时执行一些代码。**可以为任何类型提供 `Drop` trait 的实现**，同时所指定的代码被用于释放类似于文件或网络连接的资源。例如，`Box<T>` 自定义了 `Drop` 用来释放 box 所指向的堆空间。

在 Rust 中，可以指定每当值离开作用域时被执行的代码，编译器会自动插入这些代码。于是我们就不需要在程序中到处编写在实例结束时清理这些变量的代码 —— 而且还不会泄漏资源。

指定在值离开作用域时应该执行的代码的方式是实现 `Drop` trait。`Drop` trait 要求实现一个叫做 `drop` 的方法，它获取一个 `self` 的可变引用。

```rust
struct CustomSmartPointer{
    data:String,
}
impl Drop for CustomSmartPointer{
    fn drop(&mut self){
        println!("Dropping CustomSmartPointer with data `{}`", self.data);
    }
}

fn main(){
    let c = CustomSmartPointer{data:String::from("my stuff")};
    let d = CustomSmartPointer{data:String::from("other stuff")};
    println!("CustomSmartPointer created.");
}

```

`Drop` trait 包含在 prelude 中，所以无需导入它。`drop` 函数体是放置任何当类型实例离开作用域时期望运行的逻辑的地方。

当实例离开作用域 Rust 会自动调用 `drop`，并调用我们指定的代码。变量以被创建时相反的顺序被丢弃，所以 `d` 在 `c` 之前被丢弃。

### 通过 `std::mem::drop` 提早丢弃值

不幸的是，我们并不能直截了当的禁用 `drop` 这个功能。通常也不需要禁用 `drop` ；整个 `Drop` trait 存在的意义在于其是自动处理的。然而，有时可能需要提早清理某个值。一个例子是当使用智能指针管理锁时；你可能希望强制运行 `drop` 方法来释放锁以便作用域中的其他代码可以获取锁。

**Rust 并不允许我们主动调用 `Drop` trait 的 `drop` 方法；**如果像下列那样显式地调用

```rust
fn main() {
    let c = CustomSmartPointer { data: String::from("some data") };
    println!("CustomSmartPointer created.");
    c.drop();
    println!("CustomSmartPointer dropped before the end of main.");
}
```

在编译时会发生如下错误

```sh
error[E0040]: explicit use of destructor method
  --> src/main.rs:4:7
   |
 4 |     c.drop();
   |       ^^^^ explicit destructor calls not allowed

```

错误信息使用了术语 **析构函数**（*destructor*），这是一个清理实例的函数的通用编程概念。**析构函数** 对应创建实例的 **构造函数**。Rust 中的 `drop` 函数就是这么一个析构函数。

Rust 不允许我们显式调用 `drop` 因为 Rust 仍然会在 `main` 的结尾对值自动调用 `drop`，这会导致一个 **double free** 错误，因为 Rust 会尝试清理相同的值两次。

当我们希望在作用域结束之前就强制释放变量的话，我们应该使用的是由标准库提供的 `std::mem::drop`。

`std::mem::drop` 也位于 prelude。`std::mem::drop` 函数不同于 `Drop` trait 中的 `drop` 方法。可以通过传递希望提早强制丢弃的值作为参数调用 `drop` 函数。

```rust
fn main(){
    let c = CustomSmartPointer{
        data:String::from("Some Data")
    };
    println!("CustomSmartPointer created.");
    drop(c);
    println!("CustomSmartPointer dropped before the end of main");
}
```

`Drop` trait 实现中指定的代码可以用于许多方面，来使得清理变得方便和安全：比如可以用其创建我们自己的内存分配器！通过 `Drop` trait 和 Rust 所有权系统，你无需担心之后的代码清理，Rust 会自动考虑这些问题。也无需担心意外的清理掉仍在使用的值，这会造成编译器错误：所有权系统确保引用总是有效的，也会确保 `drop` 只会在值不再被使用时被调用一次。

## `Rc<T>` 引用计数智能指针

大部分情况下所有权是非常明确的：可以准确地知道哪个变量拥有某个值。然而，有些情况单个值可能会有多个所有者。例如，在图数据结构中，多个边可能指向相同的节点，而这个节点从概念上讲为所有指向它的边所拥有。节点直到没有任何边指向它之前都不应该被清理。

为了启用多所有权，Rust 有一个叫做 `Rc<T>` 的类型。其名称为 **引用计数**（*reference counting*）的缩写。引用计数意味着记录一个值引用的数量来知晓这个值是否仍在被使用。如果某个值有零个引用，就代表没有任何有效引用并可以被清理。

`Rc<T>` 用于当我们希望在堆上分配一些内存供程序的多个部分读取，而且无法在编译时确定程序的哪一部分会最后结束使用它的时候。如果确实知道哪部分是最后一个结束使用的话，就可以令其成为数据的所有者，正常的所有权规则就可以在编译时生效。

> **!** 注意 `Rc<T>` 只能用于单线程场景；

### 使用`Rc<T>`共享数据

对于共用相同数据的地方，单纯使用`Box<T>`是不行的。

```rust
enum List{
    Cons(i32,Box<List>),
    Nil,
}

use create::List::{Cons,Nil};

fn main(){
    let a = Cons(5,
        Box::new(Cons(10,
      Box::new(Nil))));
    let b = Cons(3,Box::new(a));
    let c = Cons(2,Box::new(a));
}
```

上述代码编译会得出所有权被移动的错误，当创建 `b` 列表时，`a` 被移动进了 `b` 这样 `b` 就拥有了 `a`。接着当再次尝试使用 `a` 创建 `c` 时，这不被允许，因为 `a` 的所有权已经被移动。

对此，可以改变 `Cons` 的定义来存放一个引用，不过接着必须指定生命周期参数。通过指定生命周期参数，表明列表中的每一个元素都至少与列表本身存在的一样久。例如，借用检查器不会允许 `let a = Cons(10, &Nil);` 编译，因为临时值 `Nil` 会在 `a` 获取其引用之前就被丢弃了。

修改 `List` 的定义为使用 `Rc<T>` 代替 `Box<T>`，现在每一个 `Cons` 变量都包含一个值和一个指向 `List` 的 `Rc<T>`。当创建 `b` 时，不同于获取 `a` 的所有权，这里会克隆 `a` 所包含的 `Rc<List>`，这会将引用计数从 1 增加到 2 并允许 `a` 和 `b` 共享 `Rc<List>` 中数据的所有权。创建 `c` 时也会克隆 `a`，这会将引用计数从 2 增加为 3。每次调用 `Rc::clone`，`Rc<List>` 中数据的引用计数都会增加，直到有零个引用之前其数据都不会被清理。

```rust
enum List{
    Cons(i32, Rc<List>),
    Nil,
}

use crate::List::{Cons,Nil};
use std::rc::Rc;

fn main(){
    let a = Rc::new(Cons(5,Rc::new(Cons(10,Rc::new(Nil)))));
    let b = Cons(3, Rc::clone(&a));
    let c = Cons(4, Rc::clone(&a));
}
```

使用 `Rc<T>` 需要用 `use` 语句将其引入作用域，因为它不在 prelude 中。当创建 `b` 和 `c` 时，调用 `Rc::clone` 函数并传递 `a` 中 `Rc<List>` 的引用作为参数。

也可以调用 `a.clone()` 而不是 `Rc::clone(&a)`，不过在这里 Rust 的习惯是使用 `Rc::clone`。`Rc::clone` 的实现并不像大部分类型的 `clone` 实现那样对所有数据进行深拷贝，而只会增加引用计数，这并不会花费多少时间。深拷贝可能会花费很长时间。通过使用 `Rc::clone` 进行引用计数，可以明显的区别深拷贝类的克隆和增加引用计数类的克隆。当查找代码中的性能问题时，只需考虑深拷贝类的克隆而无需考虑 `Rc::clone` 调用。

### 克隆 `Rc<T>`会增加引用计数

```rust
fn main(){
    let a = Rc::new(Cons(5, Rc::new(Cons(10,Rc::new(Nil)))));
    println!("count after creting a = {}", Rc::strong_count(&a));
    let b = Cons(3,Rc::clone(&a));
    println!("count after creating b ={}", Rc::strong_count(&a));
    {
        let c = Cons(4, Rc::clone(&a));
        println!("count after creating c = {}", Rc::strong_count(&a));
    }
    println!("count after c goes out of scope = {}", Rc::strong_count(&a));
}
```

每个引用计数可以通过调用 `Rc::strong_count` 函数获得。这个函数叫做 `strong_count` 而不是 `count` 是因为 `Rc<T>` 也有 `weak_count`；

上述代码 `a` 中 `Rc<List>` 的初始引用计数为1，接着每次调用 `clone`，计数会增加1。当 `c` 离开作用域时，计数减1。不必像调用 `Rc::clone` 增加引用计数那样调用一个函数来减少计数；`Drop` trait 的实现当 `Rc<T>` 值离开作用域时自动减少引用计数。

在 `main` 的结尾当 `b` 然后是 `a` 离开作用域时，此处计数会是 0，同时 `Rc<List>` 被完全清理。使用 `Rc<T>` 允许一个值有多个所有者，引用计数则确保只要任何所有者依然存在其值也保持有效。

通过不可变引用， `Rc<T>` 允许在程序的多个部分之间只读地共享数据。如果 `Rc<T>` 也允许多个可变引用，则会违反借用规则：相同位置的多个可变借用可能造成数据竞争和不一致。如果想要数据可被多个可变引用修改则需要使用 `RefCell<T>` 类型，它可以与 `Rc<T>` 结合使用来处理不可变性的限制。

## `RefCell<T>`和内部可变模式

**内部可变性**（*Interior mutability*）是 Rust 中的一个设计模式，它允许你即使在有不可变引用时也可以改变数据，这通常是借用规则所不允许的。为了改变数据，该模式在数据结构中使用 `unsafe` 代码来模糊 Rust 通常的可变性和借用规则。所涉及的 `unsafe` 代码将被封装进安全的 API 中，而外部类型仍然是不可变的。

### 通过 `RefCell<T>` 在运行时检查借用规则

不同于 `Rc<T>`，`RefCell<T>` 代表其数据的唯一的所有权。

对于引用和 `Box<T>`，借用规则的不可变性作用于编译时。对于 `RefCell<T>`，这些不可变性作用于 **运行时**。对于引用，如果违反这些规则，会得到一个编译错误。而对于 `RefCell<T>`，如果违反这些规则程序会 panic 并退出。

在编译时检查借用规则的优势是这些错误将在开发过程的早期被捕获，同时对运行时没有性能影响，因为所有的分析都提前完成了。为此，在编译时检查借用规则是大部分情况的最佳选择，这也正是其为何是 Rust 的默认行为。

相反在运行时检查借用规则的好处则是允许出现特定内存安全的场景，而它们在编译时检查中是不允许的。静态分析，正如 Rust 编译器，是天生保守的。但代码的一些属性不可能通过分析代码发现：其中最著名的就是 [停机问题（Halting Problem）](https://zh.wikipedia.org/wiki/停机问题)。

因为一些分析是不可能的，如果 Rust 编译器不能通过所有权规则编译，它可能会拒绝一个正确的程序；从这种角度考虑它是保守的。如果 Rust 接受不正确的程序，那么用户也就不会相信 Rust 所做的保证了。然而，如果 Rust 拒绝正确的程序，虽然会给程序员带来不便，但不会带来灾难。`RefCell<T>` 正是用于当你确信代码遵守借用规则，而编译器不能理解和确定的时候。

类似于 `Rc<T>`，`RefCell<T>` 只能用于单线程场景。如果尝试在多线程上下文中使用`RefCell<T>`，会得到一个编译错误。

如下为选择 `Box<T>`，`Rc<T>` 或 `RefCell<T>` 的理由：

- `Rc<T>` 允许相同数据有多个所有者；`Box<T>` 和 `RefCell<T>` 有单一所有者。
- `Box<T>` 允许在编译时执行不可变或可变借用检查；`Rc<T>`仅允许在编译时执行不可变借用检查；`RefCell<T>` 允许在运行时执行不可变或可变借用检查。
- 因为 `RefCell<T>` 允许在运行时执行可变借用检查，所以我们可以在即便 `RefCell<T>` 自身是不可变的情况下修改其内部的值。

在不可变值内部改变值就是 **内部可变性** 模式。

### 内部可变性：不可变值的可变借用

借用规则的一个推论是当有一个不可变值时，不能可变地借用它。

如下代码将不能通过编译

```rust
fn main(){
    let x = 5;
    let y = &mut x; // 可变借用了不可变的值
}
```

然而，特定情况下，令一个值在其方法内部能够修改自身，而在其他代码中仍视为不可变，是很有用的。值方法外部的代码就不能修改其值了。`RefCell<T>` 是一个获得内部可变性的方法。`RefCell<T>` 并没有完全绕开借用规则，编译器中的借用检查器允许内部可变性并相应地在运行时检查借用规则。如果违反了这些规则，会出现 panic 而不是编译错误。

下面使用`mock`对象进行说明

> **测试替身**（*test double*）是一个通用编程概念，它代表一个在测试中替代某个类型的类型。**mock 对象** 是特定类型的测试替身，它们记录测试过程中发生了什么以便可以断言操作是正确的。

虽然 Rust 中的对象与其他语言中的对象并不是一回事，Rust 也没有像其他语言那样在标准库中内建 mock 对象功能，不过我们确实可以创建一个与 mock 对象有着相同功能的结构体。

```rust
// mock 对象所需要拥有的接口。
pub trait Messenger{
    // 获取一个 self 的不可变引用和文本信息。
    fn send(&self, msg:&str);
}

pub struct LimitTracker<'a, T:Messenger>{
    messenger:&'a T,
    value:usize,
    max:usize
}

impl<'a, T> LimitTracker<'a, T>
 where T:Messenger{
        pub fn new(messenger:&T, max:usize)->LimitTracker<T>{
            LimitTracker{
                messenger,
                value:0,
                max,
            }
        }
        
        pub fn set_value(&mut self, value:usize){
            self.value = value;
            let percentage_of_max = self.value as f64 / self.max as f64;
            if percentage_of_max >= 1.0{
                self.messenger.send("Error: You are over your quota!");
            }
            else if percentage_of_max >= 0.9{
                self.messenger.send("Urgent warning: You've used up over 90% of your quota!")
            }
            else if percentage_of_max >= 0.75{
                self.messenger.send("Warning: you've used up over 75% of your quota!")
            }
        }
}
```

我们需要测试 `LimitTracker` 的 `set_value` 方法的行为。可以改变传递的 `value` 参数的值，不过 `set_value` 并没有返回任何可供断言的值。也就是说，如果使用某个实现了 `Messenger` trait 的值和特定的 `max` 创建 `LimitTracker`，当传递不同 `value` 值时，消息发送者应被告知发送合适的消息。

下面使用常规的方法实现`send`方法和一个mock对象

```rust
#[cfg(test)]
mod tests{
    use super::*;
    struct MockMessenger{
        sent_meesages:Vec<String>,
    }
    impl MockMessenger{
        fn new() -> MockMessenger{
            MockMessenger{sent_meesages:vec![]}
        }
    }
    impl Messenger for MockMessenger{
        // 这里报错，
     fn send(&self, message:&str){
           // 不实际发送 email 或消息，而是只记录信息被通知要发送了。
            self.sent_meesages.push(String::from(message))
        }
    }

    #[test]
    fn if_sends_an_over_75_percent_warning_message(){
     let mock_message = MockMessenger::new();
        let mut limit_tracker = LimitTracker::new(&mock_message, 100);
        
        limit_tracker.set_value(80);
        assert_eq!(mock_message.sent_messages.len(),1);
    }
}
```

这里不能修改 `MockMessenger` 来记录消息，因为 `send` 方法获取了 `self` 的不可变引用。不能参考编译的错误文本的建议使用 `&mut self` 替代，因为这样 `send` 的签名就不符合 `Messenger` trait 定义中的签名了。

所以可以通过 `RefCell` 来储存 `sent_messages`，然后 `send` 将能够修改 `sent_messages` 并储存消息。

```rust
#[cfg(test)]
mod tests{
    use super::*;
    use std::cell::Refcell;
    
    struct MockMessenger{
        sent_messages:Refcell<Vec<String>>;
    }
    impl MockMessenger{
        fn new()->MockMessenger{
            MockMessenger{
                sent_messages:Refcell::new(vec![])
            }
        }
    }
    impl Messenger for MockMessenger{
        fn send(&self, message:&str){
            self.sent_messages.borrow_mut().push(String::from(message));
        }
    }
    #[test]
    fn it_sends_an_over_75_percent_warning_message(){
        assert_eq!(mock_messenger.sent_messages.borrow().len(),1);
    }
}
```

现在 `sent_messages` 字段的类型是 `RefCell<Vec<String>>` 而不是 `Vec<String>`。在 `new` 函数中新建了一个 `RefCell<Vec<String>>` 实例替代空 vector。

对于 `send` 方法的实现，第一个参数仍为 `self` 的不可变借用，这是符合方法定义的。我们调用 `self.sent_messages` 中 `RefCell` 的 `borrow_mut` 方法来获取 `RefCell` 中值的可变引用，这是一个 vector。接着可以对 vector 的可变引用调用 `push` 以便记录测试过程中看到的消息。

最后必须做出的修改位于断言中：为了看到其内部 vector 中有多少个项，需要调用 `RefCell` 的 `borrow` 以获取 vector 的不可变引用。

### `Refcell<T>` 在运行时记录借用

当创建不可变和可变引用时，我们分别使用 `&` 和 `&mut` 语法。对于 `RefCell<T>` 来说，则是 `borrow` 和 `borrow_mut` 方法，这属于 `RefCell<T>` 安全 API 的一部分。`borrow` 方法返回 `Ref<T>` 类型的智能指针，`borrow_mut` 方法返回 `RefMut` 类型的智能指针。这两个类型都实现了 `Deref`，所以可以当作常规引用对待。

`RefCell<T>` 记录当前有多少个活动的 `Ref<T>` 和 `RefMut<T>` 智能指针。每次调用 `borrow`，`RefCell<T>` 将活动的不可变借用计数加一。当 `Ref<T>` 值离开作用域时，不可变借用计数减一。就像编译时借用规则一样，`RefCell<T>` 在任何时候只允许有多个不可变借用或一个可变借用。

如果我们尝试违反这些规则，相比引用时的编译时错误，`RefCell<T>` 的实现会在运行时出现 panic。例如下列代码会发生panic

```rust
impl Messenger for MockMessenger{
    fn send($self, message:&str){
        let mut one_borrow = self.sent_messages.borrow_mut();
        // 下面这句会发生panic
        let mut two_borrow = self.sent_messages.borrow_mut();

        one_borrow.push(String::from(message));
        two_borrow.push(String::from(message));
    }
}
```

### 结合 `Rc<T>` 和 `RefCell<T>` 来拥有多个可变数据所有者

`RefCell<T>` 的一个常见用法是与 `Rc<T>` 结合。回忆一下 `Rc<T>` 允许对相同数据有多个所有者，不过只能提供数据的不可变访问。如果有一个储存了 `RefCell<T>` 的 `Rc<T>` 的话，就可以得到有多个所有者 **并且** 可以修改的值了。

```rust
#[derive(Debug)]
enum List {
    Cons(Rc<RefCell<i32>>, Rc<List>),
    Nil,
}

use crate::List::{Cons, Nil};
use std::rc::Rc;
use std::cell::RefCell;

fn main() {
    let value = Rc::new(RefCell::new(5));

    let a = Rc::new(Cons(Rc::clone(&value), Rc::new(Nil)));

    let b = Cons(Rc::new(RefCell::new(6)), Rc::clone(&a));
    let c = Cons(Rc::new(RefCell::new(10)), Rc::clone(&a));

    *value.borrow_mut() += 10;

    println!("a after = {:?}", a);
    println!("b after = {:?}", b);
    println!("c after = {:?}", c);
}
```

标准库中也有其他提供内部可变性的类型，比如 `Cell<T>`，它类似 `RefCell<T>` 但有一点除外：它并非提供内部值的引用，而是把值拷贝进和拷贝出 `Cell<T>`。还有 `Mutex<T>`，其提供线程间安全的内部可变性

### 引用循环与内存泄漏

Rust 的内存安全性保证使其难以意外地制造永远也不会被清理的内存（被称为 **内存泄漏**（*memory leak*）），但并不是不可能。与在编译时拒绝数据竞争不同， Rust 并不保证完全地避免内存泄漏，这意味着内存泄漏在 Rust 被认为是内存安全的。这一点可以通过 `Rc<T>` 和 `RefCell<T>` 看出：创建引用循环的可能性是存在的。这会造成内存泄漏，因为每一项的引用计数永远也到不了 0，其值也永远不会被丢弃。

下列代码会造成循环引用

```rust
use std::rc::Rc;
use std::cell::RefCell;
use crate::List::{Cons, Nil};

#[derive(Debug)]
enum List{
    Cons(i32, RefCell<Rc<List>>),
    Nil
}

impl List{
    // 获取List中的RefCell<Rc<List>> 的引用
    fn tail(&self) -> Option<&RefCell<Rc<List>>>{
        match self{
            Cons(_, item) => Some(item),
            Nil => None,
        }
    }
}

fn main(){
    let a = Rc::new(Cons(5,RefCell::new(Rc::new(Nil))));
    println!("a initial rc count = {}", Rc::strong_count(&a));
    println!("a next item = {:?}", a.tail());

    let b = Rc::new(Cons(10, RefCell::new(Rc::clone(&a))));

    println!("a rc count after b creation = {}", Rc::strong_count(&a));
    println!("b initial rc count = {}", Rc::strong_count(&b));
    println!("b next item = {:?}", b.tail());

    if let Some(link) = a.tail() {
        *link.borrow_mut() = Rc::clone(&b);
    }

    println!("b rc count after changing a = {}", Rc::strong_count(&b));
    println!("a rc count after changing a = {}", Rc::strong_count(&a));

    // Uncomment the next line to see that we have a cycle;
    // it will overflow the stack
    // println!("a next item = {:?}", a.tail());
}
```

在变量 `a` 中创建了一个 `Rc<List>` 实例来存放初值为 `5, Nil` 的 `List` 值。接着在变量 `b` 中创建了存放包含值 10 和指向列表 `a` 的 `List` 的另一个 `Rc<List>` 实例。最后，修改 `a` 使其指向 `b` 而不是 `Nil`，这就创建了一个循环。

运行以后输出如下

```shell
a initial rc count = 1
a next item = Some(RefCell { value: Nil })
a rc count after b creation = 2
b initial rc count = 1
b next item = Some(RefCell { value: Cons(5, RefCell { value: Nil }) })
b rc count after changing a = 2
a rc count after changing a = 2
```

即将 `a` 修改为指向 `b` 之后，`a` 和 `b` 中都有的 `Rc<List>` 实例的引用计数为 2。在 `main` 的结尾，Rust 会尝试首先丢弃 `b`，这会使 `a` 和 `b` 中 `Rc<List>` 实例的引用计数减 1。然而，因为 `a` 仍然引用 `b` 中的 `Rc<List>`，`Rc<List>` 的引用计数是 1 而不是 0，所以 `Rc<List>` 在堆上的内存不会被丢弃。其内存会因为引用计数为 1 而永远停留。

如果取消最后 `println!` 的注释并运行程序，Rust 会尝试打印出 `a` 指向 `b` 指向 `a` 这样的循环直到栈溢出。

### 避免引用循环：将 `Rc<T>` 变为 `Weak<T>`

调用 `Rc::clone` 会增加 `Rc<T>` 实例的 `strong_count`，和只在其 `strong_count` 为 0 时才会被清理的 `Rc<T>` 实例。

如果想要避免引用计数通过调用 `Rc::downgrade` 并传递 `Rc<T>` 实例的引用来创建其值的 **弱引用**（*weak reference*）。

调用 `Rc::downgrade` 时会得到 `Weak<T>` 类型的智能指针。不同于将 `Rc<T>` 实例的 `strong_count` 加1，调用 `Rc::downgrade` 会将 `weak_count` 加1。`Rc<T>` 类型使用 `weak_count` 来记录其存在多少个 `Weak<T>` 引用，类似于 `strong_count`。其区别在于 `weak_count` 无需计数为 0 就能使 `Rc<T>` 实例被清理。

强引用代表如何共享 `Rc<T>` 实例的所有权，但弱引用并不属于所有权关系。他们不会造成引用循环，因为任何弱引用的循环会在其相关的强引用计数为 0 时被打断。

因为 `Weak<T>` 引用的值可能已经被丢弃了，为了使用 `Weak<T>` 所指向的值，我们必须确保其值仍然有效。为此可以调用 `Weak<T>` 实例的 `upgrade` 方法，这会返回 `Option<Rc<T>>`。如果 `Rc<T>` 值还未被丢弃，则结果是 `Some`；如果 `Rc<T>` 已被丢弃，则结果是 `None`。因为 `upgrade` 返回一个 `Option<T>`，我们确信 Rust 会处理 `Some` 和 `None` 的情况，所以它不会返回非法指针。

下面以创建一个简单的树形数据结构来说明。

```rust
use std::rc::Rc;
use std::cell::RefCell;

// 树的节点
#[derive(Debug)]
struct Node{
    value:i32,
 parent: RefCell<Weak<Node>>,
    children: RefCell<Vec<Rc<Node>>>,
}

fn main(){
    // 叶子节点
    let reaf = Rc::new(Node{
        value:3,
        parent:RefCell::new(Weak::new()),
        children: RefCell::new(vec![]),
    });
    
    println!("leaf parent = {:?}", leaf.parent.borrow().upgrade());
    
    // 分支
    let branch = Rc::new(Node{
        value:5,
        parent:RefCell::new(Weak::new()),
        children:RefCell::new(vec![Rc::clone(&leaf)]),
    })
    
    *leaf.parent.borrow_mut() = Rc::downgrade(&branch);
    
    println!("leaf parent = {:?}", leaf.parent.borrow().upgrade());
}
```

`leaf` 开始时没有父节点，所以新建了一个空的 `Weak` 引用实例。则第一个 `println!` 输出：

```shell
leaf parent = None
```

当创建 `branch` 节点时，其也会新建一个 `Weak<Node>` 引用，因为 `branch` 并没有父节点。`leaf` 仍然作为 `branch` 的一个子节点。一旦在 `branch` 中有了 `Node` 实例，就可以修改 `leaf` 使其拥有指向父节点的 `Weak<Node>` 引用。这里使用了 `leaf` 中 `parent` 字段里的 `RefCell<Weak<Node>>` 的 `borrow_mut` 方法，接着使用了 `Rc::downgrade` 函数来从 `branch` 中的 `Rc<Node>` 值创建了一个指向 `branch` 的 `Weak<Node>` 引用。

当再次打印出 `leaf` 的父节点时，这一次将会得到存放了 `branch` 的 `Some` 值，同时也避免了会导致栈溢出的循环，`Weak<Node>` 引用将会被打印为 `(Weak)`。

```shell
leaf parent = Some(Node { value: 5, parent: RefCell { value: (Weak) },
children: RefCell { value: [Node { value: 3, parent: RefCell { value: (Weak) },
children: RefCell { value: [] } }] } })
```

# 并发与多线程

**并发编程**（*Concurrent programming*），代表程序的不同部分相互独立的执行，而 **并行编程**（*parallel programming*）代表程序不同部分于同时执行，这两个概念随着计算机越来越多的利用多处理器的优势时显得愈发重要。由于历史原因，在此类上下文中编程一直是困难且容易出错的：Rust 希望能改变这一点。

通过利用所有权和类型检查，在 Rust 中很多并发错误都是 **编译时** 错误，而非运行时错误。因此，相比花费大量时间尝试重现运行时并发 bug 出现的特定情况，Rust 会拒绝编译不正确的代码并提供解释问题的错误信息。

## 使用线程

在大部分现代操作系统中，已执行程序的代码在一个 **进程**（*process*）中运行，操作系统则负责管理多个进程。在程序内部，也可以拥有多个同时运行的独立部分。运行这些独立部分的功能被称为 **线程**（*threads*）。

将程序中的计算拆分进多个线程可以改善性能，因为程序可以同时进行多个任务，不过这也会增加复杂性。因为线程是同时运行的，所以无法预先保证不同线程中的代码的执行顺序。这会导致诸如此类的问题：

- 竞争状态（Race conditions），多个线程以不一致的顺序访问数据或资源
- 死锁（Deadlocks），两个线程相互等待对方停止使用其所拥有的资源，这会阻止它们继续运行
- 只会发生在特定情况且难以稳定重现和修复的 bug

Rust 尝试减轻使用线程的负面影响。不过在多线程上下文中编程仍需格外小心，同时其所要求的代码结构也不同于运行于单线程的程序。

编程语言有一些不同的方法来实现线程。很多操作系统提供了创建新线程的 API。这种由编程语言调用操作系统 API 创建线程的模型有时被称为 *1:1*，一个 OS 线程对应一个语言线程。

同时很多编程语言提供了自己特殊的线程实现。编程语言提供的线程被称为 **绿色**（*green*）线程，使用绿色线程的语言会在不同数量的 OS 线程的上下文中执行它们。为此，绿色线程模式被称为 *M:N* 模型：`M` 个绿色线程对应 `N` 个 OS 线程，这里 `M` 和 `N` 不必相同。

每一个模型都有其优势和取舍。对于 Rust 来说最重要的取舍是运行时支持。**运行时**（*Runtime*）是一个令人迷惑的概念，其在不同上下文中可能有不同的含义。

在当前上下文中，**运行时** 代表二进制文件中包含的由语言自身提供的代码。这些代码根据语言的不同可大可小，不过任何非汇编语言都会有一定数量的运行时代码。为此，通常人们说一个语言 “没有运行时”，一般意味着 “小运行时”。更小的运行时拥有更少的功能不过其优势在于更小的二进制输出，这使其易于在更多上下文中与其他语言相结合。虽然很多语言觉得增加运行时来换取更多功能没有什么问题，但是 Rust 需要做到几乎没有运行时，同时为了保持高性能必须能够调用 C 语言，这点也是不能妥协的。

绿色线程的 M:N 模型需要更大的语言运行时来管理这些线程。因此，Rust 标准库只提供了 1:1 线程模型实现。由于 Rust 是较为底层的语言，如果你愿意牺牲性能来换取抽象，以获得对线程运行更精细的控制及更低的上下文切换成本，你可以使用实现了 M:N 线程模型的 crate。

### `spawn`创建新线程

为了创建一个新线程，需要调用 `thread::spawn` 函数并传递一个闭包，并在其中包含希望在新线程运行的代码。

```rust
use std::thread;
use std::time::Duration;

fn main(){
    thread::spawn(||{
        for i in 1..10{
            println!("hi number {} from the spawned thread!", i);
            thread::sleep(Duration::from_millis(1));
        }
    });
    
    for i in 1..5{
        println!("hi number {} from the main thread!", i);
        thread::sleep(Duration::from_millis(1));
    }
}
```

这个程序的输出可能每次都略有不同，不过它大体上看起来像这样：

```shell
hi number 1 from the main thread!
hi number 1 from the spawned thread!
hi number 2 from the main thread!
hi number 2 from the spawned thread!
hi number 3 from the main thread!
hi number 3 from the spawned thread!
hi number 4 from the main thread!
hi number 4 from the spawned thread!
hi number 5 from the spawned thread!
```

`thread::sleep` 调用强制线程停止执行一小段时间，这会允许其他不同的线程运行。这些线程可能会轮流运行，不过并不保证如此：这依赖操作系统如何调度线程。在这里，主线程首先打印，即便新创建线程的打印语句位于程序的开头，甚至即便我们告诉新建的线程打印直到 `i` 等于 9 ，它在主线程结束之前也只打印到了 5。

### 使用`join`等待其他线程

由于主线程结束，代码大部分时候不光会提早结束新建线程，甚至不能实际保证新建线程会被执行。其原因在于无法保证线程运行的顺序！

可以通过将 `thread::spawn` 的返回值储存在变量中来修复新建线程部分没有执行或者完全没有执行的问题。`thread::spawn` 的返回值类型是 `JoinHandle`。`JoinHandle` 是一个拥有所有权的值，当对其调用 `join` 方法时，它会等待其线程结束。

```rust
use std::thread;
use std::time::Duration;

fn main() {
    let handle = thread::spawn(|| {
        for i in 1..10 {
            println!("hi number {} from the spawned thread!", i);
            thread::sleep(Duration::from_millis(1));
        }
    });

    for i in 1..5 {
        println!("hi number {} from the main thread!", i);
        thread::sleep(Duration::from_millis(1));
    }

    handle.join().unwrap();
}
```

通过调用 handle 的 `join` 会阻塞当前线程直到 handle 所代表的线程结束。**阻塞**（*Blocking*） 线程意味着阻止该线程执行工作或退出。因为我们将 `join` 调用放在了主线程的 `for` 循环之后，所以输出如下

```shell
hi number 1 from the main thread!
hi number 2 from the main thread!
hi number 1 from the spawned thread!
hi number 3 from the main thread!
hi number 2 from the spawned thread!
hi number 4 from the main thread!
hi number 3 from the spawned thread!
hi number 4 from the spawned thread!
hi number 5 from the spawned thread!
hi number 6 from the spawned thread!
hi number 7 from the spawned thread!
hi number 8 from the spawned thread!
hi number 9 from the spawned thread!
```

这两个线程仍然会交替执行，不过主线程会由于 `handle.join()` 调用会等待直到新建线程执行完毕。

而如果将 `handle.join()` 移动到 `main` 中 `for` 循环之前，则主线程会等待直到新建线程执行完毕之后才开始执行 `for` 循环，所以输出将不会交替出现。

### 线程与`move`闭包

`move` 闭包，其经常与 `thread::spawn` 一起使用，因为它允许我们在一个线程中使用另一个线程的数据。

可以在参数列表前使用 `move` 关键字强制闭包获取其使用的环境值的所有权。这个技巧在创建新线程将值的所有权从一个线程移动到另一个线程时最为实用。

不过对于多线程，线程执行持续时间未知，所以隐式推断捕获将不能起作用。如下代码

```rust
use std::thread;

fn main() {
    let v = vec![1, 2, 3];

    let handle = thread::spawn(|| {
        println!("Here's a vector: {:?}", v);
    });

    handle.join().unwrap();
}
```

闭包使用了 `v`，所以闭包会捕获 `v` 并使其成为闭包环境的一部分。因为 `thread::spawn` 在一个新线程中运行这个闭包，所以可以在新线程中访问 `v`。然而当编译这个例子时，会得到如下错误：

```shell
error[E0373]: closure may outlive the current function, but it borrows `v`,
which is owned by the current function
 --> src/main.rs:6:32
  |
6 |     let handle = thread::spawn(|| {
  |                                ^^ may outlive borrowed value `v`
7 |         println!("Here's a vector: {:?}", v);
  |                                           - `v` is borrowed here
  |
help: to force the closure to take ownership of `v` (and any other referenced
variables), use the `move` keyword
  |
6 |     let handle = thread::spawn(move || {
  |                                ^^^^^^^
```

因为 `println!` 只需要 `v` 的引用，闭包尝试借用 `v`，但在线程中无法知晓 `v` 的引用是否一直有效。

例如对于如下代码

```rust
 use std::thread;

fn main(){
    let v = vec![1,2,3];
    let handle = thread::spawn(||{
       println!("Here's a vector: {:?}", v); 
    });
    
    drop(v);
    handle.join().unwrap();
}
```

假如能正常运行的话，则新建线程则可能会立刻被转移到后台并完全没有机会运行。新建线程内部有一个 `v` 的引用，不过主线程立刻就使用 `drop` 丢弃了 `v`。接着当新建线程开始执行，`v` 已不再有效，所以其引用也是无效的。

通过在闭包之前增加 `move` 关键字，我们强制闭包获取其使用的值的所有权，而不是任由 Rust 推断它应该借用值。

```rust
use std::thread;

fn main() {
    let v = vec![1, 2, 3];

    let handle = thread::spawn(move || {
        println!("Here's a vector: {:?}", v);
    });

    handle.join().unwrap();
}
```

使用了 `move` 闭包，将会把 `v` 移动进闭包的环境中，如此将不能在主线程中对其调用 `drop` 了。

## 消息传递

一个日益流行的确保安全并发的方式是 **消息传递**（*message passing*），这里线程或 actor 通过发送包含数据的消息来相互沟通。

Rust 中一个实现消息传递并发的主要工具是 **通道**（*channel*），Rust 标准库提供了其实现的编程概念。你可以将其想象为一个水流的通道，比如河流或小溪。如果你将诸如橡皮鸭或小船之类的东西放入其中，它们会顺流而下到达下游。

编程中的通道有两部分组成，一个发送者（transmitter）和一个接收者（receiver）。发送者位于上游位置，在这里可以将橡皮鸭放入河中，接收者则位于下游，橡皮鸭最终会漂流至此。代码中的一部分调用发送者的方法以及希望发送的数据，另一部分则检查接收端收到的消息。当发送者或接收者任一被丢弃时可以认为通道被 **关闭**（*closed*）了。

```rust
use std::sync::mpsc;

fn main(){
    // 创建一个通道，并将其两端赋值给 tx 和 rx
    let (tx, ry) = mpsc::channel();
}
```

这里使用 `mpsc::channel` 函数创建一个新的通道；`mpsc` 是 **多个生产者，单个消费者**（*multiple producer, single consumer*）的缩写。

简而言之，Rust 标准库实现通道的方式意味着一个通道可以有多个产生值的 **发送**（*sending*）端，但只能有一个消费这些值的 **接收**（*receiving*）端。想象一下多条小河小溪最终汇聚成大河：所有通过这些小河发出的东西最后都会来到下游的大河。

`mpsc::channel` 函数返回一个元组：第一个元素是发送端，而第二个元素是接收端。由于历史原因，`tx` 和 `rx` 通常作为 **发送者**（*transmitter*）和 **接收者**（*receiver*）的缩写，所以这就是我们将用来绑定这两端变量的名字。这里使用了一个 `let` 语句和模式来解构了此元组，使用 `let` 语句是一个方便提取 `mpsc::channel` 返回的元组中一部分的手段。

就像声明变量时一样，创建了一个通道但没有做任何事，所以上述代码还不能编译，因为 Rust 不知道我们想要在通道中发送什么类型。

```rust
use std::thread;
use std::sync::mpsc;

fn main(){
    let (tx, rx) = mpsc::channel();
    thread::spawn(move || {
        let val = String::from("hi");
        tx.send(val).unwrap();
    })
}
```

这里使用 `thread::spawn` 创建了一个新线程并使用 `move` 将 `tx` 移动到闭包中这样新建线程就拥有 `tx` 了。新建线程需要拥有通道的发送端以便能向通道发送消息。

通道的发送端有一个 `send` 方法用来获取需要放入通道的值。`send` 方法返回一个 `Result<T, E>` 类型，所以如果接收端已经被丢弃了，将没有发送的目标对象，则发送操作会返回错误。在这个例子中，出错的时候调用 `unwrap` 产生 panic。

下面定义接收端

```rust
use std::thread;
use std::sync::mpsc;

fn main() {
    let (tx, rx) = mpsc::channel();

    thread::spawn(move || {
        let val = String::from("hi");
        tx.send(val).unwrap();
    });
 // 接收端
    let received = rx.recv().unwrap();
    println!("Got: {}", received);
}

```

通道的接收端有两个有用的方法：`recv` 和 `try_recv`。这里，我们使用了 `recv`，它是 *receive* 的缩写。这个方法会阻塞主线程执行直到从通道中接收一个值。一旦发送了一个值，`recv` 会在一个 `Result<T, E>` 中返回它。当通道发送端关闭，`recv` 会返回一个错误`Err`表明不会再有新的值到来了。同样的，这里为了简洁，使用了`unwrap`。

`try_recv` 不会阻塞，相反它立刻返回一个 `Result<T, E>`：`Ok` 值包含可用的信息，而 `Err` 值代表此时没有任何消息。如果线程在等待消息过程中还有其他工作时使用 `try_recv` 很有用：可以编写一个循环来频繁调用 `try_recv`，在有可用消息时进行处理，其余时候则处理一会其他工作直到再次检查。

### 所有权转移

所有权规则在消息传递中扮演了重要角色，其有助于我们编写安全的并发代码。防止并发编程中的错误是在 Rust 程序中考虑所有权的一大优势。

在新建线程中的通道中发送完 `val` 值 **之后** 再使用它是不允许的。

```rust
use std::tthread;
use std::sync::mpsc;

fn main(){
    let(tx, rx) = mpsc::channel();
    
    thread::spawn(move ||{
        let val = string::from("hi");
        tx.send(val).unwrap();
        println!("val is {}", val); // 这里会发生编译错误
    });
    
    let received = rx.recv().unwrap();
    println!("Got: {}", received);
}
```

这里尝试在通过 `tx.send` 发送 `val` 到通道中之后将其打印出来。允许这么做是一个坏主意：一旦将值发送到另一个线程后，那个线程可能会在我们再次使用它之前就将其修改或者丢弃。其他线程对值可能的修改会由于不一致或不存在的数据而导致错误或意外的结果。

`send` 函数获取其参数的所有权并移动这个值归接收者所有。这可以防止在发送后再次意外地使用这个值；所有权系统检查一切是否合乎规则。

一个发送者当然可以接连发送多个消息

```rust
use std::thread;
use std::sync::mpsc;

fn main(){
    let (tx, rx) = mpsc::channel();
    
    thread::spawn(move ||{
        let vals = vec![
            String::from("hi"),
            String::from("from"),
            String::from("the"),
            String::from("thread"),
        ];
        
        for val in vals{
            tx.send(val).unwrap();
            thread::sleep(Duration::from_secs(1));
        }
        
    });
    
    for received in rx{
        println!("Got: {}", received);
    }
}
```

这一次，在新建线程中有一个字符串 vector 希望发送到主线程。我们遍历他们，单独的发送每一个字符串并通过一个 `Duration` 值调用 `thread::sleep` 函数来暂停一秒。

在主线程中，不再显式调用 `recv` 函数：而是将 `rx` 当作一个迭代器。对于每一个接收到的值，我们将其打印出来。当通道被关闭时，迭代器也将结束。

因为主线程中的 `for` 循环里并没有任何暂停或等待的代码，所以可以说主线程是在等待从新建线程中接收值。

### 多发送者

`mpsc`是 *multiple producer, single consumer* 的缩写，即其支持多个生产者。那么可以运用 `mpsc` 来创建向同一接收者发送值的多个线程，这可以通过克隆通道的发送端来做到

```rust
use std::thread;
use std::sync::mpsc;

fn main(){
 let (tx, rx) = mpsc::channel();
    
    let tx1 = tx.clone();
    
    thread::spawn(move ||{
        let vals = vec![
            String::from("hi"),
            String::from("from"),
            String::from("the"),
            String::from("thread"),
        ];
        
        for val in vals{
            tx1.send(val).unwrap();
            thread::sleep(Duration::from_secs(1));
        }
    });
    
    thread::spawn(move ||{
        let vals = vec![
            String::from("more"),
            String::from("messages"),
            String::from("for"),
            String::from("you"),
        ];
        
        for val in vals{
            tx.send(val).unwrap();
            thread::sleep(Duration::from_secs(1));
        }
    });
    
    for received in rx{
        println!("Got: {}", received);
    }
}

```

这一次，在创建新线程之前，对通道的发送端调用了 `clone` 方法。这会给我们一个可以传递给第一个新建线程的发送端句柄。将原始的通道发送端传递给第二个新建线程。这样就会有两个线程，每个线程将向通道的接收端发送不同的消息。

发送和接受的顺序不是一定的，这依赖于你的系统。这也就是并发既有趣又困难的原因。如果通过 `thread::sleep` 做实验，在不同的线程中提供不同的值，就会发现他们的运行更加不确定，且每次都会产生不同的输出。

## 共享状态并发

消息传递是一个很好的处理并发的方式，但并不是唯一一个。也可以使用共享内存。

在某种程度上，任何编程语言中的通道都类似于单所有权，因为一旦将一个值传送到通道中，将无法再使用这个值。共享内存类似于多所有权：多个线程可以同时访问相同的内存位置。

智能指针如何使得多所有权成为可能，然而这会增加额外的复杂性，因为需要以某种方式管理这些不同的所有者。Rust 的类型系统和所有权规则极大的协助了正确地管理这些所有权。作为一个例子，让我们看看互斥器，一个更为常见的共享内存并发原语。

### 互斥器

**互斥器**（*mutex*）是 *mutual exclusion* 的缩写，也就是说，任意时刻，其只允许一个线程访问某些数据。为了访问互斥器中的数据，线程首先需要通过获取互斥器的 **锁**（*lock*）来表明其希望访问数据。锁是一个作为互斥器一部分的数据结构，它记录谁有数据的排他访问权。因此，我们描述互斥器为通过锁系统 **保护**（*guarding*）其数据。

互斥器以难以使用著称，因为你不得不记住：

1. 在使用数据之前尝试获取锁。
2. 处理完被互斥器所保护的数据之后，必须解锁数据，这样其他线程才能够获取锁。

正确的管理互斥器异常复杂，这也是许多人之所以热衷于通道的原因。然而，在 Rust 中，得益于类型系统和所有权，我们不会在锁和解锁上出错。

### `Mutex<T>`的API

从在单线程上下文使用互斥器开始认识一下`Mutex<T>`的API

```rust
use std::sync::Mutex;

fn main(){
    let m = Mutex::new(5);

    {
        let mut num = m.lock().unwrap();
        *num = 6;
    }

    println!("m = {:?}", m);
}
```

像很多类型一样，我们使用关联函数 `new` 来创建一个 `Mutex<T>`。使用 lock 方法获取锁，以访问互斥器中的数据。这个调用会阻塞当前线程，直到我们拥有锁为止。

如果另一个线程拥有锁，并且那个线程 panic 了，则 `lock` 调用会失败。在这种情况下，没人能够再获取锁，所以这里选择 `unwrap` 并在遇到这种情况时使线程 panic。

一旦获取了锁，就可以将返回值（在这里是num）视为一个其内部数据的可变引用了。类型系统确保了我们在使用 m 中的值之前获取锁：`Mutex<i32>` 并不是一个 i32，所以必须获取锁才能使用这个 i32 值。我们是不会忘记这么做的，因为反之类型系统不允许访问内部的 i32 值。

正如你所怀疑的，`Mutex<T>` 是一个智能指针。更准确的说，lock 调用返回一个叫做 `MutexGuard` 的智能指针。这个智能指针实现了 `Deref` 来指向其内部数据；其也提供了一个 `Drop` 实现当 `MutexGuard` 离开作用域时自动释放锁。为此，我们不会冒忘记释放锁并阻塞互斥器为其它线程所用的风险，因为锁的释放是自动发生的。

丢弃了锁之后，可以打印出互斥器的值，并发现能够将其内部的 i32 改为 6。

### 线程间共享`Mutex<T>`

尝试使用 `Mutex<T>` 在多个线程间共享值。我们将启动十个线程，并在各个线程中对同一个计数器值加一，这样计数器将从 0 变为 10。

```rust
use std::sync::Mutex;
use std::thread;

fn main(){
    let counter = Mutex::new(0);
    let mut handles = vec![];

    for _ in 0..10 {
        let handle =  thread::spawn(move || {
            let mut num = counter.lock().unwrap();

            *num += 1;
        });
        handles.push(handle);
    }

    for handle in handles{
        handle.join().unwrap();
    }

    println!("Result: {}", *counter.lock().unwrap());
}
```

创建了一个 counter 变量来存放内含 i32 的 `Mutex<T>`。接下来遍历 range 创建了 10 个线程。使用了 `thread::spawn` 并对所有线程使用了相同的闭包：他们每一个都将调用 `lock` 方法来获取 `Mutex<T>` 上的锁，接着将互斥器中的值加一。当一个线程结束执行，num 会离开闭包作用域并释放锁，这样另一个线程就可以获取它了。

在主线程中，收集了所有的 `join` 句柄，调用它们的 `join` 方法来确保所有线程都会结束。这时，主线程会获取锁并打印出程序的结果。

但是这个程序仍不能编译成功，因为在每次新建线程时，都要把`counter` 所有权移入新线程，而这是不允许的。

但是这种场景下也不能使用智能指针 `Rc<T>` 来创建引用计数的值，使拥有多所有者。

```rust
use std::rc::Rc;
use std::sync::Mutex;
use std::thread;

fn main() {
    let counter = Rc::new(Mutex::new(0));
    let mut handles = vec![];

    for _ in 0..10 {
        let counter = Rc::clone(&counter);
        let handle = thread::spawn(move || {
            let mut num = counter.lock().unwrap();

            *num += 1;
        });
        handles.push(handle);
    }

    for handle in handles {
        handle.join().unwrap();
    }

    println!("Result: {}", *counter.lock().unwrap());
}
```

这里编译会报错

```shell
error[E0277]: `std::rc::Rc<std::sync::Mutex<i32>>` cannot be sent between threads safely
  --> src/main.rs:11:22
   |
11 |         let handle = thread::spawn(move || {
   |                      ^^^^^^^^^^^^^ `std::rc::Rc<std::sync::Mutex<i32>>`
cannot be sent between threads safely
   |
   = help: within `[closure@src/main.rs:11:36: 14:10
counter:std::rc::Rc<std::sync::Mutex<i32>>]`, the trait `std::marker::Send`
is not implemented for `std::rc::Rc<std::sync::Mutex<i32>>`
   = note: required because it appears within the type
`[closure@src/main.rs:11:36: 14:10 counter:std::rc::Rc<std::sync::Mutex<i32>>]`
   = note: required by `std::thread::spawn`
```

第一行错误表明 `std::rc::Rc<std::sync::Mutex<i32>>` cannot be sent between threads safely。编译器也告诉了我们原因 the trait bound `Send` is not satisfied。

`Send`：这是确保所使用的类型可以用于并发环境的 trait 之一。

这里也就说明，`Rc<T>` 并不能安全的在线程间共享。当 `Rc<T>` 管理引用计数时，它必须在每一个 clone 调用时增加计数，并在每一个克隆被丢弃时减少计数。`Rc<T>` 并没有使用任何并发原语，来确保改变计数的操作不会被其他线程打断。在计数出错时可能会导致诡异的 bug，比如可能会造成内存泄漏，或在使用结束之前就丢弃一个值。

所幸 `Arc<T>` 正是 这么一个类似 `Rc<T>` 并可以安全的用于并发环境的类型。字母 "a" 代表 原子性（atomic），所以这是一个原子引用计数（atomically reference counted）类型。原子性是另一类这里还未涉及到的并发原语：请查看标准库中 std::sync::atomic 的文档来获取更多细节。其中的要点就是：原子性类型工作起来类似原始类型，不过可以安全的在线程间共享。

你可能会好奇为什么不是所有的原始类型都是原子性的？为什么不是所有标准库中的类型都默认使用 `Arc<T>` 实现？原因在于线程安全带有性能惩罚，我们希望只在必要时才为此买单。如果只是在单线程中对值进行操作，原子性提供的保证并无必要，代码可以因此运行的更快。

所以，只需要将程序中的 `Rc<T>` 更改成 `Arc<T>`

```rust
use std::sync::{Mutex, Arc};
use std::thread;

fn main() {
    let counter = Arc::new(Mutex::new(0));
    let mut handles = vec![];

    for _ in 0..10 {
        let counter = Arc::clone(&counter);
        let handle = thread::spawn(move || {
            let mut num = counter.lock().unwrap();

            *num += 1;
        });
        handles.push(handle);
    }

    for handle in handles {
        handle.join().unwrap();
    }

    println!("Result: {}", *counter.lock().unwrap());
}
```

最后，程序将打印 10 并退出。

> **`RefCell<T>`/`Rc<T>` 与 `Mutex<T>`/`Arc<T>`的相似性**
>  
> `Mutex<T>` 提供了内部可变性，就像 Cell 系列类型那样。`RefCell<T>` 可以改变 `Rc<T>` 中的内容那样，同样的可以使用 `Mutex<T>` 来改变 `Arc<T>` 中的内容。
>

但是， Rust 不能避免使用 `Mutex<T>` 的全部逻辑错误。正如使用 `Rc<T>` 就有造成引用循环的风险，这时两个 `Rc<T>` 值相互引用，造成内存泄漏。同理，`Mutex<T>` 也有造成 死锁（deadlock） 的风险。这发生于当一个操作需要锁住两个资源而两个线程各持一个锁，这会造成它们永远相互等待。

## `Sync` 与 `Send trait`可扩展并发

Rust 的并发模型中一个有趣的方面是：语言本身对并发知之甚少。我们之前讨论的几乎所有内容，都属于标准库，而不是语言本身的内容。由于不需要语言提供并发相关的基础设施，并发方案不受标准库或语言所限：我们可以编写自己的或使用别人编写的并发功能。

然而有两个并发概念是内嵌于语言中的：std::marker 中的 Sync 和 Send trait。

### `Send` 允许在线程间转移所有权

`Send` 标记 trait 表明类型的所有权可以在线程间传递。几乎所有的 Rust 类型都是Send 的，不过有一些例外，包括 `Rc<T>`（这是不能 Send 的，因为如果克隆了 `Rc<T>` 的值并尝试将克隆的所有权转移到另一个线程，这两个线程都可能同时更新引用计数）。为此，`Rc<T>` 被实现为用于单线程场景，这时不需要为拥有线程安全的引用计数而付出性能代价。

因此，Rust 类型系统和 trait bound 确保永远也不会意外的将不安全的 `Rc<T>` 在线程间发送。使用标记为 `Send` 的 `Arc<T>`， 就没有问题了。

任何完全由 Send 的类型组成的类型也会自动被标记为 Send。几乎所有基本类型都是 Send 的，除了裸指针（raw pointer）。

### `Sync` 允许多线程访问

`Sync` 标记 trait 表明一个实现了 `Sync` 的类型可以安全的在多个线程中拥有其值的引用。换一种方式来说，对于任意类型 `T`，如果 `&T`（`T` 的引用）是 `Send` 的话 `T` 就是 Sync 的，这意味着其引用就可以安全的发送到另一个线程。

类似于 `Send` 的情况，基本类型是 `Sync` 的，完全由 `Sync` 的类型组成的类型也是 `Sync` 的。

智能指针 `Rc<T>` 也不是 Sync 的，出于其不是 Send 相同的原因。`RefCell<T>`和 `Cell<T>` 系列类型不是 Sync 的。`RefCell<T>` 在运行时所进行的借用检查也不是线程安全的。`Mutex<T>` 是 Sync 的，它可以被用来在多线程中共享访问。

### 不需要手动实现`Send`和`Sync`

通常并不需要手动实现 Send 和 Sync trait，因为由 Send 和 Sync 的类型组成的类型，自动就是 Send 和 Sync 的。因为他们是标记 trait，甚至都不需要实现任何方法。他们只是用来加强并发相关的不可变性的。

手动实现这些标记 trait 涉及到编写不安全的 Rust 代码，

# 面向对象编程

面向对象编程（Object-Oriented Programming，OOP）是一种模式化编程方式。对象（Object）来源于 20 世纪 60 年代的 Simula 编程语言。这些对象影响了 Alan Kay 的编程架构中对象之间的消息传递。他在 1967 年创造了 面向对象编程 这个术语来描述这种架构。

关于 OOP 是什么有很多相互矛盾的定义；在一些定义下，Rust 是面向对象的；在其他定义下，Rust 不是。

## 面向对象语言的特征

关于一个语言被称为面向对象所需的功能，在编程社区内并未达成一致意见。Rust 被很多不同的编程范式影响，包括面向对象编程；

面向对象编程语言所共享的一些特性往往是对象、封装和继承。

### 对象

对象包含数据和行为，由 Erich Gamma、Richard Helm、Ralph Johnson 和 John Vlissides（Addison-Wesley Professional, 1994）编写的书 Design Patterns: Elements of Reusable Object-Oriented Software 被俗称为 The Gang of Four (字面意思为“四人帮”)，它是面向对象编程模式的目录。它这样定义面向对象编程：

> 面向对象的程序是由对象组成的。一个 对象 包含数据和操作这些数据的过程。这些过程通常被称为 方法 或 操作。

在这个定义下，Rust 是面向对象的：结构体和枚举包含数据而 impl 块提供了在结构体和枚举之上的方法。虽然带有方法的结构体和枚举并不被 称为 对象，但是他们提供了与对象相同的功能。

### 封装

另一个通常与面向对象编程相关的方面是 封装（encapsulation）的思想：对象的实现细节不能被使用对象的代码获取到。所以唯一与对象交互的方式是通过对象提供的公有 API；使用对象的代码无法深入到对象内部并直接改变数据或者行为。封装使得改变和重构对象的内部时无需改变使用对象的代码。

Rust可以使用 `pub` 关键字来决定模块、类型、函数和方法是公有的，而默认情况下其他一切都是私有的。例如

```rust
pub struct AveragedCollection {
    list: Vec<i32>,
    average: f64,
}
```

结构体自身被标记为 pub，这样其他代码就可以使用这个结构体，但是在结构体内部的字段仍然是私有的。这是非常重要的，因为我们希望保证变量被增加到列表或者被从列表删除时，也会同时更新平均值。可以通过在结构体上实现 add、remove 和 average 方法来做到这一点。

```rust
#![allow(unused)]
fn main() {
    pub struct AveragedCollection {
        list: Vec<i32>,
        average: f64,
    }
    impl AveragedCollection {
        pub fn add(&mut self, value: i32) {
            self.list.push(value);
            self.update_average();
        }

        pub fn remove(&mut self) -> Option<i32> {
            let result = self.list.pop();
            match result {
                Some(value) => {
                    self.update_average();
                    Some(value)
                },
                None => None,
            }
        }

        pub fn average(&self) -> f64 {
            self.average
        }

        fn update_average(&mut self) {
            let total: i32 = self.list.iter().sum();
            self.average = total as f64 / self.list.len() as f64;
        }
    }
}
```

公有方法 add、remove 和 average 是修改 AveragedCollection 实例的唯一方式。当使用 add 方法把一个元素加入到 list 或者使用 remove 方法来删除时，这些方法的实现同时会调用私有的 update_average 方法来更新 average 字段。

list 和 average 是私有的，所以没有其他方式来使得外部的代码直接向 list 增加或者删除元素，否则 list 改变时可能会导致 average 字段不同步。average 方法返回 average 字段的值，这使得外部的代码只能读取 average 而不能修改它。

因为我们已经封装好了 AveragedCollection 的实现细节，将来可以轻松改变类似数据结构这些方面的内容。例如，可以使用 `HashSet<i32>` 代替 `Vec<i32>` 作为 list 字段的类型。只要 add、remove 和 average 公有函数的签名保持不变，使用 AveragedCollection 的代码就无需改变。相反如果使得 list 为公有，就未必都会如此了： `HashSet<i32>` 和 `Vec<i32>` 使用不同的方法增加或移除项，所以如果要想直接修改 list 的话，外部的代码可能不得不做出修改。

如果封装是一个语言被认为是面向对象语言所必要的方面的话，那么 Rust 满足这个要求。在代码中不同的部分使用 pub 与否可以封装其实现细节。

### 继承

继承（Inheritance）是一个很多编程语言都提供的机制，一个对象可以定义为继承另一个对象的定义，这使其可以获得父对象的数据和行为，而无需重新定义。

如果一个语言必须有继承才能被称为面向对象语言的话，那么 Rust 就不是面向对象的。无法定义一个结构体继承父结构体的成员和方法。然而，如果你过去常常在你的编程工具箱使用继承，根据你最初考虑继承的原因，Rust 也提供了其他的解决方案。

选择继承有两个主要的原因。第一个是为了重用代码：一旦为一个类型实现了特定行为，继承可以对一个不同的类型重用这个实现。相反 Rust 代码可以使用默认 trait 方法实现来进行共享，例如在 Summary trait 上增加的 summarize 方法的默认实现。任何实现了 Summary trait 的类型都可以使用 summarize 方法而无须进一步实现。这类似于父类有一个方法的实现，而通过继承子类也拥有这个方法的实现。当实现 Summary trait 时也可以选择覆盖 summarize 的默认实现，这类似于子类覆盖从父类继承的方法实现。

第二个使用继承的原因与类型系统有关：表现为子类型可以用于父类型被使用的地方。这也被称为 多态（polymorphism），这意味着如果多种对象共享特定的属性，则可以相互替代使用。

> 多态（Polymorphism）
>
>很多人将多态描述为继承的同义词。不过它是一个有关可以用于多种类型的代码的更广泛的概念。对于继承来说，这些类型通常是子类。 Rust 则通过泛型来对不同的可能类型进行抽象，并通过 trait bounds 对这些类型所必须提供的内容施加约束。这有时被称为 bounded parametric polymorphism。

## trait对象与多态

Rust的标准类型 vector 有只能存储同种类型元素的局限。如果想要存储不同类型，可以使用枚举储存整型，浮点型和文本成员的替代方案。这意味着每个vector的每个单元存储的都是枚举类型，在每个单元中储存不同类型的数据，并仍能拥有一个代表一排单元的 vector。这在当编译代码时就知道希望可以交替使用的类型为固定集合的情况下是完全可行的。

然而这样没有扩展性，有时我们希望库用户在特定情况下能够扩展有效的类型集合。例如一个图形用户接口（Graphical User Interface， GUI）工具的例子，它通过遍历列表并调用每一个项目的 draw 方法来将其绘制到屏幕上——此乃一个 GUI 工具的常见技术。

我们将要创建一个叫做 gui 的库 crate，它含一个 GUI 库的结构。这个 GUI 库包含一些可供开发者使用的类型，比如 Button 或 TextField。在此之上，gui 的用户希望创建自定义的可以绘制于屏幕上的类型：比如，一个程序员可能会增加 Image，另一个可能会增加 SelectBox。

在拥有继承的语言中，可以定义一个名为 Component 的类，该类上有一个 draw 方法。其他的类比如 Button、Image 和 SelectBox 会从 Component 派生并因此继承 draw 方法。它们各自都可以覆盖 draw 方法来定义自己的行为，但是框架会把所有这些类型当作是 Component 的实例，并在其上调用 draw。不过 Rust 并没有继承，我们得另寻出路。

### 定义通用行为的 trait

为了实现 gui 所期望的行为，让我们定义一个 `Draw` trait，其中包含名为 `draw` 的方法。

接着可以定义一个存放 trait 对象（trait object） 的 vector。trait 对象指向一个实现了我们指定 trait 的类型的实例，以及一个用于在运行时查找该类型的trait方法的表。我们通过指定某种指针来创建 trait 对象，例如 `&` 引用或 `Box<T>` 智能指针，还有 `dyn` keyword， 以及指定相关的 trait。

我们可以使用 trait 对象代替泛型或具体类型。任何使用 trait 对象的位置，Rust 的类型系统会在编译时确保任何在此上下文中使用的值会实现其 trait 对象的 trait。如此便无需在编译时就知晓所有可能的类型。

之前提到过，Rust 刻意不将结构体与枚举称为 “对象”，以便与其他语言中的对象相区别。在结构体或枚举中，结构体字段中的数据和 impl 块中的行为是分开的，不同于其他语言中将数据和行为组合进一个称为对象的概念中。trait 对象将数据和行为两者相结合，从这种意义上说 则 其更类似其他语言中的对象。不过 trait 对象不同于传统的对象，因为不能向 trait 对象增加数据。trait 对象并不像其他语言中的对象那么通用：其（trait 对象）具体的作用是允许对通用行为进行抽象。

```rust
pub trait Draw {
    fn draw(&self);
}
```

定义一个带有 draw 方法的 trait Draw。

```rust
pub struct Screen{
    pub components: Vec<Box<dyn Draw>>
}
```

定义了一个存放了名叫 `components` 的 `vector` 的结构体 `Screen`。这个 vector 的类型是 `Box<dyn Draw>`，此为一个 trait 对象：它是 Box 中任何实现了 Draw trait 的类型的替身。

在 Screen 结构体上，我们再定义一个 run 方法，该方法会对其 components 上的每一个组件调用 draw 方法

```rust
impl Screen{
    pub fn run(&self){
        for component in self.components.iter(){
            component.draw();
        }
    }
}
```

这与定义使用了带有 trait bound 的泛型类型参数的结构体不同。泛型类型参数一次只能替代一个具体类型，而 trait 对象则允许在运行时替代多种具体类型。

> 可以定义 Screen 结构体来使用泛型和 trait bound
>
> ```rust
> pub struct Screen<T: Draw> {
>     pub components: Vec<T>,
> }
>
> impl<T> Screen<T>
>     where T: Draw {
>     pub fn run(&self) {
>         for component in self.components.iter() {
>             component.draw();
>         }
>    }
> }
> ```
>
> 这限制了 Screen 实例必须拥有一个全是 Button 类型或者全是 TextField 类型的组件列表。如果只需要同质（相同类型）集合，则倾向于使用泛型和 trait bound，因为其定义会在编译时采用具体类型进行单态化。
>
> 另一方面，通过使用 trait 对象的方法，一个 Screen 实例可以存放一个既能包含 `Box<Button>`，也能包含 `Box<TextField>` 的 `Vec<T>`。

现在来增加一些实现了 Draw trait 的类型，例如一个`Button`。一个 Button 结构体可能会拥有 width、height 和 label 字段。

```rust
#![allow(unused)]
fn main() {
    pub trait Draw {
        fn draw(&self);
    }

    pub struct Button {
        pub width: u32,
        pub height: u32,
        pub label: String,
    }

    impl Draw for Button {
        fn draw(&self) {
            // 实际绘制按钮的代码
        }
    }
}
```

在 Button 上的 width、height 和 label 字段会和其他组件不同，比如 TextField 可能有 width、height、label 以及 placeholder 字段。每一个我们希望能在屏幕上绘制的类型都会使用不同的代码来实现 Draw trait 的 draw 方法来定义如何绘制特定的类型，像这里的 Button 类型。除了实现 Draw trait 之外，比如 Button 还可能有另一个包含按钮点击如何响应的方法的 impl 块。这类方法并不适用于像 TextField 这样的类型。

如果一些库的使用者决定实现一个包含 width、height 和 options 字段的结构体 SelectBox，并且也为其实现了 Draw trait

```rust

use gui::Draw;

struct SelectBox {
    width: u32,
    height: u32,
    options: Vec<String>,
}

impl Draw for SelectBox {
    fn draw(&self) {
        // code to actually draw a select box
    }
}

```

库使用者现在可以在他们的 main 函数中创建一个 Screen 实例。至此可以通过将 SelectBox 和 Button 放入 `Box<T>` 转变为 trait 对象来增加组件。接着可以调用 Screen 的 run 方法，它会调用每个组件的 draw 方法。

```rust
use gui::{Screen, Button};

fn main() {
    let screen = Screen {
        components: vec![
            Box::new(SelectBox {
                width: 75,
                height: 10,
                options: vec![
                    String::from("Yes"),
                    String::from("Maybe"),
                    String::from("No")
                ],
            }),
            Box::new(Button {
                width: 50,
                height: 10,
                label: String::from("OK"),
            }),
        ],
    };

    screen.run();
}
```

当编写库的时候，我们不知道何人会在何时增加 SelectBox 类型，不过 Screen 的实现能够操作并绘制这个新类型，因为 SelectBox 实现了 Draw trait，这意味着它实现了 draw 方法。

这个概念 —— 只关心值所反映的信息而不是其具体类型 —— 类似于动态类型语言中称为 鸭子类型（duck typing）的概念：如果它走起来像一只鸭子，叫起来像一只鸭子，那么它就是一只鸭子！

使用 trait 对象和 Rust 类型系统来进行类似鸭子类型操作的优势是无需在运行时检查一个值是否实现了特定方法或者担心在调用时因为值没有实现方法而产生错误。如果值没有实现 trait 对象所需的 trait 则 Rust 不会编译这些代码。

例如

```rust
use gui::Screen;

fn main() {
    let screen = Screen {
        components: vec![
            Box::new(String::from("Hi")),
        ],
    };

    screen.run();
}
```

会爆出

```shell
error[E0277]: the trait bound `std::string::String: gui::Draw` is not satisfied
  --> src/main.rs:7:13
   |
 7 |             Box::new(String::from("Hi")),
   |             ^^^^^^^^^^^^^^^^^^^^^^^^^^^^ the trait gui::Draw is not
   implemented for `std::string::String`
   |
   = note: required for the cast to the object type `gui::Draw`

```

因为 String 没有实现 rust_gui::Draw trait.

### trait 对象执行动态分发

当对泛型使用 trait bound 时编译器所进行单态化处理：编译器为每一个被泛型类型参数代替的具体类型生成了非泛型的函数和方法实现。单态化所产生的代码进行**静态分发（static dispatch）**。静态分发发生于编译器在编译时就知晓调用了什么方法的时候。这与**动态分发（dynamic dispatch）**相对，这时编译器在编译时无法知晓调用了什么方法。在动态分发的情况下，编译器会生成在运行时确定调用了什么方法的代码。

当使用 trait 对象时，Rust 必须使用动态分发。编译器无法知晓所有可能用于 trait 对象代码的类型，所以它也不知道应该调用哪个类型的哪个方法实现。为此，Rust 在运行时使用 trait 对象中的指针来知晓需要调用哪个方法。动态分发也阻止编译器有选择的内联方法代码，这会相应的禁用一些优化。尽管在编写代码的过程中确实获得了额外的灵活性，但仍然需要权衡取舍。

### Trait 对象要求对象安全

只有 对象安全（object safe）的 trait 才可以组成 trait 对象。围绕所有使得 trait 对象安全的属性存在一些复杂的规则，不过在实践中，只涉及到两条规则。如果一个 trait 中所有的方法有如下属性时，则该 trait 是对象安全的：

- 返回值类型不为 `Self`
- 方法没有任何泛型类型参数

`Self` 关键字是我们要实现 trait 或方法的类型的别名。对象安全对于 trait 对象是必须的，因为一旦有了 trait 对象，就不再知晓实现该 trait 的具体类型是什么了。如果 trait 方法返回具体的 Self 类型，但是 trait 对象忘记了其真正的类型，那么方法不可能使用已经忘却的原始具体类型。同理对于泛型类型参数来说，当使用 trait 时其会放入具体的类型参数：此具体类型变成了实现该 trait 的类型的一部分。当使用 trait 对象时其具体类型被抹去了，故无从得知放入泛型参数类型的类型是什么。

例如标准库的`Clone`trait，其函数签名是：

```rust
pub trait Clone{
    fn clone(&self)->Self;
}
```

`String`实现了`Clone`trait，当在`String`示例上得到一个`String`实例。类似的，当调用`Vec<t>`实例的`clone`方法会得到一个`Vec<T>`实例。`clone`的签名需要知道什么类型会代替`Self`，因为这是它的返回值。

如果尝试做一些违反有关 trait 对象的对象安全规则的事情，编译器会提示你。例如，如果尝试实现 `Screen` 结构体来存放实现了 `Clone` trait 而不是 `Draw` trait 的类型，像这样：

```rust
pub struct Screen {
    pub components: Vec<Box<dyn Clone>>,
}
```

将会得到如下错误：

```shell
error[E0038]: the trait `std::clone::Clone` cannot be made into an object
 --> src/lib.rs:2:5
  |
2 |     pub components: Vec<Box<dyn Clone>>,
  |     ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ the trait `std::clone::Clone`
  cannot be made into an object
  |
  = note: the trait cannot require that `Self : Sized`
```

这意味着不能以这种方式使用此 trait 作为 trait 对象。

## 面向对象设计模式

**状态模式**（*state pattern*）是一个面向对象设计模式。该模式的关键在于一个值有某些内部状态，体现为一系列的 **状态对象**，同时值的行为随着其内部状态而改变。状态对象共享功能：当然，在 Rust 中使用结构体和 trait 而不是对象和继承。每一个状态对象负责其自身的行为，以及该状态何时应当转移至另一个状态。持有一个状态对象的值对于不同状态的行为以及何时状态转移毫不知情。

使用状态模式意味着当程序的业务需求改变时，无需改变值持有状态或者使用值的代码。我们只需更新某个状态对象中的代码来改变其规则，或者是增加更多的状态对象。

# 模式与匹配

模式是 Rust 中特殊的语法，它用来匹配类型中的结构，无论类型是简单还是复杂。结合使用模式和 `match` 表达式以及其他结构可以提供更多对程序控制流的支配权。模式由如下一些内容组合而成：

- 字面值
- 解构的数组、枚举、结构体或者元组
- 变量
- 通配符
- 占位符

这些部分描述了我们要处理的数据的形状，接着可以用其匹配值来决定程序是否拥有正确的数据来运行特定部分的代码。

## 能用到模式的位置

### `match`分支

一个模式常用的位置是 `match` 表达式的分支。在形式上 `match` 表达式由 `match` 关键字、用于匹配的值和一个或多个分支构成，这些分支包含一个模式和在值匹配分支的模式时运行的表达式：

```rust
match VALUE{
    PATTERN => EXPERSSION,
    PATTERN => EXPERSSION,
    PATTERN => EXPERSSION,
}
```

`match` 表达式必须是 **穷尽**（*exhaustive*）的，意为 `match` 表达式所有可能的值都必须被考虑到。一个确保覆盖每个可能值的方法是在最后一个分支使用捕获所有的模式：比如，一个匹配任何值的名称永远也不会失败，因此可以覆盖所有匹配剩下的情况。

有一个特定的模式 `_` 可以匹配所有情况，不过它从不绑定任何变量。这在例如希望忽略任何未指定值的情况很有用。

### `if let`表达式

`if let`等同于只关心一个情况的 `match` 语句，可以视为其简写。同时它对应一个可选的带有代码的 `else` 在 `if let` 中的模式不匹配时运行。

也可以组合并匹配 `if let`、`else if` 和 `else if let` 表达式。这相比 `match` 表达式一次只能将一个值与模式比较提供了更多灵活性；一系列 `if let`、`else if`、`else if let` 分支并不要求其条件相互关联。

```rust
fn main() {
    let favorite_color: Option<&str> = None;
    let is_tuesday = false;
    let age: Result<u8, _> = "34".parse();

    if let Some(color) = favorite_color {
        println!("Using your favorite color, {}, as the background", color);
    } else if is_tuesday {
        println!("Tuesday is green day!");
    } else if let Ok(age) = age {
        if age > 30 {
            println!("Using purple as the background color");
        } else {
            println!("Using orange as the background color");
        }
    } else {
        println!("Using blue as the background color");
    }
}
```

注意 `if let` 也可以像 `match` 分支那样引入覆盖变量：`if let Ok(age) = age` 引入了一个新的覆盖变量 `age`，它包含 `Ok` 成员中的值。这意味着 `if age > 30` 条件需要位于这个代码块内部；不能将两个条件组合为 `if let Ok(age) = age && age > 30`，因为我们希望与 30 进行比较的被覆盖的 `age` 直到大括号开始的新作用域才是有效的。

`if let` 表达式的缺点在于其穷尽性没有为编译器所检查，而 `match` 表达式则检查了。如果去掉最后的 `else` 块而遗漏处理一些情况，编译器也不会警告这类可能的逻辑错误。

### `while let`条件循环

一个与 `if let` 结构类似的是 `while let` 条件循环，它允许只要模式匹配就一直进行 `while` 循环。

```rust
let mut stack = Vec::new();

stack.push(1);
stack.push(2);
stack.push(3);

while let Some(top) = stack.pop(){
    println!("{}", top);
}

```

`pop` 方法取出 vector 的最后一个元素并返回 `Some(value)`。如果 vector 是空的，它返回 `None`。`while` 循环只要 `pop` 返回 `Some` 就会一直运行其块中的代码。一旦其返回 `None`，`while` 循环停止。

### `for`循环

`for` 循环是 Rust 中最常见的循环结构，`for` 可以获取一个模式。在 `for` 循环中，模式是 `for` 关键字直接跟随的值，正如 `for x in y` 中的 `x`。

```rust
let v = vec!['a', 'b', 'c'];

for (index, value) in v.iter().enumerate(){
    println!("{}, is at index {}", value, index);
}
```

这里使用 `enumerate` 方法适配一个迭代器来产生一个值和其在迭代器中的索引，他们位于一个元组中。第一个 `enumerate` 调用会产生元组 `(0, 'a')`。当这个值匹配模式 `(index, value)`，`index` 将会是 0 而 `value` 将会是 `'a'`，并打印出第一行输出。

### `let`语句

`let` 语句正是在使用模式，其更为正式的样子如下：

```rust
let PATTERN = EXPRESSION;
```

像 `let x = 5;` 这样的语句中变量名位于 `PATTERN` 位置，变量名不过是形式特别朴素的模式。我们将表达式与模式比较，并为任何找到的名称赋值。所以例如 `let x = 5;` 的情况，`x` 是一个模式代表 “将匹配到的值绑定到变量 x”。同时因为名称 `x` 是整个模式，这个模式实际上等于 “将任何值绑定到变量 `x`，不管值是什么”。

为了更清楚的理解 `let` 的模式匹配方面的内容，考虑使用 `let` 和模式解构一个元组：

```rust
let (x, y, z) = (1, 2, 3);
```

这里将一个元组与模式匹配。Rust 会比较值 `(1, 2, 3)` 与模式 `(x, y, z)` 并发现此值匹配这个模式。在这个例子中，将会把 `1` 绑定到 `x`，`2` 绑定到 `y` 并将 `3` 绑定到 `z`。你可以将这个元组模式看作是将三个独立的变量模式结合在一起。

如果模式中元素的数量不匹配元组中元素的数量，则整个类型不匹配，并会得到一个编译时错误。如以下代码是不行的

```rust
let (x, y) = (1, 2, 3);
```

如果希望忽略元组中一个或多个值，也可以使用 `_` 或 `..`。如果问题是模式中有太多的变量，则解决方法是通过去掉变量使得变量数与元组中元素数相等。

### 函数参数

函数参数也可以是模式。下面代码声明了一个叫做 `foo` 的函数，它获取一个 `i32` 类型的参数 `x`：

```rust
fn foo(x:i32){
    // 代码
}
```

`x` 部分就是一个模式！类似于之前对 `let` 所做的，可以在函数参数中匹配元组。

```rust
fn print_coordinates(&(x,y):&(i32, i32)){
    println!("Current location:({}, {})", x, y);
}

fn main(){
 let point = (3, 5);
    print_coordinates(&point);
}
```

这会打印出 `Current location: (3, 5)`。值 `&(3, 5)` 会匹配模式 `&(x, y)`，如此 `x` 得到了值 `3`，而 `y`得到了值 `5`。

另外因为闭包类似于函数，也可以在闭包参数列表中使用模式。

## Refutability（可反驳性）: 模式是否会匹配失效

模式有两种形式：refutable（可反驳的）和 irrefutable（不可反驳的）。

能匹配任何传递的可能值的模式被称为是 **不可反驳的**（*irrefutable*）。一个例子就是 `let x = 5;` 语句中的 `x`，因为 `x` 可以匹配任何值所以不可能会失败。对某些可能的值进行匹配会失败的模式被称为是 **可反驳的**（*refutable*）。一个这样的例子便是 `if let Some(x) = a_value` 表达式中的 `Some(x)`；如果变量 `a_value` 中的值是 `None` 而不是 `Some`，那么 `Some(x)` 模式不能匹配。

模式在每个使用它的地方并不以相同的方式工作；在一些地方，模式必须是 *irrefutable* 的，意味着他们必须匹配所提供的任何值。在另一些情况，他们则可以是 refutable 的。

- 函数参数、 `let` 语句和 `for` 循环只能接受不可反驳的模式，因为通过不匹配的值程序无法进行有意义的工作。

- `if let` 和 `while let` 表达式被限制为只能接受可反驳的模式，因为根据定义他们意在处理可能的失败：条件表达式的功能就是根据成功或失败执行不同的操作。

尝试在 Rust 要求不可反驳模式的地方使用可反驳模式或是相反都是有问题的。

```rust
let Some(x) = some_option_value;
```

如果 `some_option_value` 的值是 `None`，其不会成功匹配模式 `Some(x)`，表明这个模式是可反驳的。然而 `let` 语句只能接受不可反驳模式因为代码不能通过 `None` 值进行有效的操作。Rust 会在编译时抱怨我们尝试在要求不可反驳模式的地方使用可反驳模式：

```rust
error[E0005]: refutable pattern in local binding: `None` not covered
 -->
  |
3 | let Some(x) = some_option_value;
  |     ^^^^^^^ pattern `None` not covered
```

因为我们没有覆盖（也不可能覆盖！）到模式 `Some(x)` 的每一个可能的值, 所以 Rust 会合理地抗议。

而如果为 `if let` 提供了一个总是会匹配的模式，编译器会给出一个警告：

```rust
if let x = 5 {
    println!("{}", x);
};
```

Rust 会抱怨将不可反驳模式用于 `if let` 是没有意义的：

```rust
warning: irrefutable if-let pattern
 --> <anon>:2:5
  |
2 | /     if let x = 5 {
3 | |     println!("{}", x);
4 | | };
  | |_^
  |
  = note: #[warn(irrefutable_let_patterns)] on by default
```

基于此，`match`匹配分支必须使用可反驳模式，除了最后一个分支需要使用能匹配任何剩余值的不可反驳模式。Rust允许我们在只有一个匹配分支的`match`中使用不可反驳模式，不过这么做不是特别有用，并可以被更简单的 `let` 语句替代。

## 模式的语法

### 匹配字面量

```rust
let x = 1;

match x {
    1 => println!("one"),
    2 => println!("two"),
    3 => println!("three"),
    _ => println!("other"),
}
```

### 匹配命名变量

命名变量是匹配任何值的不可反驳模式，这在之前已经使用过数次。然而当其用于 `match` 表达式时情况会有些复杂。因为 `match` 会开始一个新作用域，`match` 表达式中作为模式的一部分声明的变量会覆盖 `match` 结构之外的同名变量，与所有变量一样。

```rust
fn main() {
    let x = Some(5);
    let y = 10;

    match x {
        Some(50) => println!("Got 50"),
        Some(y) => println!("Matched, y = {:?}", y),
        _ => println!("Default case, x = {:?}", x),
    }

    println!("at the end: x = {:?}, y = {:?}", x, y);
}
```

一旦 `match` 表达式执行完毕，其作用域也就结束了，同理内部 `y` 的作用域也结束了。最后的 `println!` 会打印 `at the end: x = Some(5), y = 10`。

### 多个模式

在 `match` 表达式中，可以使用 `|` 语法匹配多个模式，它代表 **或**（*or*）的意思。例如，如下代码将 `x` 的值与匹配分支相比较，第一个分支有 **或** 选项，意味着如果 `x` 的值匹配此分支的任一个值，它就会运行：

```rust
let x = 2;

match x{
    1 | 2 => println!("one or two"),
    3 => println!("three"),
    _ => println!("anything"),
}
```

### 通过 `..=` 匹配值的范围

`..=` 语法允许你匹配一个闭区间范围内的值。在如下代码中，当模式匹配任何在此范围内的值时，该分支会执行：

```rust
let x = 5;

match x {
    1..=5 => println!("one through five"),
    _ => println!("something else"),
}
```

如果 `x` 是 1、2、3、4 或 5，第一个分支就会匹配。这相比使用 `|` 运算符表达相同的意思更为方便；相比 `1..=5`，使用 `|` 则不得不指定 `1 | 2 | 3 | 4 | 5`。相反指定范围就简短的多，特别是在希望匹配比如从 1 到 1000 的数字的时候！

范围只允许用于数字或 `char` 值，因为编译器会在编译时检查范围不为空。`char` 和 数字值是 Rust 仅有的可以判断范围是否为空的类型。

```rust
let x = 'c';

match x {
    'a'..='j' => println!("early ASCII letter"),
    'k'..='z' => println!("late ASCII letter"),
    _ => println!("something else"),
}
```

### 解构并分解值

也可以使用模式来解构结构体、枚举、元组和引用，以便使用这些值的不同部分。

#### 解构结构体

```rust
struct Point {
    x: i32,
    y: i32,
}

fn main() {
    let p = Point { x: 0, y: 7 };

    let Point { x: a, y: b } = p;
    assert_eq!(0, a);
    assert_eq!(7, b);
}
```

这段代码创建了变量 `a` 和 `b` 来匹配结构体 `p` 中的 `x` 和 `y` 字段。这个例子展示了模式中的变量名不必与结构体中的字段名一致。不过通常希望变量名与字段名一致以便于理解变量来自于哪些字段。

因为变量名匹配字段名是常见的，同时因为 `let Point { x: x, y: y } = p;` 包含了很多重复，所以对于匹配结构体字段的模式存在简写：只需列出结构体字段的名称，则模式创建的变量会有相同的名称。

```rust
struct Point {
    x: i32,
    y: i32,
}

fn main() {
    let p = Point { x: 0, y: 7 };

    let Point { x, y } = p;
    assert_eq!(0, x);
    assert_eq!(7, y);
}
```

这段代码创建了变量 `x` 和 `y`，与变量 `p` 中的 `x` 和 `y` 相匹配。其结果是变量 `x` 和 `y` 包含结构体 `p` 中的值。

也可以使用字面值作为结构体模式的一部分进行进行解构，而不是为所有的字段创建变量。这允许我们测试一些字段为特定值的同时创建其他字段的变量。

```rust
fn main() {
    let p = Point { x: 0, y: 7 };

    match p {
        Point { x, y: 0 } => println!("On the x axis at {}", x),
        Point { x: 0, y } => println!("On the y axis at {}", y),
        Point { x, y } => println!("On neither axis: ({}, {})", x, y),
    }
}
```

第一个分支通过指定字段 `y` 匹配字面值 `0` 来匹配任何位于 `x` 轴上的点。此模式仍然创建了变量 `x` 以便在分支的代码中使用。

类似的，第二个分支通过指定字段 `x` 匹配字面值 `0` 来匹配任何位于 `y` 轴上的点，并为字段 `y` 创建了变量 `y`。第三个分支没有指定任何字面值，所以其会匹配任何其他的 `Point` 并为 `x` 和 `y` 两个字段创建变量。

在这个例子中，值 `p` 因为其 `x` 包含 0 而匹配第二个分支，因此会打印出 `On the y axis at 7`。

#### 结构枚举

解构枚举的模式需要对应枚举所定义的储存数据的方式。

```rust
enum Message {
    Quit,
    Move { x: i32, y: i32 },
    Write(String),
    ChangeColor(i32, i32, i32),
}

fn main() {
    let msg = Message::ChangeColor(0, 160, 255);
    match msg {
        Message::Quit => {
            println!("The Quit variant has no data to destructure.")
        }
        Message::Move { x, y } => {
            println!(
                "Move in the x direction {} and in the y direction {}",
                x,
                y
            );
        }
        Message::Write(text) => println!("Text message: {}", text),
        Message::ChangeColor(r, g, b) => {
            println!(
                "Change the color to red {}, green {}, and blue {}",
                r,
                g,
                b
            )
        }
    }
}

```

对于像 `Message::Quit` 这样没有任何数据的枚举成员，不能进一步解构其值。只能匹配其字面值 `Message::Quit`，因此模式中没有任何变量。

对于像 `Message::Move` 这样的类结构体枚举成员，可以采用类似于匹配结构体的模式。在成员名称后，使用大括号并列出字段变量以便将其分解以供此分支的代码使用。

对于像 `Message::Write` 这样的包含一个元素，以及像 `Message::ChangeColor` 这样包含三个元素的类元组枚举成员，其模式则类似于用于解构元组的模式。模式中变量的数量必须与成员中元素的数量一致。

#### 嵌套的结构体和枚举

match 也可以匹配嵌套的项

```rust
enum Color {
   Rgb(i32, i32, i32),
   Hsv(i32, i32, i32),
}

enum Message {
    Quit,
    Move { x: i32, y: i32 },
    Write(String),
    ChangeColor(Color),
}

fn main() {
    let msg = Message::ChangeColor(Color::Hsv(0, 160, 255));

    match msg {
        Message::ChangeColor(Color::Rgb(r, g, b)) => {
            println!(
                "Change the color to red {}, green {}, and blue {}",
                r,
                g,
                b
            )
        }
        Message::ChangeColor(Color::Hsv(h, s, v)) => {
            println!(
                "Change the color to hue {}, saturation {}, and value {}",
                h,
                s,
                v
            )
        }
        _ => ()
    }
}
```

`match` 表达式第一个分支的模式匹配一个包含 `Color::Rgb` 枚举成员的 `Message::ChangeColor` 枚举成员，然后模式绑定了 3 个内部的 `i32` 值。第二个分支的模式也匹配一个 `Message::ChangeColor` 枚举成员， 但是其内部的枚举会匹配 `Color::Hsv` 枚举成员。

#### 结构体和元组

可以用复杂的方式来混合、匹配和嵌套解构模式。

```rust
#![allow(unused)]
fn main() {
    struct Point {
        x: i32,
        y: i32,
    }

    let ((feet, inches), Point {x, y}) = ((3, 10), Point { x: 3, y: -10 });
}
```

这将复杂的类型分解成部分组件以便可以单独使用我们感兴趣的值。

### 忽略模式中的值

有时忽略模式中的一些值是有用的，比如 `match` 中最后捕获全部情况的分支实际上没有做任何事，但是它确实对所有剩余情况负责。有一些简单的方法可以忽略模式中全部或部分值：使用 `_` 模式，在另一个模式中使用 `_` 模式，使用一个以下划线开始的名称，或者使用 `..` 忽略所剩部分的值。

忽略值得语法不会发生所有权转移。

#### 使用 `_` 忽略整个值

使用过下划线（`_`）是作为匹配但不绑定任何值的通配符模式。`_` 模式既可以作为 `match` 表达式最后的分支，也可以将其用于任意模式，包括函数参数中。

```rust
fn foo(_: i32, y: i32) {
    println!("This code only uses the y parameter: {}", y);
}

fn main() {
    foo(3, 4);
}
```

这段代码会完全忽略作为第一个参数传递的值 `3`，并会打印出 `This code only uses the y parameter: 4`。

大部分情况当你不再需要特定函数参数时，最好修改签名不再包含无用的参数。在一些情况下忽略函数参数会变得特别有用，比如实现 trait 时，当你需要特定类型签名但是函数实现并不需要某个参数时。此时编译器就不会警告说存在未使用的函数参数，就跟使用命名参数一样。

#### 使用嵌套的 `_` 忽略部分值

也可以在一个模式内部使用`_` 忽略部分值，例如，当只需要测试部分值但在期望运行的代码中没有用到其他部分时。

```rust
#![allow(unused)]
fn main() {
    let mut setting_value = Some(5);
    let new_setting_value = Some(10);

    match (setting_value, new_setting_value) {
        (Some(_), Some(_)) => {
            println!("Can't overwrite an existing customized value");
        }
        _ => {
            setting_value = new_setting_value;
        }
    }

    println!("setting is {:?}", setting_value);
}
```

这段代码会打印出 `Can't overwrite an existing customized value` 接着是 `setting is Some(5)`。在第一个匹配分支，我们不需要匹配或使用任一个 `Some` 成员中的值；重要的部分是需要测试 `setting_value` 和 `new_setting_value` 都为 `Some` 成员的情况。在这种情况，我们打印出为何不改变 `setting_value`，并且不会改变它。

对于所有其他情况（`setting_value` 或 `new_setting_value` 任一为 `None`），这由第二个分支的 `_` 模式体现，这时确实希望允许 `new_setting_value` 变为 `setting_value`。

也可以在一个模式中的多处使用下划线来忽略特定值，

```rust
#![allow(unused)]
fn main() {
    let numbers = (2, 4, 8, 16, 32);

    match numbers {
        (first, _, third, _, fifth) => {
            println!("Some numbers: {}, {}, {}", first, third, fifth)
        },
    }
}
```

这会打印出 `Some numbers: 2, 8, 32`, 值 4 和 16 会被忽略。

#### 名字前以一个下划线开头来忽略未使用的变量

创建了一个变量却不在任何地方使用它, Rust 通常会给你一个警告，因为这可能会是个 bug。但是有时创建一个还未使用的变量是有用的，比如正在设计原型或刚刚开始一个项目。这时希望告诉 Rust 不要警告未使用的变量，为此可以用下划线作为变量名的开头。

```rust
fn main() {
    let _x = 5;
    let y = 10;
}
```

创建了两个未使用变量，不过当运行代码时只会得到 `y` 未使用的警告。

只使用 `_` 和使用以下划线开头的名称有些微妙的不同：比如 `_x` 仍会将值绑定到变量，而 `_` 则完全不会绑定。

```rust
let s = Some(String::from("Hello!"));

if let Some(_s) = s {
    println!("found a string");
}

println!("{:?}", s); // 会报错，s所有权已经移到了Some(_s)里
```

`s` 的值仍然会移动进 `_s`，并阻止我们再次使用 `s`。然而只使用下划线本身，并不会绑定值。

#### 用 `..` 忽略剩余值

对于有多个部分的值，可以使用 `..` 语法来只使用部分并忽略其它值，同时避免不得不每一个忽略值列出下划线。`..` 模式会忽略模式中剩余的任何没有显式匹配的值部分。有一个 `Point` 结构体存放了三维空间中的坐标，而在 `match` 表达式中，我们希望只操作 `x` 坐标并忽略 `y` 和 `z` 字段的值：

```rust
#![allow(unused)]
fn main() {
    struct Point {
        x: i32,
        y: i32,
        z: i32,
    }

    let origin = Point { x: 0, y: 0, z: 0 };

    match origin {
        Point { x, .. } => println!("x is {}", x),
    }
}
```

这里列出了 `x` 值，接着仅仅包含了 `..` 模式。这比不得不列出 `y: _` 和 `z: _` 要来得简单，特别是在处理有很多字段的结构体，但只涉及一到两个字段时的情形。

`..` 会扩展为所需要的值的数量。

```rust
fn main() {
    let numbers = (2, 4, 8, 16, 32);

    match numbers {
        (first, .., last) => {
            println!("Some numbers: {}, {}", first, last);
        },
    }
}
```

这里用 `first` 和 `last` 来匹配第一个和最后一个值。`..` 将匹配并忽略中间的所有值。

然而使用 `..` 必须是无歧义的。如果期望匹配和忽略的值是不明确的，Rust 会报错。如下面的例子，会得到编译错误

```rust
fn main() {
    let numbers = (2, 4, 8, 16, 32);

    match numbers {
        // second 不知道要匹配到哪个
        (.., second, ..) => {
            println!("Some numbers: {}", second)
        },
    }
}

```

### 匹配守卫

**匹配守卫**（*match guard*）是一个指定于 `match` 分支模式之后的额外 `if` 条件，它也必须被满足才能选择此分支。匹配守卫用于表达比单独的模式所能允许的更为复杂的情况。

这个条件可以使用模式中创建的变量。

```rust
let num = Some(4);

match num {
 Some(x) if x < 5 => println!("less than five: {}", x),
    Some(x) => println!("{}",x),
    None => (),
}
```

可以使用匹配守卫来解决模式中变量覆盖的问题

```rust
fn main() {
    let x = Some(5);
    let y = 10;

    match x {
        Some(50) => println!("Got 50"),
        Some(n) if n == y => println!("Matched, n = {}", n),
        _ => println!("Default case, x = {:?}", x),
    }

    println!("at the end: x = {:?}, y = {}", x, y);
}
```

第二个匹配分支中的模式不会引入一个覆盖外部 `y` 的新变量 `y`，这意味着可以在匹配守卫中使用外部的 `y`。相比指定会覆盖外部 `y` 的模式 `Some(y)`，这里指定为 `Some(n)`。此新建的变量 `n` 并没有覆盖任何值，因为 `match` 外部没有变量 `n`。

匹配守卫 `if n == y` 并不是一个模式所以没有引入新变量。这个 `y` **正是** 外部的 `y` 而不是新的覆盖变量 `y`，这样就可以通过比较 `n` 和 `y` 来表达寻找一个与外部 `y` 相同的值的概念了。

也可以在匹配守卫中使用 **或** 运算符 `|` 来指定多个模式，同时匹配守卫的条件会作用于所有的模式。

```rust
#![allow(unused)]
fn main() {
    let x = 4;
    let y = false;

    match x {
        4 | 5 | 6 if y => println!("yes"),
        _ => println!("no"),
    }
}
```

这个匹配条件表明此分支值匹配 `x` 值为 `4`、`5` 或 `6` **同时** `y` 为 `true` 的情况。运行这段代码时会发生的是第一个分支的模式因 `x` 为 `4` 而匹配，不过匹配守卫 `if y` 为假，所以第一个分支不会被选择。代码移动到第二个分支，这会匹配，此程序会打印出 `no`。

### `@`绑定

*at* 运算符（`@`）允许我们在创建一个存放值的变量的同时测试其值是否匹配模式。

我们希望测试 `Message::Hello` 的 `id` 字段是否位于 `3..=7` 范围内，同时也希望能将其值绑定到 `id_variable` 变量中以便此分支相关联的代码可以使用它。

```rust
#![allow(unused)]
fn main() {
    enum Message {
        Hello { id: i32 },
    }

    let msg = Message::Hello { id: 5 };

    match msg {
        Message::Hello { id: id_variable @ 3..=7 } => {
            println!("Found an id in range: {}", id_variable)
        },
        Message::Hello { id: 10..=12 } => {
            println!("Found an id in another range")
        },
        Message::Hello { id } => {
            println!("Found some other id: {}", id)
        },
    }
}
```

上例会打印出 `Found an id in range: 5`。通过在 `3..=7` 之前指定 `id_variable @`，我们捕获了任何匹配此范围的值并同时测试其值匹配这个范围模式。

第二个分支只在模式中指定了一个范围，分支相关代码代码没有一个包含 `id` 字段实际值的变量。`id` 字段的值可以是 10、11 或 12，不过这个模式的代码并不知情也不能使用 `id` 字段中的值，因为没有将 `id` 值保存进一个变量。

最后一个分支指定了一个没有范围的变量，此时确实拥有可以用于分支代码的变量 `id`，因为这里使用了结构体字段简写语法。不过此分支中没有像头两个分支那样对 `id` 字段的值进行测试：任何值都会匹配此分支。

使用 `@` 可以在一个模式中同时测试和保存变量值。

# 不安全的Rust

Rust 在编译时会强制执行的内存安全保证。然而，Rust 还隐藏有第二种语言，它不会强制执行这类内存安全保证：这被称为 **不安全 Rust**（*unsafe Rust*）。它与常规 Rust 代码无异，但是会提供额外的超级力量。

不安全 Rust 之所以存在，是因为静态分析本质上是保守的。当编译器尝试确定一段代码是否支持某个保证时，拒绝一些有效的程序比接受无效程序要好一些。这必然意味着有时代码可能是合法的，但是 Rust 不这么认为！在这种情况下，可以使用不安全代码告诉编译器，“相信我，我知道我在干什么。”这么做的缺点就是你只能靠自己了：如果不安全代码出错了，比如解引用空指针，可能会导致不安全的内存使用。

另一个 Rust 存在不安全一面的原因是：底层计算机硬件固有的不安全性。如果 Rust 不允许进行不安全操作，那么有些任务则根本完成不了。Rust 需要能够进行像直接与操作系统交互，甚至于编写你自己的操作系统这样的底层系统编程！这也是 Rust 语言的目标之一。

## 不安全的超级力量

可以通过 `unsafe` 关键字来切换到不安全 Rust，接着可以开启一个新的存放不安全代码的块。这里有五类可以在不安全 Rust 中进行而不能用于安全 Rust 的操作，它们称之为 “不安全的超级力量。” 这些超级力量是：

- 解引用裸指针
- 调用不安全的函数或方法
- 访问或修改可变静态变量
- 实现不安全 trait
- 访问 `union` 的字段

`unsafe` 并不会关闭借用检查器或禁用任何其他 Rust 安全检查：如果在不安全代码中使用引用，它仍会被检查。`unsafe` 关键字只是提供了那五个不会被编译器检查内存安全的功能。你仍然能在不安全块中获得某种程度的安全。

`unsafe` 不意味着块中的代码就一定是危险的或者必然导致内存安全问题：其意图在于作为程序员你将会确保 `unsafe` 块中的代码以有效的方式访问内存。

通过要求这五类操作必须位于标记为 `unsafe` 的块中，就能够知道任何与内存安全相关的错误必定位于 `unsafe` 块内，所以保持 `unsafe` 块尽可能小。

为了尽可能隔离不安全代码，将不安全代码封装进一个安全的抽象并提供安全 API 是一个好主意。标准库的一部分被实现为在被评审过的不安全代码之上的安全抽象。这个技术防止了 `unsafe` 泄露到所有你或者用户希望使用由 `unsafe` 代码实现的功能的地方，因为使用其安全抽象是安全的。

### 解引用裸指针

不安全 Rust 有两个被称为 **裸指针**（*raw pointers*）的类似于引用的新类型。和引用一样，裸指针是不可变或可变的，分别写作 `*const T` 和 `*mut T`。这里的星号不是解引用运算符；它是类型名称的一部分。在裸指针的上下文中，**不可变** 意味着指针解引用之后不能直接赋值。

与引用和智能指针的区别在于，记住裸指针

- 允许忽略借用规则，可以同时拥有不可变和可变的指针，或多个指向相同位置的可变指针
- 不保证指向有效的内存
- 允许为空
- 不能实现任何自动清理功能

通过去掉 Rust 强加的保证，你可以放弃安全保证以换取性能或使用另一个语言或硬件接口的能力，此时 Rust 的保证并不适用。

```rust
#![allow(unused)]
fn main() {
    let mut num = 5;

    let r1 = &num as *const i32;
    let r2 = &mut num as *mut i32;
}
```

注意这里没有引入 `unsafe` 关键字。可以在安全代码中 **创建** 裸指针，只是不能在不安全块之外 **解引用** 裸指针

使用 `as` 将不可变和可变引用强转为对应的裸指针类型。因为直接从保证安全的引用来创建他们，可以知道这些特定的裸指针是有效，但是不能对任何裸指针做出如此假设。

```rust
#![allow(unused)]
fn main() {
    let address = 0x012345usize;
    let r = address as *const i32;
}
```

创建一个指向任意内存地址的裸指针。尝试使用任意内存是未定义行为：此地址可能有数据也可能没有，编译器可能会优化掉这个内存访问，或者程序可能会出现段错误（segmentation fault）。

```rust
#![allow(unused)]
fn main() {
    let mut num = 5;

    let r1 = &num as *const i32;
    let r2 = &mut num as *mut i32;

    unsafe {
        println!("r1 is: {}", *r1);
        println!("r2 is: {}", *r2);
    }
}
```

创建一个指针不会造成任何危险；只有当访问其指向的值时才有可能遇到无效的值。

上述代码创建了同时指向相同内存位置 `num` 的裸指针 `*const i32` 和 `*mut i32`。相反如果尝试创建 `num` 的不可变和可变引用，这将无法编译因为 Rust 的所有权规则不允许拥有可变引用的同时拥有不可变引用。通过裸指针，就能够同时创建同一地址的可变指针和不可变指针，若通过可变指针修改数据，则可能潜在造成数据竞争。

既然存在这么多的危险，为何还要使用裸指针呢？一个主要的应用场景便是调用 C 代码接口，另一个场景是构建借用检查器无法理解的安全抽象。

### 调用不安全函数或方法

第二类要求使用不安全块的操作是调用不安全函数。不安全函数和方法与常规函数方法十分类似，除了其开头有一个额外的 `unsafe`。在此上下文中，关键字`unsafe`表示该函数具有调用时需要满足的要求，而 Rust 不会保证满足这些要求。通过在 `unsafe` 块中调用不安全函数，表明我们已经阅读过此函数的文档并对其是否满足函数自身的契约负责。

必须在一个单独的 `unsafe` 块中调用 `dangerous` 函数。如果尝试不使用 `unsafe` 块调用 `dangerous`，则会得到一个编译错误。

```rust
#![allow(unused)]
fn main() {
    unsafe fn dangerous() {}

    unsafe {
        dangerous();
    }
}
```

不安全函数体也是有效的 `unsafe` 块，所以在不安全函数中进行另一个不安全操作时无需新增额外的 `unsafe` 块。

#### 创建不安全代码的安全抽象

仅仅因为函数包含不安全代码并不意味着整个函数都需要标记为不安全的。事实上，将不安全代码封装进安全函数是一个常见的抽象。

作为一个例子，标准库中的函数，`split_at_mut`，它需要一些不安全代码。这个安全函数定义于可变 slice 之上：它获取一个 slice 并从给定的索引参数开始将其分为两个 slice。

`split_at_mut` 的用法如下：

```rust
#![allow(unused)]
fn main() {
    let mut v = vec![1, 2, 3, 4, 5, 6];

    let r = &mut v[..];

    let (a, b) = r.split_at_mut(3);

    assert_eq!(a, &mut [1, 2, 3]);
    assert_eq!(b, &mut [4, 5, 6]);
}
```

这个函数无法只通过安全 Rust 实现。一个尝试可能看起来像下面，它不能编译。出于简单考虑，我们将 `split_at_mut` 实现为函数而不是方法，并只处理 `i32` 值而非泛型 `T` 的 slice。

```rust
fn split_at_mut(slice: &mut [i32], mid: usize) -> (&mut [i32], &mut [i32]) {
    let len = slice.len();

    assert!(mid <= len);

    (&mut slice[..mid], &mut slice[mid..])
}
```

此函数首先获取 slice 的长度，然后通过检查参数是否小于或等于这个长度来断言参数所给定的索引位于 slice 当中。该断言意味着如果传入的索引比要分割的 slice 的索引更大，此函数在尝试使用这个索引前 panic。

在一个元组中返回两个可变的 slice：一个从原始 slice 的开头直到 `mid` 索引，另一个从 `mid` 直到原 slice 的结尾。

如果尝试编译代码，会得到一个错误：

```shell
error[E0499]: cannot borrow `*slice` as mutable more than once at a time
 -->
  |
6 |     (&mut slice[..mid],
  |           ----- first mutable borrow occurs here
7 |      &mut slice[mid..])
  |           ^^^^^ second mutable borrow occurs here
8 | }
  | - first borrow ends here
```

Rust 的借用检查器不能理解我们要借用这个 slice 的两个不同部分：它只知道我们借用了同一个 slice 两次。本质上借用 slice 的不同部分是可以的，因为结果两个 slice 不会重叠，不过 Rust 还没有智能到能够理解这些。当我们知道某些事是可以的而 Rust 不知道的时候，就是触及不安全代码的时候了。

```rust
#![allow(unused)]
fn main() {
    use std::slice;

    fn split_at_mut(slice: &mut [i32], mid: usize) -> (&mut [i32], &mut [i32]) {
        let len = slice.len();
        let ptr = slice.as_mut_ptr();

        assert!(mid <= len);

        unsafe {
            (slice::from_raw_parts_mut(ptr, mid),
             slice::from_raw_parts_mut(ptr.add(mid), len - mid))
        }
    }
}
```

slice 是一个指向一些数据的指针，并带有该 slice 的长度。可以使用 `len` 方法获取 slice 的长度，使用 `as_mut_ptr` 方法访问 slice 的裸指针。

在这个例子中，因为有一个 `i32` 值的可变 slice，`as_mut_ptr` 返回一个 `*mut i32` 类型的裸指针，储存在 `ptr` 变量中。保持索引 `mid` 位于 slice 中的断言。接着是不安全代码：`slice::from_raw_parts_mut` 函数获取一个裸指针和一个长度来创建一个 slice。这里使用此函数从 `ptr` 中创建了一个有 `mid` 个项的 slice。之后在 `ptr` 上调用 `add` 方法并使用 `mid` 作为参数来获取一个从 `mid` 开始的裸指针，使用这个裸指针并以 `mid` 之后项的数量为长度创建一个 slice。

`slice::from_raw_parts_mut` 函数是不安全的因为它获取一个裸指针，并必须确信这个指针是有效的。裸指针上的 `add` 方法也是不安全的，因为其必须确信此地址偏移量也是有效的指针。因此必须将 `slice::from_raw_parts_mut` 和 `add` 放入 `unsafe` 块中以便能调用它们。通过观察代码，和增加 `mid` 必然小于等于 `len` 的断言，我们可以说 `unsafe` 块中所有的裸指针将是有效的 slice 中数据的指针。这是一个可以接受的 `unsafe` 的恰当用法。

注意无需将 `split_at_mut` 函数的结果标记为 `unsafe`，并可以在安全 Rust 中调用此函数。我们创建了一个不安全代码的安全抽象，其代码以一种安全的方式使用了 `unsafe` 代码，因为其只从这个函数访问的数据中创建了有效的指针。

> 随意使用`slice::from_raw_parts_mut`是十分危险的，很有可能会造成崩溃。如下面的例子
>
> ```rust
> #![allow(unused)]
> fn main() {
>     use std::slice;
> 
>     let address = 0x01234usize;
>     let r = address as *mut i32;
> 
>     let slice: &[i32] = unsafe {
>         slice::from_raw_parts_mut(r, 10000)
>     };
> }
> ```
>
> 获取任意内存地址并创建了一个长为一万的 slice。虽然编译通过，并可运行，但会直接造成运行时的错误。

### 使用 `extern` 函数调用外部代码

 Rust 代码可能需要与其他语言编写的代码交互。为此 Rust 有一个关键字，`extern`，有助于创建和使用 **外部函数接口**（*Foreign Function Interface*， FFI）。外部函数接口是一个编程语言用以定义函数的方式，其允许不同（外部）编程语言调用这些函数。

`extern` 块中声明的函数在 Rust 代码中总是不安全的。因为其他语言不会强制执行 Rust 的规则且 Rust 无法检查它们，所以确保其安全是程序员的责任：

```rust
extern "C" {
    // C 标准库中的 abs 函数
    fn abs(input: i32) -> i32;
}

fn main() {
    unsafe {
        println!("Absolute value of -3 according to C: {}", abs(-3));
    }
}
```

在 `extern "C"` 块中，列出了我们希望能够调用的另一个语言中的外部函数的签名和名称。`"C"` 部分定义了外部函数所使用的 **应用二进制接口**（*application binary interface*，ABI） —— ABI 定义了如何在汇编语言层面调用此函数。`"C"` ABI 是最常见的，并遵循 C 编程语言的 ABI。

> 从其它语言调用 Rust 函数
>
> 也可以使用 `extern` 来创建一个允许其他语言调用 Rust 函数的接口。不同于 `extern` 块，就在 `fn` 关键字之前增加 `extern` 关键字并指定所用到的 ABI。还需增加 `#[no_mangle]` 注解来告诉 Rust 编译器不要 mangle 此函数的名称。*Mangling* 发生于当编译器将我们指定的函数名修改为不同的名称时，这会增加用于其他编译过程的额外信息，不过会使其名称更难以阅读。每一个编程语言的编译器都会以稍微不同的方式 mangle 函数名，所以为了使 Rust 函数能在其他语言中指定，必须禁用 Rust 编译器的 name mangling。
>
> 在如下的例子中，一旦其编译为动态库并从 C 语言中链接，`call_from_c` 函数就能够在 C 代码中访问：
>
> ```rust
> #[no_mangle]
> pub extern "C" fn call_from_c() {
>     println!("Just called a Rust function from C!");
> }
> ```
>
> `extern` 的使用无需 `unsafe`。

### 访问和修改可变静态变量

对于 全局变量（global variables），Rust 是支持的，不过这对于 Rust 的所有权规则来说是有问题的。如果有两个线程访问相同的可变全局变量，则可能会造成数据竞争。

全局变量在 Rust 中被称为 **静态**（*static*）变量。

```rust
static HELLO_WORLD:&str = "Hello, World";

fn main(){
    println!("name is: {}", HELLO_WORLD);
}
```

`static` 变量类似于常量。通常静态变量的名称采用 `SCREAMING_SNAKE_CASE` 写法，并 **必须** 标注变量的类型，在这个例子中是 `&'static str`。静态变量只能储存拥有 `'static` 生命周期的引用，这意味着 Rust 编译器可以自己计算出其生命周期而无需显式标注。访问不可变静态变量是安全的。

常量与不可变静态变量可能看起来很类似，不过一个微妙的区别是静态变量中的值有一个固定的内存地址。使用这个值总是会访问相同的地址。另一方面，常量则允许在任何被用到的时候复制其数据。

常量与静态变量的另一个区别在于静态变量可以是可变的。访问和修改可变静态变量都是 **不安全** 的。

```rust
static mut COUNTER: u32 = 0;

fn add_to_count(inc: u32) {
    unsafe {
        COUNTER += inc;
    }
}

fn main() {
    add_to_count(3);

    unsafe {
        println!("COUNTER: {}", COUNTER);
    }
}
```

就像常规变量一样，我们使用 `mut` 关键来指定可变性。任何读写 `COUNTER` 的代码都必须位于 `unsafe` 块中。这段代码可以编译并如期打印出 `COUNTER: 3`，因为这是单线程的。拥有多个线程访问 `COUNTER` 则可能导致数据竞争。

拥有可以全局访问的可变数据，难以保证不存在数据竞争，这就是为何 Rust 认为可变静态变量是不安全的。

### 实现不安全 trait

最后一个只能用在 `unsafe` 中的操作是实现不安全 trait。当至少有一个方法中包含编译器不能验证的不变量时 trait 是不安全的。可以在 `trait` 之前增加 `unsafe` 关键字将 trait 声明为 `unsafe`，同时 trait 的实现也必须标记为 `unsafe`

```rust
#![allow(unused)]
fn main() {
    unsafe trait Foo {
        // methods go here
    }

    unsafe impl Foo for i32 {
        // method implementations go here
    }
}
```

编译器会自动为完全由 `Send` 和 `Sync` 类型组成的类型自动实现他们。如果实现了一个包含一些不是 `Send` 或 `Sync` 的类型，比如裸指针，并希望将此类型标记为 `Send` 或 `Sync`，则必须使用 `unsafe`。Rust 不能验证我们的类型保证可以安全的跨线程发送或在多线程间访问，所以需要我们自己进行检查并通过 `unsafe` 表明。

### 访问联合体中的字段

`union` 和 `struct` 类似，但是在一个实例中同时只能使用一个声明的字段。联合体主要用于和 C 代码中的联合体交互。访问联合体的字段是不安全的，因为 Rust 无法保证当前存储在联合体实例中数据的类型。
