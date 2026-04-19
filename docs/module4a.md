---
icon: lucide/git-branch
---

# Module 4a: Control Flow — If, Elif & Else

> 🚦 So far your code runs top to bottom, every single line, every single time. But real programs need to **make decisions**. Should I show this page or that one? Is the user an admin or a guest? That's what control flow is for.

---

## 🤔 What is Control Flow?

Control flow is how your program decides **which lines of code to run** based on conditions.

Without it, your program is like a train on a single track — it goes straight and never turns. With control flow, it's like a GPS — it takes different routes depending on the situation.

!!! info "Real-world analogy 🚦"
    Think of a traffic light:

    - **If** the light is green → Go
    - **Elif** the light is yellow → Slow down
    - **Else** → Stop

    Your code works exactly the same way.

---

## 1️⃣ The `if` Statement — Basic Decision

The `if` statement runs a block of code **only if** the condition is `True`.

```python
temperature = 25

if temperature > 20:
    print("It's a warm day!")
# Output: It's a warm day!
```

```python
is_raining = True

if is_raining:
    print("Remember to take an umbrella.")
# Output: Remember to take an umbrella.
```

!!! tip "Indentation matters! 🔑"
    Python uses **indentation** (4 spaces) to define what's inside the `if` block. No curly braces like other languages — just indent.

    ```python
    if condition:
        # this runs if condition is True
        # this too
    # this runs regardless — it's outside the if block
    ```

---

## 2️⃣ The `else` Statement — The Fallback

`else` runs when the `if` condition is `False`. It's the "otherwise" path.

```python
age = 17

if age >= 18:
    print("You are eligible to vote.")
else:
    print("You are not yet eligible to vote.")
# Output: You are not yet eligible to vote.
```

```python
has_license = False

if has_license:
    print("You can drive.")
else:
    print("You need a license to drive.")
# Output: You need a license to drive.
```

---

## 3️⃣ The `elif` Statement — Multiple Choices

`elif` (short for "else if") lets you check **multiple conditions** in sequence. Python checks them top to bottom and runs the **first one that's True**, then skips the rest.

```python
score = 85

if score >= 90:
    print("Grade: A")
elif score >= 80:
    print("Grade: B")
elif score >= 70:
    print("Grade: C")
else:
    print("Grade: F")
# Output: Grade: B
```

```python
user_role = "admin"

if user_role == "guest":
    print("Welcome, Guest! Limited access.")
elif user_role == "user":
    print("Welcome, User! Standard access.")
elif user_role == "admin":
    print("Welcome, Admin! Full access.")
else:
    print("Unknown role. Access denied.")
# Output: Welcome, Admin! Full access.
```

---

## ⚠️ Order of `elif` Matters!

Python checks conditions **top to bottom** and stops at the first `True`. If you put conditions in the wrong order, you'll get wrong results.

=== "✅ Correct Order"
    ```python
    score = 85

    if score >= 90:
        print("Grade A")
    elif score >= 80:
        print("Grade B")   # ✅ This runs correctly
    elif score >= 70:
        print("Grade C")
    else:
        print("Grade F")
    ```

=== "❌ Wrong Order"
    ```python
    score = 85

    if score >= 70:
        print("Grade C")   # ❌ This runs first — wrong!
    elif score >= 80:
        print("Grade B")   # Never reached
    elif score >= 90:
        print("Grade A")   # Never reached
    else:
        print("Grade F")
    ```

!!! warning "Always go from most specific to least specific ⚠️"
    Put the **strictest/highest** condition first. If you check `>= 70` before `>= 80`, a score of 85 will match the first condition and never reach the correct one.

---

## 🔗 Combining Conditions with Logical Operators

You can combine multiple conditions using `and`, `or`, `not`:

```python
age = 20
has_id = True

if age >= 18 and has_id:
    print("You can enter the club.")
elif age < 18:
    print("You are too young to enter.")
else:
    print("You need to show your ID to enter.")
# Output: You can enter the club.
```

