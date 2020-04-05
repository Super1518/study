
## Python 语法
Python 采用缩进的方式来标识代码，虽然没有明确规定缩进使用几个空格还是 Tab，但是约定的习惯使用 4 个空格的缩进。

Python 通常将一行作为一个语句，当语句以冒号```:```结尾时，缩进的语句视为代码块。Python 通常一行为一个语句，但是可以通过反斜杠```\```将一个语句分为多行显示。一行中也可以有多个语句，但是语句之间需要使用分号```;```分开。

以```#```开头的语句为注释，注释可以是任意内容，解释器会自动忽略以```#```开头的注释内容。以```#```开头的注释可以放在一行的开始，也可以放在语句或表达的结尾。以三个单引号```'''```或三个双引号```"""```之间的内容为多行注释。

> 三个单引号```'''```或三个双引号```"""```也用来表示多行字符串。

```python
# 这是单行注释

'''
这是
多行注释
'''
```

Python 语法是大小写敏感的，写错大小写将会报错。

## 变量和常量
Python 标识符的命名由字母、数字和下划线```_```组成，且不能以数字开头。以下划线开头标识符的通常具有特俗意义，以单下划线开头的标识符代表不能直接访问的属性，需要通过类方法访问。以双下划线开头的标识符代表类的私有成员，以双下划线开头和结尾的标识符是 Python 中特俗方法专用标识符。

在 Python 中变量可以是任意数据类型，且变量本身并不具有类型，可以将不同类型的值赋值给同一个变量「大多数语言中变量定义后，只能赋值定义时的类型，否则编译器将报错」。
>这种变量本身类型不固定的语言称之为动态语言，与之对应的是静态语言。静态语言在定义变量时必须指定变量类型，如果赋值的时候类型不匹配，就会报错。
```python
a = 1
print(a)
a = 'Hello World!'
print(a)
```
Python 使用等号```=```变量进行赋值，在 Python 中变量的赋值就是变量的定义「当该变量不存在时」，```a = 1```定义变量 a 并将整数 1 赋值给变量 a。

Python 中约定使用大写字母作为常量的标识「Python 中并没有常量和变量之分，解释器无法阻止你修改常量的数据，使用大写字母只是大家的一种约定」。



## 数据类型
Python 中的数据类型「或者叫对象」分为可变类型和不可变类型。在对变量进行重新赋值，可变类型将在原有对象的基础上进行修改，不可变类型将新建一个对象并将其赋值给当前变量。

### 不可变类型
**不可变类型**是指对象本身不能被修改，即修改该指向该对象的变量时，是新建一个对象，并将新对象赋值给变量。Python 中不可变类型包括数字、字符串和元组。
```python
a = 1
b = a
print("a=%d, addr=%s" % (a, id(a)))
print("b=%d, addr=%s" % (b, id(b)))
a = 2
print("a=%d, addr=%s" % (a, id(a)))
print("b=%d, addr=%s" % (b, id(b)))
```
以上示例解释了不可变类型的修改过程，对变量 a 修改后，变量 a 的值改变地址同时改变，但是变量 b 的值和地址都未改变。变量 a 的地址改变是由于它的指向从 1 的地址更改为 2 的地址；变量 b 的地址为改变表示 1 所在地址的值一直未被修改。
#### 数字「Number」
Python 的 numbers 模块定义 Number 类，它是数字的层次结构的基础。Python 的数字类型包括整数「int」、浮点数「float」、布尔值「bool」、复数「complex」」，他们都是 Number 的子类。
```python
from numbers import Number
a = 1
b = 1.0
c = True
d = complex(1, 2)
print(isinstance(a, Number))
print(isinstance(b, Number))
print(isinstance(c, Number))
print(isinstance(d, Number))
```
##### **整数「int」**
Python 使用 int 来标识所有的整数，int 类型可以是任意大小的整数包括负整数。

