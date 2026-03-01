+++ 
draft = false
date = 2026-02-28T17:24:51+08:00
title = "Python zero to hero-I"
description = "python 从零到专家"
slug = ""
authors = []
tags = []
categories = []
externalLink = ""
series = []
+++
![Stephen Avatar](/images/posts/python-z2h/python-basic.png)

编程语言用的多了难免有些混乱, 有段时间写c++ 看到指针操作符 **->** 总和golang chanel联系起来.
接下来打算先从python开始做一个技术回顾,记录一些我对python的一些理解.


### 函数定义与调用
我认为函数是程序运行的最小单元,程序有职责不同的函数组合调用完成要执行的任务, python 中定一个函数的格式是`def 函数名称():`, 函数可以接受一个或多个参数以及不接受任何参数，不同参数在`()`中使用`,`分割. 如果函数有返回值使用 `return`关键字进行返回, 如果没有使用`return`关键字函数会return 一个None.下面根据参数的不同来对函数做一个回顾

---

#### 简单函数
```python
# 不接受任何参数
def simple_function():
    print("this is simple")

simple_function()

# run
(.venv) ➜  python-basic python baisc/basic-01.py
this is simple
(.venv) ➜  python-basic 
```

---

#### 固定参数的函数
```python
# 接收一个
def paly(game):
    print(f"玩{game}")

# 接收多个
def say_hi(name, age):
    print(f"我是{name}, 我今年{age}了")

def detail_say_hi(name,age,school):
     print(f"我是{name}, 我今年{age}, 我在{school}上学")

paly("王者荣耀")
say_hi("小明", 18)
detail_say_hi("小红", 20, "清华大学")
paly()
# run
(.venv) ➜  python-basic python baisc/basic-01.py
玩王者荣耀
我是小明, 我今年18了
我是小红, 我今年20, 我在清华大学上学
Traceback (most recent call last):
  File "/Users/stephenzzz/code/python-basic/baisc/basic-01.py", line 19, in <module>
    paly()
    ~~~~^^
TypeError: paly() missing 1 required positional argument: 'game'
```

这里说明一下既然参数数量与位置是固定的，那么在调用函数时必须严格按照参数数量以及参数位置进行传参，如果数量不一致解释器会`raise error`会导致程序直接退出, 如果位置不一致会导致函数运行结果不符合预期, python管这种类型的参数叫做位置参数.
当然还有一种方式是 我们定义函数时使用位置参数定义，但是调用时我们可以按照关键字参数来调用，这点在python这里比较灵活下面举个例子
```python
def say_hi(name, age):
    print(f"我是{name}, 我今年{age}了")
 # 通过参数名传递参数, 不需要考虑参数顺序
say_hi(age=26, name="小李")

# run
(.venv) ➜  python-basic python baisc/basic-01.py
我是小李, 我今年26了
```

---
#### 具有返回值的函数
```python
# 单一返回值
def add(a,b):
    return a+b

# 多返回值
# 这类函数其实返回值也是一个单一的Tuple 元组类型的值
def add_and_times(a,b):
    return(a+b,a*b) # return a+b, a*b 二者等效

print(add(4,4))
result = add_and_times(4,4)
print(result)
print(f"Index 加法结果：{result[0]}, 乘法结果：{result[1]}")
add_result, times_result = add_and_times(4,4)
print(f"Unpack 加法结果：{add_result}, 乘法结果：{times_result}")

_, times_result = add_and_times(4,4)
# 这里和golang的处理方式一致, 不需要处理的返回值使用 _ 表示
print(f"Unpack 乘法结果：{times_result}")

# run
(.venv) ➜  python-basic python baisc/basic-01.py
8
(8, 16)
Index 加法结果：8, 乘法结果：16
Unpack 加法结果：8, 乘法结果：16
Unpack 乘法结果：16
```
---
#### 默认值参数
要说明的是随着函数本身处理的业务越来越复杂,可能参数列表越来越长,参数类型越来越复杂. 如果一味的增加参数列表的长度代码难免变得难以维护 这是默认值参数出现了 `default`, 需要注意的是如果一个函数同时包含非默认值以及默认值参数要确保默认值参数在非默认值参数后面,否则解释器也是会报错`SyntaxError: parameter without a default follows parameter with a default`, 程序无法运行
```python
def student_info(name, age=7):
    print(f"学生姓名：{name}, 年龄：{age}")

student_info("小明")
student_info("小红", 8)

# run
(.venv) ➜  python-basic python baisc/basic-01.py
学生姓名：小明, 年龄：7
学生姓名：小红, 年龄：8
```
---
#### 可变位置参数与可变关键字参数
在实际开发中，我们有时无法预知函数具体会接收多少个参数。Python 提供了 `*args` 和 `**kwargs` 来处理这种“不确定性”，它们是编写高复用性代码（如装饰器或通用接口）的利器。

