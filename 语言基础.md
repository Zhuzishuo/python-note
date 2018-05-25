# Python学习笔记 —— 语法基础
[TOC]

## 一、基础语句

简单的语句一些直接上代码了

- 输出`print()`

- 输入`input()`

- 循环`if 条件:`

- 循环`for i in rang:`或`while 条件:`

- 注释`#`

```python
username = "zhuzishuo"
password = "123654"

counter = 0

# test1
for i in range(3):
    user_name = input("Name :")
    pass_word = input("Password :")
    if user_name == username and pass_word == password :
        print("welcome %s login..." % username)
        break
    else :
        print("the name or the password is error!")
        print("you have %d chances" % int(2-counter))
else:
    print("臭不要脸，错这么多次还来！")

# test2
while counter < 3 :
    user_name = input("Name :")
    pass_word = input("Password :")
    if user_name == username and pass_word == password:
        print("welcome %s login..." % username)
        break
    else:
        print("the name or the password is error!")
        print("you have %d chances" % int(2-counter))
    counter += 1
else:
    print("臭不要脸，错这么多次还来！")
```


## 二、数据类型

### 1、数字Number



### 2、字符串String

**String 操作**

- 重复输出字符串

```python
print('hello'*20)
```

- []，[:] 通过索引获取字符串中字符，这里和列表的切片操作是相同的

```python
print('helloworld'[2:])
```

- 关键字 in

```python
print(123 in [23,45,123])
print('el' in 'hello')
```

- 字符串格式 % 

> '''  '''也可作多行注释用

```python
name = input("Name is :")
age = int(input("Age is :"))#默认输入字符串
job = input("Job is :")
salary = input("Salary is :")

if salary.isdigit(): #判断salary长的像不像数字
    salary = int(salary)
else:
    # print()
    exit("must print digit") #直接退出程序

msg = '''
------------------ info of %s -----------------
Name is : %s
Age is : %d
Job is : %s
Salary is : %d
you will be retired in %d year
------------------- end ------------------------
''' % (name,name,age,job,salary,65-age)
print(msg)
```

- 字符串拼接

```python
a = '123'
b = 'abc'
d = 'dfgfgfdgdsddfdf'
c = a + b
print(c)
# join():可在拼接的字符串之间加上''中的字符串
c = '*********'.join([a,b,d])
print(c)
```

## 三、数据结构

### 1、列表list

**查**

- 索引（下标），都是从0开始  
- 切片  

- count：查某个元素的出现次数  

```python
t = ['to','be','or','not','to','be'].count('to')
```

- index：根据内容找其对应的位置  

```python
a = ['to','be','or','not','to','be','to']
print(a.index('to',5))
#通过元素的值获取对应的下标，若有重复则获取第一个的下标，后面的数字是表示从这一下标开始找该值，可用于重复值找非第一个的下标
```

- 关键字in：查看元素是否在列表中  

```python
a = ['to','be','or','not','to','be','to']
print("afdfasd" in a)
```

**增**

- a.append() 追加  

```python
a = [1,2,3,4,5,6,7,8,9,10]
a.append(11)#将数据添加到列表最后位置
print(a)
```

- a.insert(index,"context")  

```python
a = [1,2,3,4,5,6,7,8,9,10]
a.insert(1,12)#将数据添加到指定下标处
print(a)
```

- a.extend(b) 拓展  

```python
a = [1,2,3]
b = [4,5,6]
a.extend(b)
print(a)
print(b)
```

**改**

- a[index] = "context"  

```python
a = [1,2,3,4,5,6,7,8,9,10]
a[1] = 100
print(a)
```

- a[start:end] = [a,b,c]  

```python
a = [1,2,3,4,5,6,7,8,9,10]
a[2:5] = [101,102,103,104,105]#5-2个
print(a)
```

**删**

- a.remove("context")  

```python
a = [1,2,3,4,5,6,7,8,9,10]
a.remove(2)#只能通过值删除，不能通过下标删除，以单个元素进行删除，不能删除片（可以列表为元素）
print(a)
```

- b = a.pop(index)  

```python
b = a.pop(2)#能通过下标删除值，并将删除的值返回，没填写index，默认是最后一个值
print(a)
c = a.pop(2)
print(a)
print("b : %d ; c : %d" % (b,c))
```

- del a[index]，del a  

```python
del a[2]#删除下标对应元素
print(a)
del a#可删除整个列表
print(a)
```

- a.clear() 清空  

**排序**

- x.reverse() 倒置  

```python
a = ['to','be','or','not','to','be','to']
a.reverse()#倒置元素位置
print(a)
```

- x.sort() 排序  

```python
x = [3,1,5,4,5,2,6]
x.sort()
print(x)
```

- x.sort(reverse=True) 反向排序  

```python
a = ['to','be','or','not','to','be','to']
a.sort(reverse=True)#反向排序
print(a)
```

**身份判断**

- type(a) is list  