整数的表示方法：
- 十进制：100
- 十六进制：0x64
- 八进制：0o144
- 二进制：0b01100100
```python
a = 100
print("abin=0b{:b}".format(a))
print("aoct=0o{:o}".format(a))
print("ahex=0x{:x}".format(a))
print("adec={:d}".format(a))
```
以上是对十进制整数 100 的各进制输出转换，运行输出结果
```shell
abin=0b1100100
aoct=0o144
ahex=0x64
adec=100
```
##### **浮点数「float」**

浮点数就是我们常说的小数，之所以称为浮点数，是因为按照科学记数法表示时，一个浮点数的小数点位置是可变的。浮点数有两种写法
- 数学写法：1.23
- 科学计数法：0.123e1

由于计算机存储的特性，整数运算总是精确的，浮点数运算会存在误差。判断浮点数相等，通过判断两个浮点数的差值是否小于一个很小的数来实现。
```python
a = 4.2
b = 2.1

print (a + b)
print ((a + b) == 6.3)
print ((a + b) - 6.3)
print ((a + b) - 6.3 < 0.000001)
```
以上代码的执行结果
```shell
6.3
False
8.881784197e-16
True
```

>浮点数计算过程中产生的误差是由底层CPU和IEEE 754标准通过自己的浮点单位去执行算术时的特征。 由于Python的浮点数据类型使用底层表示存储数据，因此你没办法去避免这样的误差。

如果你需要精确的计算，你可以选择使用 ```decimal``` 模块，Python 原生的浮点数类型可以即可满足大多数应用场景，通常推荐使用原生的类型「除非你用在特殊场景中」。
```python
from decimal import Decimal
a = Decimal('4.2')
b = Decimal('2.1')
print(a + b)
print((a + b) == Decimal('6.3'))
```

> decimal 模块能够让你控制运算过程中的所有方面，但是它也占用了大量的 CPU 算力，通常应用在不允许出现任何误差的特俗场合。

##### **布尔值「bool」**

布尔值和数学表示中的一致，只有两种表示方式真 ```True``` 和假 ```False```。一个布尔值要么是 True 要么是 Flase，在 Python 中布尔值要么直接使用 True、False，要么通过运算获取。

```python
a = True
b = False
print ("a = %s, b = %s" % (a, b))
a = 1
b = 2
print ("a > b is %s, a < b is %s" % (a > b, a < b))
```

##### **复数「complex」**

复数有一个实数和一个虚数构成，表示为：a+bj。python 中复数的几个概念：
1. 虚数不能单独存在，它们总是和一个值为 0.0 的实数部分一起构成一个复数
2. 复数由实数部分和虚数部分构成
3. 表示虚数的语法：real+imagej
4. 实数部分和虚数部分都是浮点数
5. 虚数部分必须有后缀j或J

##### **数值类型的运算**
1. 算术运算
    - ```+``` ：加，两个对象相加。
    - ```-``` ：减或负，在一个数值的左侧时表示复数；或一个数减去另一个数。
    - ```*``` : 乘，两个数相乘。
    - ```/``` : 除，一个数除以另外一个数。
    - ```%``` : 取模或求余，返回除法的余数。
    - ```**``` : 幂， x**y 返回 x 的 y 次幂。
    - ```//``` : 地板除，x // y 返回结果的整数部分。
    > Python 的除法总是返回小数，结果需要整数时需要使用```//```。
2. 关系运算符
    - ```==``` : 相等，比较两个对象是否相等。
    - ```!=``` : 不等，比较两个事项是否不等。
    - ```>``` : 大于，比较一个对象是否大于另一个对象，返回x>y的结果。
    - ```<``` : 小于，返回x<y的结果。
    - ```>=``` : 大于等于。
    - ```<=``` : 小于等于。
    > 关系运算符的返回结果总是布尔值（True 或 False)。
