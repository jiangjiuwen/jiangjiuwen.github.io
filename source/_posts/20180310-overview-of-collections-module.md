---
title: collections模块常用数据结构
categories: 编程之美
date: 2018-03-10 13:57:08
tags:
- Python
- Collections
- 数据结构
---
collections模块是python自带的模块，为开发者提供了很多方便且高效的数据结构，帮助开发者写出简单高效且可读性高的python代码。

# collections模块常用数据结构

collections模块中最常被使用的数据结构主要包括deque、defaultdict、namedtuple、Counter、OrderedDict、ChainMap这6个，如下表所示。

| 名称 | 定义 |
|:------:|------|
|deque|双端队列|
|defaultdict|具有默认值的字典类型|
|namedtuple|具有类的特性的tuple类型|
|Counter|具有计数功能的字典类型|
|OrderedDict|按添加顺序排列的字典类型|
|ChainMap|将多个字典链接为一个字典|

# tuple和namedtuple

tuple特点：
- 不可变（不完全不可变，比如某个item为list或其他可变类型时，其值就可变，不推荐这种做法）
- 可遍历
- 可拆包
- 可哈希（使其可以作为字典的key）
- 线程安全

tuple的item不像字典一样有key对应，为了使tuple可以像类一样通过指定字段名称来调用tuple的值，于是就有了namedtuple。

```python
User = namedtuple("User", ('name', 'age', 'height'))
fields = ('jim', 22, 176)
user = User(*fields)
name, *other = user # 拆包
print(name, other)
```

namedtuple特点：
- 节省空间。不会生成不必要的类字段
- 代码简洁、美观。少量的代码即能实现类和tuple的特性

# defaultdict

通常当我们操作的索引不在字典当前字典中时，编译器会报错，此时就要通过if else之类的语句进行判断该索引是否在字典中，然后再进行处理，通常这样做效率不高而且代码不优雅。而defaultdict就可以定义一个默认值，当操作不存在的索引时，就会给该索引赋予默认值，然后再进行后续操作。

```python
default_dict = defaultdict(int)
default_dict['age'] += 22
```

# deque
deque具有list相似的特性，但也有一些区别。
- 不同的是list默认函数只能对list对尾进行操作，而deque对对头和队尾都可以操作
- deque是线程安全的，而list不是

另外，python3中的queue就是用deque来实现。

```python
name_deque = deque(['jim', 'jack', 'tony'])
name_deque.appendleft('lily')
name_deque.reverse()
```

# Counter
Counter可以统计任意可迭代的类型，比如list、tuple、dict、string等。Counter中比较重要的函数有update和most_comon。

```python
counter1 = Counter('dsagwdsdagwecsada')
print(counter1)
counter2 = Counter({'k1':'v1', 'k2':'v2', 'k3':'v1'})
print(counter2)
counter3 = Counter('ggggggg')
counter1.update(counter3)
print(counter1)
```

# OrderedDict
顾名思义，OrderedDict就是用来表征已经排好序的字典，由于字典具有无序性，即字典中的key的位置是不固定的，而OrderedDict中的键值对则是按照添加时间按序排列的,即保证了时序性，又有dict的方便性。

值得注意的是，在python3中，dict默认是有序的，在python2中不是，但是OrderedDict中包含了一些dict没有的方法，比如clear，所以在python3中还是有必要使用OrderedDict的。

# ChainMap
当我们需要遍历多个字典的时候，需要写大量重复代码，这个时候ChainMap就能将多个字典链接起来，使其看起来像是一个字典。

```python
users1 = {'a':'jack', 'b':'lily'}
users2 = {'b':'tony', 'd':'jason'}
users = ChainMap(users1, users2)
print(users)
print(users.popitem(), users)
print(users.maps)
```

值得注意的是，如果不同的字典中具有相同的key，则只操作第一个key，后面出现相同的key时会将其忽略

# 总结

python内置的基本类型已经具有非常丰富的功能，而以上数据类型作为基本类型的升级版不仅增强了性能，保证编写的代码更健壮，而且使代码编写更方便，写出来的代码更优雅。