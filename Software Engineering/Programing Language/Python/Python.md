# Compilation
**CPython** (standard Python interpreter): compiles Python code into bytecode
	The bytecodes are saved in `.pyc` files under folder `__pycache__`, which allow it to be reused in subsequent runs, skipping the compilation step unless the source code has changed.
**Python Virtual Machine (PVM)**: interpret the bytecode from CPython
**pyinstaller**: compiles code into an executable
## Shebang
Specify the interpreter location
```python
#!/bin/bash
#!/usr/bin/env python3
```
# Devtools
static analyzer: `pyflakes`
debugger: `ipdb`
linter: flake8
formatter: black
[[Project Environments]]
[[Python Profiling]]
[[Logging]]
[[Python Test]]
# Basic Grammar
## Variables
### Object Meta
```python
from collections.abc import Container, Iterable

isinstance(obj, (int, float)) # or
isinstance(obj, Container)
isinstance(obj, Iterable)
callable(obj) # check callable
hasattr(obj, "public_attribute_name")

a is b # compare memory location
a == b # compare values stored in potentially different location
```
### Number
```python
# complex numbers
1+1j

# inf
float("inf")

# compare floats
## approximate
math.isclose(f1, f2, rel_tol=1e-5)
## accurate
from decimal import Decimal, getcontext
getcontet().prec = 6
Decimal('3.14') * Decimal('0.3') # string is recommended
```
### String formatting
```python
'Hello, %s and %s', name1, name2
'Hello, %s and %s' % (name1, name2)
'Hello, {} and {}'.format(name1, name2)
f'Hello, {name1} and {name2}'
```
## Function
### Namespace
**LEGB Rules**: python search for variable in this order: Local（局部）→ Enclosing（封闭）→ Global（全局）→ Built-in（内置）
```python
global var # 从最外层找模块级变量 # globals
nonlocal var # 从最近的函数作用域找非全局变量 # 上层的locals()

# 命名空间以字典的形式存在python中
locals() # 返回当前作用域的命名空间
globals() # 任意位置获得全局命名空间
```
### Arguments
When passing keyword argument, you have to specify the parameter name. For positional argument, this is optional.
```Python
def myFunc(arg1, *argv, **kwargs, argn=0):
  print("First argument is a positional argument: ", arg1)
  for arg in argv:
    print("Any number of positional argument through *argv: ", arg)
	for key, value in kwargs.items():
    print("Any number of keyworded argument %s == %s" % (key, value))
	print("Last argument is a keyword argument: ", argn)
```
### Lambda
```Python
lambda a, b: a + b
```
## Class
```Python
class ClassName(ParentClass):
    # Class attributes (variables)
    attribute1 = value1
    
    # Constructor method (optional)
    def __init__(self, publicParam, protectedParam, privateParam):
        self.publicParam = publicParam
        self._protectedParam = protectedParam
        self.__privateParam = privateParam # in fact can still be accessed by ClassName._ClassName__privateParam

	def __new__(cls):
		print("create and return a new instance")
		
	def __del__(self):
		print("This is called before garbage collection. No var is pointing at this object now.")

	def __str__(self):
		return "string representation of the instance"

	def __repr__(self):
		return "for repr() function and interactive interpreter"
		
	@classmethod
	# call by instance is not recommended
	def classMethod(cls)
	    print("class method")

	@staticmethod
	# call by instance is not recommended
	def staticMethod(cls)
	    print("static method")
	    
    def publicMethod(self):
        ...
        
    def __privateMethod(self):
    # in fact can still be accessed by ClassName._ClassName__privateMethod
        ...
	
	@property
	def protectedParam(self):
		return self._protectedParam # c.protectedParam
	
	@protectedParam.setter
	def protectedParam(self, newval): # c.protectedParam = newval
		self._protectedParam = newval

	@protectedParam.deleter
	def protectedParam(self): # del c.protectedParam
		del self._protectedParam
```
### Underlying Mechanism
[[Python Reflection]]
#### Descriptor
**Descriptor**: an instance stored in `ClassName.__dict__` and implemented at least one of `__get__`, `__set__`, or `__delete__`
**Data descriptor**: a descriptor that defines `__set__` or `__delete__`
**Non-data descriptor**: a descriptor that only defines `__get__`

查找属性的流程：
	1. 查类及其父类的Data Descriptor
	2. 查找实例字典`__dict__`
	3. 查类及其父类的类字典`__dict__`（含Non-Data Descriptor ）
	4. 查找调用`__getattr__`
	5. 抛出`AttributeError`

```python
__dict__
# class: 存放类方法、槽描述器等固定结构，member descriptor
# instance: 存放该实例的所有动态属性

class Entity:
	__slots__ = ('__eid', '_race', 'age')
# 省内存: 节省了多个实例的__dict__；访问更快：查找属性的第2步可以直接index寻址，不必走哈希
# 在类定义阶段为你列出的每个名字自动创建一个槽（slot）描述器（`member descriptor`）
# 这些描述器都实现了实现了 `__get__` 和 `__set__`
```
### Iterable
## Concurrency & Parallelism
**GIL Global Interpreter Lock** makes sure only one thread is running in a PVM at the same time
[[python池化问题]]
# Modules
> package: a folder containing `__init__.py`

[[Python OS]]