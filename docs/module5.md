---
icon: lucide/wrench
---

# Module 5: Functions — Write Once, Use Forever

> ♻️ Imagine if every time you wanted a cup of tea, you had to grow the tea plant, harvest the leaves, build a kettle, and boil water from scratch. That's what coding without functions feels like. Functions let you **package a task once and reuse it forever**.

---

## 🤔 What is a Function?

A function is a **reusable block of code** that does one specific job. You define it once, and then "call" it whenever you need it — from anywhere in your program.

!!! info "Real-world analogy ☕"
    Think of a **coffee machine**:

    - You give it **inputs** (coffee beans, water)
    - It does all the work **inside** (grind, heat, brew)
    - It gives you an **output** (coffee)

    You don't care *how* it works inside. You just press the button and get coffee.

    Functions work exactly the same way — you give inputs (arguments), it does the work, and gives you an output (return value).

---

## 🎯 Why Do We Need Functions?

| Problem without functions | How functions solve it |
|---|---|
| Copy-pasting the same 10 lines everywhere | Write once, call anywhere |
| One bug means fixing it in 50 places | Fix it once inside the function |
| 500-line scripts that are impossible to read | Break into small named chunks |
| Hard to understand what code does | `send_email()` tells you exactly what it does |

!!! example "Real-world analogy 🏭"
    A car factory doesn't build each car from raw iron ore every time. They have **machines** (functions) — one welds the frame, one paints, one installs the engine. Each machine does one job, and the factory calls them in sequence.

    Your program is the factory. Functions are the machines.

---

## ✏️ Defining & Calling a Function

```python
# Define the function
def greet():
    print("Hello, World!")

# Call it — as many times as you want
greet()   # Hello, World!
greet()   # Hello, World!
```

The anatomy of a function:

```
def  function_name  (parameters):
 ↑        ↑               ↑
keyword  your name    inputs (optional)
    indented body goes here
    return value (optional)
```

---

## 📥 Parameters & Arguments

**Parameters** are the inputs your function expects. **Arguments** are the actual values you pass when calling it.

### Positional Arguments — Order Matters

```python
def greet_person(name):
    print(f"Hello, {name}!")

greet_person("Alice")   # Hello, Alice!
greet_person("Bob")     # Hello, Bob!
```

```python
def add_numbers(a, b):
    print(f"{a} + {b} = {a + b}")

add_numbers(5, 3)    # 5 + 3 = 8
add_numbers(10, 20)  # 10 + 20 = 30
```

### Keyword Arguments — Order Doesn't Matter

```python
def describe_pet(animal_type, pet_name):
    print(f"I have a {animal_type} named {pet_name}.")

describe_pet(animal_type="dog", pet_name="Buddy")
describe_pet(pet_name="Whiskers", animal_type="cat")   # order swapped — still works!
```

### Default Parameters — Optional Inputs

```python
def greet(name="Guest", message="Hello"):
    print(f"{message}, {name}!")

greet()                          # Hello, Guest!      — both defaults used
greet("Charlie")                 # Hello, Charlie!    — name overridden
greet(message="Hi", name="Dave") # Hi, Dave!          — both overridden
```

!!! tip "Real-world analogy 🍕"
    Default parameters are like a pizza order form. The default is "medium size, regular crust". If you don't specify, you get the default. If you want something different, you say so.

---

## 📤 Return Values

Functions can **send back a result** using `return`. Without `return`, a function just does something but gives nothing back.

```python
def multiply(x, y):
    result = x * y
    return result

product = multiply(4, 6)
print(f"Product: {product}")   # Product: 24
print(multiply(10, 20))        # 200 — use directly
```

### Returning Multiple Values

```python
def get_name_parts(full_name):
    parts = full_name.split()
    first = parts[0]
    last = parts[-1]
    return first, last   # returns a tuple

first_name, last_name = get_name_parts("John Doe")
print(f"First: {first_name}, Last: {last_name}")
# First: John, Last: Doe
```

