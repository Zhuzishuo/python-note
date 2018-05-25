# Python学习笔记 —— 多进程

## multiprocessing

`multiprocessing`是`python`提供的跨平台支持多进程的模块。

### Process

```English
In multiprocessing, processes are spawned by creating a Process object and then calling its start() method. Process follows the API of threading.Thread.
```

在`multiprocessing`模块中提供了`Process`类来创建进程对象，并提供了`start()`方法启动进程。

`Process`类和`threading`中的`Thread`类类似，一个是创建进程，一个是创建线程。

```python
from multiprocessing import Process
import os


def info(title):
    print(title)
    print('module name:', __name__)
    print('parent process:', os.getppid())
    print('process id:', os.getpid())


def f(name):
    info('function f')
    print('hello', name)


if __name__ == '__main__':
    info('main line')
    p = Process(target=f, args=('bob',))
    p.start()
    p.join()
```

* **Process\(target=run\_process, args=\('process1',\)\)**：`Process`表示一个进程，`target=`后面跟一个方法，而`args=`后面则是这个方法需要传入的参数。

* **.run\(\)**：`Method representing the process’s activity.`

* **.start\(\)**：`Start the process’s activity.`启动一个进程。

  * 该方法在单独的进程中调用process对象的 `run()`方法启动一个进程。

    ```English
    This must be called at most once per process object.  It arranges for the object’s `run()` method to be invoked in a separate process.
    ```

* **.join\(\[timeout\]\)**：主进程需要等待子进程完成才进行下一步操作。

  * 当_timeout_ 为`None` 时，外程序遇到`join()`方法会阻塞，直到使用`join()`方法的进程终止后才继续`join()`方法后面的内容。

    ```English
    If the optional argument timeout is None (the default)，the method blocks until the process whose join() method is called terminates。
    ```

  * 当_timeout_ 为正数时，外程序遇到`join()`方法会阻塞，直到使用`join()`方法的进程终止，或者超出了_timeout_ 时间，都会让该程序终止然后进行外程序`join()`方法后面的内容。

    ```English
    If timeout is a positive number，it blocks at most timeout seconds。Note that the method returns None if its process terminates or if the method times out。Check the process’s exitcode to determine if it terminated。
    ```

* **.is\_alive\(\)**：`Return whether the process is alive.`返回进程存活状态。

* **.terminate\(\)**：`Terminate the process.` 终止进程。

  > Warning:
  >
  > ​    If this method is used when the associated process is using a pipe or queue then the pipe or queue is liable to become corrupted and may become unusable by other process.  Similarly, if the process has acquired a lock or semaphore etc. then terminating it is liable to cause other processes to deadlock.
  >
  > ​    使用该方法会使进程中的管道和队列损坏，如果进程中正使用锁，则会造成死锁。

### Pool

`Pool`可以提供指定数量的进程供用户调用，当有新的请求提交到`pool`中时，如果池还没满，那么会在池中创建一个新的进程用来执行请求；如果池中的进程数量已经达到规定的最大值，那么请求就会等待，直到池中有进程结束，才会创建新的进程执行请求。

```python
from multiprocessing import Pool
from time import sleep


def run_pool(i):
    print('this process`s i is: %d' % i)
    sleep(1)
    print('%d end' % i)


if __name__ == '__main__':
    p = Pool(3)
    for i in range(5):
        p.apply_async(run_pool, (i, ))  # 非阻塞
        # p.apply(run_pool, (i, ))      # 阻塞
    p.close()
    p.join()
```

---

```python
from multiprocessing import Pool
from time import sleep


def process1():
    print('this process is process1')
    sleep(1)
    print('process1 end')


def process2():
    print('this process is process2')
    sleep(1)
    print('process2 end')


def process3():
    print('this process is process3')
    sleep(1)
    print('process3 end')


if __name__ == '__main__':
    process_list = [process1, process2, process3]
    p = Pool(3)
    for func in process_list:
        p.apply_async(func)
    p.close()
    p.join()
