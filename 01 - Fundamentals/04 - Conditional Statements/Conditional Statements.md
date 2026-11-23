---
title: Conditional Statements
aliases: [if statements, if elif else, branching]
tags: [python, fundamentals, conditionals, control-flow]
category: fundamentals
level: 2
status: not-started
difficulty: beginner
created: 2026-11-23
updated: 2026-11-23
---

# Conditional Statements

## 1. Definition

Conditional statements execute different code paths based on whether a condition evaluates as truthy or falsy. Python uses `if`, `elif`, and `else`.

---

## 2. Basic Structure

```python
if condition:
    # executed if condition is truthy
elif another_condition:
    # executed if condition was falsy AND another_condition is truthy
elif yet_another:
    # additional conditions, as many as needed
else:
    # executed if no condition above was truthy
```

**Indentation defines blocks.** No braces, no `end`. Python uses 4 spaces per level (PEP 8).

```python
age = 20

if age < 18:
    print("minor")
elif age < 65:
    print("adult")
else:
    print("senior")
```

Python evaluates conditions top to bottom and executes the first matching branch only. Once a branch executes, the rest are skipped.

---

## 3. Truthiness

Any object can be used as a condition. Python evaluates it via `bool()`:

**Falsy values** (evaluate to `False` in boolean context):
```python
False
None
0        # int zero
0.0      # float zero
0j       # complex zero
""       # empty string
[]       # empty list
()       # empty tuple
{}       # empty dict
set()    # empty set
b""      # empty bytes
```

**Everything else is truthy**, including:
- Non-zero numbers: `1`, `-1`, `0.001`
- Non-empty containers: `[0]`, `{""}`, `(False,)`
- Object instances (unless `__bool__` or `__len__` returns falsy)

```python
# These are equivalent
if items:          # truthy if list is non-empty
    process(items)

if len(items) > 0:   # more verbose, same meaning
    process(items)

# Prefer the first form for readability
```

---

## 4. Common Patterns and Anti-Patterns

### Anti-pattern: Comparing to True/False explicitly

```python
# Wrong — verbose and un-Pythonic
if is_valid == True:
    ...
if is_valid == False:
    ...
if result is not None:
    if result:   # nested when you can combine

# Correct
if is_valid:
    ...
if not is_valid:
    ...
if result:   # checks both not-None and truthy in one
    ...
```

**Exception:** When you specifically need to distinguish `True` from other truthy values, use `is True`. This is rare.

### Guard clauses — reduce nesting

```python
# Deep nesting — hard to read
def process(user, data):
    if user is not None:
        if user.is_active:
            if data:
                return transform(data)
    return None

# Guard clauses — return early for invalid cases
def process(user, data):
    if user is None:
        return None
    if not user.is_active:
        return None
    if not data:
        return None
    return transform(data)
```

Guard clauses reduce nesting and make the happy path clear. This is a core pattern in Pythonic code.

### Avoid `else` after `return`/`raise`

```python
# Unnecessary else
def classify(n):
    if n > 0:
        return "positive"
    else:
        return "non-positive"

# Cleaner — else is implicit after return
def classify(n):
    if n > 0:
        return "positive"
    return "non-positive"
```

---

## 5. Nested Conditions

Avoid deep nesting. If you have more than 2 levels, refactor:

```python
# Too nested
if status == "active":
    if permissions:
        if "write" in permissions:
            if data:
                save(data)

# Better — combine with and
if status == "active" and permissions and "write" in permissions and data:
    save(data)

# Or extract to a function that checks validity
def can_save(status, permissions, data):
    return (
        status == "active"
        and permissions
        and "write" in permissions
        and bool(data)
    )

if can_save(status, permissions, data):
    save(data)
```

---

## 6. `match` / `case` — Structural Pattern Matching (Python 3.10+)

For complex branching on structure, Python 3.10 added `match`:

```python
# Python 3.10+
command = ("move", 10, 20)

match command:
    case ("move", x, y):
        print(f"Moving to {x}, {y}")
    case ("rotate", angle):
        print(f"Rotating {angle} degrees")
    case ("stop",):
        print("Stopping")
    case _:
        print("Unknown command")
```

`match` is more than a switch statement — it matches structure, type, and value simultaneously. Use it when you're branching on the shape of data, not just a simple value comparison.

---

## 7. Common Mistakes

### Checking for empty collection wrong

```python
# Fragile — only works for exact None
if data == None:   # fails if data is an empty list (False but not None)
    ...

# Correct depends on intent:
if data is None:    # was it set at all?
    ...
if not data:        # is it empty/falsy?
    ...
```

### Assignment in condition (Python 3.8+ walrus operator)

```python
# Python 3.8+
# Without walrus — computes twice
if get_data():
    process(get_data())

# With walrus — compute once, check and bind
if data := get_data():
    process(data)
```

The walrus operator (`:=`) assigns and returns the value. Use sparingly — only when it genuinely simplifies code.

---

## 8. Related Concepts

- [[Conditional Expressions]] — ternary operator
- [[Basic Operators]] — comparison and logical operators
- [[Dynamic Typing]] — truthiness explained via `bool()`

---

## 9. Exercises

### Beginner

1. Write a function `classify_number(n)` that returns `"positive"`, `"negative"`, or `"zero"`.
2. Write a function that takes a list and returns `"empty"`, `"single"`, or `"multiple"`.
3. Rewrite this with guard clauses: `if user: if user.active: if user.email: send_email(user.email)`.

### Intermediate

1. Write a function `validate_user(user: dict) -> str` that returns a specific error message for each of: missing name, empty name, missing email, invalid email (no `@`). Return `"valid"` if all checks pass. Use guard clauses.
2. Write a function that uses `match` to handle commands: `("add", item)`, `("remove", item)`, `("list",)`, and anything else.

---

## 10. Mastery Checklist

- [ ] I know all falsy values in Python
- [ ] I write `if items:` not `if len(items) > 0:` or `if items != []:`
- [ ] I use guard clauses to reduce nesting
- [ ] I avoid unnecessary `else` after `return`
- [ ] I never write `if x == True:` or `if x == False:`
- [ ] I know when to use the walrus operator (`:=`)
