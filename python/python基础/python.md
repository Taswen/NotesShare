# Python

**shuffle()** 方法将序列的所有元素随机排序。



list对象同时除以一个数不能使用a/b的形式，想要实现要么循环，要么转化成numpy对象

list求取元素之和可以使用 sum()函数，或是循环



## 函数

### 参数

位置参数



关键值参数



python的函数没有指定参数的类型，则一个形参可以传入任意类型的实参。有时，函数编写者需要提供参数的说明则可以使用 注解 `:` 来声明参数的类型

```python
def test(x:int,y:int):
    return x+y
```

不过声明没有强制性，只会提醒。在不符合规范时不会报语法错误（好的编辑器会显示一个警告`Expected type '<>', got '<>' instead`）。

> 该注解在python3才有的，如果在python2下运行会报语法错误！







### 可变参数

`*参数名`与`**参数`是Python参数中的可变参数（任意参数）。

作为函数定义时：

- `*形参参数名`收集所有未匹配的位置参数组成一个tuple对象，形参便指向此tuple对象。

- `**形参参数名`收集所有未匹配的关键字参数组成一个dict对象，形参便指向此dict对象。

```python
def temp(*args,**kwargs):
    if(len(args) != 0):
        print(args)
    if(len(kwargs) !=0):
        print(kwargs)
```

作为函数调用时实参参数名的前缀时：

- `*实参参数名`用于解包tuple对象的每个元素，作为一个一个的位置参数传入到函数中

    ```python
    my_tuple = ("wang","yuan","wai")
    temp(*my_tuple)
    # 输出   ('wang', 'yuan', 'wai')
    # 相当于 temp("wang","yuan","wai")
    ```

- `**实参参数名`用于解包dict对象的每个元素，作为一个一个的关键字参数传入到函数中

    ```python
    my_dict = {"name":"wangyuanwai","age":32}
    temp(**my_dict)
    # 输出    {'name': 'wangyuanwai', 'age': 32}
    # 相当于  temp(name="wangyuanwai",age=32)
    ```



> 注意：函数定义时，二者同时存在，一定需要将`*args`放在`**kwargs`之前
>
> 同时，可变参数必须定义在普通参数（也称位置参数、必选参数、选中参数等名称）以及默认值参数的后面，因为可变参数会收集所有未匹配的参数，如果定义在前面，那么普通参数与默认值参数就无法匹配到传入的参数。

### 返回值

python函数的返回值很自由，可以是任意类型，不同分支可以是不同类型，可以返回多个。

一般编写python函数不会对返回值类型做限定。但有时为了规范和提醒调用者，可以使用注解`->`进行声明。

```python
def test(x,y)->int:
    return x+y
```

不过声明没有强制性，只会提醒。在不符合规范时不会报语法错误（好的编辑器会显示一个警告`Expected type '<>', got '<>' instead`）。



多返回值



## 类与对象

类通过 class 关键字定义。

```python
class Person(object):
    pass
```

> 编程习惯，类名以大写字母开头

类名后加`(object)`，表示继承自哪个类。

类的帮助信息会放在 `<className>.__doc__` 变量中



### 使用

#### 创建实例对象

```python
# 无参
i = Person()
# 有参
j = Person("张三",18)
```

Python 中并没有类似于`new`一样用来表示申请内存初始化对象的关键字，类的实例化类似函数调用，通过调用 `__init__ ` 方法接收参数并初始化对象。



在 `__init__`以及其他的成员方法中，直接使用 `self.<name>`就可创建成员变量。



#### 导入其他包的类







### 组成

#### 属性

python 的类属性包含类方法和成员变量及类变量。



##### 私有属性

python规定使用两个下划线开头的属性是私有属性，不能在类的外部被使用或直接访问，只能被解释器调用或是。

```python
class A:
    def __io__(self):
        self.__inputWay = 0
        pass
```



> python的私有属性和其他的私有属性不同，它并不是真的私有。
>
> 对于 `self.__privateVar` ，python实际上创建了一个属性`self._类名__privateVar`，这个属性可以在实例外部使用，和使用共有的属性没什么区别（如此以来便可以访问父类的私有属性了）。
>
> 甚至可以在实例外部
>
> 







