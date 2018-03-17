---
title: Python的几种导入操作
categories: 编程之美
date: 2018-03-16 22:10:41
tags:
- Python
- import
---
# 模块(Module)和包(Package)

Python中一个源文件就是一个模块，即一个py文件就是一个模块，模块中可能包含字段、函数、类等定义，也可能什么都没有，即一个空的模块。

Python包就是一个管理着一个或者一些模块的文件夹，其中有一个特殊的模块，即`__init__.py`，当加载一个包的时候，包中的`__init__.py`模块将被首先执行。包可以嵌套包含，即一个包下面还包含这一些子包。

以下就是一个常见的包结构。

```
package/
    __init__.py
    moduleA.py
    moduleB.py
    subPackage/
        __init__.py
        subModuleA.py
        subModuleB.py
```

# 几种import方式

以上面的包结构为例，假设包中没有`__init__.py`模块，`subPackage`包中的`subModuleA.py`模块的代码如下。
```python
# subModuleA.py

def subModuleFun():
    print('Hello from subModuleFun')
    
def subModuleFunTwo():
    print('Hello from subModuleFunTwo')
```

那么`package`包中的`moduleA.py`可以通过以下几种方式调用。
```python
# moduleA.py

# 1. 按照绝对路径导入subModuleA
import subPackage.subModuleA
subPackage.subModuleA.subModuleFun() # Hello from subModuleFun
subPackage.subModuleA.subModuleFunTwo() # Hello from subModuleFunTwo

# 2. 为导入指定别名
import subPackage.subModuleA as sma
sma.subModuleFun() # Hello from subModuleFun
sma.subModuleFunTwo() # Hello from subModuleFunTwo

# 3. 按需导入，即只导入需要调用的内容
from subPackage.subModuleA import subModuleFun
subModuleFun() # Hello from subModuleFun
subModuleFunTwo() # Error
subPackage.subModuleA.subModuleFunTwo() # Error

# 4. 导入模块中所有内容
from subPackage.subModuleA import *
subModuleFun() # Hello from subModuleFun
subModuleFunTwo() # Hello from subModuleFunTwo
```

# `__init__.py`模块的作用
仍然以上面的包结构为例，包中有`__init__.py`模块，且`__init__.py`模块的代码如下。
```python
# package/__init__.py
from moduleA import *
from moduleB import *

# package/subPackage/__init__.py
from subModuleA import *
from subModuleB import *

# moduleA.py调用subPackage.subModuleA中的subModuleFun方法
from subPackage import subModuleFun
subModuleFun() # Hello from subModuleFun
```

由此可见，此时我如果想从某个包里面调用某个模块中的某个函数时就可以直接从包里面导入，而不需要指定该函数所在模块的路径。从而带来两个好处，一个是简化了import路径，二是便于开发者控制各个模块或方法的访问级别。


# import的作用域

如果在模块的最顶端进行导入，则其作用域为整个模块，表示整个模块的内容都能访问导入的内容。

如果仅在某个代码块中进行导入，则其作用域为该代码块。如下所示。

```python
import globalModule

def funcA():
    import subModule
    globalModule.func()
    subModule.func()

def funcB():
    globalModule.func()
    subModule.func() # Error

globalModule.func()
```

# 循环导入

循环导入即A模块导入B模块，同时B模块中又导入了A模块，此时运行任意一个模块都将收到`AttributeError`的报错。

如下所示。

```python
# A.py
import B

def funcA():
    print('funcA')

funcA()
B.funcB()


# B.py
import A

def funcB():
    print('funcB')

funcB()
A.funcA()
```

# 导入和标准模块重名的模块

当我们自定义的模块名称和标准库中的模块名称重复的时候，导入就有可能会发生混乱，此时在调用标准库中的函数时，编译器可能会报错该方法不存在，不过在高版本的python中好像会自动到标准库中寻找该方法。不过，为了保证代码的正确性和可读性，不建议自定义的模块和标准库模块重名。

# 相对路径导入

如果子包中的模块想要访问上级包中的内容，则可以使用相对路径导入。

**注意，此时正在导入的模块的`__name__`不能是`__main__`，即此时执行的主函数不在当前正在导入的模块中。**

```python
# subModuleA.py
from ..moduleA import moduleFun # 一个.表示一级目录
moduleFun()
```
