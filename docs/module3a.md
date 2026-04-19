---
icon: lucide/box
---

# Module 3a: Variables & Data Types

> 🧠 This is where your code gets a memory. Without variables, your program forgets everything the moment it runs — like that one friend who never remembers anything.

---

## 📦 What is a Variable? (The Real-World Analogy)

Before we touch any code, let's think about the real world.

Imagine a **warehouse** full of containers. Each container:

- Has a **label** on it (so you know what's inside)
- Holds a **specific type of material** (you don't put water in a box meant for bricks)
- Can be **opened and changed** anytime

That's exactly what variables are in programming — **labeled containers for storing information**.

!!! info "The container analogy 🏭"
    - A jar labeled `sugar` holds sugar 🍬
    - A bottle labeled `water` holds water 💧
    - A box labeled `books` holds books 📚

    In code:
    ```python
    sugar = 500       # grams
    water = 2.5       # liters
    books = 3         # count
    ```

Now here's where Python gets interesting...

### 🐍 Python's Smart Containers

In most languages, you have to declare what *type* of container you're using before you put anything in it. Python doesn't care. Python's containers are **flexible** — they can hold anything, and you can change what's inside anytime.

```python
x = 10        # x holds a number
x = "Hello"   # now x holds text — Python is totally fine with this
x = 3.14      # now x holds a decimal — still fine!
```

This is called **dynamic typing** — Python figures out the type automatically based on what you put in.

![Variables as containers](images/image_b4bd5805.png)

---

## 🧠 Why Do We Even Need Variables?

Think about this: what if every time you needed your name in a program, you had to type `"Alice"` manually — 50 times across 200 lines of code? Then your name changes. Now you have to find and fix all 50 places. 😩

Variables solve this:

```python
student_name = "Alice"   # define once

print(student_name)      # use it anywhere
print(f"Welcome, {student_name}!")
print(f"Hello {student_name}, your report is ready.")
```

Change it in one place → updates everywhere. ✅

Here's the full picture of why variables matter:

| Reason | What it means |
|---|---|
| **Temporary Storage** | Hold data while your program runs (user input, results, file paths) |
| **Reusability** | Define once, use everywhere — no copy-pasting values |
| **Readability** | `total_price` is clearer than `99.99` scattered everywhere |
| **Dynamic Calculations** | Work with data you don't know yet (scores, counters, results) |
| **GIS Data Management** | Label coordinates, file paths, layer names clearly |

!!! tip "The sticky note analogy 🗒️"
    Your brain is the computer. Your thoughts are data. Variables are **sticky notes** you use to label those thoughts.

    - Data: `95`
    - Variable: `math_score`

    When someone asks your score, you don't recall the number — you just look at the sticky note labeled `math_score`.

---

## ✏️ Creating & Reassigning Variables

Creating a variable in Python is as simple as:

```python
name = "Pritam"
age = 25
salary = 50000

print(name)    # Pritam
print(age)     # 25
print(salary)  # 50000
```

And you can **reassign** them anytime — the old value is simply replaced:

```python
name = "Python"
age = 70
salary = 2000

print(name)    # Python
print(age)     # 70
print(salary)  # 2000
```

---

## 📏 Rules for Naming Variables

Python has a naming standard called **`snake_case`** — all lowercase, words separated by underscores. This comes from Python's style guide called **PEP 8**.

### The Golden Rules

| Rule | Description |
|---|---|
| **Be Descriptive** | `user_age` not `a` — make it obvious what it holds |
| **snake_case** | `lowercase_with_underscores` |
| **No Keywords** | Don't use `print`, `if`, `list`, `class` — Python already uses these |
| **Case Sensitive** | `my_var` and `My_Var` are two completely different variables |

### ✅ Correct vs ❌ Wrong

| Status | Variable Name | Why? |
|---|---|---|
| ✅ | `student_name = "Sam"` | Descriptive, lowercase, underscores |
| ❌ | `1st_student = "Sam"` | Cannot start with a number |
| ✅ | `total_price = 49.99` | Clear, follows snake_case |
| ❌ | `total price = 49.99` | Spaces are not allowed |
| ✅ | `_temp_value = 10` | Underscore start is allowed |
| ❌ | `class = "Math"` | `class` is a reserved Python keyword |
| ✅ | `is_valid = True` | Great naming for True/False values |
| ❌ | `is-valid = True` | Hyphens not allowed — Python reads it as minus |

```python
# ✅ Correct
student_name = "Alice"
total_price = 129.99
is_active_user = True
buffer_distance_meters = 100

# ❌ These would cause errors (uncomment to see)
# 1st_name = "Bob"
# total-cost = 50
# class = "Science"
# item id = 123
```

!!! tip "GIS-specific naming tip 🗺️"
    Be extra descriptive when working with spatial data:

    - ✅ `buffer_distance_meters = 500` — you know it's meters
    - ❌ `dist = 500` — is it miles? meters? pixels? who knows
    - ✅ `river_shapefile_path = "C:/data/rivers.shp"`
    - ❌ `path1 = "C:/data/rivers.shp"` — path to what?

---

## 🗂️ Python Data Types

Now that we know what variables are, let's talk about **what they can hold** — the different types of data.

Python is **dynamically typed** — you don't declare the type, Python figures it out from the value you assign.

### Primitive Data Types — The Simple Ones

These store a **single value** and are the building blocks of everything else.

```python
# Integer — whole numbers
age = 30
print(f"Value: {age}, Type: {type(age)}")
# Value: 30, Type: <class 'int'>

# Float — decimal numbers
price = 19.99
print(f"Value: {price}, Type: {type(price)}")
# Value: 19.99, Type: <class 'float'>

# String — text
product_name = "Laptop"
print(f"Value: {product_name}, Type: {type(product_name)}")
# Value: Laptop, Type: <class 'str'>

# Boolean — True or False only
is_available = True
print(f"Value: {is_available}, Type: {type(is_available)}")
# Value: True, Type: <class 'bool'>
```

| Type | What it stores | Example |
|---|---|---|
| `int` | Whole numbers | `30`, `-5`, `0` |
| `float` | Decimal numbers | `19.99`, `40.7128`, `-0.5` |
| `str` | Text (any characters) | `"Hello"`, `"New York"` |
| `bool` | Only True or False | `True`, `False` |

### Dynamic Typing in Action

The same variable can hold different types at different times:

```python
x = 10          # x is an integer
print(f"x = {x}, Type: {type(x)}")

x = "Hello"     # now x is a string
print(f"x = {x}, Type: {type(x)}")

x = 3.14159     # now x is a float
print(f"x = {x}, Type: {type(x)}")
```

!!! warning "With great flexibility comes great responsibility ⚠️"
    Dynamic typing is convenient, but type-related bugs can sneak in at runtime. Always be intentional about what type a variable should hold.

---

## 🎯 Quick Recap

| Concept | What it is | Key point |
|---|---|---|
| **Variable** | Named container for data | Flexible, reassignable |
| **int** | Whole number | `age = 25` |
| **float** | Decimal number | `lat = 40.71` |
| **str** | Text | `name = "Alice"` |
| **bool** | True or False | `is_valid = True` |
| **snake_case** | Python naming standard | `my_variable_name` |
| **dynamic typing** | Python auto-detects type | No need to declare type |

!!! success "Up next ➡️"
    Now that you know how to store single values, let's learn how to store **collections** of data — Lists, Tuples, Sets, and Dictionaries!