##### 访问属性

python没有指针概念，所有变量都是引用，使用点号 `.` 来访问对象的属性和方法，同样使用类名和 `.` 来访问类的属性和方法。

```python
emp = Emploee();	
emp.displayCount()	# 实例的方法
Employee.empCount  	# 对象的方法
```







##### 修改属性

python的属性可以动态修改。

当一个类或实例的属性不存在时，直接进行赋值可以添加属性

```python
# -*- coding: utf-8 -*-
from utils import typeassert 
class Person:
    num = 0
    @typeassert(name=str,age=int)
    def __init__(self,name,age):
        self.name = name
        self.age = age
        Person.num += 1
    
    def printSelf(self):
        print(self.name+"今年"+str(self.age)+"岁")

    def forgetAge(self):
        del self.age

zhangsan = Person("张三",14)
zhangsan.email = "123@qq.com"	# 添加邮箱属性
Person.sex = "man"
print(zhangsan.email)
```



对于已经存在的属性，可使用 `del` 关键字删除

```python
# -*- coding: utf-8 -*-
from utils import typeassert 
class Person:
    num = 0
    @typeassert(name=str,age=int)
    def __init__(self,name,age):
        self.name = name
        self.age = age
        Person.num += 1
    
    def printSelf(self):
        print(self.name+"今年"+str(self.age)+"岁")

    def forgetAge(self):
        del self.age

zhangsan = Person("张三",14)
zhangsan.printSelf()
zhangsan.forgetAge()
zhangsan.printSelf()	# 报错 AttributeError: 'Person' object has no attribute 'age'
```



对于属性的操作，python提供了各种接口

- `getattr(obj, name[, default]) `: 访问对象的属性。

    ```python
    hasattr(emp1, 'age')    # 如果存在 'age' 属性返回 True。
    ```

- `hasattr(obj,name)` : 检查是否存在一个属性。

    ```python
    getattr(emp1, 'age')    # 返回 'age' 属性的值
    ```

- `setattr(obj,name,value)` : 设置一个属性。如果属性不存在，会创建一个新属性。

    ```python
    setattr(emp1, 'age', 8) # 设置属性的值为 8，如果没有则添加属性 'age'并设置值
    ```

- `delattr(obj, name)` : 删除属性。

    ```python
    delattr(emp1, 'age')    # 删除属性 'age'
    ```

    

##### 内置属性

```python
__dict__
```

代表类的属性集合（是一个字典，由类的数据属性组成）



```python
__doc__
```

类的文档字符串。是类名下的多行注释的内容



```python
__name__
```

类名。在类中，其值就为所在类的名字。

如果是在类外，其值就为所在模块的名字。当运行的文件为当前文件时，其值为 `__main__`



```python
__module__
```

类定义所在的模块。（类的全名是`'__main__.className'`，如果类位于一个导入模块`mymod`中，那么`className.__module__ `等于 `mymod`）



```python
__bases__
```

类的所有父类构成元素（包含了一个由所有父类组成的元组）







#### 变量相关

数据成员，包括类变量或者实例变量



##### 类变量

类变量定义在类中且在函数体之外，在所有实例化的对象中是公用的。

```python
class Employee:
    '''所有员工的基类'''
    empCount = 0		# 类变量,所有员工共享

    def __init__(self, name, salary):
        self.name = name
        self.salary = salary
        Employee.empCount += 1

	def displayCount(self):
        print "Total Employee %d" % Employee.empCount

    def displayEmployee(self):
        print "Name : ", self.name,  ", Salary: ", self.salary
```





##### 实例变量





类变量通常不作为实例变量使用。



##### 成员函数

类中定义的函数，可以被实例调用。

其第一个参数必须是 `self`。`self` 在python中是一个关键字，代表了调用函数的实例本身，代表当前对象的地址。



 `self.__class__` 则指向类本身（类是一个特殊的对象）。



### 对象销毁

Python 使用了引用计数这一简单技术来跟踪和回收垃圾。在 Python 内部记录着所有使用中的对象各有多少引用。

