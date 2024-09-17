---
title: "Manual"
date: 2019-04-26T14:17:01Z
draft: false
categories: [Python]
tags: []
card: false
weight: 0
---

Python手册

<!--more-->

## 基本数据类型

### 整数

- Python可以处理"任意"大小的整数，包括负数
- 支持二进制，八进制，十进制，十六进制

| 前缀     | 例子       | 进制 |
| -------- | ---------- | ---- |
| 0b或者0B | a = 0b1010 | 2    |
| 0o或者0O | a = 0o12   | 8    |
| 无       | a = 10     | 10   |
| 0x或者0X | a = 0xa    | 16   |

### 浮点数

- 定义
  - a = 1.2
  - b = .4
  - c = 1.2e-4

- 计算时可能会丢失精度

### 字符串

- 定义
  - a = 'test'
  - a = "test"
  - a = '''test'''
    - 可以直接使用单/双引号

```python
a = "test"
len(a)
```

#### 切割字符串

```python
x = 'hello world'
# 分割字符串
a = x.split(' ')
# ['hello', 'world']

# 合并为字符串
' '.join(a)
# 'hello world'


# 切片
s = '12 34'
s[2:4]
# ' 3'
```

#### 格式化字符串

```python
name = "Akvicor"
age = 19

# print I'm Akvicor
new_str = "I'm " + name + ", " + str(age) + " years old"
print("Case0: " + new_str)

# in python2
new_str1 = "I'm %s, %d years old" % (name, age)
print("Case1: " + new_str1)

# in python3
new_str2 = "I'm {}, {} years old".format(name, age)
print("Case2: " + new_str2)
new_str3 = "I'm {names}, {ages} years old".format(
    names='Akvicor', ages=age
)
print("Case3: " + new_str3)
new_str4 = f"I'm {name}, {age} years old"
print("Case4: " + new_str4)

# Case0: I'm Akvicor, 19 years old
# Case1: I'm Akvicor, 19 years old
# Case2: I'm Akvicor, 19 years old
# Case3: I'm Akvicor, 19 years old
# Case4: I'm Akvicor, 19 years old
```

### 运算符

**+**：两个字符串拼接

*****：将字符串重复

### 布尔型

- True
- False

#### NONE空值

- 空值就是没有值
- a = '' 不叫空
- a = 0 也不叫空
- a = None 叫空

```python
a = None
a is None
# True

a = ''
a is None
# False
```

## turtle

1. 设置画笔大小 `pensize(4)`
2. 隐藏海龟 `hideturtle()`
3. 切换RGB色彩模式 `colormode(255)`
4. 颜色 `color((255, 155, 192), "pink")`
5. 设置画布宽和高 `setup(840, 500)`
6. 设置画笔速度 `speed(10)`

<details>
<summary> turtle peppa pig source code </summary>

```python
"""
绘制小猪佩奇
"""
from turtle import *


def nose(x, y):
    """画鼻子"""
    # 将海龟移动到指定的坐标
    goto(x, y)
    pendown()
    # 设置海龟的方向（0-东、90-北、180-西、270-南）
    setheading(-30)
    begin_fill()
    a = 0.4
    for i in range(120):
        if 0 <= i < 30 or 60 <= i < 90:
            a = a + 0.08
            # 向左转3度
            left(3)
            # 向前走
            forward(a)
        else:
            a = a - 0.08
            left(3)
            forward(a)
    end_fill()
    penup()
    setheading(90)
    forward(25)
    setheading(0)
    forward(10)
    pendown()
    # 设置画笔的颜色(红, 绿, 蓝)
    pencolor(255, 155, 192)
    setheading(10)
    begin_fill()
    circle(5)
    color(160, 82, 45)
    end_fill()
    penup()
    setheading(0)
    forward(20)
    pendown()
    pencolor(255, 155, 192)
    setheading(10)
    begin_fill()
    circle(5)
    color(160, 82, 45)
    end_fill()


def head(x, y):
    """画头"""
    color((255, 155, 192), "pink")
    penup()
    goto(x, y)
    setheading(0)
    pendown()
    begin_fill()
    setheading(180)
    circle(300, -30)
    circle(100, -60)
    circle(80, -100)
    circle(150, -20)
    circle(60, -95)
    setheading(161)
    circle(-300, 15)
    penup()
    goto(-100, 100)
    pendown()
    setheading(-30)
    a = 0.4
    for i in range(60):
        if 0 <= i < 30 or 60 <= i < 90:
            a = a + 0.08
            lt(3)  # 向左转3度
            fd(a)  # 向前走a的步长
        else:
            a = a - 0.08
            lt(3)
            fd(a)
    end_fill()


def ears(x, y):
    """画耳朵"""
    color((255, 155, 192), "pink")
    penup()
    goto(x, y)
    pendown()
    begin_fill()
    setheading(100)
    circle(-50, 50)
    circle(-10, 120)
    circle(-50, 54)
    end_fill()
    penup()
    setheading(90)
    forward(-12)
    setheading(0)
    forward(30)
    pendown()
    begin_fill()
    setheading(100)
    circle(-50, 50)
    circle(-10, 120)
    circle(-50, 56)
    end_fill()


def eyes(x, y):
    """画眼睛"""
    color((255, 155, 192), "white")
    penup()
    setheading(90)
    forward(-20)
    setheading(0)
    forward(-95)
    pendown()
    begin_fill()
    circle(15)
    end_fill()
    color("black")
    penup()
    setheading(90)
    forward(12)
    setheading(0)
    forward(-3)
    pendown()
    begin_fill()
    circle(3)
    end_fill()
    color((255, 155, 192), "white")
    penup()
    seth(90)
    forward(-25)
    seth(0)
    forward(40)
    pendown()
    begin_fill()
    circle(15)
    end_fill()
    color("black")
    penup()
    setheading(90)
    forward(12)
    setheading(0)
    forward(-3)
    pendown()
    begin_fill()
    circle(3)
    end_fill()


def cheek(x, y):
    """画脸颊"""
    color((255, 155, 192))
    penup()
    goto(x, y)
    pendown()
    setheading(0)
    begin_fill()
    circle(30)
    end_fill()


def mouth(x, y):
    """画嘴巴"""
    color(239, 69, 19)
    penup()
    goto(x, y)
    pendown()
    setheading(-80)
    circle(30, 40)
    circle(40, 80)


def setting():
    """设置参数"""
    pensize(4)
    # 隐藏海龟
    hideturtle()
    colormode(255)
    color((255, 155, 192), "pink")
    setup(840, 500)
    speed(10)


def main():
    """主函数"""
    setting()
    nose(-100, 100)
    head(-69, 167)
    ears(0, 160)
    eyes(0, 140)
    cheek(80, 10)
    mouth(-20, 30)
    done()


if __name__ == '__main__':
    main()

```

