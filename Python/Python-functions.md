什么是函数？函数是将可重复使用的代码片段「或程序块」单独提取出来，并给予命名，在需要的地方直接通过名字来调用函数并获取计算结果。函数几乎是所有高级编程语言的必备功能，Python 也不例外，其本身就内置很多常用的函数可以直接调用。
[Python 内置函数列表](https://docs.python.org/3/library/functions.html#abs)

## 函数的使用
函数的使用和变量类似，直接使用其名称进行调用即可「函数的调用需要在名称后加上小括号」。如果函数有返回值可以直接使用赋值语句赋值给其他的变量「如果不需要返回值也可以忽略」，如果需要向函数中传入参数，将参数依次填写在括号内。
```python
l1 = [1, 2, 3, 4, 5]
length = len(l1)
print(length)
# 5
max_number = max(1, 4, 3)
print(max_number)
# 4
print(dir())
# ['__annotations__', '__builtins__', ...]
```

## 函数的定义
在 Python 中函数通过关键字 ```def``` 来定义，后面依次写出名称、括号、括号内的参数「如果有的话」和冒号，改行结束。新起一行作为函数体，以缩进来标识开始和退出函数体。
```
def 函数名称(参数列表):
    函数体
```
下面来定义一个实际的函数，简单的打印一行 ```Hello World!```。
```python
def helloWorld():
    print('Hello World!')
    
helloWorld()
# Hello World!
```

一个函数如果只有函数名称而没有函数体，解释器将会报 ```SyntaxError``` 错误。然而在有些情况下我们却需要能够定义空函数，此时我们可以通过 ```pass``` 占位符来解决这个问题。
```python
def nop():
    pass
```

### 函数参数
函数的调用者通过函数参数向函数传递信息，函数通过参数计算返回给调用者相应的结果。函数参数与变量类似，在函数调用时已经被定义，并且在函数运行时已完成赋值，在函数内部我们可以直接使用。

函数参数放置在定义函数时的圆括号内，多个参数之间使用逗号分开。当调用函数时，只需要知道函数定义且按照正确的方式传入相应的参数，就可以获取正确的返回结果了。

> 形参：在定义函数时给定的参数名称。实参：在调用函数时给定的参数值。

```python
def max(a, b):
    if a > b:
        print('a > b')
    else:
        print('a <= b')

max(1, 3)
# a <= b
x = 4
y = 3
max(x, y)
# a > b
```

Python 的函数定义虽然简单，但是根据不同的需要，函数参数除了正常定义的参数外，还可以使用默认参数、可变参数和关键字参数，使得函数定义出来的接口不仅可以处理复杂的参数，还可以简化调用者的代码。
#### 默认参数
对于有些函数来说，可能希望部分参数可选或者具有默认值，这样在函数调用时既可以减少代码，又可以保证函数的正常运行。此时可以在函数定义时通过给参数附件一个赋值运算符为其提供一个默认值。
```python
def say(message, times = 1):
    print(message * times)

say('hello')
# hello
say('world', 2)
# worldworld
```
> 默认参数的要求：
> 1. 默认参数的值必须是常数「默认参数值应该是不可变的」。
> 2. 必选参数在前，默认参数在后，否则解释器会报错。
> 3. 具有多个参数时，变化大的参数在前，变化小的参数在后，变化小的参数可以作为默认参数。

#### 可变参数
在定义函数时，有时需要函数能够接收任意数量的参数「比如我们经常用的 ```print``` 函数就可以接收任意数量的函数」。通过在参数名称前加星号的方式，将该函数的参数定义为可变参数。
```python
def calc(*nums):
    print(type(nums))
    for num in nums:
        print(num)

calc(1, 2, 3, 4)
```
运行以上代码将输出以下内容：
```shell
<class 'tuple'>
1
2
3
4
```
从以上结果可以看出可变参数在函数内部被定义为一个元组「tuple」来使用，那么如果我们需要向函数传入一个元组或列表会出现什么情况。我们来看以下代码。
```python
def calc(*nums):
    print(type(nums))
    for num in nums:
        print(num)

l1 = [1, 2, 3, 4]
calc(l1)
t1 = (1, 2, 3, 4)
calc(t1)
```
运行后的输出结果
```shell
<class 'tuple'>
[1, 2, 3, 4]
<class 'tuple'>
(1, 2, 3, 4)
```
很明显输出结果并不符合我们的预期，实际上函数是将整个列表作为一个参数来使用了，相当于只给函数了一个参数，此时可以通过在传入参数前加星号来讲列表或元组作为一个可变参数传入函数。
```python
def calc(*nums):
    print(type(nums))
    for num in nums:
        print(num)

l1 = [1, 2, 3, 4]
calc(*l1)
t1 = (1, 2, 3, 4)
calc(*t1)
```

#### 关键字参数
关键字参数允许传入 0 个或多个含有参数名的参数，这些参数在函数内部将自动装载为一个字典。此时只需要在参数名称前加两颗星即可。
```python
def person(name, age, **kw):
    print ('name = {}, age = {}'.format(name, age))
    for (key, value) in kw.items():
        print('{} = {}'.format(key, value))

person(name='kein', age='30', city='Shenzhen', country='China')
```
以上代码的执行结果如下：
```shell
name = kein, age = 30
city = Shenzhen
country = China
```
> 关键字参数的优点：
> 1. 不再需要考虑参数的顺序，函数的使用将更加容易。
> 2. 可以只对那些我们希望赋予的参数以赋值，只要其它的参数都具有默认参数值。




### 局部变量
当在函数体内声明变量时，在写变量将只能在函数体内使用，如果函数体外存在相同名称的变量，则其不会影响函数体外变量的值，但是在函数体内访问的永远是函数体内变量的值。

```python
x = 5

def func(x):
    x += 2
    print('local x = ', x)
    # local x = 7
func(x)
print('global x = ', x)
# global x = 5
```

### 函数返回值
Python 中函数没有明确返回值的类型，函数通过 ```retrun``` 语句来返回一个值，如果 ```return``` 语句后没有携带任何值或函数中没有 ```return``` 语句则默认返回 ```None```。

```python
def max(a, b):
    if a > b:
        return a
    elif a < b:
        return 

max1 = max(1, 3)
max2 = max(2, 1)
max3 = max(2, 2)
print('max1 = {}, max2 = {}, max3 = {}'.format(max1, max2, max3))
# max1 = None, max2 = 2, max3 = None
```

## 函数的递归调用
在函数内部可以调用其他函数，如果函数调用其自身，那么就是递归函数。
```python
def fact(n):
    if n==1:
        return 1
    return n * fact(n - 1)

print(fact(1))
# 1
print(fact(20))
# 2432902008176640000
```
所有的递归函数都可以用循环来实现，但是循环的逻辑没有递归清晰。使用递归函数容易引起内存溢出，应谨慎使用递归函数。