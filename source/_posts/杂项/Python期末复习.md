---
title: Python期末复习
sticky: false
math: true
categories:
  - [杂项]
---

#### 前言

博主对老师公布的考点进行自己的总结，希望和大家一起学习，一起进步。如果有不对或者不准确的地方，希望各位大佬给以指正和补充,资料主要为老师的 PPT，加上自己的一些理解（菜鸡，理解的很少）不喜勿喷

#### 选择填空

###### python 中的数据类型那些是可哈希的？

- 先搞清楚可哈希的意思 -> 一个对象在其生命周期内，如果保持不变，那就是 hashable(可哈希的), 就是不变的数据结构
- 结合哈希表的定义: 通过给定的关键字的值直接访问到具体对应的值的一个数据结构
- 课件上写: ++可哈希对象是指拥有**hash**(self)内置函数的对象++{.dot}
- 那么回想 python 中满足上述的 可哈希的数据类型就只有 ++整形++, ++浮点型++, ++字符串++, ++元组(tuple)++
- 不可哈希的有 ++列表++, ++集合++, ++字典++.

参考链接:

- https://blog.csdn.net/qq_17753903/article/details/85345996
- https://cloud.tencent.com/developer/article/1534815

###### 按位运算

![](https://tva3.sinaimg.cn/large/006xVoHRly8gx476nxokyj30pb0aiq57.jpg)
!!我真是垃圾现在还在用别人的图床!!{.bulr}
其实位运算在之前的离散都学过，差不多的

- 按位与:
  > 回想与运算: 必须两个都是真，才能为真, 那么现在对应编程语言就是，双方都为 1 的时候为 1，其他的时候就是 0
  > 例: i1,i2 = 3, 6 对应的二进制数是 11B 和 110B 则
  > i1 & i2 = $\frac{011B \atop 110B}{010B}$ = 2
  > 掉头发，写数学公式，还好是个简单的公式
- 按位或
  > 同样和或运算有相通的地方: 只要有一个为真，结果就为真
  > 例: i1 | i2 = $\frac{011B \atop 110B}{111B}$ = 7
- 按位异或
  > 当且仅当两个数的真值不相同的时候是真，不然为假
  > 用公式表示为: $P \bar V Q \Leftrightarrow (P \land \neg Q) V (\neg P \land Q)$
  > 例:i1 ^ i2 = $\frac{011B \atop 110B}{101B}$ = 5
- 左位移
  > 缺位用 0 补
  > 例: i1 << 1 = 11B << 1 = 110B = 6
- 右位移
  > 同上
  > 例: i1 >> 1 = 11B >> 1 = 1B = 1
- 按位取反
  > ~x
  > 将 1 变为 0,将 0 变为 1
  > 例: ~i1 = 00B = 0

###### 身份运算符

用于比较两个对象是否对应同样的存储单元
| 运算符 | 使用方法 | 功能描述 |
| :----: | :----: | :----: |
| is | x is y | 如果 x 和 y 对应同样的存储单元，则返回 True；否则，返回 False |
| is not | x is not y | 如果 x 和 y 不对应同样的存储单元，则返回 True；否则，返回 False |
![](https://tva1.sinaimg.cn/large/006xVoHRly8gx4czaieiij30qr07jdi4.jpg)
先上一个例子:

```Python demo
x, y = 15,15
print(x is y)
print(x is not y)
print(x is 15)
# 输出为: True False True
```

Why, 为什么就会出现上面的情况，用我们学习 C 语言的经历来看，这就是完全不可能的！！
![](https://tva2.sinaimg.cn/large/006xVoHRly8gx49znxnxoj30fi06bjs0.jpg)
从上面大家可以看见虽然 a, b 的值是一样的，但是他们是存储在不同的地址中的！
但为什么 Python 和 C 不一样呢？
这是因为: Python 中使用了++引用语义++ ==> 变量保存的仅仅是 ++值的引用++，即变量保存的是++内存及其地址的抽象++

- 变量的每一次初始化，都会开辟一个新的存储空间，并将新的内容的地址赋值给变量。
  > 就像 x = 15, y = 15,那么就会在 x = 15 的时候，开辟一块空间用于存储 15，然后将这个地址赋给 x,当 y = 15 的时候，再将 15 的地址给 y 一份。

```Python demo
a = 15
b = 15
print(a is b)
print(id(a))
print(id(b))
```

- 对于 Python 中的复杂数据类型（如：列表、元组、字典等），如果仅仅是对其中的一个或多个元素进行修改，并不会改变其地址，如果进行删除或者添加操作，也只是单纯的改变内部元素的地址引用，但是，如果对整体进行重新赋值，那么就会对这些复杂数据类型重新赋予一个新的地址，来覆盖之前的地址，也只有在这种情况下，这些复杂数据类型的地址才会发生改变
- 并且可变类型和不可变类型也不一样
  > 不可变数据类型(int, float, str, tuple)，不允许变量的值发生变化，如果改变了变量的值，相当于是新建了一个对象，而对于相同的值的对象，在内存中则只有一个对象，就是不可变数据类型引用的地址的值不可改变

> 可变数据类型(list, dict)，允许变量的值发生变化，即如果对变量进行 append、+=等这种操作后，只是改变了变量的值，而不会新建一个对象，变量引用的对象的地址不会改变，只是对应地址的内容改变或者地址发生了扩充，所以对于相同的值的不同对象，会存在多份，即每个对象都有自己的地址

```Python demo
x1 = [1,2,3]
x2 = [1,2,3]
print(id(x1))
print(id(x2))
x2 = [1, 2]
print(id(x2))
x2.append(3)
print(x2)
print(id(x2))
b1 = (1, 2, 3)
b2 = (1, 2, 3)
print(id(b1))
print(id(b2))
b2 = (1, 2, 3)
print(id(b2))
```

这方面深究的话，就已经涉及到 Python 的内部底层实现了，大家如果有兴趣的话，可以去看看 -资料链接: https://www.cnblogs.com/traditional/p/13391098.html

###### 位置参数， 关键字参数

1. 位置参数(就是最简单的那个)
   通过位置来体现实参和形参的对应关系的方法

```Python demo
def sum(x, y):
  return x + y
x = 1
y = 2
z = sum(x, y)
print(z)
```

2. 关键字参数
   就是使用关键字进行一个强绑定, 形式为: "形参=实参"

```Python demo
def sum(x, y=1, z=2):
  return  x + y + z
x = 1
print(sum(x, z = 1))
```

###### 函数关键字，全局变量，内层函数中使用外层函数变量

其实全局变量，局部变量，还有函数中使用全局变量，在 JavaScript 中是有作用域链查找规则的，Python 中也类似

1. 全局变量
   在所有函数外定义的变量就是全局变量，其在所有函数中都可以使用
   :::info
   相对而言,局部变量就是在一个函数中定义的变量，其作用域是从定义局部变量的位置至函数结束位置
   :::

```Python demo
def GlobalVar1():
  print(f'全局x = {x}')
def GlobalVar2():
  x = 100
  print(f'局部x = {x}')
x = 20
GlobalVar1()
GlobalVar2()
GlobalVar1()
# 输出为 20 , 100, 20
```

2. global 关键字 global(全局)
   在一个函数中使用 global 关键字，可以声明在该函数中使用的是全局变量，而非局部变量

```Python demo
def GlobalVar1():
  print(f'全局x = {x}')
def GlobalVar2():
  global x
  x = 100
  print(f'局部x = {x}')
x = 20
GlobalVar1()
GlobalVar2()
GlobalVar1()
```

3. nonlocal 关键字 nonlocal(不是本地的)
   在 Python 中，函数的定义可以嵌套，即在一个函数的函数体中可以包含另一个函数的定义。通过 nonlocal 关键字，可以使内层的函数直接使用外层函数中定义的变量

```Python demo
# 没有使用关键字
def outer():
    x = 10
    def inner():
        x = 20
        print(f'inner x = {x}')
    inner()
    print(f'outer x = {x}')
outer()
```

```Python demo
# 使用了nonlocal关键字
def outer():
    x = 10
    def inner():
        nonlocal x
        x = 20
        print(f'inner x = {x}')
    inner()
    print(f'outer x = {x}')
outer()
```

###### 为类添加方法

类中的方法分为两类

- 普通方法需要通过类的实例对象根据方法名调用
- 内置方法是在特定情况下由系统自动执行

1. 在定义类的普通方法时，要求第一个参数需要对应调用方法时所使用的实例对象（一般命名为 self，但也可以改为其他名字）。然后就像定义普通函数一样，使用 def 定义就行
   :::warning
   个人习惯，喜欢将 self 改为 this,下面全是 this
   :::

```Python Number
class Num:
    x = 1
    y = 2
    def __init__(this, x, y):
        this.x = x
        this.y = y
    def sum(this):
        print(this.x + this.y)

if __name__ == "__main__":
    num1 = Num(1, 3)
    num1.sum()
```

###### 类方法和静态方法

1. 类方法是指使用++@classmethod++(class(类)+method(方法)))修饰的方法，其++第一个参数是类本身++（而不是类的实例对象）

```Python demo
class Complex: #定义Complex类
    def __init__(this,real=0,image=0): #定义构造方法
        this.real=real #初始化一个复数的实部值
        this.image=image #初始化一个复数的虚部值
    @classmethod
    def add(cls,c1,c2): #定义类方法add，实现两个复数的加法运算
        print(cls) #输出cls
        c=Complex() #创建Complex类对象c
        c.real=c1.real+c2.real #实部相加
        c.image=c1.image+c2.image #虚部相加
        return c
if __name__=='__main__':
    c1=Complex(1,2.5)
    c2=Complex(2.2,3.1)
    c=Complex.add(c1,c2) #直接使用类名调用类方法add
    print('c1+c2的结果为%.2f+%.2fi'%(c.real,c.image))
```

2. 静态方法
   静态方法是指使用@staticmethod 修饰的方法。
   > 与类方法不同的地方在于，静态方法中没有类方法中的第一个类参数。

```python demo
class Complex: #定义Complex类
    def __init__(this,real=0,image=0): #定义构造方法
        this.real=real #初始化一个复数的实部值
        this.image=image #初始化一个复数的虚部值
    @staticmethod
    def add(c1,c2): #定义类方法add，实现两个复数的加法运算
        c=Complex() #创建Complex类对象c
        c.real=c1.real+c2.real #实部相加
        c.image=c1.image+c2.image #虚部相加
        return c
if __name__=='__main__':
    c1=Complex(1,2.5)
    c2=Complex(2.2,3.1)
    c=Complex.add(c1,c2) #直接使用类名调用类方法add
    print('c1+c2的结果为%.2f+%.2fi'%(c.real,c.image))
```

###### 为类动态扩展方法

在给对象绑定方法时，需要使用 types 模块中的 MethodType 方法，其第一个参数是要绑定的函数名，第二个参数是绑定的对象名。

```Python demo
#就是老师的ppt
from types import MethodType
class Student:
    pass
def SetName(this,name): #定义SetName函数
  this.name=name
def SetSno(this,sno): #定义SetSno函数
  this.sno=sno
if __name__=='__main__':
    stu1=Student() #定义Student类对象stu1
    stu2=Student() #定义Student类对象stu2
    stu1.SetName=MethodType(SetName,stu1)
    Student.SetSno=SetSno #为Student类绑定SetSno方法
    stu1.SetName('李晓明')
    stu1.SetSno('1810100')
    #stu2.SetName('张刚') #取消注释则会报错
    stu2.SetSno('1810101')
```

> 1.  只有给那个实例上动态绑定，才可以在那个上面使用。
> 2.  由于绑定的是类上的方法,所以方法的第一个参数是 self

###### 控制关于类的访问

使用 @property 装饰器

- 使用 @property 来定义一个用于获取属性值的方法(getter)
- 使用 @属性名.setter 的装饰器
- 如果一个属性只有用于获取属性值的 getter 方法，而没有用于设置属性的 setter 方法，则该属性是一个只读属性

```Python demo
import datetime
class Student:
    @property
    def score(self):
        return self._score
    @score.setter
    def score(self,score):
        if score < 0 or score >100:
            print('成绩不合规范')
        else:
            self._score = score
    @property
    def age(self):
        return datetime.datetime.now().year-self.birthyear
if __name__ == '__main__':
    stu = Student()  # 创建Student类对象stu
    stu.score = 80  # 将stu对象的score属性赋值为80
    stu.birthyear = 2000  # 将stu对象的birthyear属性赋值为2000
    print('年龄：%d,成绩：%d' % (stu.age, stu.score))
    # stu.age = 19 #只读属性
    stu.score = 105
    print('年龄：%d,成绩：%d'%(stu.age,stu.score))
```

:::info
在类的 setter 和 getter 方法中使用 self 访问属性时，需要在属性名前加上下划线，否则系统会因不断递归调用而报错(其实就是套娃,不加\_你就一直在 get 那个属性)
:::

> 总结: 其实@property 和@属性名.setter 就是起到一个代理的作用，将你对类属性的访问代理了，经过人家的处理，你才能进行 get 或者 set

###### 生成器

生成器简单实现:

```Python demo
g = (x * x for x in range(10))
print(type(g))
for i in g:
    print(i, end=' ')
```

上面我们打印出 g 的类型为 class'generator', 那么 generator 是什么呢？

> 在语义上可以把它理解成一个状态机，内部封装了多个状态
> 执行 Generator 函数会返回一个遍历器对象，也就是说，Generator 函数除了状态机，还是一个遍历器对象生成函数。返回的遍历器对象(迭代器)，可以依次遍历 Generator 函数内部的每一个状态。
> 上面是使用 for 去将遍历器的状态打印出来，其实还可以 使用 .**next**()或 next()

```Python demo
g = (x * x for x in range(10))
print(g.__next__())
print(g.__next__())
print(g.__next__())
print(next(g))
```

复杂实现:

```Python demo
def faclist(n): #定义函数faclist
    result=1
    for i in range(2,n+1): #i在2至n范围内依次取值
        yield result #遇到yield即暂停执行并返回result，下次执行时继续		#从此处开始执行
        result*=i #将i乘到result上
f = faclist(10)
print(f)
print(f.__next__())
print(f.__next__())
print(f.__next__())
print(f.__next__())
```

执行上面函数，可以看出当我们调用 faclist 函数的时候，该函数也不执行，返回的也不是函数运行结果，而是一个指向内部状态的指针对象（Iterator Object）。
下一步，必须调用遍历器对象的 next 方法，使得指针移向下一个状态。也就是说，每次调用 next 方法，内部指针就从函数头部或上一次停下来的地方开始执行，直到遇到下一个 yield 表达式（或 return 语句）为止。换言之，Generator 函数是分段执行的，yield 表达式是暂停执行的标记，而 next 方法可以恢复执行。

遍历(迭代)器对象的 next 方法运行逻辑如下:

- 遇到 yield 表达式，就暂停执行后面的操作，并将紧跟在 yield 后面的那个表达式的值，作为返回的对象的 value 属性值。
- 下一次调用 next 方法时，再继续往下执行，直到遇到下一个 yield 表达式。
- 如果没有再遇到新的 yield 表达式，就一直运行到函数结束
- 如果还继续调用 next，那么就会引发 StopIteration 异常

###### 迭代器

- 可直接使用 for 循环遍历的对象统称为++可迭代对象++（Iterable）
- ++迭代器++（Iterator）是指可以通过 next 函数不断获取下一个值的对象，++并不是所有的可迭代对象都是迭代器++
- 可以使用 isinstance 方法判断一个对象是否是可迭代对象或迭代器。

1. 对于可迭代对象，通过 iter 函数得到迭代器

```Python demo
ls = [1, 2, 3, 4]
print(next(ls))  # 会报错
it = iter(ls)
print(it.__next__())
print(ls)
```

2. next 函数
   上面已经讲的很详细了
