
Python本身是单线程：
	**Python Global Interpreter Lock (GIL)**: a mechanism used in the default CPython interpreter that ensures only *one* thread of Python bytecode executes at a time, even on multi-core processors.
## 概览
- **IO 多** → asyncio + semaphore
	- **阻塞** → ProcessPoolExecutor
- **CPU 重** → ProcessPool
- **外部服务** → 连接池
- **模型 / Engine** → 对象池
- **GPU / 显存紧张** → 严格限流池
## 进程池（Process Pool）
**适合：** CPU 密集型
### `concurrent.futures.ProcessPoolExecutor`

```python
from concurrent.futures import ProcessPoolExecutor
# 底层是from multiprocessing import Pool

def cpu_work(x):
    return x ** 10

# usage1
with ProcessPoolExecutor(max_workers=4) as pool:
    results = list(pool.map(cpu_work, range(10)))
    
# usage2
with ProcessPoolExecutor(4) as executor:
    futures = [executor.submit(func, x) for x in data]
    results = [f.result() for f in futures]
```

- 绕过 GIL，每个进程有独立的python解释器和GIL
- 通过pickle实现进程创建/通信，序列化成本高，不适合大对象频繁传递
## 线程池（Thread Pool）
**适合：** I/O 密集型，除非有*阻塞代码*，不然尽量用asyncio做单线程多协程
	*阻塞代码*：sleep(5)会导致多协程暂停五秒，不会影响多线程，其他例子：
		requests.get(url)
		pandas.read_excel("file.xlsx")
### `concurrent.futures.ThreadPoolExecutor`

```python
from concurrent.futures import ThreadPoolExecutor, as_completed

def work(x):
    return x * x

with ThreadPoolExecutor(max_workers=8) as pool:
    futures = [pool.submit(work, i) for i in range(10)]
    for f in as_completed(futures):
        print(f.result())
```

- 受 GIL 限制，不适合 CPU 密集，线程在等待 I/O 时会释放 GIL
- 由操作系统控制的**抢占式调度（preemptive）**
## 协程池（asyncio）
**适合：** 高并发 I/O，相比ThreadPoolExecutor并发量更高、成本更低
### `asyncio`

```python
await asyncio.gather(*(task(i) for i in range(100)))
```

- 单线程内部事件控制，**协作式调度（cooperative）**
	- await时控制权交还给event loop
#### 用 `Semaphore` 控制并发（最常见“协程池”写法）
```python
sem = asyncio.Semaphore(10)

async def limited_task(x):
    async with sem:
        return await task(x)
```
#### 官方线程池桥接
```python
loop = asyncio.get_running_loop()
await loop.run_in_executor(None, blocking_func)
```
## 连接池（Connection Pool）
**适合：** DB、HTTP、RPC 连接复用
### 数据库连接池
- **SQLAlchemy**
```python
from sqlalchemy import create_engine

engine = create_engine(
    DB_URL,
    pool_size=10,
    max_overflow=20,
)
```
- **asyncpg**
```python
import asyncio
import asyncpg

async def main():
    pool = await asyncpg.create_pool(
        user="postgres",
        password="123",
        database="test",
        host="127.0.0.1",
        port=5432,
        min_size=5,
        max_size=20
    )

    async with pool.acquire() as conn:
        rows = await conn.fetch("SELECT * FROM users")
        print(rows)

    await pool.close()

asyncio.run(main())
```
### HTTP 连接池
- **requests**
```python
# Session内有一个连接池（urllib3 PoolManager）
session = requests.Session()
for _ in range(100):
    session.get(url)
    # requests.get(url)则是创建session后又销毁
```
- **httpx / aiohttp**
```python
async with httpx.AsyncClient(limits=httpx.Limits(max_connections=100)) as client:
    ...
```
## 对象池（Object Pool）
**适合：** 构造昂贵、可复用对象（Tokenizer、Engine、Session）
### 简单对象池：queue
```python
from queue import Queue

class ObjectPool:
    def __init__(self, size, factory):
        self.q = Queue(size)
        for _ in range(size):
            self.q.put(factory())

    def acquire(self):
        return self.q.get()

    def release(self, obj):
        self.q.put(obj)
```
### 异步对象池：asyncio
```python
class AsyncPool:
    def __init__(self, size, factory):
        self.sem = asyncio.Semaphore(size)
        self.factory = factory
        self.items = []

    async def acquire(self):
        await self.sem.acquire()
        if self.items:
            return self.items.pop()
        return await self.factory()

    async def release(self, item):
        self.items.append(item)
        self.sem.release()
```
## 严格限流池
容量不能超卖的资源调度器
```python
class GPUMemoryPool:
    def __init__(self, total):
        self.total = total
        self.used = 0
        self.cond = asyncio.Condition()

    async def acquire(self, amount):
        async with self.cond:
            while self.used + amount > self.total:
                await self.cond.wait()
            self.used += amount

    async def release(self, amount):
        async with self.cond:
            self.used -= amount
            self.cond.notify_all()

```
## 第三方库池化方案

|场景|库|
|---|---|
|高性能线程池|`loky`|
|进程池增强|`pathos`|
|通用池|`psutil + queue`|
|缓存池|`cachetools`|
|Actor 模型|`ray`|
|高并发服务|`gunicorn + uvicorn workers`|
