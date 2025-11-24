# program(程序)

## 定义

Program is a sequence of instructions(程序是一系列指令)。

# process(进程)

## 定义

Process is a running program(进程就是一个“正在运行的程序”)。

## 示例

```
# @Filename: multiprocess_demo.py
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

**注：**

1.“正在运行的程序”含义是：进程包含“程序+资源(resource)”。

## feature

1.进程之间的数据不共享。



# thread(线程)

## 定义

A thread is a unit of execution that runs a sequence of instructions within a process(线程是在进程中执行指令的一个执行单元)。

```
Process
│
├─ Thread 1 
│   └─ Instruction 1 → Instruction 2 → Instruction 3
│
├─ Thread 2
│   └─ Instruction 1 → Instruction 2
│
└─ Resources (process ID, process state, address space, files, data)
```

如上所示：一个进程中可以包含多个线程，每个线程的作用是执行一系列指令。

**我们运行的每个程序都会促使操作系统创建一个进程，而每个进程至少包含一个线程；不存在不包含线程的进程。每个线程还维护着自身独立的执行上下文，以确保其指令能够被安全且独立地执行。**

## 示例

```
# @Filename: multithreading_demo.py
# @Author: codists
# @Created: 2025-11-11 14:30:10

import time
import threading
from threading import Thread
import os


def cpu_waster(i: int) -> None:
    name = threading.current_thread().name
    print(f'{name} doing {i} work.')
    time.sleep(3)


def display_threads() -> None:
    print('-' * 10)
    print(f'Current process PID: {os.getpid()}')
    print(f'Thread Count: {threading.active_count()}')
    print('Active threads:')
    for thread in threading.enumerate():
        print(thread)


def main(num_threads: int) -> None:
    display_threads()
    print(f'Starting {num_threads} CPU wasters...')
    for i in range(num_threads):
        thread = Thread(target=cpu_waster, args=(i,))
        thread.start()
    display_threads()


if __name__ == '__main__':
    '''
    输出：
    ----------
    Current process PID: 35564
    Thread Count: 1
    Active threads:
    <_MainThread(MainThread, started 15116)>
    Starting 5 CPU wasters...
    Thread-1 (cpu_waster) doing 0 work.
    Thread-2 (cpu_waster) doing 1 work.
    Thread-3 (cpu_waster) doing 2 work.
    Thread-4 (cpu_waster) doing 3 work.
    Thread-5 (cpu_waster) doing 4 work.
    ----------
    Current process PID: 35564
    Thread Count: 6
    Active threads:
    <_MainThread(MainThread, started 15116)>
    <Thread(Thread-1 (cpu_waster), started 21056)>
    <Thread(Thread-2 (cpu_waster), started 25468)>
    <Thread(Thread-3 (cpu_waster), started 27832)>
    <Thread(Thread-4 (cpu_waster), started 34800)>
    <Thread(Thread-5 (cpu_waster), started 3212)>
    '''
    num_threads = 5
    main(num_threads)

```

## feature

1.同一个进程里面的所有线程共享该进程的数据。

# 说明

1.有时候会看到 a sequence of, a set of 交换使用，如：a set of instructions, a sequence of instructions。两者没有区别，都表示“一系列”，“一连串”，“一组”，“一些”。