!!! info "Real-world analogy 🧾"
    `return` is like a **receipt from a shop**. The shop (function) does the work, and hands you back a result (receipt). Without `return`, the function is like a shop that does the work but throws the receipt in the bin.

---

## 🌍 Variable Scope — Where Variables Live

**Scope** defines where a variable is visible and accessible. Think of it like rooms in a house.

!!! info "Real-world analogy 🏠"
    - A **local variable** is like something in your bedroom — only you can access it in that room.
    - A **global variable** is like the TV in the living room — everyone in the house can see it.

```python
global_var = "I am global"   # visible everywhere

def my_function():
    local_var = "I am local"   # only visible inside this function
    print(global_var)          # ✅ can access global from inside
    print(local_var)           # ✅ can access local from inside

my_function()
print(global_var)   # ✅ accessible outside
# print(local_var)  # ❌ NameError — local_var doesn't exist here
```

### The LEGB Rule — Python's Scope Order

Python looks for a variable in this order:

```
L → Local       (inside the current function)
E → Enclosing   (inside any outer function)
G → Global      (at the top of the file)
B → Built-in    (Python's built-in names like print, len)
```

```python
global_message = "I am global"

def outer_function():
    enclosing_message = "I am enclosing"

    def inner_function():
        local_message = "I am local"
        print(local_message)      # L — local
        print(enclosing_message)  # E — enclosing
        print(global_message)     # G — global
        print(len("test"))        # B — built-in

    inner_function()

outer_function()
```

### Modifying a Global Variable

```python
counter = 0

def increment():
    global counter       # declare you want to modify the global
    counter += 1

increment()
increment()
print(counter)   # 2
```

!!! warning "Use `global` sparingly ⚠️"
    Modifying global variables from inside functions makes code hard to debug. Prefer passing values as arguments and returning results instead.

---

## 🎒 `*args` and `**kwargs` — Flexible Inputs

Sometimes you don't know how many arguments a function will receive. Python handles this elegantly.

### `*args` — Any Number of Positional Arguments

```python
def my_sum(*numbers):
    total = 0
    for num in numbers:
        total += num
    return total

print(my_sum(1, 2, 3))          # 6
print(my_sum(10, 20, 30, 40))   # 100
print(my_sum())                 # 0
```

!!! tip "Real-world analogy 🛒"
    `*args` is like a shopping bag — you can put as many items as you want in it. The function doesn't care how many, it just loops through whatever you gave it.

### `**kwargs` — Any Number of Keyword Arguments

```python
def print_user_info(**user_data):
    print("User Info:")
    for key, value in user_data.items():
        print(f"  {key}: {value}")

print_user_info(name="Alice", age=30, city="Mumbai")
print_user_info(product="Laptop", price=1200, brand="Dell")
```

!!! tip "Real-world analogy 📋"
    `**kwargs` is like a form where you can fill in any fields you want. The function accepts whatever key-value pairs you provide.

### The Full Order — Combining All Parameter Types

```python
# Order MUST be: positional → default → *args → keyword-only → **kwargs
def complex_function(a, b=10, *args, kw_only1, kw_only2=None, **kwargs):
    print(f"a: {a}")
    print(f"b (default 10): {b}")
    print(f"extra positional args: {args}")
    print(f"kw_only1 (required): {kw_only1}")
    print(f"kw_only2 (optional): {kw_only2}")
    print(f"extra keyword args: {kwargs}")

# Minimal call
complex_function(1, kw_only1="hello")

# Full call
complex_function(1, 2, 3, 4, kw_only1="world", kw_only2="test", color="red", size="large")
```

| Parameter type | Syntax | Example |
|---|---|---|
| Positional | `a` | `func(1)` |
| Default | `b=10` | `func(1, 20)` or `func(1)` |
| Extra positional | `*args` | `func(1, 2, 3, 4)` |
| Keyword-only | after `*args` | `func(1, key="val")` |
| Extra keyword | `**kwargs` | `func(1, x=1, y=2)` |

---

## 📌 Constants — Variables That Shouldn't Change

