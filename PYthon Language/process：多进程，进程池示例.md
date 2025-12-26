# Process

## 示例一：

```
# @Filename: child_processes.py
# @Author: codists
# @Created: 2025-11-10 23:41:57

import os
import time
from multiprocessing import Process

def run_child() -> None:
    print('Child: I am the child process')
    print(f'Child: Child\'s PID: {os.getpid()}')
    print(f'Child: Parent\'s PID: {os.getppid()}')

def start_parent(num_children: int) -> None:
    print("Parent: I am the parent process")
    print(f"Parent: Parent's PID: {os.getpid()}")

    for i in range(num_children):
        print(f"Starting Process {i}")
        Process(target=run_child).start()
        time.sleep(5)
        print()

if __name__ == '__main__':
    ''' 输出：
    Parent: I am the parent process
    Parent: Parent's PID: 34652
    Starting Process 0
    Child: I am the child process
    Child: Child's PID: 12108
    Child: Parent's PID: 34652
    
    Starting Process 1
    Child: I am the child process
    Child: Child's PID: 24456
    Child: Parent's PID: 34652
    
    Starting Process 2
    Child: I am the child process
    Child: Child's PID: 25664
    Child: Parent's PID: 34652
    '''
    num_children = 3
    start_parent(num_children)


```

# Pool

## **map**(*func*, *iterable*[, *chunksize*])

适用于只有一个参数的 func。

## **starmap**(*func*, *iterable*[, *chunksize*])

适用于有多个参数的 func。

### 示例一：

```
```



## imap()