</details>


## 列表和元组

- 元组是不可变的(Immutable)Python对象，储存在固定的一块内存里
- 列表是可变的(mutable)Python对象，需要两块存储空间，一块固定用来存储实际的列表数据，一块可变的空间用于扩展。
- **结论就是：元组创建和访问要比列表块，但是不如列表灵活**

### list basic

```python
a = [1, 2, 3]

b = [1, 'abc', 2.0, ['a', 'b', 'c']]

print(a)
# [1, 2, 3]
print(b)
# [1, 'abc', 2.0, ['a', 'b', 'c']]
print(a[0])
# 1
print(a[0], a[1])
# 1 2
# 默认两个之间加入空格
# 默认分隔符为空格，结尾为换行。但可以修改
print(a[0], a[1], a[2], sep='*', end='-')
1*2*3-

c = b[1:3]
print(c)
# ['abc', 2.0]
s = 'abcdefghijklmn'
print(s[3:7], s[-5:-2])
# defg jkl
```

### list method

```python
# 获取列表的一些基本信息
list1 = [9, 1, -4, 3, 7, 11, 3]

print('list1的长度 = ', len(list1))

print('list1里的最大值 = ', max(list1))

print('list1里的最小值 = ', min(list1))

print('list1里3这个元素一共出现了{}次'.format(list1.count(3)))

# 列表复制
list_copy = list1[:]
# 不能使用 list_copy = list1 这种方法
# 这只会让list_copy和list1指向同一个列表

# 列表的改变
list2 = ['a', 'c', 'd']
print('list2 = ', list2)

# 给list2结尾添加一个元素'e'
list2.append('e')
print('list2 = ', list2)

# 在list2的'a'和'b'之间插入一个'b'
list2.insert(1, 'b')
print('list2 = ', list2)

# 删除list2里的 'b'  根据值删除元素
list2.remove('b')
print('list2 = ', list2)

# 列表反转
list3 = [1, 2, 3]
print('list3 = ', list3)
list3.reverse()
print('list3 = ', list3)

# 列表排序
list4 = [9, 1, -4, 3, 7, 11, 3]

# 永久排序
list4.sort()
print('list4 = ', list4)
list4.sort(reverse=True)
print('list4 = ', list4)
# 临时排序
list4Temp = sorted(list4)
list4Temp = sorted(list4, reverse=True)

# 删除列表元素  根据位置
list4 = [9, 1, -4, 3, 7, 11, 3]
# 是用del删除 
del list4[6]  # -> [9, 1, -4, 3, 7, 11]
# 是用pop()删除  默认是最后一个元素，也可以是指定元素
p = list4.pop()  # -> [9, 1, -4, 3, 7, 11]
# p = 3
p = list4.pop(0)  # -> [1, -4, 3, 7, 11, 3]
# p = 9

# ETC
list5 = [1, 2, 3, 4]
print(list5 * 2 + [4, 5, 6])
# [1, 2, 3, 4, 1, 2, 3, 4, 4, 5, 6]
```

