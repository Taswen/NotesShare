# 包管理

## 包声明

代码文件的开头一般为包的声明，使用`packge`关键字声明代码文件所在的包：

```kotlin
package zk.test.Hello

fun main(args: Array<String>) {
    println("Hello Kotlin")    
}

```

Kotlin源文件不需要相匹配的目录和包，源文件可以放在任何文件目录。

如果没有指定包，默认为 `default` 包。



## 包导入

使用`import`关键字声明要导入的依赖包：

```kotlin
package zk.test.Hello
import java.util.*
fun main(args: Array<String>) {
    System.out.println("From java")
    println("Hello Kotlin")    
}
```



有多个包会默认导入到每个 Kotlin 文件中：

- kotlin.*
- kotlin.annotation.*
- kotlin.collections.*
- kotlin.comparisons.*
- kotlin.io.*
- kotlin.ranges.*
- kotlin.sequences.*
- kotlin.text.*



另外，也可以导入Java的包，而java的默认包也会导入，所以可以在Kotlin源码中无缝使用Java方法。









# 变量

## 定义

使用`val`和`var`关键字来进行变量的定义，如果未指明变量类型，在会根据赋值使用自动推导

```kotlin
val	a = 1		# 声明不可变变量 ，对应Java中的final变量
var b = 1.0		# 用来声明可变变量，可以被重新赋值
```

可以显示指定类型，需要注意的是基本类型（Int，double）在kotlin中都是使用类实现的。

```kotlin
var a:Int = 10
```

常量与变量都可以没有初始化值,但是在引用前必须初始化。如果没有初始化则必须在声明时指定变量类型。











## 字符串模板

`$` 表示一个变量名或者变量值

`$varName` 表示变量值

`${varName.fun()} ` 表示变量的方法返回值:

```kotlin
var a = 1
// 模板中的简单名称：
val s1 = "a is $a" 

a = 2
// 模板中的任意表达式：
val s2 = "${s1.replace("is", "was")}, but now is $a"
```



## 数据类型

在kotlin中，万物皆对象，没有基础数据类型，只有封装的数字类型，你每定义的一个变量，其实 Kotlin 帮你封装了一个对象，这样可以保证不会出现空指针。

### Kotlin 基本数据类型

Kotlin 的基本数值类型包括 Byte、Short、Int、Long、Float、Double 等。不同于 Java 的是，字符不属于数值类型，是一个独立的数据类型。

| 类型   | 位宽度 |
| :----- | :----- |
| Double | 64     |
| Float  | 32     |
| Long   | 64     |
| Int    | 32     |
| Short  | 16     |
| Byte   | 8      |



|          | java写法              | kotlin写法                             |          | java写法              | kotlin写法                        |
| :------- | :-------------------- | :------------------------------------- | :------- | :-------------------- | :-------------------------------- |
| 基础类型 | byte byteValue;       | var byteValue: Byte = 0                | 包装类型 | Byte byteValue;       | val byteValue: Byte? = null       |
|          | short shortValue;     | var shortValue: Short = 0              |          | Short shortValue;     | val shortValue: Short? = null     |
|          | int intValue;         | var intValue: Int = 0                  |          | Integer intValue;     | val intValue: Int? = null         |
|          | long longValue;       | var longValue: Long = 0                |          | Long longValue;       | val longValue: Long? = null       |
|          | float floatValue;     | var floatValue: Float = 0.toFloat()    |          | Float floatValue;     | val floatValue: Float? = null     |
|          | double doubleValue;   | var doubleValue: Double = 0.toDouble() |          | Double doubleValue;   | val doubleValue: Double? = null   |
|          | char charValue;       | var charValue: Char = ' '              |          | Character charValue;  | val charValue: Char? = null       |
|          | boolean booleanValue; | var booleanValue: Boolean = false      |          | Boolean booleanValue; | val booleanValue: Boolean? = null |



和 Java 不一样，Kotlin 中的 Char 不能直接和数字操作，Char 必需是单引号 **'** 包含起来的。比如普通字符 '0'，'a'。

特殊字符可以用反斜杠转义。 支持这几个转义序列：\t、 \b、\n、\r、\'、\"、\\ 和 \$。 编码其他字符要用 Unicode 转义序列语法：'\uFF00'。







### 键值对

Kotlin使用 `to` 可以建立映射关系的键值对，`to`不是关键字，而是一个infix函数

