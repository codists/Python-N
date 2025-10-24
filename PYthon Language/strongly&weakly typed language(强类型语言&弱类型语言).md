# 定义(what)

## 官方定义

Python 官方文档 glossary 章节对 iterable(可迭代对象)的定义为( https://docs.python.org/3/glossary.html#term-iterable)：

> An object capable of returning its members one at a time...

## 自定义

todo.

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
