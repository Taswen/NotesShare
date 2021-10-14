# Cmake 常用命令

## 基本知识

### 变量

```cmake
#CMake使用${}来获取值，但是在IF语句里面可以直接使用变量名
${make_env_dir}
```

### 环境变量

```cmake
#环境变量使用$ENV{}来获取，设置环境变量还用：set({ENV{VAR} VALUE)来设置
$ENV{QT_dir}
set()
```

### 指令

指令和大小写无关，但是参数以及变量是和大小有关的

```cmake
#cmake指令格式：指令（参数1 参数2）：参数与参数中间使用空格分开
add_executable(Dmeo Source.cpp)
```



## 基本命令



### Cmake本身

#### `SET` 设定变量值

可以使用`SET` 对变量进行赋值。如果变量存在则刷新值。如果不存在则新建一个变量并赋值。

```cmake
SET(SRC_FILE "src/main.cpp")
```



#### `FOREACH`语句块范围遍历





### 工具与环境相关

#### `cmake_minimum_required` cmake版本规范

使用`cmake_minimum_required`指定构建所需的最低Cmake版本。

```cmake
cmake_minimum_required(VERSION 3.8)  ///检查cmake的版本，至少为3.8
```



#### `configure_file` 生成配置头文件





### 项目与组织相关

#### `PROJECT` 指定工程信息

使用`PROJECT` 关键字可以指定生成一个工程。

`project`命令并非必不可少，如果没有调用`project`命令，cmake仍然会生成一个默认的工程名“`Project`”，以及工程名对应的变量。但是`VERSION`、`DESCRIPTION`、`HOMEPAGE_URL`等选项对应的变量不会被赋值（`LANGUAGES`例外，即使不指定，默认语言为`C`和`CXX`）。

`project`命令需要放置在其他命令调用之前，在`cmake_minimum_required`之后。

如果多次调用`project`命令，那么`CMAKE_PROJECT_NAME`、`CMAKE_PROJECT_VERSION`、`CMAKE_PROJECT_DESCRIPTION`、`CMAKE_PROJECT_HOMEPAGE_URL`等变量是以最近一次调用的`project`命令为准。

##### 工程名

默认情况下，只填入一个参数（不是任何关键字），那么指定的是工程的名称

```cmake
PROJECT(helloworld)    # 工程名为 helloworld
```

> 当工程名含有空白符时，可以使用`"`将其作为一个字符串。
>
> ```cmake
> PROJECT("hello world") 
> ```
>

也可以显示的使用 `NAME`关键字指定

```cmake
PROJECT(NAME "hello world")
```

在调用project命令指定当前工程名字的同时，cmake内部会为如下变量赋值：

- `PROJECT_NAME`：将当前工程的名称赋值给`PROJECT_NAME`
- `PROJECT_SOURCE_DIR`：当前工程的源码路径。
- `<PROJECT-NAME>_SOURCE_DIR`：指定工程的源码路径，这个变量和`PROJECT_SOURCE_DIR`的区别就是，`<PROJECT-NAME>_SOURCE_DIR`跟具体的工程名字关联起来，若`<PROJECT-NAME>`就是当前工程，则该变量和`PROJECT_SOURCE_DIR`相等。

- `PROJECT_BINARY_DIR`：当前工程的二进制路径。
- `<PROJECT-NAME>_BINARY_DIR`：指定工程的二进制路径，这个变量和`PROJECT_BINARY_DIR`的区别就是，`<PROJECT-NAME>_BINARY_DIR`跟具体的工程名字关联起来，若`<PROJECT-NAME>`就是当前工程，则该变量和`PROJECT_BINARY_DIR`相等。
- `CMAKE_PROJECT_NAME`：顶层工程的名称。例如当前调用的CMakeLists.txt位于顶层目录（可以理解为使用cmake命令首次调用的那个CMakeLists.txt），那么工程名还会赋值给`CMAKE_PROJECT_NAME`。





