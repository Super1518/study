在 Python 中可以使用列表生成式或生成器来快速创建一个列表。

> 列表生成式和生成器均是 Python 中的语法糖，这些语法糖使我们可以更加简洁、快速的实现功能。Python 中还有其他的语法糖，比如 if..else 三元表达式、with 语句、装饰器等等。

## 列表生成式
列表生成式是 Python 内置的强大的列表创建工具，可以用来快速的创建一个具有一定规则的列表。

正常情况下我们可以通过穷举的方式来创建一个列表，但是这种方式只适用于在列表元素较少的情况下「50 个元素以下」，如果元素太多或者元素的生成规则太复杂就不太现实了，当面对这种情况的时候，首先想到的是使用 for 循环的方式来实现，事实上我们可以用更加简单高效的列表生成式来实现。

列表生成式的语法结构：[exp for iter_var in iterable]，这是最简单的列表生成式语法，可以在此基础上增加 if...else 语句、嵌套循环等来实现更加复杂的功能。

```python
l1 = []
for x in range(1, 101):
    l1.append(x * x)

l2 = [x * x for x in range(1, 101)]

print(l1 == l2)
```
以上代码分别通过 for 循环和列表生成式来创建了一个 1~100 的平方的列表，虽然使用不同的方式来创建，但是他们完全相等。列表生成式无需创建一个空列表，代码更加清晰和简单。

我们可以通过在 for...in 循环后增加 if 语句来计算出 1~100 中能被 10 整除的书的平方。
```python
l1 = [x * x for x in range(1, 101) if x % 10 == 0]
print(l1)
# [100, 400, 900, 1600, 2500, 3600, 4900, 6400, 8100, 10000]
```
我们可以通过在 for...in 循环前面增加 if...else 表达式来对元素进行判断。
```python
l1 = [1, 2, -4, 5, 6, -8, -10]
l2 = [x if x > 0 else -x for x in l1]
print(l2)
# [1, 2, 4, 5, 6, 8, 10]
```
> 当 if 语句位于循环后面是不能带有 else，当 if 语句位于循环前面时必须带有 else，否则会报 SyntaxError 错误。

可以通过嵌套循环来实现一个全排列，看一下代码：
```python
l1 = [x + y for x in 'abc' for y in '1234']
print(l1)
# ['a1', 'a2', 'a3', 'a4', 'b1', 'b2', 'b3', 'b4', 'c1', 'c2', 'c3', 'c4']
```

## 生成器
列表生成式一次性生成一个列表，此时会引入另外一个问题，列表很大的时候会占用很大的内存，会产生内存耗尽的问题，此时就要用到生成器，生成器和列表生成式最大的不同就是，生成器在运行过程中根据需要生成相应的元素。
> 生成器就想一段代码，调用一次就生成一个元素，尽可能的减少内存的占用。

**普通生成器**

生成器和列表生成式最大的区别，一个是一次生成所有内容，一个是需要多少生成多少。列表生成式非常方便就可以转换为生成器，只需要将方括号换成圆括号即可。
```python
l1 = (x + y for x in 'abc' for y in '1234')
print(l1)
# <generator object <genexpr> at 0x10dccb8c0>
```
从以上结果可以看出，将方括号换成圆括号后，结果就不再是一个具有实际数据的列表了，而是一个生成器「generator」。那么我们如何才能获取列表中的元素呢？可以通过 Python 内置的 next 函数或使用 for 循环来从生成器中获取所有的数据。
```
l1 = (x + y for x in 'abc' for y in '1234')
print(next(l1))
print(next(l1))
for x in l1:
    print(x)
```
以上代码的输出结果：
```shell
a1
a2
a3
a4
b1
b2
b3
b4
c1
c2
c3
c4
Traceback (most recent call last):
  File "test.py", line 9, in <module>
    print(next(l1))
StopIteration
```
生成器保存的是算法，每次调用都生成一个新的元素，且生成器内部会保存当前状态，再次调用的话会从当前位置接续计算。因此在上面的代码中我们先调用 next 在使用 for 循环，但是生成的元素确实接续没有间断的。

> 当生成器计算到最后一个元素时，再次调用 next 函数解释器会抛出 StoIteration，因此对于生成器来说尽量不要使用 next 函数而使用 for 循环。

**生成器函数**

使用 for 循环来创建的生成器实现的功能比较简单，只能应用在比较简单的场合，生成器本身的功能却非常强大，这明显不符合它的设计预期，那是因为生成器还有一个大招，那就是使用函数来创建一个生成器。

> 生成器函数和普通函数的最大区别就是生成器函数包含有关键字 yield 关键字。

如果一个函数中包含 yield 关键字，那么它就不再是一个普通函数，而是一个生成器函数。生成器函数每次执行到 yield 语句时会从当前函数返回，当下次运行时会从 yield 语句开始执行，从而实现生成器的功能。

下面我将前面计算 1~100 以内所有能被 10 整除的数的平方使用生成器函数来实现。看一下代码：
```python
def square_generator():
    for x in range(1, 101):
        if (x % 10 == 0):
            yield x * x
square = square_generator()
print(square)
for x in square:
    print(x)
```

执行以上代码，将输出如下结果：
```shell
<generator object square_generator at 0x10b99d820>
100
400
900
1600
2500
3600
4900
6400
8100
10000
```

生成器函数也可以带有 return 用于从函数返回一个值，当执行到该语句时整个生成器将结束。使用 for 循环是无法获取 return 语句的返回值的，若要获取 return 语句的返回值需要使用 next 函数来捕获 StopIteration 错误。
```python
def square_generator():
    for x in range(1, 101):
        if (x % 10 == 0):
            yield x * x
    return 'Done'

square = square_generator()

while True:
    try:
        print(next(square))
    except StopIteration as e:
        print("return value = ", e.value)
        break
```
代码的运行结果如下：
```shell
100
400
900
1600
2500
3600
4900
6400
8100
10000
return value =  Done
```
> 带有 return 语句的生成器函数，只有在 python3 方才有效，在 python2 使用时解释器将抛出 SyntaxError: 'return' with argument inside generator。
>
> 对于生成器函数来说，当遇到 return 语句或者执行到函数体最后一行语句，就是结束生成器函数的条件，生成器随之结束。