```kotlin
println(12 to 13)
```





## 字面常量

下面是所有类型的字面常量：

- 十进制：123
- 长整型以大写的 L 结尾：123L
- 16 进制以 0x 开头：0x0F
- 2 进制以 0b 开头：0b00001011
- 注意：8进制不支持

Kotlin 同时也支持传统符号表示的浮点数值：

- Doubles 默认写法: `123.5`, `123.5e10`
- Floats 使用 f 或者 F 后缀：`123.5f`

你可以使用下划线使数字常量更易读：

```kotlin
val oneMillion = 1_000_000
val creditCardNumber = 1234_5678_9012_3456L
val socialSecurityNumber = 999_99_9999L
val hexBytes = 0xFF_EC_DE_5E
val bytes = 0b11010010_01101001_10010100_10010010
```









### NULL检查机制

Kotlin的空安全设计对于声明可为空的参数，在使用时要进行空判断处理，有两种处理方式，字段后加`!!`像Java一样抛出空异常，另一种字段后加`?`可不做处理返回值为`null`或配合`?:`做空判断处理

```kotlin
//类型后面加?表示可为空
var age: String? = "23" 
//抛出空指针异常
val ages = age!!.toInt()
//不做处理返回 null
val ages1 = age?.toInt()
//age为空返回-1
val ages2 = age?.toInt() ?: -1
```

当一个引用可能为 `null` 值时, 对应的类型声明必须明确地标记为可为 `null`。如下当 `str` 中的字符串内容不是一个整数时, 返回 `null`:

```kotlin
fun parseInt(str: String): Int? {
  // ...
}
```

以下实例演示如何使用一个返回值可为 null 的函数:

```kotlin
fun main(args: Array<String>) {
  if (args.size < 2) {
    print("Two integers expected")
    return
  }
  val x = parseInt(args[0])
  val y = parseInt(args[1])
  // 直接使用 `x * y` 会导致错误, 因为它们可能为 null.
  if (x != null && y != null) {
    // 在进行过 null 值检查之后, x 和 y 的类型会被自动转换为非 null 变量
    print(x * y)
  }
}
```





### 类型检测及自动类型转换

可以使用 `is` 运算符检测一个表达式是否某类型的一个实例(类似于Java中的`instanceof`关键字)。

```kotlin
fun getStringLength(obj: Any): Int? {
  if (obj is String) {
    // 做过类型判断以后，obj会被系统自动转换为String类型
    return obj.length 
  }

  //在这里还有一种方法，与Java中instanceof不同，使用!is
  // if (obj !is String){
  //   // XXX
  // }

  // 这里的obj仍然是Any类型的引用
  return null
}
```

或者

```kotlin
fun getStringLength(obj: Any): Int? {
  if (obj !is String)
    return null
  // 在这个分支中, `obj` 的类型会被自动转换为 `String`
  return obj.length
}
```

甚至还可以

```kotlin
fun getStringLength(obj: Any): Int? {
  // 在 `&&` 运算符的右侧, `obj` 的类型会被自动转换为 `String`
  if (obj is String && obj.length > 0)
    return obj.length
  return null
}
```









# 函数

## 定义

函数定义使用关键字 `fun`，如下

```kotlin
fun testFunc(a:Int,b:Int):Int{
	return a+b    
}
```

函数参数格式为：`参数名:类型`。形参和实参之间不支持隐式转换。

返回值类型则在参数列表后面指定`fun f():类型`，当函数没有返回值时可以默认不写，如下

```kotlin
fun testFunc(a:Int,b:Int){
	println(a+b)  
}
```

函数没有返回值时，如果使用 `return` 语句则返回空值 `null`



### 单行函数定义

当函数的语句只有一句时，可以以赋值表达式的形式定义函数，函数的返回值就是表达式的值，而一般情况下返回类型不指定的话会自动推断（ public 方法则必须明确写出返回类型）：

```kotlin
fun equ(a:Int,b:Int) = a==b
public fun sum(a: Int, b: Int): Int = a + b
```

需要注意的是，赋值语句不是表达式，没有值，不能用作单行函数定义。



### 可变长参数函数定义

函数的变长参数可以用 `vararg` 关键字进行标识：