Python doesn't have true constants, but the convention is **ALL_CAPS** names to signal "don't change this":

```python
PI = 3.14159
MAX_RETRIES = 5
DATABASE_HOST = "localhost"

def calculate_circumference(radius):
    return 2 * PI * radius

print(calculate_circumference(5))   # 31.4159
```

!!! info "Real-world analogy 🚧"
    Constants are like **road signs** — they're there to inform everyone of a fixed rule. Python won't stop you from changing them (like someone could technically move a sign), but the convention says you shouldn't.

---

## 📝 Docstrings — Document Your Functions

Always document what your function does, what it takes, and what it returns:

```python
def calculate_area(length, width):
    """
    Calculates the area of a rectangle.

    Args:
        length (float): The length of the rectangle.
        width (float): The width of the rectangle.

    Returns:
        float: The area of the rectangle.
    """
    return length * width

# Access the docstring
print(calculate_area.__doc__)

area = calculate_area(5.0, 10.0)
print(f"Area: {area}")   # Area: 50.0
```

!!! tip "Why bother? 🤔"
    Six months from now, you won't remember what `calc_a(l, w)` does. Your future self (and teammates) will thank you for a good docstring.

---

## ⚡ Lambda Functions — One-Line Anonymous Functions

Lambda functions are tiny, throwaway functions for quick tasks. No `def`, no name, just logic.

```python
# Syntax: lambda arguments: expression

add_ten = lambda x: x + 10
print(add_ten(5))   # 15

multiply = lambda a, b: a * b
print(multiply(7, 3))   # 21
```

### Lambda with `map()` — Apply to Every Item

```python
numbers = [1, 2, 3, 4, 5]

squared = list(map(lambda x: x**2, numbers))
print(squared)   # [1, 4, 9, 16, 25]
```

### Lambda with `filter()` — Keep Only Matching Items

```python
numbers = [1, 2, 3, 4, 5, 6]

evens = list(filter(lambda x: x % 2 == 0, numbers))
print(evens)   # [2, 4, 6]
```

### Lambda with `sorted()` — Custom Sort Key

```python
people = [
    {"name": "Amit", "age": 25},
    {"name": "Ravi", "age": 20},
    {"name": "Neha", "age": 30}
]

sorted_people = sorted(people, key=lambda p: p["age"])
print(sorted_people)
# [{'name': 'Ravi', 'age': 20}, {'name': 'Amit', 'age': 25}, {'name': 'Neha', 'age': 30}]
```

### Lambda with if-else

```python
check_even_odd = lambda num: "Even" if num % 2 == 0 else "Odd"
print(check_even_odd(4))   # Even
print(check_even_odd(7))   # Odd
```

!!! info "Lambda vs `def` — when to use which? 🤔"
    | Use `lambda` when... | Use `def` when... |
    |---|---|
    | One-liner, used once | More than one line of logic |
    | Passed as argument to `map`, `filter`, `sorted` | Needs a docstring |
    | Quick throwaway logic | Reused in multiple places |

---

## 🎯 Quick Recap

| Concept | What it does | Example |
|---|---|---|
| `def` | Define a function | `def greet():` |
| Parameters | Inputs the function expects | `def add(a, b):` |
| Arguments | Values passed when calling | `add(5, 3)` |
| `return` | Send back a result | `return a + b` |
| Default params | Optional inputs with fallback | `def greet(name="Guest"):` |
| `*args` | Any number of positional args | `def sum(*nums):` |
| `**kwargs` | Any number of keyword args | `def info(**data):` |
| Scope (LEGB) | Where variables are visible | Local → Enclosing → Global → Built-in |
| Constants | Variables that shouldn't change | `MAX_SIZE = 100` |
| Docstring | Document your function | `"""Does X, takes Y, returns Z"""` |
| Lambda | One-line anonymous function | `lambda x: x * 2` |

!!! success "Up next ➡️"
    Functions can fail — files go missing, users type wrong input, networks drop. Next up: **Error Handling** — making your code survive the real world!
