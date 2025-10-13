# 定义(what)

## 官方定义

Python 官方文档 glossary 章节对 iterable(可迭代对象)的定义为( https://docs.python.org/3/glossary.html#term-iterable)：

> An object capable of returning its members one at a time...

## 自定义

如果一个对象定义了  `__iter__() ` 方法或定义了 `__getitem__()` 方法，那么这样的对象称为可迭代对象(iterable)。

**说明：**

1.“对象”：对应到代码就是使用 class 定义的类。

2.“迭代”：中文语境下有时候称为“遍历”，对应到代码就是使用 for/while 循环。

3.Python 官方文档写的是“或实现了 sequence 语义的 `__getitem__()` 方法的自定义类的对象”，这里省略“实现了 sequence 语义(即支持 self[key])， 如 list 支持 sequence 语义：lst = [1,0, 2, 4], lst[0] = 1”，因为严格来说只要一个对象实现了  `__getitem__()` 方法，那么这个对象就是可迭代对象。官方文档之所以说   `__getitem__()` 方法要实现 sequence 语义，个人觉得这是从实际应用的层面来说的——如果在实际应用中定义了 `__getitem__()` 方法， 那么根据实际需要，必然要实现 sequence 语义。

# 动机(why)

之所以创建“可迭代对象”这一概念，其实就是为了让对象变得可迭代。

在实际应用中需要自己实现可迭代对象的场景很少，大多数是如何操作可迭代对象(如：对 list, str, tuple, dict 进行迭代)。

# 实现(how)

## (1) `__iter__() `

既然是 iterable, 所以方法名用 iter 命名。

```
class Demo:
    def __init__(self, initial=None):
        self._data = initial if initial else []

    def __iter__(self):
        pass

```

如上所示，因为类 Demo里面定义了  `__iter__() ` 方法，所以 Demo 是一个可迭代对象，虽然这里的  `__iter__() `  方法什么都不做。

##  (2)`__getitem__()`

用于需要需要通过 self[key] 获取元素的对象。

```
class Demo:
    def __init__(self, initial=None):
        self._data = list(initial) if initial else []

    def __getitem__(self, index):
        return self._data[index]
```

如上所示，因为类 Demo里面定义了  `__getitem__()` 方法，所以 Demo 是一个可迭代对象，虽然这里的 `__getitem__()` 方法什么都不做。

# 实战(where)

TBD，暂未在实际应用中看到自定义的可迭代对象，待后续遇到补充。

# 参考资料

[1] Python Document Glossary，iterable：: https://docs.python.org/3/glossary.html#term-iterable
