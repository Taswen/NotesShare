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

### 信息指定

```cmake
cmake_minimum_required(VERSION 2.8)  ///检查cmake的版本，至少为2.8
```

### 项目生成

使用`PROJECT` 关键字可以指定生成一个工程

```cmake
PROJECT(helloworld)    # 工程名为 helloworld
```



当工程名含有空白符时，可以使用`"`将其作为一个字符串。同时可以添加第二个参数来表明工程使用的语言。

```cmake
PROJECT("hello world" C) 
```



### 源文件指定

源代码文件和头文件加入变量SRC_LIST

```cmake
aux_source_directory(.  SRC_LIST) ///查找当前目录下所有的源文件并保存到SRC_LIST变量中
```





#### 指定头文件

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

### 链接库文件

```cmake
# 通过在主工程文件CMakeLists.txt中修改ADD_SUBDIRECTORY (lib) 指令来指定一个编译输出位置;
# 指定本工程中静态库libhello.so生成的位置，即 build/lib;
ADD_SUBDIRECTORY(lib)	
```



### 生成结果

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





# 多种构建情况

## 单目录

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



## 多目录

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