```python
is_sunny = False
is_warm = True

if is_sunny or is_warm:
    print("It's a pleasant day, maybe go outside.")
else:
    print("Stay indoors.")
# Output: It's a pleasant day, maybe go outside.
```

```python
is_logged_in = False

if not is_logged_in:
    print("Please log in to access this feature.")
else:
    print("You are logged in.")
# Output: Please log in to access this feature.
```

!!! example "GIS real-world example 🗺️"
    ```python
    elevation = 1500
    is_protected_zone = False
    land_type = "forest"

    if elevation > 2000:
        print("Too high — no construction allowed.")
    elif is_protected_zone:
        print("Protected zone — no construction allowed.")
    elif land_type == "forest" and elevation > 1000:
        print("Forest at high elevation — restricted construction.")
    else:
        print("Construction permitted.")
    # Output: Forest at high elevation — restricted construction.
    ```

---

## 🪆 Nested If/Else — Decisions Inside Decisions

Sometimes you need to make a decision *inside* another decision. That's nesting.

```python
weather = "sunny"
temp = 28

if weather == "sunny":
    print("It's sunny today.")
    if temp > 25:
        print("It's also very hot! Wear light clothes.")
    else:
        print("It's a pleasant temperature.")
elif weather == "rainy":
    print("It's rainy today. Don't forget your umbrella.")
else:
    print("The weather is cloudy.")

# Output:
# It's sunny today.
# It's also very hot! Wear light clothes.
```

!!! tip "Don't nest too deep 🪆"
    Nesting more than 2-3 levels deep makes code hard to read. If you find yourself deeply nesting, consider restructuring with `elif` or functions.

---

## ⚡ Shorthand If — The One-Liner (Ternary Operator)

For simple conditions, Python lets you write `if-else` in a single line:

```python
# Syntax: value_if_true if condition else value_if_false

age = 20
status = "Adult" if age >= 18 else "Minor"
print(f"Status: {status}")
# Output: Status: Adult
```

```python
# Directly in a print
light_color = "green"
print("Go" if light_color == "green" else "Stop")
# Output: Go
```

```python
# In a function return
def get_max(a, b):
    return a if a > b else b

print(get_max(15, 25))   # 25
```

!!! tip "When to use the one-liner 🤔"
    Use it for **simple, readable** conditions. If the logic is complex, stick to the full `if/else` — readability always wins.

---

## ⌨️ Taking User Input — `input()`

The `input()` function lets your program **receive data from the user**. It always returns a string.

```python
name = input("Please enter your name: ")
print(f"Hello, {name}!")
```

```python
# Always convert input to the right type
age_str = input("Please enter your age: ")

try:
    age = int(age_str)   # convert string → int
    print(f"You are {age} years old.")
    if age >= 18:
        print("You are an adult.")
    else:
        print("You are a minor.")
except ValueError:
    print("Invalid input. Please enter a valid number for age.")
```

!!! warning "input() always returns a string ⚠️"
    ```python
    age = input("Enter age: ")   # user types 25
    print(age + 1)               # ❌ TypeError — "25" + 1 doesn't work
    print(int(age) + 1)          # ✅ 26
    ```
    Always convert with `int()`, `float()` etc. when you need a number.

---

## 🎯 Quick Recap

| Statement | What it does | When to use |
|---|---|---|
| `if` | Runs block if condition is True | Single condition check |
| `else` | Runs if `if` was False | Fallback / default action |
| `elif` | Checks another condition | Multiple possible outcomes |
| `and` / `or` / `not` | Combine conditions | Complex multi-condition logic |
| Ternary `x if c else y` | One-line if/else | Simple value assignment |
| `input()` | Get data from user | Interactive programs |

!!! success "Up next ➡️"
    Now that your code can make decisions, let's teach it to **repeat tasks** — Loops!