```kotlin
fun vars(vararg v:Int){
    for(vt in v){
        print(vt)
    }
}

// 测试
fun main(args: Array<String>) {
    vars(1,2,3,4,5)  // 输出12345
}
```



### lambda(匿名函数)

Lambda表达式的语法结构：

```kotlin
{参数名1: 参数类型, 参数名2: 参数类型 -> 函数体}
```

最外层是一对大括号，如果有参数传入到 Lambda表达式中的话，还需要声明参数列表，参数列表的结尾使用一个`->`符号，表示参数列表的结束以及函数体的开始。

函数体中可以编写任意行代码（虽然不建议编写太长的代码），并且最后一行代码会自动作为Lambda表达式的返回值。

lambda表达式使用实例：

```kotlin
// 测试
fun main(args: Array<String>) {
    val sumLambda: (Int, Int) -> Int = {x,y -> x+y}
    println(sumLambda(1,2))  // 输出 3
}
```





以上是Lambda表达式完整的语法结构，还有有很多种简化的写法。

Kotlin规定，当Lambda参数是函数的最后一个参数时，可以将Lambda表达式移到函数括号的外面

```kotlin
val maxLengthFruit = list.maxBy({ fruit: String -> fruit.length })
// list.maxBy是一个普通的函数,它接收的是一个Lambda类型的参数，并且会
// 在遍历集合时将每次遍历的值作为参数传递给Lambda表达式，并获取处理后的值
// 作为判断依据。根据传入的条件来遍历集合，从而找到该条件下的最大值，
```

可简化为

```kotlin
val maxLengthFruit = list.maxBy() { fruit: String -> fruit.length }
```

Kotlin还规定，如果Lambda参数是函数的唯一一个参数的话，还可以将函数的括号省略

则上述表达式可以进一步化简为

```kotlin
val maxLengthFruit = list.maxBy { fruit: String -> fruit.length }
```

Kotlin拥有出色的类型 推导机制，Lambda表达式中的参数列表其实在大多数情况下不必声明参数类型

```kotlin
val maxLengthFruit = list.maxBy{fruit->fruit.lenth}
```

同时kotlin规定，当Lambda表达式的参数列表中只有一个参数时，也不必声明参数名，而是可以使用it 关键字来代替，则最后其可化简为

```kotlin
val maxLengthFruit = list.maxBy { it.length }
```



这种形式的lambda表达式最常用在函数式API，用来操作集合

例如集合中的map函数是最常用的一种函数式API，它用于将集合中的每个元素都映射成一个另外的 值，映射的规则在Lambda表达式中指定，最终生成一个新的集合。

```kotlin
val newList = list.map { it.toUpperCase() }	// 让所有的水果名都变成大写模式
```





### 匿名表达式

```kotlin
// 使用匿名函数绑定点击事件
button1.setOnClickListener{
    Toast.makeTexxt(this,"You click Button 1",Tosat.LENGTH_SHORT).show()
}
```





## 函数API

### filter函数

用来过滤集合中的数据的，它可以单独使用，也可以配合map函数等一起使用

```kotlin
//只想保留5个字母以内的水果
val list = listOf("Apple", "Banana", "Orange", "Pear", "Grape", "Watermelon")
val newList = list.filter{ it.length <= 5 }.map{ it.toUpperCase() }
```



### any函数

用于判断集 合中是否至少存在一个元素满足指定条件





### all函数

用于判断集合中是否所有元素都满足指定条件



# 语句

## 注释

Kotlin 支持单行和多行注释，实例如下：

```kotlin
// 这是一个单行注释

/* 这是一个多行的
   块注释。 */
```

与 Java 不同, Kotlin 中的块注释允许嵌套。





## 条件语句

### if语句

kotlin的if语句与普通的类C语言形式相同，也和它们具有相同的功能。

而相较于普通的类C语言，Kotlin的if语句不同的地方在于有返回值，为语句块中的最后一行代码的值。

```kotlin
fun maxInt(num1: Int, num2: Int): Int {
    val value = if (num1 > num2) {
                    num1
                } else {
                    num2
                }
    return value
}
// 可简化为以下形式
fun maxInt(num1:int, num2:Int) = if(num1>num2) num1 else num2
```

### when语句

相当于C语言的switch语句，其和if语句一样以每个语句块的最后一句为返回值。

```kotlin
fun f(name:String) = when(name){
    "Jake"-> true
    else -> 0
}
```