* **`*args`**：接收任意数量的**位置参数**。在函数内部，`args` 是一个 **Tuple（元组）** 类型。
* **`**kwargs`**：接收任意数量的**关键字参数**。在函数内部，`kwargs` 是一个 **Dict（字典）** 类型。

```python
# 可变位置参数
def sum_all(*args):
    print(f"args type: {type(args)}, value: {args}")
    return sum(args)

# 可变关键字参数
def build_profile(**kwargs):
    print(f"kwargs type: {type(kwargs)}, value: {kwargs}")
    for key, value in kwargs.items():
        print(f"{key}: {value}")

print(f"Sum: {sum_all(1, 2, 3, 4, 5)}")
build_profile(name="小明", role="班长", tech="Python")

# run
(.venv) ➜  python-basic python basic/basic-01.py
args type: <class 'tuple'>, value: (1, 2, 3, 4, 5)
Sum: 15
kwargs type: <class 'dict'>, value: {'name': '小明', 'role': '班长', 'tech': 'Python'}
name: 小明
role: 班长
tech: Python
```
---
#### 混合类型参数
混合类型参数
当我们在一个函数中同时使用多种类型的参数时，Python 对参数的排列顺序有着严格的物理定义。一旦顺序错误，解释器会直接报 SyntaxError。

标准定义的推荐顺序为：位置参数, 可变位置参数, 默认参数,可变关键字参数
```python
def complex_function(a, b, *args, name="Default", **kwargs):
    print(f"a: {a}, b: {b}")
    print(f"args: {args}")
    print(f"name: {name}")
    print(f"kwargs: {kwargs}")

complex_function(1, 2, 3, 4, name="小明", task="编程", debug=True)

# run
(.venv) ➜  python-basic python basic/basic-01.py
a: 1, b: 2
args: (3, 4)
name: 小明
kwargs: {'task': '编程', 'debug': True}
```
### 类型与操作
对于类型而言 这小节讨论python的内置的基础数据类型和容器类型, 标准库还提供很多类似`datetime`,`file`,`decimal`封装好的数据类型,后续有机会再统一回顾.
#### 数值类型
python中数值类型分为三种 int 整型,float浮点型,bool 布尔型. 有一点就是 int,和float不区分或者python不关心是否有符号, 声明为`number = -100`就是有符号 声明为`pi = 3.1415`就是无符号. 这点也符合duck type 或者动态类型语言的一些特征, 简化一些步骤, 将控制权交给开发者

##### int 与 float
python 中 `int` 类型并不像c++ 或者golang那样区分 int8 int32这些不同长度. 声明为多大的值就是多大,理论上是没有上限的取决于能为变量分配多大的内存. 
python 中没有像 golang区分 float32 或者float 或者c++ 提供一个double类型给开发者使用. 因为float本身在底层C语言实现上其实就是double. 当然既然是数值类型python也提供了 `+` `-` `*` `/` `%`这些常用的数值运算，同时python也像其他语言一样提供Floor地板除的运算 不过他无需显示调用floor 或者 math.Floor函数而是使用`//`两个除号代替.
```python
print(f"1+1={1+1}")
print(f"100-50={100-50}")
print(f"10*10={10*10}")
print(f"10/4={10/4}")
print(f"10//4={10//4}")
print(f"10%3={10%3}")
print(f"2**3={2**3}")
print(f"-5/2={-5/2}")
print(f"2.5*4={2.5*4}")
print(f"-13.5/2={13.5//2}")

# run
(.venv) ➜  python-basic python basic/basic-01.py
1+1=2
100-50=50
10*10=100
10/4=2.5
10//4=2
10%3=1
2**3=8
-5/2=-2.5
2.5*4=10.0
-13.5/2=6.0

```