```
list1的长度 =  7
list1里的最大值 =  11
list1里的最小值 =  -4
list1里3这个元素一共出现了2次
list2 =  ['a', 'c', 'd']
list2 =  ['a', 'c', 'd', 'e']
list2 =  ['a', 'b', 'c', 'd', 'e']
list2 =  ['a', 'c', 'd', 'e']
list3 =  [1, 2, 3]
list3 =  [3, 2, 1]
list4 =  [-4, 1, 3, 3, 7, 9, 11]
list4 =  [11, 9, 7, 3, 3, 1, -4]
[1, 2, 3, 4, 1, 2, 3, 4, 4, 5, 6]
```



### tuple basic

```python
# 元组的创建

a = (1, 2, 3)
b = 1,
print(a, type(a))
print(b, type(b))

# 元组的访问

print(a)
print(a[1])
print(a[-1])
print(a[1:3])
print(a[1:])
print(a[:2])
```

### tuple methods

元组里面的值与字符串类似，不可以改变

```python
# 获取元组的一些基本信息
tuple1 = (9, 1, -4, 3, 7, 11, 3)

print('tuple1的长度 = ', len(tuple1))

print('tuple1里的最大值 = ', max(tuple1))

print('tuple1里的最小值 = ', min(tuple1))

print('tuple1里3这个元素一共出现了{}次'.format(tuple1.count(3)))

tuple5 = (1, 2, 3, 4)
print(tuple5 * 2 + (4, 5, 6))
```

```
tuple1的长度 =  7
tuple1里的最大值 =  11
tuple1里的最小值 =  -4
tuple1里3这个元素一共出现了2次
(1, 2, 3, 4, 1, 2, 3, 4, 4, 5, 6)
```

### dict basic

```python
# 字典创建

a = {
    1: 'a',
    2: 'b',
    '3': 'c'
}


# 不可改变的数据类型

l1 = [1, 2, 3]
# 因为list是可以改变的，所以下面这种是错误的
# b = {
#     l1: 1
# }

t1 = (1, 2, 3)
# 这是可以的，因为tuple不可改变
c = {
    t1: l1
}
print(c)


d = dict()
print(d)

e = dict(a=1, b=2, d='a')
print(e)

# 字典的访问
print(e['d'])

e['c'] = 123
print(e)

e['c'] = 3
print(e)
```

```
{(1, 2, 3): [1, 2, 3]}
{}
{'a': 1, 'b': 2, 'd': 'a'}
a
{'a': 1, 'b': 2, 'd': 'a', 'c': 123}
{'a': 1, 'b': 2, 'd': 'a', 'c': 3}
```

### dict methods

```python
d = {
    'Name': 'Jack',
    'Age': 9,
    'Grade': 5
}

#  不安全，在没有Name时会报错
print(d['Name'])
# 在没有Name时输出None
print(d.get('Name'))

print(d.keys())

print(d.values())

print(d.items())

c = d.pop('Name')
print(c, d)

d.clear()
print(d)


# 字典的更新

c = {
    1: 1,
    2: 2
}
print(c)
c[3] = 3
c[4] = 4

d = {
    5: 5,
    6: 6
}

# 字典删除  按照键删除

del c[3]

# c.update(d)
# print(c)
# 或
e = {**c, **d}
print(e)

```

```
Jack
Jack
dict_keys(['Name', 'Age', 'Grade'])
dict_values(['Jack', 9, 5])
dict_items([('Name', 'Jack'), ('Age', 9), ('Grade', 5)])
Jack {'Age': 9, 'Grade': 5}
{}
{1: 1, 2: 2}
{1: 1, 2: 2, 3: 3, 4: 4, 5: 5, 6: 6}
```

### set basic

```python
a = {'f', 'a', 'b', 'c'}

print(a)

print('c' in a)
print('d' in a)

li = [1, 2, 3, 2, 4, 5, 2]

s1 = set(li)
print(s1, type(s1), list(s1))
```

```
{'a', 'b', 'f', 'c'}
True
False
{1, 2, 3, 4, 5} <class 'set'> [1, 2, 3, 4, 5]
```

### set methods

```python
s = {1, 2, 3, 4}

s.add(5)
print(s)

# 如果元素不存在会报错
s.remove(5)
# s.remove(5)
print(s)

a = '123452'
s1 = set(a)
print(s1)


s1 = {1, 2, 3, 4}
s2 = {3, 4, 5, 6}

print(s1 & s2)
print(s1 | s2)
print(s1 ^ s2)
print(s1 - s2)
print(s2 - s1)
```

```
{1, 2, 3, 4, 5}
{1, 2, 3, 4}
{'2', '4', '3', '5', '1'}
{3, 4}
{1, 2, 3, 4, 5, 6}
{1, 2, 5, 6}
{1, 2}
```

## 条件语句和循环语句

### if elif else

```python
a = eval(input('Please input a integer: '))
print('Information: ', a, type(a))

if a > 0:
    print('This integer is large than 0')
elif a == 0:
    print('This integer is equal 0')
else:
    print('This integer is smaller than 0')
```

### while

```python
a = 10

while a > 0:
    print(a)
    a -= 1
```

