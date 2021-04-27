## 窗口

### 设置窗口的大小和位置

geometry(width`x`height+x+y)

- 窗口的长宽分别为**width**和**height**；
- 窗口到主窗口的间距为 **x**和 **y** ；
- 注意可以使用减号，例如 10x10-10-10 代表10*10大小的窗口出现在右下角，但是不能直接使x或y为负值，然后带入 *wxh+x+y*;同时这个格式里不能有空格。
- 当没有参数时，用此方法能返回当前的尺寸位置参数。

```python
# -*- coding:utf-8 -*-
from tkinter import *

root = Tk()
width, height, padx, pady = 800, 600, 40, 300
root.geometry('%dx%d-%d+%d' % (width, height, padx, pady))  123456
```

------

### 设置窗口样式、透明和全屏

- **-toolwindow** 可设置窗口为工具栏样式;
- **-alpha** 可设置透明度，0完全透明，1不透明。这里透明是窗口内的所有内容，不仅是窗体，所以要特别小心一个完全透明的窗口！ 
- **-fullscreen** 设置全屏  注意前面的短横杠(**-**) 不能少
- **-topmost** 设置窗口置顶。两个同时被置顶的窗口为同级(能互相遮盖)，但他们都能同时遮盖住没有被设置为置顶的窗口。

```python
root.attributes('-toolwindow', False, 
                '-alpha', 0.9, 
                '-fullscreen', True, 
                '-topmost', True)1234
```

------

### 去掉标题栏

- 去掉窗口的框架，脱离windows窗口管理。所以此时你也不能拖动它。并且这个窗口也不会出现在任务栏。

```python
root.overrideredirect(True)  
```





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



