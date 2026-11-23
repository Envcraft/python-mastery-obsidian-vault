---
title: Naming Conventions
aliases: []
tags: [python, fundamentals, pep8, style]
category: fundamentals
level: 1
status: not-started
difficulty: beginner
created: 2026-11-23
updated: 2026-11-23
---

# Naming Conventions

## 1. Definition

Python naming conventions are style rules (primarily from PEP 8) that govern how to name variables, functions, classes, constants, and modules. They are conventions, not syntax rules — Python doesn't enforce them. But violating them makes your code immediately recognizable as non-idiomatic.

---

## 2. The Rules

### Variables and Functions → `snake_case`

```python
user_name = "alice"
item_count = 42
max_retry_attempts = 3

def calculate_total_price(items, tax_rate):
    pass

def get_user_by_id(user_id: int):
    pass
```

### Classes → `PascalCase` (UpperCamelCase)

```python
class UserAccount:
    pass

class DatabaseConnectionPool:
    pass

class HTTPRequestHandler:   # acronyms: keep all caps if well-known
    pass
```

### Constants → `UPPER_SNAKE_CASE`

```python
MAX_CONNECTIONS = 100
DEFAULT_TIMEOUT = 30
API_BASE_URL = "https://api.example.com"
DATABASE_URL = "postgresql://localhost/mydb"
```

Python has no true constants (you can rebind any name). `UPPER_SNAKE_CASE` is a signal to other developers: "don't change this."

### Modules and Packages → `lowercase` or `lowercase_with_underscores`

```python
# Good
import os
import sys
import user_service
import database_utils

# Bad
import UserService   # looks like a class
import DatabaseUtils
```

### Private by convention → `_single_leading_underscore`

```python
class MyClass:
    def __init__(self):
        self._internal_cache = {}   # "private" — don't use from outside
        self.public_data = []       # public

    def _helper_method(self):       # private method
        pass
```

A single leading underscore is a signal, not enforcement. Python doesn't prevent external access — `obj._internal_cache` works fine. It just means "this is an implementation detail; don't depend on it."

A single leading underscore on a module-level name means it won't be exported with `from module import *`.

### Name mangling → `__double_leading_underscore`

```python
class Base:
    def __init__(self):
        self.__secret = 42   # mangled to _Base__secret

class Child(Base):
    def __init__(self):
        super().__init__()
        # self.__secret doesn't exist here — it's _Base__secret
        # self._Child__secret is a different thing entirely
```

Name mangling rewrites `__name` to `_ClassName__name`. It's designed to prevent accidental override in subclasses — not for security. Use it sparingly.

### Dunder names → `__double_both_sides__`

```python
__init__   # constructor
__repr__   # string representation
__len__    # length
__enter__  # context manager
```

These are reserved for Python's data model. Never create your own `__something__` names — Python may add them in future versions.

### Throwaway variable → `_`

```python
for _ in range(10):         # loop variable not needed
    do_something()

x, _, z = (1, 2, 3)        # ignore the middle value

result, _ = get_pair()     # ignore second return value
```

---

## 3. Common Violations and Why They Matter

| Wrong | Correct | Why |
|-------|---------|-----|
| `userName` | `user_name` | Python uses snake_case, not camelCase |
| `class user_service` | `class UserService` | Classes need PascalCase |
| `MAX_CONNECTIONS = 100` stored as `maxConnections` | `MAX_CONNECTIONS` | Constants signal immutability |
| `def GetUser()` | `def get_user()` | Functions are snake_case |
| `l`, `O`, `I` as single-char names | Use `l_val`, `count`, `idx` | These look like 1, 0, 1 in some fonts |

Violations don't break your code. They make your code look like it was written by someone who doesn't know Python — which is a signal to code reviewers that other things may also be wrong.

---

## 4. Type Alias Convention (Python 3.12+)

```python
# Python 3.12+
type UserId = int
type UserMap = dict[str, "User"]

# Pre-3.12
UserId = int   # just a regular name, by convention capitalized like a class
```

---

## 5. Related Concepts

- [[Variables and Assignment]]
- [[Dynamic Typing]]

---

## 6. Mastery Checklist

- [ ] I use snake_case for variables and functions without thinking about it
- [ ] I use PascalCase for classes
- [ ] I use UPPER_SNAKE_CASE for constants
- [ ] I understand the difference between `_private`, `__mangled`, and `__dunder__`
- [ ] I never create my own `__something__` names