### for

```python
a = '12345'
b = [1, 2, 3, 4]
c = ('a', 'b', 'c', 'd')
d = {
    1: 'a',
    2: 'b',
    3: 'c'
}
e = {1, 2, 3, 4, 9}

for item in a:
    print(item)

print('\ndict')
for item in d:
    print(item)
for a, b in d.items():
    print(f'{a}={b}')

print('\nrange')
for i in range(1, 20, 3):  # 默认步长等于1,也可以指定步长，例如三
    # 等价于 C++ 中的 for(int i = 1; i < 20; i+=3)
    print(i)

```

### break continue

```python
for i in range(10):
    print(i)
    if i == 3:
        break
print('#2')
for i in range(10):
    if i % 2 == 0:
        print(i)
print('#3')
for i in range(10):
    if i % 2 == 0:
        continue
    print(i)
```

```
0
1
2
3
#2
0
2
4
6
8
#3
1
3
5
7
9
```

### Game: guss number

```python
import random

a = random.randint(0, 100)

while True:
    num = int(input("Please input your choice: "))
    if num == a:
        print('\nCongratulation!')
        break
    elif num > a:
        print('To large')
    else:
        print('To small')
print(f'The number is {a}')

```

## 函数

### 函数的定义和调用

```python

def demo():
    print('Hello World')
    print('demo')


demo()


def demo1(a, b):
    print(a, b)
    print('demo1')


demo1(a=[1, 2, 3], b={1: 1, 2: 3})


def my_sum(a: int, b: int):
    return a + b


print(my_sum(2, 3))


def my_max(a):
    if not a:
        return None
    max_value = 0
    for i in a:
        if i > max_value:
            max_value = i
    return max_value


a = [1, 4, 5, 2, 3, 8, 10]
print(my_max(a))

```

```
Hello World
demo
[1, 2, 3] {1: 1, 2: 3}
demo1
5
10
```

### 命名空间和范围

```python
x = 1

x += 1

print(x)


def demo():
    x = 10
    print(x)


demo()

print(x)


def demo1(a):
    a = a + 10
    print(a)


demo1(a=x)

print(x)

print("Next")
y = [1, 2, 3]
print('This is y', y)


def demo2(a):
    a.append(4)
    print('demo2', a)


def demo22(a):
    a = a + [4]
    print('demo22', a)


demo22(a=y)
print('After demo22', y)
demo2(a=y)
print('After demo2', y)
print('可以发现这两种方法中 .append() 会影响原数组，而 + 不会影响')



z = 1
print('This is z', z)


def demo3(a):
    global z
    z = z + a
    print('In function', z)


demo3(a=10)
print('After Function', z)


```

```
2
10
2
12
2
Next
This is y [1, 2, 3]
demo22 [1, 2, 3, 4]
After demo22 [1, 2, 3]
demo2 [1, 2, 3, 4]
After demo2 [1, 2, 3, 4]
可以发现这两种方法中 .append() 会影响原数组，而 + 不会影响
This is z 1
In function 11
After Function 11
```

### 可变参数\*arge

```python
def add(a, b):
    return a + b


print(add(1, 2))


def add1(a, b, c):
    return a + b + c


print(add1(1, 2, 3))


def add2(*args):
    print(args)
    result = 0
    for i in args:
        result += i
    return result


print(add2(1, 2, 3, 4, 5, 6, 7, 8, 9, 10))


```

```
3
6
(1, 2, 3, 4, 5, 6, 7, 8, 9, 10)
55
```

### 可变参数\*\*wargs

```python
def add(**kwargs):
    print(kwargs)


print(add(a=1, b=2))


def test(a, b, c):
    print(a + b + c)


def add(x, **kwargs):
    if x == 2:
        test(**kwargs)


add(x=2, a=1, b=2, c=2)
```

```
{'a': 1, 'b': 2}
None
5
```

### 参数默认值

```python
def test(a, b=False):
    if b:
        return a
    else:
        return a * a


print(test(3))
print(test(3, True))

```

```
9
3
```

### 递归的实现

```python
def demo(n):
    result = 1
    for i in range(1, n+1):
        result *= i
    return result


def rec(n):
    if n <= 1:
        return 1
    return n * rec(n-1)


print(rec(5))
print(demo(5))
# 120
# 120
```

##  面向对象

### 类和对象的基本概念

```python
class MyClass:
    pass


# __init__() method 方法 function(函数)
# attribute 属性
# method 方法


class People:
    def __init__(self, name, age):
        self.name = name
        self.age = age

    def say_hi(self):
        print("Hi, my name is {}, and I'm {}".format(self.name, self.age))


someone = People('Jack', 20)
print(someone.name, someone.age)
someone.say_hi()

```

### 私有属性和受保护属性

保护的属性为变量名前有一个`_`，私有属性为变量名前有两个`__`。

保护属性可以在类外更改，但IDE会警告

私有属性不可以在类外更改

但是通过某种方法也可以更改私有属性，但并不推荐在类外对这两种属性进行更改

