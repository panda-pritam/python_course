---
icon: lucide/zap
---

# Module 7: Concurrency — Multithreading, AsyncIO & Multiprocessing

> ⚡ What if instead of doing tasks one by one, your program could do many things at the same time? That's concurrency — and it's what separates slow scripts from fast, production-ready programs.

---

## 🤔 The Problem: Doing Things One at a Time

By default, Python runs code **sequentially** — one line at a time, top to bottom.

```python
import time

def task(i):
    print(f"Start {i}")
    time.sleep(2)       # simulate waiting (network, file, etc.)
    print(f"End {i}")

task(0)   # waits 2 seconds
task(1)   # waits another 2 seconds
task(2)   # waits another 2 seconds
# Total: 6 seconds 😴
```

For 3 tasks that each take 2 seconds, sequential execution takes **6 seconds**. But what if they could all run at the same time and finish in **~2 seconds**?

That's what concurrency solves.

---

## 🍳 The Kitchen Analogy

Before diving into code, here's the mental model you need:

!!! info "Real-world analogy 🍳"
    Imagine a **restaurant kitchen** with different cooking scenarios:

    === "Sequential (No concurrency)"
        One chef. Cooks dish 1 completely, then dish 2, then dish 3.
        **Total time = sum of all dishes.** Slow. 🐢

    === "Multithreading"
        One chef, but they're smart about waiting. While pasta boils (I/O wait), they chop vegetables. They switch between tasks during idle time.
        **Good for tasks that spend time waiting** (network, files). 🔄

    === "AsyncIO"
        One chef, but they *voluntarily* pause a task and switch to another when waiting. Very efficient, no wasted time.
        **Best for high-volume waiting tasks** (thousands of API calls). ⚡

    === "Multiprocessing"
        Multiple chefs, each with their own kitchen. All cooking simultaneously, truly in parallel.
        **Best for heavy CPU work** (math, image processing). 💪

---

## 🔒 The GIL — Python's "One at a Time" Rule

Before understanding the solutions, you need to know the problem:

!!! warning "The Global Interpreter Lock (GIL) 🔒"
    Python has a rule called the **GIL** — only **one thread can execute Python code at a time**, even on a multi-core machine.

    Think of it like a **single kitchen counter** — even if you have 8 chefs (CPU cores), only one can use the counter at a time.

    **The workaround:**
    - **Multithreading** — threads release the GIL while *waiting* (I/O), so others can run
    - **AsyncIO** — single thread, cooperative switching, no GIL issue
    - **Multiprocessing** — each process has its *own* GIL, so true parallelism is possible

---

## 🔄 Multithreading — Concurrent I/O

Multithreading runs multiple **threads** inside one process. The OS switches between them. While one thread waits for a network response, another thread can run.

!!! info "Real-world analogy 📞"
    A **call center agent** handling multiple customers on hold. While customer A is on hold (waiting), the agent talks to customer B. They're not truly doing two things at once — they're switching between tasks during idle time.

### Basic Threading

```python
import threading
import time

def task(i):
    print(f"Start {i}")
    time.sleep(2)        # simulate I/O wait (network, file read, etc.)
    print(f"End {i}")

threads = []
start = time.perf_counter()

for i in range(3):
    t = threading.Thread(target=task, args=(i,))
    threads.append(t)
    t.start()            # start all 3 threads

for t in threads:
    t.join()             # wait for all threads to finish

end = time.perf_counter()
print(f"Total time: {end - start:.2f} seconds")
# Total time: ~2.0 seconds (not 6!) ✅
```

### Sequential vs Threaded — Side by Side

=== "🐢 Sequential (6 seconds)"
    ```python
    import time

    def task(i):
        print(f"Start {i}")
        time.sleep(2)
        print(f"End {i}")

    start = time.perf_counter()
    for i in range(3):
        task(i)   # one at a time
    end = time.perf_counter()
    print(f"Time: {end - start:.2f}s")   # ~6.0 seconds
    ```

=== "⚡ Threaded (~2 seconds)"
    ```python
    import threading, time

    def task(i):
        print(f"Start {i}")
        time.sleep(2)
        print(f"End {i}")

    start = time.perf_counter()
    threads = [threading.Thread(target=task, args=(i,)) for i in range(3)]
    for t in threads: t.start()
    for t in threads: t.join()
    end = time.perf_counter()
    print(f"Time: {end - start:.2f}s")   # ~2.0 seconds
    ```

### Key Methods

| Method | What it does |
|---|---|
| `Thread(target=fn, args=(x,))` | Create a thread that will run `fn(x)` |
| `.start()` | Start the thread — begins execution |
| `.join()` | Wait for this thread to finish before continuing |

!!! warning "`.join()` is important! ⚠️"
    Without `.join()`, your main program might finish and exit before the threads complete their work. Always join your threads.

---

## 🏊 ThreadPoolExecutor — Managed Thread Pool

Instead of manually creating and managing threads, `ThreadPoolExecutor` does it for you:

!!! info "Real-world analogy 🏭"
    You have 10 tasks and 3 workers. The workers pick up tasks from a queue, finish them, and pick the next one. You don't manage who does what — the pool handles it.

```python
from concurrent.futures import ThreadPoolExecutor
import time

def task(i):
    print(f"Start {i}")
    time.sleep(2)
    print(f"End {i}")
    return i * 10

# 3 workers handle 6 tasks
with ThreadPoolExecutor(max_workers=3) as executor:
    results = list(executor.map(task, range(6)))

print("Results:", results)
# Results: [0, 10, 20, 30, 40, 50]
```