```

* `apply_async(func[, args[, kwds[, callback]]])`：是**非阻塞**的。

  ```English
      A variant of the apply() method which returns a result object.

      If callback is specified then it should be a callable which accepts a single argument. When the result becomes ready callback is applied to it, that is unless the call failed, in which case the error_callback is applied instead.

      If error_callback is specified then it should be a callable which accepts a single argument. If the target function fails, then the error_callback is called with the exception instance.

      Callbacks should complete immediately since otherwise the thread which handles the results will get blocked.
  ```

* `apply(func[, args[, kwds]])`：是**阻塞**的。

  ```English
  Call func with arguments args and keyword arguments kwds. It blocks until the result is ready. Given this blocks, apply_async() is better suited for performing work in parallel. Additionally, func is only executed in one of the workers of the pool.
  ```

* `close()`：关闭pool，使其不在接受新的任务。

  ```English
  Prevents any more tasks from being submitted to the pool. Once all the tasks have been completed the worker processes will exit.
  ```

* `terminate()`：结束工作进程，不再处理未完成的任务。

  ```English
  Stops the worker processes immediately without completing outstanding work. When the pool object is garbage collected terminate() will be called immediately.
  ```

* `join()`：主进程阻塞，等待子进程的退出， join方法要在close或terminate之后使用。

  ```English
  Wait for the worker processes to exit. One must call close() or terminate() before using join().
  ```

### Proxy

缺

### Logging

缺

### Synchronization

**Lock**

缺

**RLock**

缺

**Event **

缺

**Semaphore**

缺

### 进程间通信

`python`中父子进程是并发进行的，**默认不支持共享全局变量。**

当使用`fork()`进行系统调用时，子进程使用的页目录和页表是复制父进程的，所以此时父子进程是共享所有数据的，全局变量在父子进程的页表中都映射到了同一块物理内存中。但是当父子进程中有一方执行了写操作时，系统会先给子进程申请一页新内存，并拷贝父进程中原全局变量的数据进新的内存中，更新子进程的页表（将子进程中的全局变量映射指向新的内存），然后才执行写操作。

```python
from multiprocessing import Process
import os


data_list = ['+++']


def add_data():
    global data_list
    data_list.append(1)
    data_list.append(2)
    data_list.append(3)
    print("子进程", os.getpid(), data_list)


if __name__ == "__main__":
    p = Process(target=add_data, args=())
    p.start()
    p.join()
    data_list.append("a")
    data_list.append("b")
    data_list.append("c")
    print("主进程", os.getpid(), data_list)
```

那么`multiprocess`是怎么实现进程间通信的呢？

```English
multiprocessing supports two types of communication channel between processes:
Queues
    The Queue class is a near clone of queue.Queue.
    Queues are thread and process safe.
Pipes
    The Pipe() function returns a pair of connection objects connected by a pipe which by default is duplex (two-way).
```

`multiprocessing`提供了两种在进程间通信的方法：`Queues`和`Pipes`。

#### Exchanging objects between processes

##### Queue

```English
multiprocessing.Queue([maxsize])
    Returns a process shared queue implemented using a pipe and a few locks/semaphores. When a process first puts an item on the queue a feeder thread is started which transfers objects from a buffer into the pipe.
    Queue implements all the methods of queue.Queue except for task_done() and join().
```

> 通过`Queue`实现进程间通信的方式，只能在通过`Process`创建进程的情况下使用，如果是通过进程池`Pool`创建的进程就不行了，进程池进程间通信的方式后面会讲。

* 父子进程间通信

```python
from multiprocessing import Process, Queue


def reader_process(q):
    print(q.get())


if __name__ == '__main__':
    q = Queue()
    p = Process(target=reader_process, args=(q,))
    p.start()
    q.put(100)
    p.join()
```

* 子进程间通信

```python
def get(q):
    for i in range(10):
        print('q get %s in queue' % q.get())


def put(q):
    for i in range(10):
        q.put(i)
        print('q put %d in queue' % i)


if __name__ == '__main__':
    q = Queue()
    p_get = Process(target=get, args=(q,))
    p_get.start()
    p_put = Process(target=put, args=(q,))
    p_put.start()
    p_put.join()
    p_get.join()