```python
class People:
    def __init__(self, name, age):
        self.name = name
        self.age = age
        self._protect_var = 10  # can be replace
        self.__private_var = 10  # can not be replace

    def say_hi(self):
        print("Hi, my name is {}, and I'm {}".format(self.name, self.age))

    def get_var(self):
        return self.__private_var

    def set_var(self, var):
        self.__private_var = var


someone = People('Jack', 20)
someone.say_hi()
someone.age = 21
someone.say_hi()
someone._protect_var = 30
print(someone._protect_var)


print(someone.get_var())
someone.set_var(30)
print(someone.get_var())

# python 只是将私有变量改名，并没有阻止我们对他的访问和修改
# 这只是变相的阻止我们对他的访问，但在实际项目中请不要这样做
print(dir(someone))
someone._People_protect_var = 31
print(someone._People_protect_var)
someone._People__private_var = 31
print(someone.get_var())

```

```
Hi, my name is Jack, and I'm 20
Hi, my name is Jack, and I'm 21
30
10
30
['_People__private_var', '__class__', '__delattr__', '__dict__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__gt__', '__hash__', '__init__', '__init_subclass__', '__le__', '__lt__', '__module__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', '__weakref__', '_protect_var', 'age', 'get_var', 'name', 'say_hi', 'set_var']
31
31
```



### 类的proterty怎么用

@property使类中的私有变量直接以 类名.变量名的形式**访问**

@变量名.setter 使类中的私有变量直接以 类名.变量名的形式**修改**（**需要先设定变量的property**）

此方法可以使私有变量在访问和修改时按照指定的方法进行处理，防止存放进去非法数据

```python
class People:
    def __init__(self, name, age, sex):
        self.__name = name
        self.__age = age
        self.__sex = sex

    def say_hi(self):
        print("Hi, my name is {}, and I'm {}".format(self.__name, self.__age))

    # def get_name(self):
    #     return self.__name

    @property
    def name(self):
        return self.__name

    def set_name(self, name):
        self.__name = name

    def get_age(self):
        return self.__age

    def set_age(self, age):
        self.__age = age

    # def get_sex(self):
    #     return self.__sex

    # def set_sex(self, sex):
    #     self.__sex = sex

    @property
    def sex(self):
        return self.__sex

    # 必须设置此变量的property
    @sex.setter
    def sex(self, sex):
        self.__sex = sex


someone = People(name='Jack', age=20, sex='Male')
print(someone.get_age())
someone.set_age(21)
print(someone.get_age())

print(someone.name)

someone.sex = 'Female'
print(someone.sex)

```

### 继承和多态

```python
class Animal:

    def eat(self):
        print('Animal is eating')


class Bird(Animal):
    def sing(self):
        print('Bird is singing')

    def eat(self):
        print('Bird is eating')


class Dog(Animal):
    def eat(self):
        print('Dog is eating')


class Cat(Animal):
    def eat(self):
        print('Cat is eating')


print('Animal')
a = Animal()
a.eat()
print('Bird')
b = Bird()
b.eat()
b.sing()
print('Dog')
d = Dog()
d.eat()
print('Cat')
c = Cat()
print('This is End\n')


def demo_eat(a):
    a.eat()


for i in [a, b, c, d]:
    demo_eat(i)

```

```
Animal
Animal is eating
Bird
Bird is eating
Bird is singing
Dog
Dog is eating
Cat
This is End

Animal is eating
Bird is eating
Cat is eating
Dog is eating
```

### type和isinstance的使用

```python
class A:
    pass


class B(A):
    pass


a = A()
b = B()
c = 1
print('Type A', type(A))
print('Type B', type(B))
print('a->A', isinstance(a, A))
print('c->int', isinstance(c, int))
print('c->str', isinstance(c, str))
print('b->B', isinstance(b, B))
print('b->A', isinstance(b, A))  # 因为B继承自A

```

### 类属性和实例属性

类属性在在创建实例时会保留在实例中，修改实例中的属性不回影响到类的属性

但修改类中的属性会影响到后续通过此类实例化出来的实例。

```python
class Student:
    count = 0

    def __init__(self, name):
        Student.count += 1
        self.name = name


s1 = Student('A')
s2 = Student('B')
s3 = Student('C')
print(Student.count)

# print(Student.count)
# # print(Student.name) 必须有实例才能访问
# s1 = Student(name='A')
# print(s1.name)
# print(s1.count)
#
# s1.name = 'B'
# s1.count = 1
# print(s1.name)
# print(s1.count)
# print(Student.count)
#
# Student.count = 2
# s2 = Student(name='C')
# print('After change')
# print(Student.count)
# print(s1.count)
# print(s2.count)
```

```
0
A
1
B
1
1
After change
3
1
3
```

### 类方法和实例方法

类中的普通方法只能先实例化之后，通过实例调用

类中的类方法可以直接调用，不用实例化。类方法因为有cls参数，所以可以调用类中的其他方法（例如静态方法和其他的类方法）

