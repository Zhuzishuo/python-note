# 函数

### 1、函数的特性

1. 代码重用
2. 保持一致性
3. 可扩展性

### 2、函数的创建

* **使用def关键字创建函数：**

```python
def hello():
    print("hello world")

hello()
```

> 请注意，函数体内部的语句在执行时，一旦执行到`return`时，函数就执行完毕，并将结果返回。因此，函数内部通过条件判断和循环可以实现非常复杂的逻辑。
>
> 如果没有`return`语句，函数执行完毕后也会返回结果，只是结果为`None`。`return None`可以简写为`return`。

* **空函数**

使用pass语句定义一个空函数，不执行任何操作：

```python
def nop():
    pass
```

> 实际上`pass`可以用来作为占位符，比如现在还没想好怎么写函数的代码，就可以先放一个`pass`，让代码能运行起来。

* **返回多个值**

函数的return 语句只能返回一个值，可以是任何类型的值

**通过返回一个tuple 类型，来间接实现返回多个值，** 在语法上，返回一个tuple可以省略括号，而多个变量可以同时接收一个tuple，按位置赋给对应的值

```python
def return_double(a,b):
    c = a / b;
    d = a % b;
    return c,d

a = int(input("Enter a:"))
b = int(input("Enter b:"))
e,f = return_double(a,b)
print("结果等于 %d,余数为 %d" %(e,f))
```

### 3、函数的参数

* **必选参数**

```python
def my_power(x,n):
    result = 1
    for i in range(n):
        result = result * x
    return result

x = int(input('Enter x:'))
n = int(input('Enter n:'))

print(my_power(x,n))
```

> 函数有两个参数：`x`和`n`，这两个参数都是位置参数，调用函数时，传入的两个值按照位置顺序依次赋给参数`x`和`n`。

* **默认参数**

```python
def students(name,gender,age=23,city='foshan'):
    print('name: ',name,'\ngender: ',gender,'\nage: ',age,'\ncity: ',city)

students('zhuzishuo','three')

students('lurenjia','three',17)
students('xuheliang','two',city='guangzhou')
```

> 有多个默认参数时，调用的时候，既可以按顺序提供默认参数，比如调用`students('Bob', 'M', 7)`，意思是，除了`name`，`gender`这两个参数外，最后1个参数应用在参数`age`上，`city`参数由于没有提供，仍然使用默认值。
>
> 也可以不按顺序提供部分默认参数。当不按顺序提供部分默认参数时，需要把参数名写上。比如调用`students('Adam', 'M', city='Tianjin')`，意思是，`city`参数用传进去的值，其他默认参数继续使用默认值。

***

> 定义默认参数要牢记一点：**默认参数必须指向不变对象！** Python函数在定义的时候，默认参数L的值就被计算出来了，即\[]，因为默认参数L也是一个变量，它指向对象\[]，每次调用该函数，如果改变了L的内容，则下次调用时，默认参数的内容就变了，不再是函数定义时的\[]了

* **可变参数**

可变参数允许你传入0个或任意个参数，这些可变参数在函数调用时自动组装为一个tuple。

```python
def calc(*numbers):
    sum = 0
    for n in numbers:
        sum = sum + n * n
    return sum

print(calc(1, 2))
nums = [1,2,3,4,5,6]
print(calc(*nums))
```

> 定义可变参数，仅仅在形参前面加了一个`*`号，在函数内部，参数`numbers`接收到的是一个tuple。
>
> `*nums`表示把`nums`这个list的所有元素作为可变参数传进去。

* **关键字参数**

关键字参数允许你传入0个或任意个含参数名的参数，这些关键字参数在函数内部自动组装为一个dict。

```python
def person(name,age,**kw):
    print('name: ',name,'\nage',age,'\nother: ',kw)

person('zhuzishuo',23)
person('zhuzishuo',23,city='foshan')
extra = {'city': 'Beijing', 'job': 'student'}
person('zhuzishuo',23,**extra)
```

> `**extra`表示把`extra`这个dict的所有key-value用关键字参数传入到函数的`**kw`参数，`kw`将获得一个dict，注意`kw`获得的dict是`extra`的一份拷贝，对`kw`的改动不会影响到函数外的`extra`。