3. 赋值运算符
    - ```=``` : 将一个兑现赋值给一个变量。
    - ```+=``` : 加法赋值运算符。
    - ```-=``` : 减法赋值运算符。
    - ```*=``` : 乘法赋值运算符。
    - ```/=``` : 除法赋值运算符。
    - ```%=``` : 求余赋值运算符。
    - ```**=``` : 幂赋值运算符。
    - ```//=``` : 整除赋值运算符。
    ```python
    a = 4
    b = 2
    c = a // b
    a //= b
    print ("c = %s, a = %s", c, a)
    ```
    以上代码输出```c = 2， a = 2```, 赋值运算符直接使用左侧的值同右侧的值进行结算，并将结果赋值给左侧的值。==```=```不计算左侧值，仅将右侧结果赋值给左侧变量==
    > 注意：Python 没有自加```++```和自减```--```运算符。
    
4. 位运算符
    - ```&``` : 按位与运算符：参与运算的两个值,如果两个相应位都为1,则该位的结果为1,否则为0
    - ```|``` : 按位或运算符：只要对应的二个二进位有一个为1时，结果位就为1
    - ```^``` : 按位异或运算符：当两对应的二进位相异时，结果为1
    - ```~``` : 按位取反运算符：对数据的每个二进制位取反,即把1变为0,把0变为1
    - ```<<``` : 左移动运算符：运算数的各二进位全部左移若干位，由 << 右边的数字指定了移动的位数，高位丢弃，低位补0
    - ```>>``` : 右移动运算符：把">>"左边的运算数的各二进位全部右移若干位，>> 右边的数字指定了移动的位数
5. 逻辑运算符
    - ```and``` : 逻辑与，参与计算的两个值，有任何一个为 False 时，结果即为 False
    - ```or``` : 逻辑或，参与计算的两个值，有任何一个为 True 时，结果即为 True
    - ```not``` : 逻辑非，如果值为 True 结果即为 False，如果值为 False，结果即为 True

#### 字符串「String」
Python 中字符串是以单引号 「```'```」、双引号 「```"```」、三引号 「```'''```或```"""```」 括起来的任意文本。使用三引号时通常为了表示多行文本，无需显示的写出换行符 ```\n```。

同大多数编程语言一样，python 使用反斜杠 ```\``` 来对字符串中的特俗字符进行转义，比如单引号。
```python
print("I'm OK!")
print('I\'m OK!')
```
以上两行代码的输出内容是完全一致的 ```I'm OK!```，但是第二行却使用了反斜杠来禁止单引号的转义，第一行却没有，这是因为在 Python 中使用单引号表示的字符串其内部的双引号将原样保留「反之亦然」。

Python 还允许使用 ```r``` 使字符串默认不进行转义，看一下代码。
```python
print('\\\\')
print(r'\\\\')
```
以上代码第一行将输出两个反斜杠 ```\\```，第二行代码将输出四个反斜杠 ```\\\\```。
> 使用 ```r``` 使字符串不转义，对单引号和双引号失效。```print(r'I'm OK!')``` 改代码将报错。

##### 格式化
格式化是字符串的一种常用功能，便于提取出共用不改变内容，只更改需要改变的内容。

第一种格式化方式，和 C 语言一样，Python 也使用百分号 ```%``` 来格式化代码。
```python
print("I'm %s, my age is %d" %("keinYe", 29))
```
上面的代码将输出 ```I'm keinYe, my age is 29```，```%``` 用来替换替换变量的内容，Python 中格式化占位符与 C 语言一致，这里就不再多做介绍，如果需要可自行搜索。

> 如果你不确定变量的类型，那么使用 ```%s``` 会是一个不错的选择。

另外一种格式化方式是使用字符串的 ```format()``` 方法。```format()``` 使用花括号 ```{0}``` 作为占位符。
```python
print("Hello {0}, Age is {1}".format("xiaoming", 22))
print("Hello {}, Age is {}".format("xiaoming", 22))
print("Hello {name}, Age is {age}".format(age=22,name="xiaoming"))
```
以上代码的输出内容完全一致，```format``` 方法允许执行变量的名称且无需严格按照顺序放置；你也可以仅用花括号来占位，在放置替换的变量时按需要的顺序放置即可。