类中的静态方法可以直接调用，不用实例化。

```php
class People:
    def __init__(self, name, age):
        self.name = name
        self.age = age

    def sayhi(self):
        print(f"Hi, my name is {self.name}, and I'm {self.age}")

    @classmethod
    def test1(cls):
        print('This is a class method')
        cls.test2()  # 因为有cls参数，所以可以调用class方法

    @staticmethod
    def test2():
        print('This is a static method')


p1 = People(name='Jack', age=20)
p1.sayhi()
p1.test1()
p1.test2()
print('-------------')
People.test1()
People.test2()

```

## 模块和包

```python
# ch8/demo/math.py
def my_sum(*args):
    result = 0
    for i in args:
        result += i
    return result


def my_max(*args):
    print("max Value")


def my_min(*args):
    print("min Value")


class People:
    def __init__(self, name, age):
        self.name = name
        self.age = age


MAX_NUM = 100


# print(my_sum(1, 2, 3, 4))
```

```python
# ch8/test.py
import sys

if '/Users/akvicor/Documents/GitHub/Course/LearnCode/Python' not in sys.path:
    sys.path.append('/Users/akvicor/Documents/GitHub/Course/LearnCode/Python')
# print(sys.path)

from ch8.demo.math import my_sum

# 只能在pycharm里才不会报错
# 默认从系统根目录里寻找或从当前目录寻找，pycharm会自动添加ch8设为系统根目录
print(my_sum(1, 2, 3, 4))

```

**一般在使用import导入包时，最上面一组为系统自带的，中间一组为第三方的，最下面一组为自己的。**

### Terminal进入venv环境

进入项目目录然后  `source venv/bin/activate`

### if \_\_name\_\_ == "\_\_main\_\_"

直接执行一个文件的时候，这个文件里面的 \_\_name\_\_ 就等于 \_\_main\_\_

否则 `__name__`等于文件所在位置

可以使用此语句可以防止此文件被别的文件引用时输出调试信息

### pypi

[pypi.org]()

查看包的一些信息

## 输入输出和文件

print和input本身就是条用了sys，所以我们可以直接使用sys来输入输出

```python
import sys
print('Hello World', 'python', sep='%', end='.\n')
sys.stdout.write('Hello world!\n')

# a = input('ddd: ')
# print(a, type(a))
a = sys.stdin.readlines()  # command + D  to stop
print(a, type(a))

```

写入文件，mode默认为r参数

```python
f = open("test2.txt", mode='w', encoding='utf8')

f.write("line 1\n")
f.writelines(['This is second line\n', "line 3\n"])
f.write("测试中文")

f.close()

"""
参数列表：
file, mode='r', buffering=None, encoding=None, errors=None, newline=None, closefd=True

    ========= ===============================================================
    Character Meaning
    --------- ---------------------------------------------------------------
    'r'       open for reading (default)
    'w'       open for writing, truncating the file first
    'x'       create a new file and open it for writing
    'a'       open for writing, appending to the end of the file if it exists
    'b'       binary mode
    't'       text mode (default)
    '+'       open a disk file for updating (reading and writing)
    'U'       universal newline mode (deprecated)
    ========= ===============================================================
"""
```

f为文件指针，当使用readline或readlines的时候指针会向后移动，再次调用时会从指针处继续读，只有用seek将指针改为文件开始处才可以重新从头读

```python
f = open("test2.txt", encoding='utf-8')

# a = f.read()  # while read all line from file, and is very slowly
# print(a, type(a))

# for line in f:  # it will be quickly  # only one times for read
#     print(line)

a = f.readline()  # while read all line from file, and is very slowly
b = f.readlines()
print(a, type(a))
print(b, type(b))

f.seek(0)  # back to first line


f.close()

```

### os

```python
import os

os.getcwd()  # 返回一个str，获取当前目录的完整路径

os.listdir()  # 返回一个list，包含当前目录里的所有文件和文件夹

os.mkdir('demo')  # 创建文件夹

os.path.isdir('demo')  # 如果参数是文件夹，返回True

os.chdir('demo')  # 进入一个目录

os.path.exists('demo.txt')  # 当前目录下有没有这个文件夹或者文件，可以是根目录

os.path.join('ch', 'demo.txt')  # 拼接目录，自动添加 '/'


```


### 使用pathlib进行文件相关操作

```python
import os
from pathlib import Path

# in_file = os.path.join(os.getcwd(), 'demo', 'test.txt')
#
# print(in_file)
#
# a = Path.cwd() / 'demo' / 'test.txt'
#
# print(a)
# print(type(a))
#
# print(a.is_file())
# print(a.is_dir())
# print(a.lstat())
# print(a.parent)

# b = Path.cwd() / 'demo'
# b = b / 'test'
# b.rmdir()

p = Path.cwd()

for file in p.glob('*'):
    print(file)

print('******************************************************************************')

for file in p.rglob('*'):
    print(file)

```

### 读写二进制文件

