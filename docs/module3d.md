---
icon: lucide/type
---

# Module 3d: Type Conversion & Strings

> 🔤 Data doesn't always come in the shape you need. Sometimes a number arrives as text, or you need to combine values into a sentence. This module covers how to **reshape data** and **work with text** like a pro.

---

## 🔀 Type Conversion

Sometimes you need to convert data from one type to another. Python gives you two ways to do this.

### Implicit Conversion — Python Does It Automatically

When you mix compatible types, Python quietly converts one to match the other:

```python
num_int = 5
num_float = 2.5

result = num_int + num_float   # Python auto-converts int → float
print(result)          # 7.5
print(type(result))    # <class 'float'>
```

Python always converts to the **more precise type** — int becomes float, not the other way around.

### Explicit Conversion — You Do It Manually

When types are incompatible, you have to convert manually using built-in functions:

```python
# String → int
str_num = "10"
result = int(str_num) + 5
print(result)   # 15

# int → string (for joining text)
greeting = "The answer is "
answer = 42
print(greeting + str(answer))   # "The answer is 42"

# String → float
str_price = "99.99"
price = float(str_price)
print(price)    # 99.99

# int → bool
print(bool(0))    # False — 0 is always False
print(bool(1))    # True  — any non-zero is True
print(bool(-5))   # True

# String repetition — Python allows int * string
print(3 * "ha ")   # "ha ha ha "
```

| Function | Converts to | Example |
|---|---|---|
| `int()` | Integer | `int("10")` → `10` |
| `float()` | Float | `float("3.14")` → `3.14` |
| `str()` | String | `str(42)` → `"42"` |
| `bool()` | Boolean | `bool(0)` → `False` |
| `list()` | List | `list((1,2,3))` → `[1,2,3]` |
| `tuple()` | Tuple | `tuple([1,2,3])` → `(1,2,3)` |

!!! warning "You can't add a string and a number directly 🚫"
    ```python
    print(5 + "hello")   # TypeError: unsupported operand type(s)
    ```
    Always convert first:
    ```python
    print(str(5) + " hello")   # "5 hello" ✅
    ```

!!! warning "Not every string can be converted to a number 🚫"
    ```python
    int("hello")    # ValueError — "hello" is not a number
    int("10.5")     # ValueError — use float() for decimals
    float("10.5")   # 10.5 ✅
    ```

!!! example "GIS real-world example 🗺️"
    ```python
    # CSV data often comes in as strings — need to convert
    lat_str = "28.6139"
    lon_str = "77.2090"

    latitude = float(lat_str)
    longitude = float(lon_str)

    print(f"Coordinates: ({latitude}, {longitude})")
    # Coordinates: (28.6139, 77.209)
    ```

---

## 🔤 String Manipulation

Strings are one of the most used data types in Python. Whether you're reading file paths, processing names, or formatting output — you'll work with strings constantly.

### Indexing & Slicing

Strings work just like lists — each character has an index:

```python
my_string = "Python Programming"
#            0123456789...

# Indexing
print(my_string[0])     # P  — first character
print(my_string[6])     # (space)
print(my_string[-1])    # g  — last character
print(my_string[-11])   # P  — 11th from end

# Slicing [start:end:step]
print(my_string[0:6])    # Python
print(my_string[7:])     # Programming
print(my_string[:6])     # Python
print(my_string[::2])    # Pto rgrimn — every 2nd character
print(my_string[::-1])   # gnimmargorP nohtyP — reversed!
```

### Common String Methods

```python
text = "  Hello, World! This is Python.   "

# Case conversion
print(text.upper())       # "  HELLO, WORLD! THIS IS PYTHON.   "
print(text.lower())       # "  hello, world! this is python.   "
print(text.title())       # "  Hello, World! This Is Python.   "
print(text.capitalize())  # "  hello, world! this is python.   " (only first char)

# Whitespace removal
print(text.strip())       # "Hello, World! This is Python."  — both ends
print(text.lstrip())      # "Hello, World! This is Python.   " — left only
print(text.rstrip())      # "  Hello, World! This is Python." — right only

# Split & Join
sentence = "Python is fun and powerful"
words = sentence.split(" ")           # ['Python', 'is', 'fun', 'and', 'powerful']
joined = "-".join(words)              # 'Python-is-fun-and-powerful'
csv_line = ",".join(["Alice", "30", "Mumbai"])  # 'Alice,30,Mumbai'

# Replace
msg = "I like apples. Apples are great."
print(msg.replace("apples", "mangoes"))
# "I like mangoes. Apples are great." — only replaces lowercase match

# Find & Count
print("Programming".find("gram"))    # 3  — index where it starts
print("Programming".count("m"))      # 2  — how many times 'm' appears

# Checking content
print("Python".startswith("Py"))     # True
print("Python".endswith("on"))       # True
print("gra" in "Programming")        # True
print("123".isdigit())               # True
print("Hello".isalpha())             # True
print("Hello123".isalnum())          # True
print("   ".isspace())               # True
```

!!! tip "Strings are immutable 🔒"
    String methods **don't change** the original string — they return a new one.

    ```python
    name = "alice"
    name.upper()        # returns "ALICE" but name is still "alice"
    name = name.upper() # now name is "ALICE" ✅
    ```

### f-Strings — The Modern Way to Format

f-strings (introduced in Python 3.6) are the cleanest way to embed values inside strings. Just put `f` before the quote and wrap variables in `{}`.

