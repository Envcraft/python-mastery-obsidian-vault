---
title: Basic Data Types
aliases: [built-in types, primitive types]
tags: [python, fundamentals, types, int, float, str, bool]
category: fundamentals
level: 1
status: not-started
difficulty: beginner
created: 2026-11-23
updated: 2026-11-23
---

# Basic Data Types

## 1. Overview

Python's built-in types are objects — not primitives in the Java/C sense. Every value you work with is an instance of one of these types.

| Type | Example | Mutable? | Notes |
|------|---------|----------|-------|
| `int` | `42`, `-7`, `0` | No | Arbitrary precision |
| `float` | `3.14`, `-0.5` | No | IEEE 754 double |
| `complex` | `3+2j` | No | Rarely needed outside math |
| `bool` | `True`, `False` | No | Subclass of `int` |
| `str` | `"hello"` | No | Unicode, immutable |
| `bytes` | `b"hello"` | No | Raw bytes |
| `bytearray` | `bytearray(b"hi")` | Yes | Mutable bytes |
| `NoneType` | `None` | — | Singleton |

---

## 2. int — Arbitrary Precision

Python integers have no size limit (unlike C's 32/64-bit int):

```python
x = 2 ** 1000   # a number with 302 digits — works fine
print(x)

# Integer literals
decimal = 42
hex_val = 0xFF      # 255
octal   = 0o17      # 15
binary  = 0b1010    # 10

# Underscores for readability (Python 3.6+)
million = 1_000_000
hex_color = 0xFF_AA_BB
```

**Operations:**
```python
10 // 3   # 3  — floor division (integer result)
10 % 3    # 1  — modulo
10 ** 3   # 1000 — exponentiation
divmod(10, 3)  # (3, 1) — quotient and remainder together
```

**Gotcha:** Integer division in Python 3 always uses `//` for floor division; `/` always returns float:
```python
10 / 3    # 3.3333... (float)
10 // 3   # 3 (int)
```

---

## 3. float — IEEE 754 Double

Python floats are 64-bit IEEE 754 floating-point numbers (same as `double` in C/Java).

```python
x = 3.14
y = 1.5e-4    # scientific notation: 0.00015
z = float('inf')    # positive infinity
w = float('nan')    # not a number

import math
math.isinf(z)   # True
math.isnan(w)   # True
```

**The critical gotcha — floating-point arithmetic is not exact:**

```python
0.1 + 0.2          # 0.30000000000000004 — not 0.3!
0.1 + 0.2 == 0.3   # False

# Why: 0.1 and 0.2 cannot be represented exactly in binary floating-point
# Their binary approximations sum to something slightly different from 0.3's approximation

# Correct comparison for floats:
import math
math.isclose(0.1 + 0.2, 0.3)   # True
abs(0.1 + 0.2 - 0.3) < 1e-9   # True

# For financial/decimal calculations, use decimal:
from decimal import Decimal
Decimal("0.1") + Decimal("0.2") == Decimal("0.3")  # True
```

---

## 4. bool — Subclass of int

`bool` is a subclass of `int`. `True == 1` and `False == 0` — this is not coincidence:

```python
isinstance(True, int)   # True
True + True             # 2
True * 5                # 5
False + 1               # 1
sum([True, False, True, True])  # 3 — count of True values

# This makes sense for counting boolean conditions
results = [check_condition(x) for x in items]
passing_count = sum(results)   # elegant idiom
```

**Falsy values** — these evaluate to `False` in boolean context:
```python
bool(0)       # False
bool(0.0)     # False
bool("")      # False
bool([])      # False
bool({})      # False
bool(set())   # False
bool(None)    # False
bool(False)   # False
# Everything else is truthy
```

---

## 5. str — Unicode Strings

Python 3 strings are Unicode by default. Every `str` is a sequence of Unicode code points.

```python
s = "hello"
s = 'hello'          # single or double quotes — identical
s = """multiline
string"""
s = '''also
multiline'''

# f-strings (Python 3.6+) — preferred for formatting
name = "Alice"
age = 30
greeting = f"Hello, {name}! You are {age} years old."
greeting = f"Math: {2 + 2}"         # expressions work
greeting = f"Pi: {3.14159:.2f}"     # format spec inside
greeting = f"Repr: {name!r}"        # !r for repr, !s for str, !a for ascii
```

**Strings are immutable:**
```python
s = "hello"
s[0] = "H"   # TypeError: 'str' object does not support item assignment

# "Modify" by creating a new string
s = "H" + s[1:]   # "Hello"
```

**Key string operations:**
```python
s = "hello world"
s.upper()           # "HELLO WORLD"
s.split()           # ["hello", "world"]
s.split(",")        # splits on comma
" ".join(["a","b"]) # "a b"
s.strip()           # remove leading/trailing whitespace
s.startswith("he")  # True
s.endswith("ld")    # True
s.replace("l","x")  # "hexxo worxd"
s.find("world")     # 6 (index) or -1 if not found
"world" in s        # True
len(s)              # 11
```

**Raw strings** (useful for regex and Windows paths):
```python
path = r"C:\Users\name\file.txt"   # r prefix: backslashes are literal
pattern = r"\d+\.\d+"              # regex pattern — no need to escape \d
```

---

## 6. bytes and bytearray

```python
# bytes: immutable sequence of integers 0–255
b = b"hello"
b = bytes([72, 101, 108, 108, 111])   # same as b"Hello"

# bytearray: mutable bytes
ba = bytearray(b"hello")
ba[0] = 72   # 'H' — works because bytearray is mutable

# Encode/decode
s = "hello"
encoded = s.encode("utf-8")   # str → bytes
decoded = encoded.decode("utf-8")   # bytes → str
```

You'll work with `bytes` when dealing with network data, file I/O, cryptography, and binary protocols.

---

## 7. Type Checking

```python
x = 42
type(x)              # <class 'int'>
type(x) is int       # True
isinstance(x, int)   # True — preferred; also handles subclasses

# isinstance handles subclasses correctly
isinstance(True, int)      # True — bool is a subclass of int
type(True) is int          # False — type() is exact class match

# Check multiple types
isinstance(x, (int, float))  # True if x is int OR float
```

Use `isinstance()` over `type(x) is T` in production code — it handles subclasses correctly and is the idiomatic approach.

---

## 8. Related Concepts

- [[Type Conversion]]
- [[Dynamic Typing]]
- [[Object References]]
- [[None]]

---

## 9. Exercises

### Beginner

1. Demonstrate that `0.1 + 0.2 != 0.3` and fix the comparison using `math.isclose()`.
2. Use `True` and `False` as integers in arithmetic. Count how many items in a list satisfy a condition using `sum()`.
3. Create strings using all four quote styles. Try to modify a character in a string and observe the error.

### Intermediate

1. Write a function that accepts an argument and returns a string describing its type accurately: `"negative int"`, `"positive float"`, `"empty string"`, `"bool"`, `"None"`, etc.
2. Convert between bytes and str using encode/decode. Handle a `UnicodeDecodeError` by trying multiple encodings.

---

## 10. Mastery Checklist

- [ ] I know Python ints have no size limit
- [ ] I know why `0.1 + 0.2 != 0.3` and how to compare floats correctly
- [ ] I know that `bool` is a subclass of `int` and what that means
- [ ] I know strings are immutable Unicode sequences
- [ ] I know the difference between `bytes` and `str`
- [ ] I use `isinstance()` over `type(x) is T` for type checks
