---
icon: lucide/repeat
---

# Module 4b: Loops — For & While

> 🔁 Computers never get bored. Loops let you say *"do this 500 times"* with just 3 lines of code. This is where Python starts feeling like a superpower.

---

## 🤔 Why Do We Need Loops?

Imagine you have 500 satellite images and need to process each one. Without loops:

```python
process(image_1)
process(image_2)
process(image_3)
# ... 497 more lines 😭
```

With a loop:

```python
for image in all_images:
    process(image)   # done. all 500. 3 lines. 😎
```

Python has **two types of loops**:

- **`for` loop** — repeat for each item in a collection, or a set number of times
- **`while` loop** — keep repeating as long as a condition is True

---

## 🔄 The `for` Loop

### Iterating with `range()` — Repeat N Times

`range()` generates a sequence of numbers. Perfect when you want to repeat something a specific number of times.

```python
# range(5) → 0, 1, 2, 3, 4
for i in range(5):
    print(i)
# 0
# 1
# 2
# 3
# 4
```

```python
# range(start, stop) — stop is NOT included
for i in range(1, 6):
    print(i)
# 1, 2, 3, 4, 5
```

```python
# range(start, stop, step) — count by step
for i in range(10, -1, -2):
    print(i)
# 10, 8, 6, 4, 2, 0
```

!!! tip "range() cheatsheet 📋"
    | Syntax | What it generates |
    |---|---|
    | `range(5)` | `0, 1, 2, 3, 4` |
    | `range(1, 6)` | `1, 2, 3, 4, 5` |
    | `range(0, 10, 2)` | `0, 2, 4, 6, 8` |
    | `range(10, 0, -1)` | `10, 9, 8, 7, 6, 5, 4, 3, 2, 1` |

---

### Iterating Through Lists & Tuples

```python
fruits = ["apple", "banana", "cherry"]

for fruit in fruits:
    # 1st iteration → fruit = "apple"
    # 2nd iteration → fruit = "banana"
    # 3rd iteration → fruit = "cherry"
    print(fruit)
```

```python
numbers_tuple = (10, 20, 30, 40)

for num in numbers_tuple:
    print(num)
# 10, 20, 30, 40
```

---

### Iterating Through Strings

Strings are sequences too — you can loop through each character:

```python
word = "Python"

for char in word:
    print(char)
# P
# y
# t
# h
# o
# n
```

---

### Iterating Through Dictionaries

```python
student_grades = {"Alice": "A", "Bob": "B", "Charlie": "A+"}

# Loop through KEYS (default)
for name in student_grades:
    print(name)
# Alice, Bob, Charlie

# Loop through VALUES
for grade in student_grades.values():
    print(grade)
# A, B, A+

# Loop through KEY-VALUE PAIRS
for name, grade in student_grades.items():
    print(f"{name}: {grade}")
# Alice: A
# Bob: B
# Charlie: A+
```

!!! tip "`.items()` is your best friend 🤝"
    When you need both the key AND the value in a loop, always use `.items()`. It unpacks each pair automatically.

---

### `enumerate()` — Index + Value Together

When you need both the **position** and the **value** while looping:

```python
fruits = ["apple", "banana", "cherry"]

for index, fruit in enumerate(fruits):
    print(f"{index}: {fruit}")
# 0: apple
# 1: banana
# 2: cherry

# Start index from 1 instead of 0
for index, fruit in enumerate(fruits, start=1):
    print(f"{index}. {fruit}")
# 1. apple
# 2. banana
# 3. cherry
```

---

### `zip()` — Loop Through Two Lists Together

```python
names = ["Alice", "Bob", "Charlie"]
scores = [95, 82, 91]

for name, score in zip(names, scores):
    print(f"{name}: {score}")
# Alice: 95
# Bob: 82
# Charlie: 91
```

!!! example "GIS real-world example 🗺️"
    ```python
    shapefiles = ["roads.shp", "rivers.shp", "parks.shp", "buildings.shp"]

    print("Processing files:")
    for i, file in enumerate(shapefiles, start=1):
        print(f"  [{i}/{len(shapefiles)}] Processing {file}...")
        # clip_to_boundary(file)

    # Output:
    #   [1/4] Processing roads.shp...
    #   [2/4] Processing rivers.shp...
    #   [3/4] Processing parks.shp...
    #   [4/4] Processing buildings.shp...
    ```

---

## 🔁 The `while` Loop

The `while` loop keeps running **as long as a condition is True**. Use it when you don't know how many times you'll need to loop.

```python
count = 0

while count < 5:
    print(count)
    count += 1   # ⚠️ always update the condition variable!
# 0, 1, 2, 3, 4
```

```python
# Keep asking until correct password is entered
password = ""
while password != "secret":
    password = input("Enter the password: ")
    if password != "secret":
        print("Incorrect. Try again.")
print("Password accepted! ✅")
```

!!! danger "Infinite loops — the classic trap 🔴"
    If you forget to update the condition variable, the loop runs forever and crashes your program.

    ```python
    # ❌ INFINITE LOOP — count never changes!
    count = 0
    while count < 5:
        print(count)
        # forgot count += 1 😱

    # ✅ Correct
    count = 0
    while count < 5:
        print(count)
        count += 1
    ```

    If you accidentally run an infinite loop, press **`Ctrl + C`** to stop it.

