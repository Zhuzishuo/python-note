# 函数式编程

## 一、高阶函数

函数名是一种指向函数的变量，因此函数名也可作为参数，即一个函数也可作为参数传入另一个函数，这种可以接受一个函数作为参数的函数被称为高阶函数。

python中内建了`map()` 、`reduce()`、`filter()`、`sorted()`高阶函数：

### 1、map()/reduce()

**map函数会根据提供的函数对指定序列做映射**

`map(function, sequence[, sequence, ...] )`

* function：是一个函数
* sequence：是一个或多个序列，取决于function需要几个参数
* 返回值是一个`map`（`Iterator`）

```python
upper_name = map(lambda x: x.title() , ['zhuzishuo','hahahah','xixixi'])
for i in upper_name:
    print(i)

upper_name = list(map(lambda x: x.title() , ['TOm','AdMin','PyTHon']))
print(upper_name)
'''
输出结果：
Zhuzishuo
Hahahah
Xixixi
['Tom', 'Admin', 'Python']
'''
```

参数序列中的每一个元素分别调用function函数。由于结果`upper_name`是一个`Iterator`，`Iterator`是惰性序列，因此通过`list()`函数让它把整个序列都计算出来并返回一个list。

**reduce函数会根据提供的函数对指定序列作累积计算**

`reduce(function, sequence)`

* function：是一个函数
* sequence：`function`作用的序列（`[x1, x2, x3, ...]`）
* 返回值是所有元素累积计算的结果

```python
from functools import reduce
a = reduce(lambda x,y:x*y,[1,23,45,67,78,9])
print(a)
'''
输出结果：
48680190
'''
```

`reduce`把一个函数作用在一个序列 `[x1, x2, x3, ...]`上，这个函数必须接收两个参数，`reduce`把结果继续和序列的下一个元素做累积计算。

**map函数和reduce函数的区别在于：**

map每次操作一个或多个参数，最后返回包含所有元素操作结果的`Iterator`；

reduce函数每次操作两个参数（前面所有元素操作累积计算的结果和下一个元素），最后返回所有元素累积计算的结果。

### 3、filter()

`filter(function, sequence)`

* function：是一个函数
* sequence：是一个序列（`Iterable`）
* 返回值是一个`Iterator`

```python
def a(x):
    for i in range(2,x):
        if x % i == 0:
            return False
    return True
c= list(filter(a,list(range(2,101))))
print(c)
'''
输出结果：
[2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37, 41, 43, 47, 53, 59, 61, 67, 71, 73, 79, 83, 89, 97]
'''
```

`filter()`函数用于过滤序列。`filter()`把传入的函数依次作用于每个元素，然后根据返回值是`True`还是`False`决定保留还是丢弃该元素。

`filter()`函数返回的是一个`Iterator`，也就是一个惰性序列，所以要强迫`filter()`完成计算结果，需要用`list()`函数获得所有结果并返回list。

### 4、sorted()

`sorted()`函数也是一个高阶函数，它还可以接收一个`key`函数来实现自定义的排序。用`sorted()`排序的关键在于实现一个原序列和key的映射函数。`sorted()`按照`key进行`排序，如下例，lambda函数提供了L中各元组的第一个元素，`sorted()`按各元组的第一个元素的大小进行排序。

```python
L = [('Bob', 75), ('Adam', 92), ('Bart', 66), ('Lisa', 88)]
print(sorted(L,key=lambda t:t[0]))
'''
输出结果：
[('Adam', 92), ('Bart', 66), ('Bob', 75), ('Lisa', 88)]
'''
```

## 二、返回函数

高阶函数除了可以接受函数作为参数外，还可以把函数作为结果值返回。

```python
def count():
    fs = []
    for i in range(1, 4):
        def f():
             return i*i
        fs.append(f)
    return fs

f1, f2, f3 = count()
print(f1())
print(f2())
print(f3())
'''
输出结果：返回函数只有返回的函数被调用时才会被执行，当被执行时，i的值已经变换为3，所以三个输出均为9
9
9
9
'''
def count():
    def f(j):
        def g():
            return j*j
        return g
    fs = []
    for i in range(1, 4):
        fs.append(f(i)) # f(i)立刻被执行，因此i的当前值被传入f()
    return fs

f1, f2, f3 = count()
print(f1())
print(f2())
print(f3())
'''
输出结果：返回的函数在被调用前已经是执行完毕，因此返回的是执行后的结果，而不是未执行的函数
1
4
9
'''
```