##### 编码
字符的编码方式多种多样，比如 ```ASCII```、```GB2312```、```Shift_JIS```、```Euc-kr```、```Unicode```、```UTF-8``` 等等。在计算机内存中，统一使用Unicode编码，当需要保存到硬盘或者需要传输的时候，就转换为UTF-8编码。

在 Python3 中字符串在内存中是以 Unicode 编码方式保存的，也就是说 Python3 的字符串支持多语言。通常情况下我们在 Python 文件的头部加上 ```# -*- coding:utf-8 -*-``` 这行注释告诉 Python 解释器以 UTF-8 的编码方式来读取源码，否则在源代码中写的其他语言字符可能会出现乱码，在声明 UTF-8 的同时，还要确保你的文本编辑器正在使用 UTF-8 without BOM 编码。

Python 的字符串类型时 ```str```，在内存中以 Unicode 编码，一个字符占有多个字节；如果需要进行传输或保存在硬盘上，需要将 ```str``` 转换为以字节为单位的 ```bytes```。

Python 中的 ```bytes``` 类型的数据用带 ```b``` 前缀的双引号或单引号表示。
```python
a = b'hello world'
b = 'hello world'
print('type(a)={}, type(b)={}'.format(type(a), type(b)))
# type(a)=<class 'bytes'>, type(b)=<class 'str'>
a = '中国'
b = '中国'.encode('utf-8')
print('a=%s, b=%s' % (a, b))
# a=中国, b=b'\xe4\xb8\xad\xe5\x9b\xbd'
print('len(a)=%d, len(b)=%d' %(len(a), len(b)))
# len(a)=2, len(b)=6
```
> 纯英文的字符串可以直接使用 ```b``` 前缀编码为 ```bytes```， 带有中文的字符串无法直接转换为 ```bytes```，需要转换为 ```utf-8``` 编码格式的 ```bytes```。

> python 中字符串通过 ```encode()``` 方法进行编码，通过 ```decode()``` 方法进行解码。
```python
a = '中国'.encode('utf-8')
print(a)
# b'\xe4\xb8\xad\xe5\x9b\xbd'
b = b'\xe4\xb8\xad\xe5\x9b\xbd'.decode('utf-8')
print(b)
# 中国
```

#### 元组 「Tube」
元组是列表的不可变类型「可以看做是 C 语言中的不可变数组」，元组中的元素一旦初始化完成就不能够修改，因此我们可以通过下标法来访问元组的内容，但是不能更改元组的内容。在 python 中元组使用圆括号进行定义
```python
t = (1, 2, 3, 4)
print(type(t))
# <class 'tuple'>
print(t[1])
# 2
```
> 由于元组是不可变类型，因此元组不存在 insert、append 等能够修改元组的方法。

定义一个空元组时，可以直接使用 ```()```，但是如果你要定义一个只含有一个元素的元组时，你需要在元素后多一个逗号 ```(1,)```。

你可以直接使用加号 + 将两个元组和并为一个。
```python
t1 = (1, 2, 3)
t2 = ('a', 'b', 'c')
t3 = t1 + t2
print(t3)
# (1, 2, 3, 'a', 'b', 'c')
```

### 可变类型
**可变类型**是指对象可以在原对象的基础上做任意的修改，不会产生新的开销。在 Python 中列表、字典、集合为可变类型。
```python
a = [1, 2, 3, 4]
print("a={}, addr={}".format(a, id(a)))
a.append(5)
print("a={}, addr={}".format(a, id(a)))
```
在以上代码中对列表 a 新增元素前后，a 的地址没有发生任何的改变，表示对象 a 依然是修改前的对象 a。

