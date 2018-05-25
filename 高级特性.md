# Python学习笔记 —— 高级特性

在python中，代码不是越多越好，而是越少越好。代码不是越复杂越好，而是越简单越好。基于这一思想，python中有一些非常有用的高级特性。**1行代码能实现的功能，决不写5行代码。代码越少，开发效率越高。** 

### 1、切片

range是一个系统函数,返回一个特定的有序的自然数，默认从零开始。

```python
a = [1,2,3,4,5,6,7,8,9,10]
print(a[1:])#取下标从1开始到最后一个的列表
print(a[1:-1])#取下标从1开始到倒数最后一个，但不包括最后一个
print(a[1:-6])#取下标从1开始到倒数第6个，但不包括倒数第6个
print(a[1::3])#最后一个数字代表步长，隔几个下标取
print(a[5:3:-2])#从下标为5的元素开始往前取，取到下标为3的元素，步长为2
```
> tuple也是一种list，唯一区别是tuple不可变。因此，tuple也可以用切片操作，只是操作的结果仍是tuple
>
> 字符串`'xxx'`也可以看成是一种list，每个元素就是一个字符。因此，字符串也可以用切片操作，只是操作结果仍是字符串
>
> 然集合和字典就不能用切片，因为集合和字典没有下标

### 2、迭代

python中可以通过`for`循环来遍历，这种遍历我们称为迭代（Iteration）。

```python
list1 = ['zhuzishuo',23,'student']
for i in list1:
    print(i, end=' ')
print()

tuple1 = ('zhuzishuo',23,'student')
for i in tuple1:
    print(i, end=' ')
print()

dic = {'name':'zhuzishuo','age':23,'job':'student'}
for i in dic:#遍历keys
    print(i, end=' ')
print()
for i in dic.values():#遍历values
    print(i, end=' ')
print()
for i in dic.items():#遍历keys和values
    print(i, end=' ')
print()

string = 'zhuzishuo'
for i in string:
    print(i,end=' ')
```

使用for循环只要是作用于一个可迭代对象（Iterable的子类），不要求有下标，`for`循环就可以正常运行。通过系统函数isinstance()可以判断对象是否是Iterable的子类。

```python
from collections import Iterable

list1 = list(range(0,10))
print(isinstance(list1,Iterable))#True
index = 1
print(isinstance(index,Iterable))#False
```

如果想对list实现下标循环，则python内置的`enumerate函数` 可以把一个list变成索引-元素对，这样就可以在for循环中同时迭代索引和元素本身：

```python
for i,v in enumerate(['a','b','c','d','e']):
    print(i,v)
```

### 3、列表生成式

列表生成式即List Comprehensions，是Python内置的非常简单却强大的可以用来创建list的生成式。

- 生成list `[1, 2, 3, 4, 5, 6, 7, 8, 9, 10]`可以用`list(range(1, 11))`：

```python
list1 = list(range(1,11))
#[1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
```

- 生成list:L = [1,4,9,16,25,36,49,64,81]，使用一条语句即可生成：

```python
list1 = [x*x for x in range(1,10)]
#[1, 4, 9, 16, 25, 36, 49, 64, 81]
```

- 在两个列表中做两层循环运算，可以生成全排列：

```python
list1 = [x + y for x in ['a','b','c'] for y in ['d','e','f']]
# ['ad', 'ae', 'af', 'bd', 'be', 'bf', 'cd', 'ce', 'cf']
```
- 使用两个变量生成list：

```python
dic1 = {'a':'A','b':'B','c':'C'}
list1 = [x + '=' + y for x,y in dic1.items()]
print(list1)
```

- 把一个list中所有的字符串变成小写：

```python
list2 = [s.lower() for s in list1]
print(list2)
```

### 4、生成器

创建`L`和`g`的区别仅在于最外层的`[]`和`()`，`L`是一个list，而`g`是一个generator。

```python
list1 = [x * x for x in range(10)]
print(list1)#[0, 1, 4, 9, 16, 25, 36, 49, 64, 81]

list2 = (x * x for x in range(10))
print(list2)#<generator object <genexpr> at 0x03736C00>
for i in list2:
    print(i,end=' ')
#0 1 4 9 16 25 36 49 64 81 
```

> generator保存的是算法，每次调用next(g)，就计算出g的下一个元素的值，直到计算到最后一个元素，没有更多的元素时，抛出StopIteration的错误。列表元素通过某种算法推算出来，不必创建一个完整的list，节省了大量的空间。

第二种创建generator的方法：一个函数定义中包含`yield`关键字，那么这个函数就不再是一个普通函数，而是一个generator：

实例：斐波拉契数列

```python
def generator_fib(max):
    n, a, b= 0, 0, 1
    while n < max:
        yield b
        a, b = b, a + b
        n = n + 1
    return 'done'
g = generator_fib(5)
while True:
    try:
        t = next(g)
        print('g:', t)
    except StopIteration as e:
        print('Generator return value:', e.value)
        break
```

实例：杨辉三角

```python
def triangles():
    N = [1]
    while True:
        yield N
        N.append(0)
        N = [N[i] + N[i - 1] for i in range(len(N))]
n = 0
for t in triangles():
    print(t)
    n = n + 1
    if n == 10:
        break
```
> 这里我们要清楚`yield`的使用规则：函数中return语句或者最后一行函数语句就返回。而变成generator的函数，在每次调用next()的时候执行，遇到yield语句返回，再次执行时从上次返回的yield语句处继续执行。

### 5、迭代器

- 可以直接作用于`for`循环的对象统称为可迭代对象：`Iterable`。

  可以直接作用于`for`循环的数据类型有以下几种：

  1. 集合数据类型，如list、tuple、dict、set、str等；

  2. 类是generator，包括生成器和带yield的generator function。

- 可以被`next()`函数调用并不断返回下一个值的对象称为迭代器：`Iterator`。

> 生成器都是`Iterator`对象，但`list`、`dict`、`str`虽然是`Iterable`，却不是`Iterator`。把`list`、`dict`、`str`等`Iterable`变成`Iterator`可以使用`iter()`函数。
>
> Python的`Iterator`对象表示的是一个数据流，Iterator对象可以被`next()`函数调用并不断返回下一个数据，直到没有数据时抛出`StopIteration`错误。可以把这个数据流看做是一个有序序列，但我们却不能提前知道序列的长度，只能不断通过`next()`函数实现按需计算下一个数据，所以`Iterator`的计算是**惰性**的，只有在需要返回下一个数据时它才会计算。