**注意：返回函数中不要引用任何可能会发生变化的变量**

## 三、匿名函数

`返回函数名 = lambda 参数列表：函数返回值表达式语句`

匿名函数有个限制，就是只能有一个表达式，不用写`return`，返回值就是该表达式的结果。

**lambda表达式数组**

`数组名 = [(lambda表达式1),(lambda表达式2),…]`

`数组名[索引](lambda表达式的参数列表)`

**lambda递归**

```python
#lambda递归启蒙式
g = lambda n,s=lambda n,f:1 if n < 2 else f(n-1,f)*n : s(n,s)
a = g(10)
print(a)
'''
输出结果：外lambda中以变量和另一个lambda为参数，执行语句执行的就是参数lambda，因此lambda递归的核心在于作为参数的lambda
3628800
'''
```

```python
a = list()
f = lambda n,m,s=lambda n,m,f: a.append(int(n)) if n < 10 else f((n-n%10)/10,a.append(int(n%10)),f): s(n,m,s)
b = f(1234567890,a)
a.reverse()
for i in a:
    print(i)
'''
输出结果：
1
2
3
4
5
6
7
8
9
0
'''
```

## 四、装饰器

```python
def log(func):
    def wrapper(*args, **kw):
        print('call %s():' % func.__name__)
        return func(*args, **kw)
    return wrapper

#观察上面的log，因为它是一个decorator，所以接受一个函数作为参数，并返回一个函数。我们要借助Python的@语法，把decorator置于函数的定义处：

@log
def now():
    print('2015-3-25')
now()
```

由于`log()`是一个decorator，返回一个函数，所以，原来的`now()`函数仍然存在，只是现在同名的`now`变量指向了新的函数，于是调用`now()`将执行新函数，即在`log()`函数中返回的`wrapper()`函数。

`wrapper()`函数的参数定义是`(*args, **kw)`，因此，`wrapper()`函数可以接受任意参数的调用。在`wrapper()`函数内，首先打印日志，再紧接着调用原始函数。

如果decorator本身需要传入参数，那就需要编写一个返回decorator的高阶函数，写出来会更复杂。比如，要自定义log的文本：

```python
def log(text):
    def decorator(func):
        def wrapper(*args, **kw):
            print('%s %s():' % (text, func.__name__))
            return func(*args, **kw)
        return wrapper
    return decorator
@log('execute')
def now():
    print('2015-3-25')
now()
'''
输出结果：
execute now():
2015-3-25
'''
```

因为返回的那个`wrapper()`函数名字就是`'wrapper'`，所以，需要把原始函数的`__name__`等属性复制到`wrapper()`函数中，否则，有些依赖函数签名的代码执行就会出错。

不需要编写`wrapper.__name__ = func.__name__`这样的代码，Python内置的`functools.wraps`就是干这个事的，所以，一个完整的decorator的写法如下：

```python
import functools

def log(func):
    @functools.wraps(func)
    def wrapper(*args, **kw):
        print('call %s():' % func.__name__)
        return func(*args, **kw)
    return wrapper
```

```python
import functools

def log(text):
    def decorator(func):
        @functools.wraps(func)
        def wrapper(*args, **kw):
            print('%s %s():' % (text, func.__name__))
            return func(*args, **kw)
        return wrapper
    return decorator
```

`import functools`是导入`functools`模块。模块的概念稍候讲解。现在，只需记住在定义`wrapper()`的前面加上`@functools.wraps(func)`即可。

## 五、偏函数

简单总结`functools.partial`的作用就是，把一个函数的某些参数给固定住（也就是设置默认值），返回一个新的函数，这个新函数就是偏函数，调用这个新函数会更简单。

```python
import functools
int2 = functools.partial(int, base=2)
a = int2('1000000')
print(a)
'''
输出结果：
64
'''
```