!!! example "GIS real-world example 🗺️"
    ```python
    # Keep retrying a file download until it succeeds
    download_success = False
    attempts = 0

    while not download_success and attempts < 3:
        print(f"Attempt {attempts + 1}: Downloading satellite data...")
        # download_success = try_download()
        download_success = True   # simulating success
        attempts += 1

    if download_success:
        print("Download complete ✅")
    else:
        print("Download failed after 3 attempts ❌")
    ```

---

## 🎮 Loop Control Statements

These let you **control what happens inside a loop** mid-execution.

### `break` — Exit the Loop Immediately

```python
for i in range(10):
    if i == 5:
        break        # stop the loop entirely when i hits 5
    print(i)
# 0, 1, 2, 3, 4
```

```python
# Find the first even number in a list
numbers = [3, 7, 9, 4, 11, 6]

for num in numbers:
    if num % 2 == 0:
        print(f"First even number: {num}")
        break
# First even number: 4
```

### `continue` — Skip This Iteration, Keep Going

```python
for i in range(10):
    if i % 2 == 0:   # if even
        continue     # skip the rest of this iteration
    print(i)
# 1, 3, 5, 7, 9  — only odd numbers printed
```

```python
# Skip invalid data
data = [10, None, 25, None, 30]

for value in data:
    if value is None:
        continue   # skip missing values
    print(f"Processing: {value}")
# Processing: 10
# Processing: 25
# Processing: 30
```

### `pass` — Do Nothing (Placeholder)

```python
for i in range(3):
    if i == 1:
        pass   # placeholder — logic not implemented yet
    print(f"Processing item {i}")
# Processing item 0
# Processing item 1
# Processing item 2
```

!!! tip "When to use each 🎯"
    | Statement | Use when... |
    |---|---|
    | `break` | You found what you needed — stop searching |
    | `continue` | Skip bad/invalid data, keep processing the rest |
    | `pass` | Placeholder while you're still writing the code |

---

## 🔢 Nested Loops — Loop Inside a Loop

```python
# Multiplication table
for i in range(1, 4):
    for j in range(1, 4):
        print(f"{i} x {j} = {i*j}")
    print("---")
# 1 x 1 = 1
# 1 x 2 = 2
# 1 x 3 = 3
# ---
# 2 x 1 = 2
# ...
```

### Fun Pattern Examples

```python
# Right-aligned star triangle
n = 5
for i in range(1, n + 1):
    print(" " * (n - i) + "*" * i)
#     *
#    **
#   ***
#  ****
# *****

# Number pyramid
n = 4
for i in range(1, n + 1):
    for j in range(1, i + 1):
        print(j, end=" ")
    print()
# 1
# 1 2
# 1 2 3
# 1 2 3 4
```

---

## 🔬 Iterables & Iterators (Under the Hood)

When Python runs a `for` loop, it's actually using something called an **iterator** behind the scenes.

```python
my_list = [1, 2, 3]
my_iterator = iter(my_list)   # create an iterator manually

print(next(my_iterator))   # 1
print(next(my_iterator))   # 2
print(next(my_iterator))   # 3
# next() after this → raises StopIteration
```

The `for` loop does this automatically — it calls `iter()` once, then `next()` on every iteration until `StopIteration` is raised.

!!! info "What's an iterable? 🤔"
    Any object you can loop over is an **iterable**: lists, tuples, strings, dicts, sets, range objects, files.

    An **iterator** is the object that actually does the stepping — it remembers where it is and gives you the next item on demand.

---

## ⚡ Generators — Memory-Efficient Loops

A **generator** is a special function that produces values **one at a time** instead of storing them all in memory. Perfect for large datasets.

```python
# Regular function — returns all values at once (stored in memory)
def get_numbers(n):
    return list(range(n))

# Generator function — yields one value at a time
def count_up_to(n):
    i = 0
    while i < n:
        yield i    # pause here, give back i, resume next time
        i += 1

for number in count_up_to(5):
    print(number)
# 0, 1, 2, 3, 4
```

```python
# Fibonacci generator
def fibonacci(max_count):
    a, b = 0, 1
    count = 0
    while count < max_count:
        yield a
        a, b = b, a + b
        count += 1

for num in fibonacci(8):
    print(num)
# 0, 1, 1, 2, 3, 5, 8, 13
```

!!! tip "Generator vs List — when to use which? 💡"
    - Use a **list** when you need all values at once, or need to access by index
    - Use a **generator** when processing large data (millions of rows) — it uses almost no memory since it generates one item at a time

---

## 🎯 Quick Recap

| Concept | What it does | Example |
|---|---|---|
| `for` loop | Repeat for each item in a sequence | `for x in my_list:` |
| `range()` | Generate a number sequence | `range(1, 10, 2)` |
| `while` loop | Repeat while condition is True | `while count < 5:` |
| `break` | Exit loop immediately | Stop when found |
| `continue` | Skip current iteration | Skip invalid data |
| `pass` | Do nothing (placeholder) | Unfinished code |
| `enumerate()` | Index + value together | `for i, v in enumerate(lst)` |
| `zip()` | Loop two lists together | `for a, b in zip(l1, l2)` |
| Generator | Yield values one at a time | Memory-efficient large loops |

!!! success "Module 4 Complete! 🎉"
    Your code can now **make decisions** (if/elif/else) and **repeat tasks** (for/while). These two concepts alone let you automate almost anything.

    Next up: **Functions** — writing reusable blocks of code so you never repeat yourself again!