一个内部跟踪变量，称为一个引用计数器。当对象被创建时， 就创建了一个引用计数， 当这个对象不再需要时， 也就是说， 这个对象的引用计数变为0 时， 它被垃圾回收。但是回收不是"立即"的， 由解释器在适当的时机，将垃圾对象占用的内存空间回收。

```python
a = 40      # 创建对象  <40>
b = a       # 增加引用， <40> 的计数
c = [b]     # 增加引用.  <40> 的计数

del a       # 减少引用 <40> 的计数
b = 100     # 减少引用 <40> 的计数
c[0] = -1   # 减少引用 <40> 的计数
```

垃圾回收机制不仅针对引用计数为0的对象，同样也可以处理循环引用的情况。循环引用指的是，两个对象相互引用，但是没有其他变量引用他们。这种情况下，仅使用引用计数是不够的。Python 的垃圾收集器实际上是一个引用计数器和一个循环垃圾收集器。作为引用计数的补充， 垃圾收集器也会留心被分配的总量很大（即未通过引用计数销毁的那些）的对象。 在这种情况下， 解释器会暂停下来， 试图清理所有未引用的循环。

python 的类中存在析构函数：`__del__`。其会在对象销毁的时候被自动调用。





### 继承

在定义类的时候可以使用 `(<类名>)`  来指定父类

#### 构造函数

子类的构造函数`__init__`可以重载。调用时没有符合的已定义的构造函数则会报错。



如果在子类中需要父类的构造方法就需要显式的调用父类的构造方法，或者不重写父类的构造方法。

对于子类没有的实现`__init__`构造函数，实例化时调用的是父类的`__init__`，如果重写了则不会调用。

如果想要显示调用父类的构造函数，需要使用`super`关键字

```python
# super(子类，self).__init__(参数1，参数2，....)
# 或写为
# 父类名称.__init__(self,参数1，参数2，...)
class A(object):
    def __init__(self):
        print("A")
        
class B(A):
    def __init__(self):
        print("B")
        super(B,self).__init__()	# 调用父类A的构造函数

```



#### 重写

对于父类中的方法，有同名覆盖的原则。即对于同名函数，调用时只会调用子类的重写版本。

```python
class Father(object):
    def __init__(self, name):
        self.name=name
        print ( "name: %s" %( self.name))
    def getName(self):
        return 'Father ' + self.name
 
class Son(Father):
    def __init__(self, name):
        super(Son, self).__init__(name)
        print ("hi")
        self.name =  name
    def getName(self):
        return 'Son '+self.name
 
if __name__=='__main__':
    son=Son('runoob')
    print ( son.getName() )
# name: runoob
# hi
# Son runoob
```

在类内调用基类的方法时，需要加上基类的类名前缀，且需要带上 self 参数变量。区别在于类中调用普通函数时并不需要带上 `self` 参数

```python

```



#### 运算符重载

Pytho支持运算符重载

加法

```python
# 重写 __add__
def __add__(self, other):
    return ...
```

减法

```python
# 重写 __sub__
def __sub__(self,other):
    return ...
```









#### 多继承

python支持多继承

```python
class C(A, B):   # 继承类 A 和 B
    pass
```



当要判断实例是否为一个类的实例化对象（或是子类的实例化对象）时，使用`isinstance()`函数

```python
isinstance(obj, Class)
# 返回布尔值，如果 obj是 Class类或是Class后代类的实例时则为True，反之则为 False
```





