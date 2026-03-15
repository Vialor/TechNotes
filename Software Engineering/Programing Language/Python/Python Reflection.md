> **反射 Reflection**：程序在运行时（runtime）检查、访问、修改自身结构的能力。
	代码可以“用字符串操作代码”。

Python 的反射本质是对命名空间字典的动态访问。模块、类、对象都维护一个`__dict__`，反射底层就是和`__dict__`交互。
## 对象反射
```python
getattr(obj, "attr_name") # 动态获取属性
setattr(obj, "attr_name", 100) # 动态设置属性
hasattr(obj, "attr_name")
delattr(obj, "attr_name")
```
## 模块反射：importlib
```python
import importlib
module = importlib.import_module("package.module")
```
动态加载模块，这是插件系统的核心。
## 类反射
```python
type(obj)
isinstance(obj, MyClass)
issubclass(A, B)
```
## inspect 模块（高级反射）
Python 提供了一个专门用于结构分析的库，可以获取：函数参数签名，类成员，方法列表，源代码
```python
import inspect

inspect.signature(func)
```