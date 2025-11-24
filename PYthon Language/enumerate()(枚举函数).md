# 定义(what)

## 官方定义

根据 Python Library reference(https://docs.python.org/3/library/functions.html#enumerate):

> - **enumerate**(*iterable*, *start=0*)
>
>   Return an enumerate object. *iterable* must be a sequence, an iterator, or some other object which supports iteration. The `__next__()` method of the iterator returned by `enumerate()` returns a tuple containing a count (from *start* which defaults to 0) and the values obtained from iterating over *iterable*.
>
>   返回一个枚举对象。*iterable* 必须是一个序列，iterator，或其他支持迭代的对象。 `enumerate()` 返回的迭代器的 `__next__()`方法返回一个元组(**笔者注：(index, item)**)，里面包含一个计数值（从 *start* 开始，默认为 0）和通过迭代 *iterable* 获得的值。
>
> - 使用示例
>
> ```
> >>>seasons = ['Spring', 'Summer', 'Fall', 'Winter']
> >>>list(enumerate(seasons))
> [(0, 'Spring'), (1, 'Summer'), (2, 'Fall'), (3, 'Winter')]
> >>>list(enumerate(seasons, start=1))
> [(1, 'Spring'), (2, 'Summer'), (3, 'Fall'), (4, 'Winter')]
> ```

**说明：**

1.上面的 enumerate object 是指 enumerate() 返回的结果是一个 enumerate 类型。

```
seasons = ['Spring', 'Summer', 'Fall', 'Winter']
data = enumerate(seasons)
print(type(data))  # <class 'enumerate'>
```

## 自定义

无(官方定义已经很好)。

# 动机(why)

创建 enumerate() 的动机是“遍历时需要计数，或者遍历时需要获取索引”。

# 实战(where)

Python Library reference 的示例代码很简洁，但是却没有体现出使用 enumerate() 的目的——获取 index 后却没有使用。这里列举一些例子。

1.将整数序列编码为二进制矩阵

```python
# 摘自《Python 深度学习(1st)》
import numpy as np
from keras.datasets import imdb


(train_data, train_labels), _ = imdb.load_data(num_words=10000)

def vectorize_sequences(sequences, dimension=10000):
    results = np.zeros((len(sequences), dimension))
    # 将第 i 行，sequence 里面包含的列的值变为 1.0
    for i, sequence in enumerate(sequences):
        results[i, sequence] = 1.0
    return results

train_data = vectorize_sequences(train_data)
```

如上所示，enumerate() 的作用是为了获取索引。

# when

Python 2.3 引入。

# 要求

虽然 enumerate() 是一个内置函数，但在实际编程中应用的场景较少，了解即可(其实也没多少内容)。

# 参考资料

1.PEP 279 – The enumerate() built-in function: https://peps.python.org/pep-0279/

