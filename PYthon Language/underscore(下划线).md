# single_leading_underscore

## 作用

 weak “internal use” indicator. E.g. `from M import *` does not import objects whose names start with an underscore.

## 示例

# single_trailing_underscore_

## 作用

used by convention to avoid conflicts with Python keyword, e.g. :

```
tkinter.Toplevel(master, class_='ClassName')
```

## 示例

# __double_leading_underscore

## 作用

when naming a class attribute, invokes name mangling (inside class FooBar, `__boo` becomes `_FooBar__boo`; see below).

示例

# __double_leading_and_trailing_underscore__

## 作用

“magic” objects or attributes that live in user-controlled namespaces. E.g. `__init__`, `__import__` or `__file__`. Never invent such names; only use them as documented.

## 示例

# 参考资料

[1] PEP 8 – Style Guide for Python Code: https://peps.python.org/pep-0008