```

`Queue`**方法介绍：**

* `.put(self, obj, block=True, timeout=None)`：`Put obj into the queue.` 将元素入队。

  * 当`block`为`True`时，在队列不满时，将元素进队，在队列满时阻塞等待队列中有空闲位置时再进队。

    ```English
    If the optional argument block is True(the default) and timeout is None (the default), block if necessary until a free slot is available.
    ```

    * 当_timeout_为正数时，在队列满时，如果超时秒时间内没有空闲位置，则报`queue.Full`。

      ```English
      If timeout is a positive number, it blocks at most timeout seconds and raises the queue.Full exception if no free slot was available within that time.
      ```

  * 当`block`为`False`时，在队列不满时将元素入队，在队列满时，报`queue.Full`。

    ```English
    Otherwise (block is False), put an item on the queue if a free slot is immediately available, else raise the queue.Full exception (timeout is ignored in that case).
    ```

* `.put_nowait(self, obj)`：相当`.put(item, block=False)`

---

* `.get(self, block=True, timeout=None)`：`Remove and return an item from the queue.`将元素移出队列并返回。

  * `Queue`是不可迭代对象，不能通过`for`循环取值，取值时每次调用`.get()`方法将值出队。

  * 当`block`为`True`时，在队列不为空时返回一个元素，不为空时阻塞等待队列中有元素时再返回。

    ```English
    If optional args block is True (the default) and timeout is None (the default), block if necessary until an item is available.
    ```

    * 当_timeout_为正数时，在队列为空时，如果在超时秒时间内没有可返回元素，则报`queue.Empty`。

      ```English
      If timeout is a positive number, it blocks at most timeout seconds and raises the queue.Empty exception if no item was available within that time.
      ```

  * 当`block`为`False`时，在队列不为空时返回一个元素，在队列为空时，报`queue.Empty`。

    ```English
    Otherwise (block is False), return an item if one is immediately available, else raise the queue.Empty exception (timeout is ignored in that case).
    ```

* `.get_nowait(self)`：相当`.get(block=False)`。

---

* `.qsize(self)`：Return the approximate size of the queue.  
* `.empty(self)`：Return **True** if the queue is empty, **False** otherwise. 
* `.full(self)`：Return **True** if the queue is full, **False** otherwise. 
* `.close(self)`：清空队列后，后台程序将退出

**multiprocessing.SimpleQueue **

```English
It is a simplified Queue type, very close to a locked Pipe.

empty()
    Return True if the queue is empty, False otherwise.
get()
    Remove and return an item from the queue.
put(item)
    Put item into the queue.
```

**multiprocessing.JoinableQueue\(\[**_**maxsize**_**\]\) **

```English
JoinableQueue, a Queue subclass, is a queue which additionally has task_done() and join() methods.

    task_done()
        Indicate that a formerly enqueued task is complete. Used by queue consumers. For each get() used to fetch a task, a subsequent call to task_done() tells the queue that the processing on the task is complete.
        If a join() is currently blocking, it will resume when all items have been processed (meaning that a task_done() call was received for every item that had been put() into the queue).
        Raises a ValueError if called more times than there were items placed in the queue.

    join()
        Block until all items in the queue have been gotten and processed.
        The count of unfinished tasks goes up whenever an item is added to the queue. The count goes down whenever a consumer calls task_done() to indicate that the item was retrieved and all work on it is complete. When the count of unfinished tasks drops to zero, join() unblocks.
```

##### Pipe

```English
multiprocessing.Pipe([duplex])
    If duplex is True (the default) then the pipe is bidirectional(双向的). 
    If duplex is False then the pipe is unidirectional(单向的): conn1 can only be used for receiving messages and conn2 can only be used for sending messages.
```

> `.Pipe()`是Queue的底层结构，但是没有`feed`线程和`put/get`的超时控制。一定程度上和`SimpleQueue`很像。

```python
def Pipe(duplex=True):
    return Connection(), Connection()
```

下面给出例子：

```python
from multiprocessing import Process, Pipe


def process1(pipe):
    s = 'Hello,This is process1'
    pipe.send(s)


def process2(pipe):
    print("process2 recieve:", pipe.recv())


if __name__ == "__main__":
    pipe = Pipe()
    p1 = Process(target=process1, args=(pipe[0],))
    p2 = Process(target=process2, args=(pipe[1],))
    p1.start()
    p2.start()
    p1.join()
    p2.join()
    print('\nend all processes.')
```

##### Manage

**Manage\(\)**返回一个已经启动的`SyncManager`对象，可用于进程间通信。

`manager`被定义在`multiprocessing.managers`模块中。

```english
Returns a started SyncManager object which can be used for sharing objects between processes. The returned manager object corresponds to a spawned child process and has methods which will create shared objects and return corresponding proxies.

The manager classes are defined in the multiprocessing.managers module:
```

`Queue`和`Pipe`实现的数据共享方式只支持两种结构 `Value`和  `Array`。

`Manage()`返回的`SyncManager`对象支持的类型非常多，包括： `Value`，`Array`，`list`,  `dict`，`Queue`, `Namespace`, `Lock`, `RLock`, `Semaphore`, `BoundedSemaphore`,  `Condition`, `Event`等。

就如前面说通过进程池`Pool`创建的进程不能通过`Queue`直接通信，通过`Manage`就可以。

```python
from multiprocessing import Pool, Manager
import os


