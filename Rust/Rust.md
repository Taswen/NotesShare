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
