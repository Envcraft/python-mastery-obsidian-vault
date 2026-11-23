---
title: None
aliases: [NoneType, null, nil]
tags: [python, fundamentals, none, null]
category: fundamentals
level: 1
status: not-started
difficulty: beginner
created: 2026-11-23
updated: 2026-11-23
---

# None

## 1. Definition

`None` is Python's null value. It is a singleton object of type `NoneType`. There is exactly one `None` object in any Python process.

---

## 2. What None Is

```python
print(type(None))    # <class 'NoneType'>
print(id(None))      # always the same id — it's a singleton

x = None
y = None
print(x is y)        # True — same object
```

`None` represents the absence of a value. It is not:
- `0` (zero is a number)
- `""` (empty string is a string)
- `[]` (empty list is a list)
- `False` (a boolean)

It is specifically the absence of any value.

---

## 3. `is None` vs `== None`

**Always use `is None`**, not `== None`.

```python
result = None

# Correct
if result is None:
    print("No result")

# Works but style-incorrect — PEP 8 says use 'is'
if result == None:
    print("No result")
```

**Why `is` is correct:** `None` is a singleton. Identity comparison is both correct and fast — there's no ambiguity. Using `==` works because `NoneType.__eq__` falls through to identity, but it also allows objects to override `__eq__` to equal `None`, which is confusing.

Flake8/Ruff will flag `== None` as E711 — it's a known anti-pattern.

---

## 4. Functions Return None by Default

```python
def do_something():
    print("done")
    # no return statement

result = do_something()
print(result)           # None
print(result is None)   # True
```

A function with no `return` statement, or `return` with no value, implicitly returns `None`.

```python
def process(data):
    if not data:
        return   # returns None implicitly
    return data.upper()
```

---

## 5. None as a Sentinel

None is frequently used as a sentinel (a signal value meaning "not provided"):

```python
def connect(host, port=None):
    if port is None:
        port = 5432   # use default
    return f"connecting to {host}:{port}"
```

This is the correct pattern for optional arguments. The alternative — using a mutable default — is a well-known trap:

```python
# WRONG — mutable default trap
def add_item(item, collection=[]):   # same list reused across calls!
    collection.append(item)
    return collection

# CORRECT — None sentinel
def add_item(item, collection=None):
    if collection is None:
        collection = []
    collection.append(item)
    return collection
```

---

## 6. None in Type Hints

```python
from typing import Optional

# Pre-3.10 style
def find_user(user_id: int) -> Optional[str]:
    ...

# Python 3.10+ style (preferred)
def find_user(user_id: int) -> str | None:
    ...

# Optional[X] is exactly equivalent to X | None
# Optional doesn't mean "you can omit the argument" — it means "the value can be None"
```

```python
# Explicit None in param default
def process(data: list[int] | None = None) -> int:
    if data is None:
        return 0
    return sum(data)
```

---

## 7. Truthiness of None

`None` is falsy:

```python
if None:
    print("truthy")   # never prints

bool(None)   # False
```

This enables concise but potentially ambiguous patterns:

```python
result = get_something()
if result:   # True if result is not None AND not empty/zero/false
    use(result)

# More explicit and always correct:
if result is not None:
    use(result)
```

Use `is not None` when you specifically mean "was a value provided?" rather than "is this value truthy?"

---

## 8. Common Mistakes

### Checking `== None` instead of `is None`

Style issue. Use `is`.

### Treating None as falsy when you mean "not provided"

```python
def update_count(value=None):
    if not value:   # Bug: also True for value=0, value=[], value=""
        value = 10
    ...

# Fix:
def update_count(value=None):
    if value is None:   # specifically checks for "not provided"
        value = 10
    ...
```

### Forgetting functions return None

```python
items = [3, 1, 2]
items = items.sort()   # Bug! list.sort() returns None, sorts in place
print(items)           # None

# Fix: don't assign the result of in-place operations
items.sort()
print(items)   # [1, 2, 3]
```

This catches many developers off guard. `list.sort()`, `list.append()`, `list.extend()`, `dict.update()` all return `None` — they mutate in place.

---

## 9. Related Concepts

- [[Object References]] — None is a singleton, `is` works because of this
- [[Dynamic Typing]] — None is a first-class object with a type
- [[Variables and Assignment]]

---

## 10. Exercises

### Beginner

1. Create a function that returns `None` explicitly and one that returns `None` implicitly (no return statement). Verify both return `None` with `is None`.
2. Demonstrate the mutable default argument trap and fix it with `None`.
3. What is the type of `None`? What is `bool(None)`?

### Intermediate

1. Write a function `safe_divide(a, b)` that returns `None` if `b` is zero and the result otherwise. Write code that uses this function and handles both cases.
2. Annotate a function with a return type of `str | None` and demonstrate calling it with both outcomes.

---

## 11. Mastery Checklist

- [ ] I know that None is a singleton — there's one None in any Python process
- [ ] I always use `is None` and `is not None`, never `== None`
- [ ] I know that functions return None implicitly when there's no return statement
- [ ] I use None as a sentinel for optional arguments to avoid the mutable default trap
- [ ] I know the difference between `if result:` and `if result is not None:`
- [ ] I know that `list.sort()`, `list.append()` etc. return None