---

##### bool
关于bool类型python 有些特殊也可以认为是它一切皆对象的缘故, 也可以说是为了开发者友好的原因, 当然我自己开来其实是有点不理解的. 
一个就是 `True` `False`是大写这个应该是很多人都不习惯的我写过的语言不多也不少了但是像这个Flase True我当年第一次见的时候还是很震惊. 
还有就是 python 只提供 `and` `or` `not` 而不提供`&&` `||` `!`这里也不过多的吐槽了只是觉得 python确实初学者友好. bool类型相对简单但是在做逻辑运算时也有一些要注意的点
```python
# 1. and（逻辑与）：全真才真
a = True
b = False
print(a and b)  # False（有一个假则假）
print(True and True)  # True

# 2. or（逻辑或）：有真就真
print(a or b)  # True（有一个真则真）
print(False or False)  # False

# 3. not（逻辑非）：取反
print(not a)  # False
print(not b)  # True

# 组合运算（优先级：not > and > or）
print(not a and b)  # False and False → False
print(a or not b)   # True or True → True

# 1. and：左为假返回左值，否则返回右值
print(0 and 10)        # 0（0 是假值）
print("hello" and "world")  # "world"（"hello" 是真值）

# 2. or：左为真返回左值，否则返回右值
print(0 or 10)         # 10（0 是假值）
print("hello" or "world")  # "hello"（"hello" 是真值）

# 3. not：仅返回 True/False
print(not 0)           # True
print(not "hello")     # False

# 实用场景：给变量设置默认值
name = ""  # 空字符串（假值）
username = name or "user123"  # name 是假值，使用默认值 "user123"
print(username)  # "user123"

# run
(.venv) ➜  python-basic python baisc/basic-01.py                                     
False
True
True
False
False
True
False
True
0
world
10
hello
True
False
user123
```

---

#### 字符串类型
字符串类型应该是程序猿最常见的一个数据类型, 但其实他也不是基础数据类型. 稍微有点经验就知道字符串其实是一个字符的数组. 开始我也打算将字符串放到容器类型中, 但是我查了python中字符串好像还真就是一个单独的class 底层也并不完全是基于字符数组实现,而是基于Unicode(utf-8)所以这里单独作为一个小节, 记录几个常用的字符串操作

```python
# 1. 切片（截取/反转）
s = "Python"
print(s[0:3])    # 截取前3个字符：Pyt
print(s[::-1])   # 反转：nohtyP

# 2. strip（去首尾空格/符号）
print("  abc  ".strip())          # 去空格：abc
print("###abc###".strip("#"))     # 去指定符号：abc

# 3. split（拆分）
print("a,b,c".split(","))        # 按逗号拆分：['a', 'b', 'c']

# 4. join（拼接，比+高效）
print("-".join(['a', 'b', 'c'])) # 拼接：a-b-c

# 5. replace（替换）
print("hello python".replace("python", "java")) # hello java

# 6. in（判断包含）
print("python" in "hello python") # True

# 7. f-string（格式化）
name = "小明"
age = 18
print(f"我是{name}，今年{age}岁") # 我是小明，今年18岁

# 8. 大小写转换
print("Python".lower()) # python
print("Python".upper()) # PYTHON

# 9. len（长度）
print(len("Python 编程")) # 8（含空格，按字符数算）

# run
(.venv) ➜  python-basic python baisc/basic-01.py
Pyt
nohtyP
abc
abc
['a', 'b', 'c']
a-b-c
hello java
True
我是小明，今年18岁
python
PYTHON
9
```

#### 容器类型
 
### 判断与循环