```python
name = "Alice"
age = 30
price = 1200.50
quantity = 2

# Basic embedding
print(f"Hello, {name}! You are {age} years old.")
# Hello, Alice! You are 30 years old.

# Expressions inside {}
total = price * quantity
print(f"Total cost: ${price * quantity}")          # no rounding
print(f"Total cost: ${price * quantity:.2f}")      # 2 decimal places → $2401.00

# Number formatting
pi = 3.14159265
print(f"Pi = {pi:.2f}")     # Pi = 3.14
print(f"Pi = {pi:.4f}")     # Pi = 3.1416
print(f"Count: {1000000:,}")  # Count: 1,000,000 — comma separator

# Dictionary values inside f-strings
city = {"name": "Mumbai", "temp": 32.567}
print(f"Weather in {city['name']}: {city['temp']:.1f}°C")
# Weather in Mumbai: 32.6°C

# Alignment & padding
item = "Book"
cost = 25.99
print(f"{'Item:':<10} {item}")        # left-aligned, 10 chars wide
print(f"{'Price:':<10} ${cost:.2f}")  # right-aligned float

# Date formatting
import datetime
now = datetime.datetime.now()
print(f"Today: {now:%Y-%m-%d}")           # Today: 2026-04-19
print(f"Time: {now:%H:%M:%S}")            # Time: 14:30:00
print(f"Full: {now:%d %B %Y, %I:%M %p}") # Full: 19 April 2026, 02:30 PM
```

!!! tip "f-string formatting cheatsheet 📋"
    | Format | Meaning | Example |
    |---|---|---|
    | `:.2f` | 2 decimal places | `3.14` |
    | `:,` | Comma separator | `1,000,000` |
    | `:<10` | Left-align, 10 wide | `"Alice     "` |
    | `:>10` | Right-align, 10 wide | `"     Alice"` |
    | `:^10` | Center, 10 wide | `"  Alice   "` |
    | `:%Y-%m-%d` | Date format | `2026-04-19` |

### Older Formatting Methods (You'll See These in Old Code)

```python
name = "Alice"
age = 30

# % formatting (old style — Python 2 era)
print("Hello %s, you are %d years old." % (name, age))

# .format() method (Python 3, before f-strings)
print("Hello {}, you are {} years old.".format(name, age))
print("Hello {name}, you are {age} years old.".format(name=name, age=age))

# f-string (modern — use this!)
print(f"Hello {name}, you are {age} years old.")
```

!!! info "Which one should you use? 🤔"
    Always use **f-strings** for new code. They're the most readable and fastest.
    You'll see `%` and `.format()` in older codebases — now you know what they mean.

---

## 🗂️ Accessing Nested Data

Real-world data is often deeply nested — dicts inside lists, lists inside dicts. Here's how to drill down:

```python
student = {
    "student_id": "S1001",
    "personal_info": {
        "first_name": "Alice",
        "contact": {
            "email": "alice@example.edu",
            "address": {
                "city": "Anytown",
                "zip_code": "12345"
            }
        }
    },
    "academic_records": [
        {"course_code": "CS101", "grade": "A"},
        {"course_code": "MA201", "grade": "B+"},
        {"course_code": "CS205", "grade": "A-"}
    ]
}

# Top-level key
print(student["student_id"])                                       # S1001

# Nested dict
print(student["personal_info"]["first_name"])                      # Alice

# Deeply nested
print(student["personal_info"]["contact"]["email"])                # alice@example.edu
print(student["personal_info"]["contact"]["address"]["city"])      # Anytown

# List inside dict — use index
print(student["academic_records"][0]["course_code"])               # CS101
print(student["academic_records"][1]["grade"])                     # B+

# Loop through list of dicts
for course in student["academic_records"]:
    print(f"{course['course_code']}: {course['grade']}")

# Safe access — no crash if key missing
middle_name = student["personal_info"].get("middle_name", "N/A")
print(middle_name)   # N/A
```

!!! tip "Always use `.get()` for keys that might not exist 🛡️"
    In real data, fields are often missing. `.get("key", default)` saves you from crashes.

---

## 🎯 Quick Recap

| Concept | What it does | Example |
|---|---|---|
| **Type conversion** | Change data type | `int("10")`, `str(42)`, `float("3.14")` |
| **Implicit conversion** | Python auto-converts | `5 + 2.5` → `7.5` (float) |
| **String indexing** | Access a character | `"Python"[0]` → `"P"` |
| **String slicing** | Extract a substring | `"Python"[0:3]` → `"Pyt"` |
| **`.upper()` / `.lower()`** | Change case | `"hello".upper()` → `"HELLO"` |
| **`.strip()`** | Remove whitespace | `"  hi  ".strip()` → `"hi"` |
| **`.split()`** | Split into list | `"a,b,c".split(",")` → `["a","b","c"]` |
| **`.join()`** | Join list into string | `",".join(["a","b"])` → `"a,b"` |
| **`.replace()`** | Replace text | `"hi".replace("hi","bye")` → `"bye"` |
| **f-string** | Modern formatting | `f"Hello {name}"` |

!!! success "Module 3 Complete! 🎉"
    You now know how to store data (variables), organize it (data structures), operate on it (operators), convert it (type conversion), and format it (strings).

    These are the **atoms of Python**. Everything else — loops, functions, AI, GIS tools — is built on top of exactly this. Next up: **Control Flow** — making your code think and decide!