当要检测类是(否为其他类的子类或是后代类时，使用`issubclass()`函数（不能检测实例）

```python
issubclass(sub,sup)
# 返回布尔值，如果 sub是 sup子类或后代类则为True，反之则为 False
```







在python中继承中的一些特点：

- 1、
- 2、
- 3、Python 总是首先查找对应类型的方法，如果它不能在派生类中找到对应的方法，它才开始到基类中逐个查找。（先在本类中查找调用的方法，找不到才去基类中找）。

如果在继承元组中列了一个以上的类，那么它就被称作"多重继承" 。



### 内部类



## 模块与库



## 注解







## 断言

python使用`assert`断言函数来进行断言

断言函数是对表达式布尔值的判断，要求表达式计算值必须为真。如果表达式为假，触发异常；如果表达式为真，不执行任何操作。可用于自动调试。

语法格式如下：

```python
assert boolExpression
```

等价于

```python
if not boolExpression:
    raise AssertionError
```



assert 后面也可以紧跟参数:

```python
assert boolExpression [, arguments]
```

等价于

```python
if not boolExpression:
    raise AssertionError(arguments)
```

默认情况下，如果断言失败则会将参数打印出来













# tkinter

## 介绍



## 窗口



## 组件

### Button | 按钮

Buuton类位于tkinter.Button

创建一个按钮

```python
w = Button(master,[option=value, ... ])
```

> `master` —— 按钮的父容器

可选属性

| 属性名             | 意义                                       | 说明                 |
| ------------------ | ------------------------------------------ | -------------------- |
| `activebackground` | 当鼠标放上去时，按钮的背景色               |                      |
| `activeforeground` | 当鼠标放上去时，按钮的前景色               |                      |
| `bd`               | 按钮边框的大小，默认为 2 个像素            |                      |
| `bg`               | 按钮的背景色                               |                      |
| `command`          | 按钮关联的函数，当按钮被点击时，执行该函数 | 值为函数名，不加括号 |
| `font`             | 文本字体                                   |                      |
| `height`           | 按钮的高度                                 |                      |
| `image`            | 按钮上要显示的图片                         |                      |
|                    |                                            |                      |



commend传参



### Canvas | 画布





## 事件









# Numpy

矩阵乘法运算中的“*”和“np.dot()”是不一样的

“*”的意思是给定一个大小为(4,3)的矩阵A和一个大小为(4,3)的矩阵B，两者使用“A\*B”得到的矩阵的形状还是(4,3)。

当使用“*”的时候，如果两个矩阵之间的形状不能对应上，则会因为无法匹配而报错。当然如果只有行或者列对应不上可以通过广播使其行和列得到一一对应。

当使用“np.dot()”时，需要一个矩阵为(4,3)，另一个矩阵为(3,4)，这样得到的矩阵的形状为(4,4)。



俩个数值列表的对应相加

直接 a+b



numpy卷积函数

convolve(a, v, mode=‘full’)

a:(N,)输入的一维数组
　　　　b:(M,)输入的第二个一维数组
　　　　mode:{‘full’, ‘valid’, ‘same’}参数可选
　　　　　　‘full’　默认值，返回每一个卷积值，长度是N+M-1,在卷积的边缘处，信号不重叠，存在边际效应。
　　　　　　‘same’　返回的数组长度为max(M, N),边际效应依旧存在。
　　　　　　‘valid’ 　返回的数组长度为max(M,N)-min(M,N)+1,此时返回的是完全重叠的点。边缘的点无效。



numpy想要统计数值出现的次数

使用`np.bincount(x, weights=None, minlength=0)`

输入的数组x是一维的，元素都是非负的整数

解释一下权重weights，以及最小bin的数量minlength。

若不指定权重，则默认x每个位置上的权重相等且为1。若给定不同位置的权重，则返回加权和。

若不规定最小bin的个数，则默认的个数为（x的最大值+1）。比如最大值是2，则bin的个数为3，即[0 1 2]。

若规定bin的个数（需大于默认值否则无效），则bin的个数等于设定值。比如np.bincount(np.array([3,1,9]),  minlength=8)，若不指定minlength，输出的个数应该为9+1=10个。但此时指定的minlength小于10，小于默认状态下的数量，参数不再作用。返回的长度仍为10。如果np.bincount(np.array([3,1,9]), minlength=20)，指定的minlength大于10，那么返回的长度就是20。





reshape()函数

数组新的shape属性应该要与原来的配套，如果等于-1的话，那么Numpy会根据剩下的维度计算出-1对应维度的值。

所以，如果只把数组变为一维的，可以使用`np.reshape(-1)` 



numpy常用统计函数

求和 sum

均值 mean

中值 median

最大最小

极差 ptp

标准差 std









# Matplotlib

## 介绍与引入

matplotlib是受MATLAB的启发构建的python的关于图像绘制的开源第三方库，主要用于数学上各种图形图像的绘制。

MATLAB语言是面向过程的。利用函数的调用，MATLAB中可以轻松的利用一行命令来绘制直线，然后再用一系列的函数调整结果。

matplotlib使用numpy进行数组运算，并调用一系列其他的Python库来实现硬件交互。所以matplotlib的依赖库有numpy库

matplotlib的核心是一套由对象构成的绘图API。



- matplotlib有一套完全仿照MATLAB的函数形式的绘图接口，在matplotlib.pyplot模块中

	```python
	import matplotlib.pyplot as plt		# 使用该命令从matplotlib库中引入pyplot模块（一个对象）
	```
	
	> 本文以后如无特殊说明不再显式书写这种引用语句，默认与最近的引用相同。如果使用更多的库将会显式书写。



## pyplot模块的绘图结构

### plt本身

 在该模块中，pyplot（以后简称plt）本身表示当前操作的子图，调用它的函数接口就是相当于对上一个最近的子图进行操作，如果使用它时没有创建好的视图和子图，它就会主动创建一个。它有很多操作的函数，后续会讲到。举例如下

```python
plt.title("New Figure")		# 设置子图的标题
plt.plot([1, 2, 3, 4])
```



### figure | 画布

matplotlib中的所有图像都是位于figure对象中，一个图像只能有一个figure对象。可以将一个figure对象看作画布，本身不可见，但子图的显示需要以此为基础

一个figure对象的生成方式有两种

- 隐式生成

  当之前没有创建过

它可以由plt的figure函数生成（如下）。实际上如果没有进一步添加子图而是直接进行绘图操作，自动生成一个子图

```python
fig = plt.figure()		# 该方法返回一个新建立的figure对象
```

figure的类型是：

```python
<class 'matplotlib.figure.Figure'>
```



### subplot | 子图。



```python

```





## plt对象的一些方法

直接使用plt的方法可以快速的对刚创建的图像进行操作，由于不需要特别指定操作，是面向过程的编程。但其实matplotlib本质上还是构建对象来构建图像。函数式编程将构建对象的过程封装在函数中，从而让我们觉得很方便。

尽管函数式绘图很便利，但利用函数式编程会有以下缺点：

1) 增加了一层“函数”调用，降低了效率。

