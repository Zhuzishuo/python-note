# 异常处理

### 1、异常

```python
"""
	class MyException(Exception):   # 自定义异常类
	def __init__(self, message):
		self.message = message

	def message(self):
		print(self.message)


	try:    # 将可能会报错的语句放入try 中
		if a == '1':
			raise Exception('我是a == 1')  # raise 异常类实例   抛出异常
		elif a == '2':
			raise MyException('我是a == 2')    # 抛出自定义异常
	except MyException as ret: # as ret 可以用ret变量引用抛出的异常对象，对其进行操作
		print(ret.message)
	except Exception:   # 可以多个except 分支捕抓不同的异常
		print('未知错误')
		raise   # 将异常直接抛出
	else:   # 只有在try 语句块中没有出现报错才会执行else 中的语句块
		print('没有报错')
	finally:    # 不管try 语句块中是否出现报错，均会执行该语句块
		print('没错，不管有没有报错我都会出现！')
"""


# 异常
class MyException(Exception):
	def __init__(self, message):
		self.message = message

	def message(self):
		print(self.message)


def test_exception(a):
	try:
		if a == '1':
			raise Exception('我是a == 1')
		elif a == '2':
			raise MyException('我是a == 2')
	except MyException as ret:
		print(ret.message)
	except Exception:
		print('未知错误')
		raise
	else:
		print('没有报错')
	finally:
		print('没错，不管有没有报错我都会出现！')


a = input('请输入你的选择：')
test_exception(a)
```

### 2、断言

断言比较简单，语法：assert 表达式 \[, 参数]

```python
def test_assert(a):
	assert a > 1, 'a必须大于1'


a = int(input('请输入数字：'))
test_assert(a)
```

### 3、上下文

可以从右边链接了解上下文：[_Python_3之 _contextlib_ - 天真莫离 - 博客园](https://www.cnblogs.com/wang-yc/p/6640873.html)

### 4、常见异常类

```
AttributeError 试图访问一个对象没有的属性，比如foo.x，但是foo没有属性x

IOError 输入/输出异常；基本上是无法打开文件

ImportError 无法引入模块或包；基本上是路径问题或名称错误

IndentationError 语法错误（的子类）；代码没有正确对齐

IndexError 下标索引超出序列边界，比如当x只有三个元素，却试图访问x[5]

KeyError 试图访问字典里不存在的键

KeyboardInterrupt Ctrl+C被按下

NameError 尝试访问一个没有申明的变量

SyntaxError Python代码非法，代码不能编译(个人认为这是语法错误，写错了）

TypeError 传入对象类型与要求的不符合

UnboundLocalError 试图访问一个还未被设置的局部变量，基本上是由于另有一个同名的全局变量，导致你以为正在访问它

ValueError 传入一个调用者不期望的值，即使值的类型是正确的

如果在编码时不知道会抛哪个异常，可以使用万能异常:

Exception
```