除了最基础的工程名称，它还可以指定cmake工程的版本号（`VERSION`关键字）、简短的描述（`DESCRIPTION`关键字）、主页URL（`HOMEPAGE_URL`关键字）和编译工程使用的语言（`LANGUAGES`关键字）。

##### 工程版本

使用`VERSION <version>`指定工程的版本，其中`<version>`为非负整数组成的一个点分版本号格式`<major>[.<minor>[.<patch>[.<tweak>]]]`,例如`1.2.3.4`。基本用法如下：

```cmake
# CMakeLists.txt
cmake_minimum_required (VERSION 3.10.2)
project (mytest VERSION 1.2.3.4)
```

当`project`命令使用了`VERSION`选项，如下变量会被相应的赋值：

- `PROJECT_VERSION`
- `<PROJECT-NAME>_VERSION`
- `PROJECT_VERSION_MAJOR`
- `<PROJECT-NAME>_VERSION_MAJOR`
- `PROJECT_VERSION_MINOR`
- `<PROJECT-NAME>_VERSION_MINOR`
- `PROJECT_VERSION_PATCH`
- `<PROJECT-NAME>_VERSION_PATCH`
- `PROJECT_VERSION_TWEAK`
- `<PROJECT-NAME>_VERSION_TWEAK`
- `CMAKE_PROJECT_VERSION`

上述带`<PROJECT_NAME>`的变量存储的是指定工程名下版本号，不带的表示当前正在调用的工程的版本号。`XXX_MAJOR`、`XXX_MINOR`、`XXX_PATCH`、`XXX_TWEAK`分别与点分版本号`<major>[.<minor>[.<patch>[.<tweak>]]]`对应。当然，如果CMakeLists.txt位于顶层目录，`CMAKE_PROJECT_VERSION`存储的是顶层CMakeLists.txt中`project`命令指定的版本号，不会随着调用工程的变化而变化。



##### 工程文本描述

使用`DESCRIPTION <project-description-string>` 指定对工程的文本简短描述，文本信息不易太长。

```cmake
# CMakeLists.txt
cmake_minimum_required (VERSION 3.10.2)
project (mytest DESCRIPTION "This is mytest project.")
```

调用该选项的会对如下变量赋值：

- `PROJECT_DESCRIPTION`
- `<PROJECT-NAME>_DESCRIPTION`
- `CMAKE_PROJECT_DESCRIPTION`

特别的，当CMakeLists.txt位于顶层目录，`CMAKE_PROJECT_DESCRIPTION`存储的是顶层CMakeLists.txt中`project`命令指定的工程描述，不会随着调用工程的变化而变化。



##### 工程的主页URL

使用`HOMEPAGE_URL <project-home_url-string>` 指定工程的URL地址。

```cmake
# CMakeLists.txt
cmake_minimum_required (VERSION 3.10.2)
project (mytest HOMEPAGE_URL “https://www.XXX(示例).com”)
```



##### 构建语言

使用`LANGUAGES <language>` 指定工程使用的语言。包括的选项有`C`、`CXX`（例如C++）、`CUDA`、`OBJC`（例如Objective-C）、`OBJCXX`、`Fortran`、`ASM`。可以指定多个。

如果没有指定，默认使用的是`C`和`CXX`。如果使用`LANGUAGES NONE`、或仅仅列出`LANGUAGES`关键字却没有指定具体的语言，那么表示不支持任何语言。如果需要使能`ASM`，把它放在列表的最后以便cmake能够检查其他语言（例如C语言）能否工作在汇编下。

```cmake
# CMakeLists.txt
cmake_minimum_required (VERSION 3.10.2)
project (mytest LANGUAGES "CPP")
# 或者
project (mytest LANGUAGES "CXX ASM")
```



如果直接跟在工程名后面，可以省略`LANGUAGES`关键字；但跟在其他关键字（例如`VERSION`）后面，`LANGUAGES`关键字不能省略。

```cmake
PROJECT("hello world" C)
```





该命令的实质是cmake会使用`LANGUAGES`后的语言选项`XXX`来检查`CMAKE_XXX_COMPILER`指定的编译器是否存在，以便工程能正确的被构建。