* **命名关键字参数**

如果要限制关键字参数的名字，就可以用命名关键字参数

```python
def person(name,age,*,city,job):
    print(name,age,city,job)

person('zhuzishuo',23,city='foshan',job='student')
```

命名关键字传参时必须要有参数和参数名，否则就会报错

若命名关键字具有默认值，则可不传入参数

```python
def person(name,age,*,city='foshan',job):
    print(name,age,city,job)

person('zhuzishuo',23,job='student')
```

> 如果函数定义中已经有了一个可变参数，后面跟着的命名关键字参数就不再需要一个特殊分隔符`*`了。
>
> 如果没有可变参数，就必须加一个`*`作为特殊分隔符。如果缺少`*`，Python解释器将无法识别位置参数和命名关键字参数

* **参数组合**

python定义函数可以将必选参数、默认参数、可变参数、命名关键字参数和关键字参数5种参数组合使用，使用时参数的顺序为必选参数、默认参数、可变参数、命名关键字参数和关键字参数。

```python
def person(name,age,city='foshan',*job,**kw):
    print(name,age,city,job,kw)

job = ['student','students']
person('zhuzishuo',23,job=job,sdfasdf='dfasdfasdf')

def person(name,age,city='foshan',*,job,**kw):
    print(name,age,city,job,kw)

person('zhuzishuo',23,job='student',dffdsd='ffssdsdsdsd')
```

> 虽然可以组合多达5种参数，但不要同时使用太多的组合，否则函数接口的可理解性很差。

* **拆包**

调用函数时在实参前加`*`表示将列表、元组拆开元素作参数值传入函数作可变参数，在实参前加`**`表示将字典拆开key元素为参数名，value元素为参数值再传入函数作关键字参数。

```python
def depart(a,b,*args,**kwargs):
    print(a)
    print(b)
    print(args)
    print(kwargs)

A = [1,2,3,4,5,6]
B = {"name":'Zhuzishuo',"age":23}
depart('hahaha','hohoho',A,B)
'''
输出结果：
hahaha
hohoho
([1, 2, 3, 4, 5, 6], {'name': 'Zhuzishuo', 'age': 23})
{}
'''
def depart(a,b,*args,**kwargs):
    print(a)
    print(b)
    print(args)
    print(kwargs)

A = [1,2,3,4,5,6]
B = {"name":'Zhuzishuo',"age":23}
depart('hahaha','hohoho',*A,*B)
'''
输出结果：
hahaha
hohoho
(1, 2, 3, 4, 5, 6)
{'name': 'Zhuzishuo', 'age': 23}
'''
```

### 4、变量的作用域

Python的作用域一共有4种，分别是：

* L （Local） 局部作用域
* E （Enclosing） 闭包函数外的函数中
* G （Global） 全局作用域
* B （Built-in） 内建作用域

```python
x = int(2.9)  # 内建作用域
 
g_count = 0  # 全局作用域
def outer():
    o_count = 1  # 闭包函数外的函数中
    def inner():
        i_count = 2  # 局部作用域
```

> 以 L –> E –> G –>B 的规则查找，即：在局部找不到，便会去局部外的局部找（例如闭包），再找不到就会去全局找，再者去内建中找。

> Python 中只有模块（module），类（class）以及函数（def、lambda）才会引入新的作用域，其它的代码块（如 if/elif/else/、try/except、for/while等）是不会引入新的作用域的，也就是说这些语句内定义的变量，外部也可以访问

**全局变量和局部变量（大访小规则）**

定义在函数内部的变量拥有一个局部作用域，称为局部变量，定义在函数外的拥有全局作用域，称为全局变量。

局部变量只能在其被声明的函数内部访问；

全局变量可以在整个程序范围内访问。

\*\*global 和 nonlocal关键字（小访大规则） \*\*

当内部作用域想修改外部作用域的变量时，就要用到global和nonlocal关键字了。

如果函数内部需要修改全局作用域中的变量，需要使用global关键字声明该变量；

如果要修改嵌套作用域（enclosing 作用域，外层非全局作用域）中的变量则需要 nonlocal 关键字声明该变量。