#### 列表「List」
列表是一种有序的数据集合，可以随意的删除、增加和修改列表的元素。列表使用方括号进行定义，列表内的元素使用逗号进行分割。
```python
country = ['China', 'Japan', 'Korea']
print(country)
# ['China', 'Japan', 'Korea']
print(type(country))
# <class 'list'>
```
通过下标法可以访问列表内的任何元素，通过 0 和正数正向索引列表的元素「0 索引的是第一个元素，依次类推」，也可以通过负数来反向索引列表的元素「-1 索引的是最后一个元素，依次类推」。使用下标来索引列表的元素时，索引不能超出列表的范围，否则 python 会报 ```IndexError``` 错误。
```python
country = ['China', 'Japan', 'Korea']
pring('country[0]={}, country[1]={}, country[2]={}'.format(country[0], country[1], country[2]))
# country[0]=China, country[1]=Japan, country[2]=Korea
pring('country[-1]={}, country[-2]={}, country[-3]={}'.format(country[-1], country[-2], country[-3]))
# country[-1]=Korea, country[-2]=Japan, country[-3]=China
print(country[3])
# Traceback (most recent call last):
#   File "test.py", line 8, in <module>
#     print(country[3])
# IndexError: list index out of range
```

##### 向列表中插入元素
列表提供 **append** 方法向列表末尾增加新的元素，同时提供 **insert** 方法向指定位置插入元素：
```python
l1 = [1, 2, 3, 'China']
print(l1)
# 1, 2, 3, 'China']
l1.append('4')
print(l1)
# [1, 2, 3, 'China', '4']
l1.insert(1, 2)
print(l1)
# [1, 2, 2, 3, 'China', '4']
```
> 如果要修改某个元素，可以直接使用下标法来修改。

同一个列表中的元素可以是任意不同类型「在其他语言中列表中的元素必须是相同的数据类型」。

**append** 和 **insert** 方法提供了单次向列表中插入一个元素的能力，列表也提供了 **extend** 方法允许一次向一个列表中追加一个序列。
```python
l1 = [1, 2, 3, 'China']
l2 = [4, 5, 6]
print("l1={}, l2={}".format(l1, l2))
# l1=[1, 2, 3, 'China'], l2=[4, 5, 6]
l1.extend(l2)
print("l1={}".format(l1))
# l1=[1, 2, 3, 'China', 4, 5, 6]
```
> extend 方法将序列添加至当前序列的末尾。

##### 从列表中移除元素
Python 为列表提供了 **pop**、**remove**、**clear** 等方法用于移除列表中的元素。

**pop** 方法用于移除指定序号的元素，并返回该元素的值，当列表为空或参数超出列表的索引范围时 Python 将抛出 ```IndexError: pop from empty list``` 错误。**pop** 方法默认移除最后一个元素「即默认参数值为 -1」。
```python
l1 = [1, 2, 3, 'China']
print(l1.pop())
# China
print(l1)
# [1, 2, 3]
l1.pop(0)
print(l1)
# [1, 2]
l1.pop()
l1.pop()
l1.pop()
#Traceback (most recent call last):
#  File "test.py", line 9, in <module>
#    l1.pop()
#IndexError: pop from empty list
```
由于 ***pop*** 在索引超出范围时会抛出异常，因此在使用时需要先判断索引是否有效，可以使用 **len** 方法来判断当前列表中元素数量。
```
l1 = [1, 2, 3, 'China']
if len(l1) > 0:
    l1.pop()
```

**remove** 方法用于移除指定元素的第一个匹配项，该方法没有任何返回值，当列表中不存在指定的元素时 Python 将抛出 ```ValueError``` 错误。
```python
l1 = [1, 2, 3, 'China']
l1.remove(1)
print(l1)
# [2, 3, 'China']
l1.remove('Japan')
#Traceback (most recent call last):
#  File "test.py", line 5, in <module>
#    l1.remove('Japan')
#ValueError: list.remove(x): x not in list
```
使用 **remove** 方法移除元素时，先判断列表中存在该元素。
```
l1 = [1, 2, 3, 'China']
if len(l1) > 0:
    l1.pop()
if 'China' in l1:
    l1.remove('China')
if 'Japan' in l1:
    l1.remove('Japan')
print(l1)
# [1, 2, 3]
```

