---
icon: lucide/shield-alert
---

# Module 6: Error Handling — The Safety Net

> 💥 Every program will eventually break. A file goes missing. A user types letters where you expected numbers. The internet drops. The question isn't *if* your code will fail — it's *how gracefully* it fails. That's what error handling is for.

---

## 🤔 What Are Errors (Exceptions)?

When something goes wrong in Python, it raises an **exception** — a signal that says "I can't continue, something unexpected happened."

Without error handling, your entire program crashes with a scary red message. With error handling, you catch the problem, deal with it, and keep going.

!!! info "Real-world analogy 🚗"
    Think of error handling like a **car's safety systems**:

    - The engine warning light (exception) tells you something's wrong
    - The airbag (try/except) catches the impact so you don't get hurt
    - The seatbelt (finally) always does its job regardless of what happens

    Without these, a small problem becomes a catastrophe.

---

## 🔴 Common Python Exceptions

Before handling errors, let's know what they look like:

| Exception | When it happens | Example |
|---|---|---|
| `ValueError` | Wrong value type | `int("hello")` |
| `TypeError` | Wrong data type | `"5" + 5` |
| `ZeroDivisionError` | Dividing by zero | `10 / 0` |
| `FileNotFoundError` | File doesn't exist | `open("missing.txt")` |
| `KeyError` | Dict key doesn't exist | `d["missing_key"]` |
| `IndexError` | List index out of range | `lst[999]` |
| `NameError` | Variable not defined | `print(undefined_var)` |
| `AttributeError` | Object has no such method | `"hello".push("!")` |

---

## 1️⃣ `try` and `except` — The Basic Safety Net

The `try` block contains code that **might fail**. The `except` block handles the failure **if it happens**.

```python
# Without error handling — program crashes
# result = 10 / 0   # ZeroDivisionError: division by zero 💥

# With error handling — program survives
def divide_numbers(numerator, denominator):
    try:
        result = numerator / denominator
        print(f"{numerator} / {denominator} = {result}")
    except ZeroDivisionError:
        print("❌ Error: Cannot divide by zero!")

divide_numbers(10, 2)   # 10 / 2 = 5.0
divide_numbers(5, 0)    # ❌ Error: Cannot divide by zero!
```

```python
def safe_int_conversion(value):
    try:
        num = int(value)
        print(f"✅ Converted '{value}' to integer: {num}")
    except ValueError:
        print(f"❌ Cannot convert '{value}' to an integer.")

safe_int_conversion("123")    # ✅ Converted '123' to integer: 123
safe_int_conversion("hello")  # ❌ Cannot convert 'hello' to an integer.
```

!!! info "Real-world analogy 🧪"
    `try/except` is like a **science experiment with safety goggles**:

    - `try` = run the experiment
    - `except` = if something explodes, the goggles protect you

    Without goggles (error handling), one explosion ends the whole lab session.

---

## 2️⃣ Handling Multiple Exceptions

Catch different errors differently — don't treat all problems the same way:

```python
data = [1, 2, "three", 4]

for item in data:
    try:
        result = item / 2
        print(f"Result for {item}: {result}")
    except TypeError:
        print(f"⚠️ Skipping '{item}' — it's not a number.")
    except ZeroDivisionError:
        print(f"⚠️ Cannot divide {item} by zero.")
    except Exception as e:
        print(f"❌ Unexpected error for {item}: {e}")

# Result for 1: 0.5
# Result for 2: 1.0
# ⚠️ Skipping 'three' — it's not a number.
# Result for 4: 2.0
```

!!! tip "Catch specific exceptions first, generic last 🎯"
    Always put specific exceptions (`ValueError`, `FileNotFoundError`) before the generic `Exception`. Python checks them top to bottom — the first match wins.

    ```python
    try:
        ...
    except ValueError:       # specific — checked first
        ...
    except FileNotFoundError: # specific
        ...
    except Exception as e:   # generic catch-all — always last
        ...
    ```

---

## 3️⃣ The `else` Block — When Everything Goes Right

The `else` block runs **only if no exception was raised** in the `try` block:

```python
def process_file(filename):
    try:
        with open(filename, "r") as f:
            content = f.read()
    except FileNotFoundError:
        print(f"❌ File '{filename}' not found.")
    else:
        # Only runs if the file opened successfully
        print(f"✅ File read successfully! ({len(content)} characters)")

process_file("missing_file.txt")   # ❌ File 'missing_file.txt' not found.

# Create a file and try again
with open("my_data.txt", "w") as f:
    f.write("Hello, this is test data.")

process_file("my_data.txt")   # ✅ File read successfully! (25 characters)
```