```python
a = [1,2,3,4,5,6,7,8,9,10]
print(type(a) is list)
```

### 2、元组tuple

> 与列表相比，两者的差别在于列表中的元素可改，元组中的元素不可改
>
> Python在显示只有1个元素的tuple时，也会加一个逗号`,`，以免你误解成数学计算意义上的括号。

### 3、字典dict

> key只能用不可变数据类型
>
> list()创建列表和dict()创建字典的理解：与int()类似，也可理解为一种强转，将()中的值强转为list和dict类型，当然，值得合理，dict得是键值对

**查**

```python
dict2 = {'name': 'zhuzishuo', 'age': 23, 'hobby': 'girl'}

print(dict2['name'])#通过键查找，返回对应的值

print(dict2.keys())#返回所有键的列表
print(dict2.values())#返回所有值的列表
print(dict2.items())#返回键值对的元组的列表

print(type(dict2.keys()))
print(list(dict2.keys()))

dict2[11]#访问不存在的键会导致KeyError
dict2.get('dfafasd')#用get来避免KeyError
```

**改**

- 通过键更改

```python
li = [34,52,3,6,24]
li[2] = 5
dict3 = {'name': 'zhuzishuo', 'age': 23, 'hobby': 'girl'}
dict3['hobby'] = 'study'
print(dict3)
```

- update 更新，没有的键值对添加进去，存在的键值对覆盖

```python
dict4 = {'name': 'zhuzishuo', 'age': 23, 'hobby': 'girl'}
dict5 = {'1':'fsds','dadfad':14314}
dict4.update(dict5)
print(dict4)
print(dict5)

dict6 = {'dadfad':'dfafshknksd'}
dict4.update(dict6)
print(dict4)
print(dict5)
print(dict6)
```

- .setdefault('key',values)

```python
dict1 = {'name':'zhuzishuo'}
dict1['age'] = 23
print(dict1)
#键存在，不改动，返回字典中相应的键对应的值
ret = dict1.setdefault('age',425)
print(ret)
#键不存在，在字典中增加新的键值对，并返回新添加的值
ret = dict1.setdefault('hobby','girl')
print(ret)
print(dict1)
```

**删**

- del

```python
dict7 = {'1':'fsds','dadfad':14314}
del dict7['1']
print(dict7)
del dict7
print(dict7)
```

- dict.pop('key')：删除字典中的键值对，并返回值

```python
dict7 = {'1':'fsds','dadfad':14314}
ret = dict7.pop('1')
print(ret)
```

- dict.popitem()：随机删除某组键值对，并以元组方式返回值

```python
dict7 = {'1':'fsds','dadfad':14314}
a = dict7.popitem()
print(a,dict7)
```

- dict.clear()：清空字典

```python
dict7 = {'1':'fsds','dadfad':14314}
dict7.clear()
print(dict7)
```

**排序**

```python
dic = {'4':'2425','9':'87464','2':'87675'}
# 通过字典的key排序
print(sorted(dic))
# 通过字典的值排序
print(sorted(dic.values()))
# 通过字典键值对的元组排序
print(sorted(dic.items()))
```

**遍历**

```python
dic2 = {'name': 'zhuzishuo', 'age': 23, 'hobby': 'girl'}
for i,v in dic2.items():
    print(i,v)
print('-------------------------')
for i in dic2:
    print(i,dic2[i])
```

> 推荐使用第二种方法
> 第一种方法中dic2.items()需要将dic2字典转换为items的列表
> 这其中的转换需要时间，而且所耗费的时间比遍历的时间还多

- in关键字：测试一个字典是否包含一个键

```python
print('one' in dict2)
```

- 其他操作以及涉及到的方法

```python
dict8 = dict.fromkeys(['host1','host2','host3'],'test')
print(dict8)#{'host1': 'test', 'host2': 'test', 'host3': 'test'}

dict8['host2'] = 'abc'
print(dict8)#{'host1': 'test', 'host2': 'abc', 'host3': 'test'}

dict8 = dict.fromkeys(['host1','host2','host3'],['test1','test2'])
print(dict8)#{'host1': ['test1', 'test2'], 'host2': ['test1', 'test2'], 'host3': ['test1', 'test2']}

dict8['host2'][1] = 'test3'
print(dict8)#{'host1': ['test1', 'test3'], 'host2': ['test1', 'test3'], 'host3': ['test1', 'test3']}
```

### 4、集合set

set是一个无序且不重复的元素集合，有如下特性：

1. 元素不重复
2. 元素为不可变对象

- 集合支持用in和not in操作符检查成员
- 由len()内建函数得到集合的基数(大小)
- 由于集合本身是无序的，不可以为集合创建索引或执行切片(slice)操作，也没有键(keys)可用来获取集合中元素的值， 用 for 循环迭代集合的元素。

> set和dict一样，只是没有value，相当于dict的key集合，由于dict的key是不重复的，且key是不可变，且set对象是一组无序排列的可哈希的值