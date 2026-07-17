**如果想要彻底的理解“协程”，直接看《Grokking Concurrency》这本书——看完这本书就不用看下面的内容了！！！**

**如果想要彻底的理解“协程”，直接看《Grokking Concurrency》这本书——看完这本书就不用看下面的内容了！！！**

**如果想要彻底的理解“协程”，直接看《Grokking Concurrency》这本书——看完这本书就不用看下面的内容了！！！**

# 定义(what)

## 官方定义

Python 官方文档 glossary 章节对 coroutine(协程) 的定义为(https://docs.python.org/3/glossary.html#term-coroutine)：

> Coroutines are a more generalized form of subroutines. Subroutines are entered at one point and exited at another point. Coroutines can be entered, exited, and resumed at many different points. They can be implemented with the async def statement. See also PEP 492.

**coroutine function(协程函数)**

> A function which returns a coroutine object. A coroutine function may be defined with the async def statement, and may contain await, async for, and async with keywords. These were introduced by PEP 492.

**注解：**

如果你是初次接触协程的人，看到协程、协程函数的定义，那么你大概会感到有点懵，因为宽泛的来说 subroutine 和 function 指的就是同一个东西啊——可复用的代码结构(如：def 定义的函数)，只是不同的语言叫法不一样。

## 自定义





# 动机(why)



# 实现(how)

async def ... await

通过 await 来切换任何任务执行。

# 实战(where)



# when



# who

# 参考资料

[1] Python Document Glossary，coroutine：https://docs.python.org/3/glossary.html#term-coroutine

[2] PEP 492 – Coroutines with async and await syntax：https://peps.python.org/pep-0492/

[3]豆瓣，Kirill Bobrov：https://book.douban.com/subject/36296797/