def write(queue):
    print('write启动 %s，父进程为 %s' % (os.getpid(), os.getppid()))
    for i in 'Hello Python':
        queue.put(i)
        print('write 将 %s 写进queue' % i)


def read(queue):
    print('read启动 %s，父进程为 %s' % (os.getpid(), os.getppid()))
    for i in range(len('Hello Python')):
        print('read 从queue 获取的到的信息：%s' % queue.get(True))


if __name__ == '__main__':
    pool = Pool(3)
    queue = Manager().Queue()
    pool.apply_async(write, (queue,))
    pool.apply_async(read, (queue,))
    pool.close()
    pool.join()
```

**class multiprocessing.managers.SyncManager**

`SyncManager`是`BaseManager`的一个子类，可用于进程的同步。

此类型的对象由`multiprocessing.Manager()`返回。

```english
A subclass of BaseManager which can be used for the synchronization of processes. Objects of this type are returned by multiprocessing.Manager().
```

`SyncManager`对象支持的共享对象类型：

```english
Barrier(parties[, action[, timeout]])
    Create a shared threading.Barrier object and return a proxy for it.
    New in version 3.3.

BoundedSemaphore([value])
    Create a shared threading.BoundedSemaphore object and return a proxy for it.

Condition([lock])
    Create a shared threading.Condition object and return a proxy for it.
    If lock is supplied then it should be a proxy for a threading.Lock or threading.RLock object.
    Changed in version 3.3: The wait_for() method was added.

Event()
    Create a shared threading.Event object and return a proxy for it.

Lock()
    Create a shared threading.Lock object and return a proxy for it.

Namespace()
    Create a shared Namespace object and return a proxy for it.

Queue([maxsize])
    Create a shared queue.Queue object and return a proxy for it.

RLock()
    Create a shared threading.RLock object and return a proxy for it.

Semaphore([value])
    Create a shared threading.Semaphore object and return a proxy for it.

Array(typecode, sequence)
    Create an array and return a proxy for it.

Value(typecode, value)
    Create an object with a writable value attribute and return a proxy for it.

dict()
dict(mapping)
dict(sequence)
    Create a shared dict object and return a proxy for it.

list()
list(sequence)
    Create a shared list object and return a proxy for it.
```

#### Sharing state between processes

##### Shared memory

共享内存

**multiprocessing.Value\(typecode\_or\_type, \*args, lock=True\)**

```English
    Return a ctypes object allocated from shared memory. By default the return value is actually a synchronized wrapper for the object. The object itself can be accessed via the value attribute of a Value.

    typecode_or_type determines the type of the returned object: it is either a ctypes type or a one character typecode of the kind used by the array module. *args is passed on to the constructor for the type.

    If lock is True (the default) then a new recursive lock object is created to synchronize access to the value. If lock is a Lock or RLock object then that will be used to synchronize access to the value. If lock is False then access to the returned object will not be automatically protected by a lock, so it will not necessarily be “process-safe”.
```

**multiprocessing.Array\(typecode\_or\_type, size\_or\_initializer, \*, lock=True\)**

```English
    Return a ctypes array allocated from shared memory. By default the return value is actually a synchronized wrapper for the array.

    typecode_or_type determines the type of the elements of the returned array: it is either a ctypes type or a one character typecode of the kind used by the array module. If size_or_initializer is an integer, then it determines the length of the array, and the array will be initially zeroed. Otherwise, size_or_initializer is a sequence which is used to initialize the array and whose length determines the length of the array.

    If lock is True (the default) then a new lock object is created to synchronize access to the value. If lock is a Lock or RLock object then that will be used to synchronize access to the value. If lock is False then access to the returned object will not be automatically protected by a lock, so it will not necessarily be “process-safe”.

    Note that lock is a keyword only argument.

    Note that an array of ctypes.c_char has value and raw attributes which allow one to use it to store and retrieve strings.
```

```python
from multiprocessing import Process, Value, Array

def f(n, a):
    n.value = 3.1415927
    for i in range(len(a)):
        a[i] = -a[i]

if __name__ == '__main__':
    num = Value('d', 0.0)
    arr = Array('i', range(10))

    p = Process(target=f, args=(num, arr))
    p.start()
    p.join()

    print(num.value)
    print(arr[:])
```

##### Server process：Managers

管理共享对象的服务进程

```English
Managers provide a way to create data which can be shared between different processes, including sharing over a network between processes running on different machines. A manager object controls a server process which manages shared objects. Other processes can access the shared objects by using proxies.
```

> 另起新篇再作研究吧

## subprocess