**clear**方法用于清空整个列表。
```
l1 = [1, 2, 3, 'China']
l1.clear()
print(l1)
# []
```

##### 列表提供的其他方法

- **count(obj)**: 统计某个元素在列表中出现的次数。
- **index(obj)**: 从列表中找出某个值第一个匹配项的索引位置。
- **reverse()**: 反向列表中的元素。
- **sort(key=None, reverse=False)**: 对原列表进行排序，
- **copy()**: 复制列表，返回一个新列表。

#### 字典「Dictionary」
Python 内置了字典的支持，字典「dict」使用键值对「key-value」来存储数据，具有非常快的数据存取速度，它直接通过键来获取对应的值，无需像列表需要通过遍历获取值。相应的字典会占用较多的内存空间，实际上字典是一种用空间来换时间的实现方法。

字典的 key 必须是不可变对象，因为字典是通过哈希算法利用 key 来计算 value 的位置，要想保证计算的结果不出错，必须保证 key 是不可变对象。

在 Python 中字典使用大括号进行定义，也可以通过 key 来读取和放入值。
```python
country = {'China' : 1, 'Japan' : 2, 'Korea' : 3}
print(country['China'])
# 1
country['Korea'] = 4
print(country)
# {'China': 1, 'Japan': 2, 'Korea': 4}
country['USA'] = 10
print(country)
# {'China': 1, 'Japan': 2, 'Korea': 4, 'USA': 10}
print(country['UK'])
#Traceback (most recent call last):
#  File "test.py", line 8, in <module>
#    print(country['UK'])
#KeyError: 'UK'
```
> 键值对在字典中的存储是无序的，存取顺序没有任何的关系。

当 key 不存在时，使用下标法来获取值会报 ```KeyError```，为了避免出现错误，可以使用 ```in``` 来判断 key 是否存在，或使用 get 方法来获取值。
```python
country = {'China' : 1, 'Japan' : 2, 'Korea' : 3}
print('China' in country)
# True
print('USA' in country)
# False
print(country.get('China'))
# 1
print(country.get('USA'))
# None
```
在字典中 key 是唯一的，当多次对同一个 key 赋值时，后面的值将会覆盖前面的值。
```python
country['Korea'] = 2
country['Korea'] = 10
pring(country['Korea'])
# 10
```
字典通过 ```pop``` 方法来删除指定 key 的键值对，并返回对应的值，若 key 不存在则返回默认值。通过 ```popitem``` 方法来删除字典中的最后一个键值对，同时放回该键值对组成的元组。
```python
country = {'China' : 1, 'Japan' : 2, 'Korea' : 3, 'USA': 4, 'UK': 5}
print(country.pop('UK', None))
# 5
print(country.pop('UK', None))
# None
print(country.popitem())
# ('USA', 4)
print(country)
# {'China': 1, 'Japan': 2, 'Korea': 3}
```
> 使用 pop 方法时，如果不确定 key 是否存在，一定要设置默认值，否则将出现 KeyError 错误。

#### 集合「Set」
集合是一组 key 的序列，由于 key 是不可重复的，因此集合可以用看做是去掉 value 的字典，或具有不重复数值的列表。

创建一个 set 需要传入一个列表，得到的集合会自动去除列表中的重复元素。
```python
l1 = [1, 2, 3, 4, 2, 3]
s = set(l1)
print(s)
# {1, 2, 3, 4}
```
set 提供了 ```add``` 方法来添加元素，```remove``` 方法来删除一个元素。
```python
l1 = [1, 2, 3, 4, 2, 3]
s = set(l1)
s.add('China')
print(s)
# {1, 2, 3, 4, 'China'}
s.remove(1)
print(s)
# {2, 3, 4, 'China'}
```
set 可以看做是数学意义上的无序和无重复数的集合，因此两个 set 可以做数学意义上的交集和并集等操作。
```python
s1 = set([1, 2, 3, 4, 5, 6])
s2 = set([2, 4, 6, 8, 10])
print(s1 & s2)
# {2, 4, 6}
print(s1 | se)
# {1, 2, 3, 4, 5, 6, 8, 10}
```

