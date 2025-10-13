# 定义

## 官方定义

Python 官方文档 glossary 章节对 generator 的定义为(https://docs.python.org/3.15/glossary.html#term-generator)：

> A function which returns a **generator iterator**...

generator 的定义里面包含了另外一个概念“generator iterator”，所以我们需要先弄清楚“generator iterator”的定义：

> An object created by a generator function...

**注解：**

不得不说，写出这个定义的人有点让人无语，两者相互嵌套了——打个比方，你问我什么是正数，什么是负数，然后我的回答是“正数是负数的相反数，负数是正数的相反数”——然后问题来了“-1 是正数还是负数？”，依据这个定义根本无法判断。

## 自定义

生成器是一种边迭代边计算的机制。

# 实现

生成器有两种实现方式：generator function(生成器函数)，generator expression(生成器表达式)。

## generator function

使用 yield 关键字返回结果的函数。

```

```

## generator expression

使用圆括号(round bracket)。

```

```

# 意义

- 节省内存。

# 实战

TBD

大部分场景都是直接生成数据，因为很多时候数据是没有规律的，什么时候能够应用生成器呢？

# 参考资料

[1] Python Document Glossary，generator：https://docs.python.org/3.15/glossary.html#term-generator

[2] PEP 255 – Simple Generators：https://peps.python.org/pep-0255/