另外，可以使用 `is` 关键字判断类型，相当于Java中的instanceof关键字

```kotlin
when(num){
    is Int -> Println("Is Int")
    is Double ->Println("Is Int")
    else -> Println("Not Surpport")
}
```



when语句还有一种不带参数的用法，将判断的表达式完整地写在when的结构体当中。

```kotlin
fun getSocre(name:String) = when{
    name.startsWith("Tom") -> 90
    name == "Jim" -> 87
    name == "Jack" -> 66
    else -> 0
}
```

> Kotlin中判断字 符串或对象是否相等可以直接使用==关键字，而不用像Java那样调用equals()方法



## 循环语句

### for-in语句

#### 区间

区间表达式由具有操作符形式 `..` 的 rangeTo 函数辅以 in 和 !in 形成。

```kotlin
val range = 0..10
```

上述代码表示创建了一个0 到10的区间，并且两端都是闭区间



Kotlin中可以使用until关键字来创建一个左闭右开的区间	

区间是为任何可比较类型定义的，但对于整型原生类型，它有一个优化的实现。以下是使用区间的一些示例:

```kotlin
for (i in 1..4) print(i) // 输出“1234”

for (i in 4..1) print(i) // 什么都不输出

if (i in 1..10) { // 等同于 1 <= i && i <= 10
    println(i)
}

// 使用 step 指定步长
for (i in 1..4 step 2) print(i) // 输出“13”

for (i in 4 downTo 1 step 2) print(i) // 输出“42”


// 使用 until 函数排除结束元素
for (i in 1 until 10) {   // i in [1, 10) 排除了 10
     println(i)
}
```





# 泛型

泛型主要有两种定义方式：一种是定义泛型类，另一种是定义泛型方法，使用的语法结构都是后跟`<T>` T为随意的字符串，用来表示实际使用时的传入的类型。

## 泛型类

```kotlin
class MyClass<T> {
    fun method(param: T): T {
        return param
    }
}
```

T类型便可用作参数和返回值。

在调用时将泛型指定成具体的类型

```kotlin
val myClass = MyClass<Int>()
val result = myClass.method(123)
```

## 泛型方法

```kotlin
fun <T> method(param: T): T {
    return param
}
```

调用

```kotlin
var result = method<Int>(123)
```



## 推导与限制

## 推导

Kotlin拥有非常出色的类型推导机制，能够根据实际传入的静态参数自动推导出泛型的类型，因此调用时可以直接省略泛型的指定

```kotlin
val result = myClass.method(123)
```

## 限制

Kotlin允许对泛型的类型进行限制。通过指定上界的方式来对泛型的类型进行约束，如下将method()方法的泛型上界设置为Number类型

```kotlin
fun <T : Number> method(param: T): T {
    return param
}
```

只能将method()方法的泛型指定成数字类型，比如Int、Float、 Double等。

在默认情况下，所有的泛型都是可以指定成可空类型的，这是因为在不手动指定上界的 时候，泛型的上界默认是`Any?`。如果想要让泛型的类型不可为空，需要将泛型的上界手动指定成Any。





# 类与对象



## 初始化

### 构造方法

kotlin的构造方法使用 `constructor`关键字，

```kotlin
class Person(){
    // 构造方法
    constractor(name:String ,gender:Boolean):this(){
        println("constractor")
    }
}
```







### init关键字

在Kotlin中，除了主构造函数和次构造函数外，还提供了`init`代码块来做一些初始化操作。

```kotlin
class Person() {

    /*属性*/
    private var gender: Boolean = true

    /*次构造方法*/
    constructor(name: String,gender: Boolean):this() {
        println("constructor")
    }

    /*初始化代码块*/
    init {
        println("Person init 2,gender:${gender}")
    }

    /*初始化代码块*/
    init {
        println("Person init 1")
    }

}
```

如上所示可以有多个`init`代码块，kotlin的init代码块优先级比构造函数更高，当生成新对象时，首先会按顺序执行类中init代码块，然后再执行相应构造方法里代码。





## 数据类



## 继承

Kotlin类默认是不可继承的，想要可以被继承，需要使用open关键字

```kotlin
opne class Person{
    
}
```

每个类只可以继承一个类，继承另一个类时使用:

```kotlin

```





# 委托

## 类委托

将一个类的具体实现委托给另一个类去完成



## 委托属性



# 接口