## 流程控制
Python 提供条件判断 ```if...elif...else...``` 和循环 ```while```、```for...in...``` 两种流程控制方式。

流程控制的主体是一个个语句块，Python 以冒号```:```来标识判断的结束紧跟的新行为语句块的内容，以缩进来标识语句块的开始和推出。

> 在 python 中条件判断和循环后必须包含有相应的语句块，否则解释器会报 SyntaxError 错误。如果你的语句块中没有需要执行的语句或暂未确定相关内容可以使用 pass 关键字。

### 条件判断

条件判断依据条件语句的真假来决定将要执行的语句块。一个完整的条件判断示例如下：
```python
age = 3
if age >= 18:
    print('adult')
elif age >= 6:
    print('teenager')
else:
    print('kid')
```
条件判断从上向下执行，当一个条件判断为真时，剩余其他的条件都将被放弃。```elif``` 和 ```else``` 并不是必须的，当你只有一个条件需要判断可以只保留```if```即可。
```python
if x:
    print(true)
```
条件判断的结果只要是非零整数、非空字符串、非空列表或其非空对象都将被判断为 True。
> 在 python 中，None，空列表、空字符串、空字典、0 等一系列代表空和无的对象都会被认定为 False，除此之外的其他对象将被判定为 True。

```python
x = None
if x:
    print('x is False')
if x is None:
    print('x is None')
if not x:
    print('not x is True')
```
以上代码的输出结果为：
```shell
x is None
not x is True
```

### 循环
> 循环是一段在程序中只出现一次，但可能会连续运行多次的代码。循环中的代码会运行特定的次数，或者是运行到特定条件成立时结束循环，或者是针对某一集合中的所有项目都运行一次。

#### while 循环
只要条件满足 while 循环就会不断执行下去，直到条件不满足时才会退出循环。
```
n = 0
while n < 5:
    print(n)
    n += 1
```
以上代码将输出：
```shell
0
1
2
3
4
```
#### for..in.. 循环
```for..in..```循环通常用来遍历列表等对象的元素，它将列表中每个元素单独提取出来进行使用。
```python
country = ['China', 'Japan', 'Koera']
for x in country:
    print(x)
```
以上代码将输出：
```shell
China
Japan
Koera
```

使用 for..in 来遍历字典的 key 和 value。
```python
country = {'China' : 1, 'Japan' : 2, 'Korea' : 3}

for (key, value) in country.items():
    print('{} = {}'.format(key, value))

for x in enumerate(country):
    print(x)
```
以上代码将输出：
```shell
China = 1
Japan = 2
Korea = 3
(0, 'China')
(1, 'Japan')
(2, 'Korea')
```
#### 循环的推出
**break**，用于提前推出循环，循环将提前结束。
```
country = {'China' : 1, 'Japan' : 2, 'Korea' : 3}

for (key, value) in country.items():
    if value == 2:
        break
    print('{} = {}'.format(key, value))
```
以上将输出：
```shell
China = 1
```
**continue**，用于退出本次循环，在循环体内 contiue 后的语句将不被执行，循环使用新的参数进行下一次循环。
```python
country = {'China' : 1, 'Japan' : 2, 'Korea' : 3}

for (key, value) in country.items():
    if value == 2:
        continue
    print('{} = {}'.format(key, value))
```
以上代码的执行结果
```shell
China = 1
Korea = 3
```

## 本节思维导图
![](https://raw.githubusercontent.com/keinYe/study/master/MindNode/Python/Python-basic.png)