#### `FILE`文件收集

一般情况下，可以通过逐个列出的方式，设定源文件集(`SET(SRC_LIST src/main.c src/other.c)`)。这样的好处是可以**明确控制那些文件会被加入到工程中**。 

但有些时候源文件较多，一个一个列出来的话可能会有些麻烦，对于这种情况，可以使用CMake提供的`FILE`命令来自动收集文件列表。

```cmake
FILE(GLOB_RECURSE 
  SRC_CORE 
	src/*.c
)
```

其会生成一个源文件列表，每一项是一个**源文件的全路径**。

其中`GLOB_RECURSE` 是对指定路径进行递归遍历。



> 如果想要剔除一些不想加入工程的文件，可以结合其他指令
>
> ```cmake
> foreach(rm_file ${SRC_CORE})
>     string(REGEX MATCH ".*/filename1.c|.*/filename2.c" need_remove_file ${rm_file})
>     if(need_remove_file)
>         list(REMOVE_ITEM SRC_CORE ${need_remove_file})
>     endif(need_remove_file)
> endforeach(rm_file)
> ```
>
> 上面使用了CMake的正则表达式，匹配两个文件，分别为`.*/filename1.c`和`.*filename2.c`，其中`.*`代表了任意字符串，`filename1.c`是文件名。



#### 源文件指定

源代码文件和头文件加入变量SRC_LIST

```cmake
aux_source_directory(.  SRC_LIST) ///查找当前目录下所有的源文件并保存到SRC_LIST变量中
```





##### 指定头文件

cmake本身不提供任何搜索库的便捷方法，所有搜索库并给变量赋值的操作必须由cmake代码完成.

```cmake
# 将根目录下的include和abc加入包含目录列表
INCLUDE_DIRECTORIES(${PROJECT_SOURCE_DIR}/include ${PROJECT_SOURCE_DIR}/abc)  
```



```cmake



LINK_DIRECTORIES(${PROJECT_SOURCE_DIR}/lib)  ///将 ./lib加入编译器链接阶段的搜索目录列表
add_executable(hello  $(SRC_LIST})  ///使用SRC_LIST源文件列表里的文件生成一个可执行文件hello #如：add_executable(hello main.cpp base.cpp base.h)  
add_library(hello STATIC ${SRC_LIST})  /// 使用SRC_LIST源文件列表里的文件生成一个静态链接libhello.a
add_library(hello SHARD ${SRC_LIST})  //// 使用SRC_LIST源文件列表里的文件生成一个动态链接库libhello.so
target_link_libraries(hello a b.a c.so) /// 将若干库文件链接到目标hello中，target_link_libraries里的库文件的顺序符合gcc/g++链接顺序的规则，即被依赖的库放在依赖它的库的后面，如果顺序有错，链接时会报错。
```

#### 链接库文件

```cmake
# 通过在主工程文件CMakeLists.txt中修改ADD_SUBDIRECTORY (lib) 指令来指定一个编译输出位置;
# 指定本工程中静态库libhello.so生成的位置，即 build/lib;
ADD_SUBDIRECTORY(lib)	
```



#### 生成结果

```cmake

```



## 预定义的变量

### `PROJECT_NAME` 

通过 project() 指定的项目名称后就可以使用这个变量来取用项目名称了。



### `PROJECT_SOURCE_DIR`

工程所在的目录，其为包含`PROJECT()`的最近一个CMakeLists.txt文件所在的文件夹相对于根文件夹的路径。

比如

```shell
project_name
   |
   +-----build
   |   
   +-----include   
   |
   +-----lib
   |
   +-----src
   |       |             
   |       +main.cpp
   |       |            
   |       +CMakeLists.txt   
   |   
   +-----CMakeLists.txt
```

如果`src/CMaleLists.txt` 中含有`PROJECT()` 语句，则在`src/CMaleLists.txt` 中所指代的`PROJECT_SOURCE_DIR`都将是`project_name/src`



### `PROJECT_BINARY_DIR`

执行 cmake 命令的目录