!!! tip "When to use ThreadPoolExecutor 🎯"
    - API calls 🌐
    - File reading/writing 📂
    - Downloading/uploading files 📤
    - Database queries 🗄️

    Basically: anything where your code **waits** for something external.

---

## ⚡ AsyncIO — Cooperative Multitasking

AsyncIO runs in a **single thread** but switches between tasks *voluntarily* when one is waiting. No OS thread switching overhead — incredibly efficient for high-volume I/O.

!!! info "Real-world analogy 👨‍🍳"
    One chef, but very organized. They put pasta on to boil, and *while it boils* they chop vegetables. When the pasta timer goes off, they come back to it. They voluntarily switch between tasks at natural pause points — not forced by anyone.

    The key word is **cooperative** — tasks *choose* to pause with `await`.

### Basic AsyncIO

```python
import asyncio

async def task(i):
    print(f"Start {i}")
    await asyncio.sleep(2)   # voluntarily pause — let others run
    print(f"End {i}")

async def main():
    t1 = asyncio.create_task(task(0))
    t2 = asyncio.create_task(task(1))
    t3 = asyncio.create_task(task(2))

    await t1
    await t2
    await t3

await main()
# All 3 start almost instantly, all end ~2 seconds later
```

**What happens step by step:**

1. `main()` starts
2. All 3 tasks are created and scheduled on the **event loop**
3. Each prints "Start" quickly one after another
4. Each hits `await asyncio.sleep(2)` — voluntarily pauses, gives control back
5. The event loop manages all 3 timers simultaneously
6. After ~2 seconds, all 3 resume and print "End"
7. **Total time: ~2 seconds** ✅

### `asyncio.gather()` — Run All and Collect Results

```python
import asyncio

async def task(i):
    await asyncio.sleep(2)
    return f"Result {i}"

async def main():
    results = await asyncio.gather(
        task(0),
        task(1),
        task(2)
    )
    print(results)
    # ['Result 0', 'Result 1', 'Result 2']

await main()
```

!!! tip "`gather()` vs `create_task()` 🤔"
    - `create_task()` — schedule tasks, await them individually
    - `gather()` — run all tasks together, collect all results at once (cleaner for multiple tasks)

### Key AsyncIO Keywords

| Keyword | What it means |
|---|---|
| `async def` | This function is a coroutine (can be paused) |
| `await` | Pause here, let other tasks run, resume when done |
| `asyncio.create_task()` | Schedule a coroutine to run on the event loop |
| `asyncio.gather()` | Run multiple coroutines concurrently, collect results |

---

## 💪 Multiprocessing — True Parallelism

Multiprocessing creates **separate processes**, each with its own memory and its own Python interpreter (and its own GIL). This means **true parallel execution** across multiple CPU cores.

!!! info "Real-world analogy 🏗️"
    Multiple **construction crews** working on different buildings simultaneously. Each crew has their own tools, their own materials, their own site. They don't share anything — they work truly in parallel.

    Unlike threading (one kitchen, one counter), multiprocessing is multiple kitchens running at the same time.

```python
import multiprocessing

def square_number(n):
    return n * n

if __name__ == "__main__":
    numbers = [1, 2, 3, 4, 5]

    # Pool creates workers equal to your CPU core count
    with multiprocessing.Pool() as pool:
        results = pool.map(square_number, numbers)

    print(results)   # [1, 4, 9, 16, 25]
```

!!! warning "Always use `if __name__ == '__main__':` ⚠️"
    On Windows, multiprocessing requires this guard. Without it, each spawned process tries to re-run the whole script, causing infinite spawning.

---

## 🗺️ Choosing the Right Tool

| Scenario | Best tool | Why |
|---|---|---|
| Download 100 files | **Multithreading** or **AsyncIO** | Waiting for I/O — GIL released during wait |
| Make 10,000 API calls | **AsyncIO** | Lightweight, handles massive concurrency |
| Process 1M rows of data | **Multiprocessing** | CPU-heavy — needs true parallelism |
| Scrape 50 websites | **AsyncIO** or **ThreadPoolExecutor** | I/O bound |
| Render 500 images | **Multiprocessing** | CPU bound |
| Simple background task | **Threading** | Easy to set up |

!!! info "The golden rule 🥇"
    - **I/O bound** (waiting for network, files, database) → **Threading or AsyncIO**
    - **CPU bound** (heavy math, image processing, data crunching) → **Multiprocessing**

---

## 🎯 Quick Recap

| Concept | How it works | Best for |
|---|---|---|
| **Sequential** | One task at a time | Simple scripts |
| **Multithreading** | OS switches between threads | I/O-bound tasks (files, network) |
| **ThreadPoolExecutor** | Managed pool of threads | Multiple I/O tasks |
| **AsyncIO** | Single thread, cooperative switching | High-volume I/O (thousands of calls) |
| **Multiprocessing** | Multiple processes, true parallelism | CPU-heavy tasks |
| **GIL** | Python's one-thread-at-a-time rule | Why threading doesn't help CPU tasks |

!!! success "🎉 You've completed the core Python modules!"
    You now know variables, data structures, operators, control flow, loops, functions, error handling, and concurrency. This is the complete foundation of Python programming. Everything from here — GIS tools, AI models, web apps — builds on exactly this.
