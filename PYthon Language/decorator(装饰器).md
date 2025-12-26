# 定义(what)

## 官方定义

Python 官方文档 glossary 关于 decorator 的定义如下(https://docs.python.org/3/glossary.html#term-decorator)：

A function returning another function, usually applied as a function transformation using the `@wrapper` syntax. Common examples for decorators are [`classmethod()`](https://docs.python.org/3/library/functions.html#classmethod) and [`staticmethod()`](https://docs.python.org/3/library/functions.html#staticmethod).

The decorator syntax is merely syntactic sugar, the following two function definitions are semantically equivalent:

```
def f(arg):
    ...
f = staticmethod(f)

@staticmethod
def f(arg):
    ...
```

The same concept exists for classes, but is less commonly used there. See the documentation for [function definitions](https://docs.python.org/3/reference/compound_stmts.html#function) and [class definitions](https://docs.python.org/3/reference/compound_stmts.html#class) for more about decorators.

## 自定义

个人觉得官方文档里面的定义——A function returning another function(装饰器是一个返回结果为函数的函数)只是陈述了装饰器的形式，没有说明装饰器的本质，故下面给出个人认为更好的定义。

装饰器是一种设计模式(design pattern)，这种模式允许我们在不修改现有函数的情况下，为现有函数添加功能。

# 实现(How)

## 闭包(closure)

从语法层面上来说，装饰器是通过闭包来实现的，是闭包的应用之一。所以学习装饰器之前可以先了解一下闭包。参考：[closure(闭包).md](./closure(闭包).md)

## 语法

示例：某个功能执行时间很长，需要计算函数执行时间看问题出现在哪里。

1.无任何封装写法

既然要计算函数执行时间，那么就记录函数的开始时间“start_time”，结束时间“end_time”，耗时=结束时间-开始时间。

```
import time

def foo():
    time.sleep(2)

def bar():
    time.sleep(3)

start_time = time.perf_counter()
foo()
end_time = time.perf_counter()
elapsed_time = end_time - start_time
print(f'执行 foo() 函数耗时：{elapsed_time} 秒')


start_time = time.perf_counter()
bar()
end_time = time.perf_counter()
elapsed_time = end_time - start_time
print(f'执行 bar() 函数耗时：{elapsed_time} 秒')
```

 2.封装写法

如果要处理的函数不多，那么上面的写法也没有什么问题。假设现在要处理的函数很多，这些写就会变得到处都是重复的代码，那就封装一下。

```
import time


def foo():
    time.sleep(2)


def bar():
    time.sleep(3)


def measure(func):
    start_time = time.perf_counter()
    func()
    end_time = time.perf_counter()
    elapsed_time = end_time - start_time
    print(f'执行 {func.__name__} 函数耗时：{elapsed_time} 秒')


if __name__ == '__main__':
    measure(foo)
    measure(bar)

```

3.装饰器写法

```
import time


def foo():
    time.sleep(2)


def bar():
    time.sleep(3)


def measure(func):
    def wrapper():
        start_time = time.perf_counter()
        func()
        end_time = time.perf_counter()
        elapsed_time = end_time - start_time
        print(f'执行 {func.__name__} 函数耗时：{elapsed_time} 秒')
    return wrapper


if __name__ == '__main__':
    f = measure(foo)
    f()

    b =  measure(bar)
    b()

```

4.装饰器语法糖

```
import time


def measure(func):
    def wrapper():
        start_time = time.perf_counter()
        func()
        end_time = time.perf_counter()
        elapsed_time = end_time - start_time
        print(f'执行 {func.__name__} 函数耗时：{elapsed_time} 秒')

    return wrapper


@measure
def foo():
    time.sleep(2)


@measure
def bar():
    time.sleep(3)


if __name__ == '__main__':
    foo()
    bar()

```

## 带参数装饰器/装饰器工厂(decorator factory)

```

```

## 类装饰器



## @functools.wraps()

这是一个便捷函数，用于在定义包装器函数时唤起 [`update_wrapper()`](https://docs.python.org/zh-cn/3.14/library/functools.html#functools.update_wrapper) 作为函数装饰器。在实际

### 不使用效果

```
from functools import wraps
def my_decorator(f):
    @wraps(f)
    def wrapper(*args, **kwds):
        print('Calling decorated function')
        return f(*args, **kwds)
    return wrapper

@my_decorator
def example():
    """Docstring"""
    print('Called example function')

example()


example.__name__

example.__doc__
```

### 使用效果

```
from functools import wraps
def my_decorator(f):
    @wraps(f)
    def wrapper(*args, **kwds):
        print('Calling decorated function')
        return f(*args, **kwds)
    return wrapper

@my_decorator
def example():
    """Docstring"""
    print('Called example function')

example()


example.__name__

example.__doc__
```



# when

Python 2.4: PEP 318

Python 3.0: PEP 3129

Python 3.9: PEP 614

# who

todo;

# 实战(where)

# 参考资料

[1] PEP 318，Decorators for Functions and Methods：https://peps.python.org/pep-0318/

[2] PEP 3129，Class Decorators：https://peps.python.org/pep-3129/

[3] PEP 614，Relaxing Grammar Restrictions On Decorators：https://peps.python.org/pep-0614/