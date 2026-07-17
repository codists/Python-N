# try 语句

## except 子句

```

```

## except* 子句

```

```

## else 子句

```
def divide(x, y):
    try:
        result = x / y
    except ZeroDivisionError:
        print("division by zero!")
    else:
        print("result is", result)
```

如果所有的 except 子句都没有触发, 那么就走 else。else 必须放在所有 except 子句之后。

## finally 子句

```

```

# 关于 else 的一些思考

官方文档说：使用 `else` 子句比向 [`try`](https://docs.python.org/zh-cn/3.14/reference/compound_stmts.html#try) 子句添加额外的代码要好，可以避免意外捕获非 `try` ... `except` 语句保护的代码触发的异常。

**本人觉得官方文档写的这个理由有点勉强，因为所有包含 try...else...的代码都可以改造成没有 else 的代码。**

## 示例

1.《Asynchronous Programming in Python》(2025, packt, 第 55 页)

```
import json
import queue
import asyncio
from sseclient import SSEClient

messages = SSEClient('https://stream.wikimedia.org/v2/stream/recentchange', headers={
    'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/143.0.0.0 Safari/537.36'})
batch_size = 3
max_vals = 10
vals = queue.Queue(maxsize=max_vals)


async def sse_client_get_values():
    batch = []
    for event in messages:
        if event.event == 'message':
            try:
                change = json.loads(event.data)
            except ValueError:
                pass
            else:
                if change['meta']['domain'] == 'canary' or change['bot'] == True:
                    continue
                if len(batch) < batch_size:
                    batch.append(change)
                else:
                    return batch


async def fetcher():
    while True:
        io_vals = await sse_client_get_values()
        try:
            for item in io_vals:
                vals.put(item, block=False)
        except queue.Full:
            print("Queue is full")
            return True
        await asyncio.sleep(1)


async def monitor():
    while True:
        curr_len = vals.qsize()
        if curr_len >= max_vals:
            return True
        else:
            print("Queue size:", curr_len)
        await asyncio.sleep(1)


async def serializer():
    while True:
        try:
            item = vals.get_nowait()
            print("item read, Wiki edited by:", item["user"])
            f = open(f'./tmp/${item["id"]}.json', "a")
            json.dump(item, f)
            f.close()
        except queue.Empty:
            print("Queue is empty")
            return True
        await asyncio.sleep(1)


async def main():
    t1 = asyncio.create_task(fetcher())
    t2 = asyncio.create_task(monitor())
    t3 = asyncio.create_task(serializer())
    await asyncio.gather(t1, t2, t3)


if __name__ == '__main__':
    asyncio.run(main())

```

上述代码可改造成：

```
import asyncio
import json
import queue

from sseclient import SSEClient

messages = SSEClient('https://stream.wikimedia.org/v2/stream/recentchange', headers={
    'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/143.0.0.0 Safari/537.36'})
batch_size = 3
max_vals = 10
vals = queue.Queue(maxsize=max_vals)


async def sse_client_get_values():
    batch = []
    for event in messages:
        if event.event == 'message':
            try:
                change = json.loads(event.data)
                if change['meta']['domain'] == 'canary' or change['bot'] == True:
                    continue
                if len(batch) < batch_size:
                    batch.append(change)
                else:
                    return batch
            except ValueError:
                pass


async def fetcher():
    while True:
        io_vals = await sse_client_get_values()
        try:
            for item in io_vals:
                vals.put(item, block=False)
        except queue.Full:
            print("Queue is full")
            return True
        await asyncio.sleep(1)


async def monitor():
    while True:
        curr_len = vals.qsize()
        if curr_len >= max_vals:
            return True
        else:
            print("Queue size:", curr_len)
        await asyncio.sleep(1)


async def serializer():
    while True:
        try:
            item = vals.get_nowait()
            print("item read, Wiki edited by:", item["user"])
            # open() 必须保证目录存在， 不知道作者为什么不用 with open
            f = open(f'./tmp/{item["id"]}.json', "a")
            json.dump(item, f)
            f.close()
        except queue.Empty:
            print("Queue is empty")
            return True
        await asyncio.sleep(1)


async def main():
    t1 = asyncio.create_task(fetcher())
    t2 = asyncio.create_task(monitor())
    t3 = asyncio.create_task(serializer())
    await asyncio.gather(t1, t2, t3)


if __name__ == '__main__':
    asyncio.run(main())

```

综上所述，本人觉得，其实没有必要使用 try...else...，使用了反而不好理解。