`CMAKE_CURRENT_SOURCE_DIR` : 当前 CMakeList.txt 文件所在的目录



`CMAKE_CURRENT_BINARY_DIR` : 编译目录，可使用 add subdirectory 来修改



`EXECUTABLE_OUTPUT_PATH` : 二进制可执行文件输出位置



`LIBRARY_OUTPUT_PATH` : 库文件输出位置



`BUILD_SHARED_LIBS` : 默认的库编译方式 ( shared 或 static ) ，默认为 static



`CMAKE_C_FLAGS` : 设置 C 编译选项



`CMAKE_CXX_FLAGS` : 设置 C++ 编译选项



`CMAKE_CXX_FLAGS_DEBUG` : 设置编译类型 Debug 时的编译选项



`CMAKE_CXX_FLAGS_RELEASE` : 设置编译类型 Release 时的编译选项



`CMAKE_GENERATOR` : 编译器名称



`CMAKE_COMMAND` : CMake 可执行文件本身的全路径



`CMAKE_BUILD_TYPE` : 工程编译生成的版本， Debug / Release





# 常见构建常见

## 单目录结构工程

可以直接指定所用的源文件

```cmake
#CMakeLists.txt文件

#CMake要求的最低点版本
cmake_minimum_requried(version 2.8)
 
#项目名称
project (projectName)
 
#指定生成的目标名称和构建所用的所有源文件，多个源文件用空格隔开
add_executable(projectName Source1.cpp Source1.cpp) 
```

也可以指定目录来读取源文件

```cmake
# CMake 最低版本号要求
cmake_minimum_required (VERSION 2.8)
 
# 项目信息
project (Demo)
 
# 查找当前目录下的所有源文件
# 并将名称保存到 DIR_SRCS 变量
aux_source_directory(. DIR_SRCS)
 
# 指定生成目标
add_executable(Demo ${DIR_SRCS})
```



## 多目录包含工程

根目录我们假设为：Source.cpp在./Source目录下面，但是./Source目录里面还有一个名叫：Child目录，里面存放Child.cpp工程。对于这种情况，我们首先需要一个在Source目录和Child目录都去编写一个CMakeLists.txt文件，为了方便，我们可以将Child目录下面的Child.cpp工程编译成一个静态库，然后在Source.cpp里面再去调用这个静态库，这样我们的CMakeLists.txt就是下面这样：

```cmake
#Source目录下的CMakeLists.txt文件
#CMake的最低版本
cmake_minimum_requried(VERSION 2.8)
 
#项目信息
project(Demo)
 
#查找当前目录下的所有源文件，然后将源文件名保存到变量DIR_SRCS里面
aux_source_directory(. DIR_SRCS)
 
#添加Child子目录,Child下面的CMakeLists.txt文件和源码也会被处理,最后生成静态库
add_subdirectory(Child)
 
#指定生成目标
add_executable(Demo Source.cpp)
 
#添加链接库，指明生成的Demo可执行文件需要连接一个叫做：Child的链接库
target_link_libraries(Demo Child)
```

```cmake
#child目录下的CMakeLists.txt文件
#查找当前目录下的所有源文件，然后将源文件的名称储存到DIR_LIB_SRCS变量里面
aux_source_directory(. DIR_LIB_SRCS)
 
#生成链接库,将变量DIR_LIB_SRCS里面的源文件编译成静态链接库
add_library(Child DIR_LIB_SRCS)
```



## 同时生成静态库和动态库

```cmake
# 添加控制选项
# BUILD_SHARED_LIBS 是CMake内置变量
# 针对WIN32可以

if(WIN32)
    option(BUILD_SHARED_LIBS "Build Shared libs" ON)
    option(BUILD_AS_DLL "Build as dll" ${BUILD_SHARED_LIBS})
endif()

add_library(libHello ${SRC_LIST})
target_link_libraries(libHello ${EXTRA_LIB})

if(BUILD_AS_DLL)
    set_target_properties(libHello PROPERTIES COMPILE_DEFINITIONS BUILD_AS_DLL)
endif()
```