```python
a = 'hello world'
b = b'hello world'

type(a)  # <class 'str'>
type(b)  # <class 'bytes'>


f = open('test3.txt', 'wb')
f.write(b'Hello world!')
f.close()

ff = open('test3.txt', 'rb')
print(ff.read())
ff.close()

```

### 序列化对象

```python
import pickle


class People:
    def __init__(self, name, age):
        self.name = name
        self.age = age

    def sayhi(self):
        print("Hi, my name is {}, and I'm {}".format(self.name, self.age))


p1 = People(name='Akvicor', age=20)
f = open('p1', 'wb')
pickle.dump(p1, f)
f.close()


# 测试序列化对象的加载 （需要类的原型去映射）

fp = open('p1', 'rb')
p2 = pickle.load(fp)
f.close()
print(p2)

```

## 函数式编程

 - **高阶函数**是至少满足下列一个条件的函数
   - 接受一个或多个函数作为输入
   - 输出一个函数

```python
print(sum([1,2,3]))
b = sum
print(b([1,2,3]))  # 因为sum是一个函数，所以b被赋值为sum后，b也是个函数，功能与sum相同
def test(x, f):
    return f(x)
printf(test([1,2,3], sum)) # 因为传入的函数为sum， 所以功能为求和
printf(test([1,2,3], max)) # 因为传入的函数为max， 所以功能为求最大值
# test就是一个高阶函数
```
### lambda 匿名函数

```python
def test(x, y):
    return x + 2 * y


f = lambda x, y: x + 2 * y

print(test(1, 2))
print(f(1, 2))


def demo(x, y, f):
    return f(x, y)


print(demo(1, 2, lambda x, y: x + 2 * y))


def add_n(n):
    return lambda x: n + x


f = add_n(40)
print(f(1))
print(f(-10))
```

### 列表解析

```python
a = [1, 2, 3, 4, 5, 6, 7, 8, 9]

b = [item**2 for item in a if item % 2 == 0]

print(b)

# 列表解析

# 字典解析

```

### 高阶函数map和reduce

```python
from functools import reduce

a = [1, 2, 3, 4]

# def f(x, y):
#     return x + y


m = map(lambda x: x * x, a)

for i in m:
    print(i)


r = reduce(lambda x, y: x + y, a)

print("r ", r)

```

### filter
```python
a = [1, 2, 3, 4, 5, 6, 7, 8, 9]

f = filter(lambda x: x % 2 == 1, a)
print(f)
for i in f:
    print(i, end=' ')

print("\nlist")

ff = [i for i in a if i % 2 == 1]
print(ff)

```

### decorator

```python
def test1(x):
    # print(test1.__name__)
    return x * 2


def test2(x):
    # print(test2.__name__)
    return x**3


test1(1)
test2(2)


def demo(f):
    def f_new(x):
        print(f.__name__)
        return f(x)
    return f_new


f = demo(test1)

print(f(3))


#  装饰器

@demo
def test3(x):
    return x * 2 * x


print("for the test3")
a = test3(4)
print(a)


@demo
def test3(x):
    print('hello world')


print('After @@')
test3(1)
```

### 完善装饰器

主要是文档注释等函数属性问题

```python
import functools


def demo(f):
    """
       This is f
       :param f:
       :return:
    """
    @functools.wraps(f)  # 解决文档注释问题
    def f_new(*args, **kwargs):
        """
        This is f-new
        :param args:
        :param kwargs:
        :return:
        """
        print(f.__name__)
        return f(*args, **kwargs)
    # 解决文档注释问题， 或者使用functiontools解决
    # f_new.__name__ = f.__name__
    # f_new.__doc__ = f.__doc__
    return f_new


@demo
def test1(x, y):
    """
    This is test 1
    :param x: integer
    :param y: integer
    :return: str
    """
    print(f'x={x}, y{y}')


test1(1, 2)

print(test1.__name__)
print(test1.__doc__)
```

## 异常和处理

```python
a = [10, 4, 3, 7, 11, 6, 0, 9, 22, 'c']
try:
    b = [item for item in a if 100 % item == 0]
    print(b)
except ZeroDivisionError:
    print('Zero Error')
except TypeError:
    print('Type Error')
except Exception as e:
    print('Other', e)

print('Finished')

```

### 自定义异常

```python
class MyException(Exception):
    pass


# raise MyException("This is user defined exception")

try:
    raise MyException("This is user defined exception")
except MyException as e:
    print(e)

```

### assert

```python
def demo(x, y):
    return x + y


print(demo(1, 2))
assert(demo(1, 2) != 3)

```

### try-except-else

```python
try:
    # 执行代码
except:
    # 如果有异常发生，执行此处代码
else:
    # 如果没有异常发生，执行此处代码
```

### try-except-else-finally

```python
try:
    # 执行代码
except:
    # 如果有异常发生，执行此处代码
else:
    # 如果没有异常发生，执行此处代码
finally:
    # 不管有没有异常都会执行此处代码
```

## 调试和测试

### print 调试