2) 隶属关系被函数掩盖。整个matplotlib包是由一系列有组织有隶属关系的对象构成的。函数掩盖了原有的隶属关系，将事情变得复杂。

3) 细节被函数掩盖。pyplot并不能完全复制对象体系的所有功能，图像的许多细节调中最终还要回到对象。

4) 每件事情都可以有至少两种方式完成，用户很容易混淆。

为此，我们引入两个类: Figure和FigureCanvas。(函数式编程也调用了这些类，只是调用的过程被函数调用所遮掩。)







matplotlib.pyplot.gcf()

获取当前的操作的figure对象





### figure()



### subplot()

为当前操作的画布添加一个子图

```python
plt.subplot(2,4,2)		# 有三个参数（整数）作为位置的指引
plt.subplot(242)		# 在三个参数（整数）都为小于10的情况下可以将他们合并成一个三位整数
```



### plot()

画线



### title()

添加标题





### figure对象

#### 对象的结构



#### 对象的一些方法







subplot可以规划figure划分为n个子图，但每条subplot命令只会创建一个子图 ，参考下面例子。

```python
#作图1
plt.subplot(221)  
plt.plot(x, x)  
#作图2
plt.subplot(222)  
plt.plot(x, -x)  
 #作图3
plt.subplot(223)  
plt.plot(x, x ** 2)  
plt.grid(color='r', linestyle='--', linewidth=1,alpha=0.3)
#作图4
plt.subplot(224)  
plt.plot(x, np.log(x))  
plt.show()  
```







figure对象下创建一个或多个subplot对象(即axes)用于绘制图像。



面向函数式







面向对象式







# re