!!! info "Real-world analogy ✈️"
    `else` is like the **"all clear" signal** at an airport security check:

    - `try` = go through the scanner
    - `except` = if the alarm goes off, handle it
    - `else` = if no alarm, proceed to the gate ✅

---

## 4️⃣ The `finally` Block — Always Runs, No Matter What

`finally` runs **regardless** of whether an exception happened or not. Perfect for cleanup — closing files, releasing connections, logging.

```python
def try_finally_example(value):
    try:
        num = int(value)
        result = 10 / num
        print(f"Result: {result}")
    except ValueError:
        print("❌ Caught ValueError — not a valid number!")
    except ZeroDivisionError:
        print("❌ Caught ZeroDivisionError — can't divide by zero!")
    finally:
        print("🧹 Cleanup done (finally always runs)\n")

try_finally_example("5")    # Result: 2.0  → cleanup
try_finally_example("abc")  # ValueError  → cleanup
try_finally_example("0")    # ZeroDivision → cleanup
```

!!! info "Real-world analogy 🏥"
    `finally` is like a **hospital's post-surgery cleanup**. Whether the surgery went perfectly or had complications, the team always cleans the operating room afterwards. No exceptions (pun intended).

---

## 5️⃣ The Full Structure

```
try:
    ┌─────────────────────────────┐
    │  Code that might fail       │
    └─────────────────────────────┘
except SpecificError:
    ┌─────────────────────────────┐
    │  Handle that specific error │
    └─────────────────────────────┘
except AnotherError:
    ┌─────────────────────────────┐
    │  Handle another error       │
    └─────────────────────────────┘
except Exception as e:
    ┌─────────────────────────────┐
    │  Catch-all for anything     │
    │  else (use sparingly)       │
    └─────────────────────────────┘
else:
    ┌─────────────────────────────┐
    │  Runs ONLY if no error      │
    └─────────────────────────────┘
finally:
    ┌─────────────────────────────┐
    │  ALWAYS runs — cleanup here │
    └─────────────────────────────┘
```

---

## 6️⃣ Raising Exceptions — Throwing Your Own Errors

Sometimes *you* want to raise an error when something invalid happens in your code:

```python
def validate_age(age):
    if not isinstance(age, (int, float)) or age < 0:
        raise ValueError("Age must be a non-negative number.")
    elif age < 18:
        print("User is a minor.")
    else:
        print("User is an adult.")

try:
    validate_age(25)    # User is an adult.
    validate_age(15)    # User is a minor.
    validate_age(-5)    # ❌ raises ValueError
except ValueError as e:
    print(f"Validation error: {e}")

try:
    validate_age("twenty")   # ❌ raises ValueError
except ValueError as e:
    print(f"Validation error: {e}")
```

!!! info "Real-world analogy 🚨"
    `raise` is like a **security guard rejecting entry**. The guard (your function) checks the rules, and if something's wrong, they raise the alarm (`raise ValueError`) instead of letting the problem sneak through.

!!! example "GIS real-world example 🗺️"
    ```python
    def load_shapefile(path):
        if not path.endswith(".shp"):
            raise ValueError(f"Expected a .shp file, got: {path}")
        if not os.path.exists(path):
            raise FileNotFoundError(f"Shapefile not found: {path}")
        # proceed to load...
        print(f"✅ Loading {path}")

    try:
        load_shapefile("roads.csv")
    except ValueError as e:
        print(f"❌ {e}")
    # ❌ Expected a .shp file, got: roads.csv
    ```

---

## 🎯 Quick Recap

| Block | When it runs | Use for |
|---|---|---|
| `try` | Always — contains risky code | Code that might fail |
| `except` | Only if an error occurs | Handle specific errors |
| `else` | Only if NO error occurred | Code that needs successful try |
| `finally` | Always, no matter what | Cleanup — close files, connections |
| `raise` | When you want to trigger an error | Input validation, enforcing rules |

!!! success "Up next ➡️"
    Now that your code can handle failures, let's learn how to make it **faster** — running multiple tasks at the same time with Multithreading, AsyncIO & Multiprocessing!