### pdb 调试

```python
import pdb

pdb.set_trace()

```
### PyCharm Debug

### doc test

适合小型的项目
```python
def func_demo(a, b):
    """ doc test demo
    >>> func_demo(1, 2)
    3
    >>> func_demo('a', 'b')
    'ab'
    >>> func_demo([1, 2], [3, 4])
    [1, 2, 3, 4]
    >>> func_demo(1, '2')
    Traceback (most recent call last):
    TypeError: unsupported operand type(s) for +: 'int' and 'str'
    >>>
    """
    return a + b


if __name__ == "__main__":
    import doctest
    doctest.testmod()

```

### 单元测试

#### Python单元测试规范
 - 测试都是以class形式定义
 - 每一个测试类都必须是 **unittest.TestCase** 的子类
 - 每一个测试类里的测试方法都必须以 **test_** 开头
 - 使用 **assert** 去检查预期结果和实际结果是否相符
 - 在测试方法运行之前需要使用 **setUp()** 方法预先定义一些测试规范和临时变量
 - 在测试方法执行完之后需要使用 **tearDown()** 方法清理和销毁临时变量等测试环境
 - 使用 **python -m unittest -v test_module** 执行测试

### unittest

```python
import unittest

from demo.math import add


class TestAdd(unittest.TestCase):
    def test_add(self):
        self.assertEqual(add(1, 4), 5)

    def test_add2(self):
        self.assertEqual(add(10, 20), 30)
        self.assertNotEqual(add(10, 20), 31)

    def test_add3(self):
        self.assertRaises(ValueError, add, 1, 1.2)


if __name__ == '__main__':
    unittest.main()

```
### pysnooper

`pip install pysnooper`

```python
import pysnooper

@pysnooper.snoop()
def f(a, b):
    return a + b


f(1, 2)

```


## logging 模块

```python
"""
low -> high
debug(), info(), warning(), error(), critical()
"""
import logging

logging.basicConfig(level='INFO')

logging.debug('This is debug')
logging.info('This is info')
logging.warning('This is warning')
logging.error('This is error')
logging.critical('This is critical')

```

### 文件写入

```python
"""
low -> high
debug(), info(), warning(), error(), critical()
"""
import logging

logging.basicConfig(level='INFO', filename='test.log', filemode='w')  # w 重新生成文件，默认在文件结尾添加内容

logging.debug('This is debug')
logging.info('This is info')
logging.warning('This is warning')
logging.error('This is error')
logging.critical('This is critical')

```

### logging format

```python
"""
https://docs.python.org/3/library/logging.html#logrecord-attributes
"""
import logging

format = '%(asctime)s-%(funcName)s-%(lineno)d-%(levelname)s %(name)s %(message)s'

logging.basicConfig(level='INFO', format=format)
# logging.BASIC_FORMAT


def main():
    logging.debug('this is debug')
    logging.info('this is info')
    logging.warning('this is warning')
    logging.error('this is erro')
    logging.critical('this is critical')


if __name__ == "__main__":
    main()

```

### 创建新的logging对象

```python
import logging

logger = logging.getLogger(name='demo')
logger.setLevel(logging.DEBUG)

# Create handlers
c_handler = logging.StreamHandler()
f_handler = logging.FileHandler('file.log')
c_handler.setLevel(logging.DEBUG)
f_handler.setLevel(logging.INFO)

# Create formatters and add it to handlers
c_format = logging.Formatter('%(name)s - %(levelname)s - %(message)s')
f_format = logging.Formatter('%(asctime)s - %(name)s - %(levelname)s - %(message)s')
c_handler.setFormatter(c_format)
f_handler.setFormatter(f_format)

# Add handlers to the logger
logger.addHandler(c_handler)
logger.addHandler(f_handler)


def main():
    logger.debug('this is debug')
    logger.info('this is info')
    logger.warning('this is warning')
    logger.error('this is erro')
    logger.critical('this is critical')


if __name__ == '__main__':
    main()

```


## pypy提高python代码执行效率

[链接](https://pypy.org/)

bin目录下为可执行文件

**测试代码**
```python
import time


def test():
    for i in range(1, 10):
        n = pow(10, i)
        start_time = time.time()
        sum(x for x in range(1, n+1))
        end_time = time.time()
        print(f'10^{i}:{end_time-start_time}')


test()

```

**Normal**

```python
10^1:3.0994415283203125e-06
10^2:8.106231689453125e-06
10^3:6.008148193359375e-05
10^4:0.0006120204925537109
10^5:0.006687164306640625
10^6:0.06196117401123047
10^7:0.49938416481018066
10^8:4.975664854049683
10^9:49.28339099884033
```

**PyPy**

```python
10^1:1.811981201171875e-05
10^2:6.198883056640625e-05
10^3:0.0033159255981445312
10^4:0.00019216537475585938
10^5:0.001216888427734375
10^6:0.0063250064849853516
10^7:0.05159306526184082
10^8:0.5082838535308838
10^9:5.0509021282196045
```

