---
title: Type Conversion
aliases: [type casting, coercion, explicit conversion]
tags: [python, fundamentals, types, conversion]
category: fundamentals
level: 1
status: not-started
difficulty: beginner
created: 2026-11-23
updated: 2026-11-23
---

# Type Conversion

## 1. Definition

Type conversion is the process of changing an object from one type to another. Python distinguishes between **explicit conversion** (you call a constructor or function) and **implicit conversion** (Python does it automatically, in limited cases).

---

## 2. Explicit Conversion

Explicit conversion uses the type constructors directly:

```python
# To int
int("42")        # 42 — string of digits
int(3.9)         # 3  — truncates toward zero, NOT rounds
int(True)        # 1
int(False)       # 0
int("0xFF", 16)  # 255 — parse hex string
int("0b1010", 2) # 10  — parse binary string

# To float
float("3.14")    # 3.14
float(42)        # 42.0
float("inf")     # inf
float("nan")     # nan

# To str
str(42)          # "42"
str(3.14)        # "3.14"
str(True)        # "True"
str(None)        # "None"
str([1,2,3])     # "[1, 2, 3]"

# To bool
bool(0)          # False
bool(1)          # True
bool("")         # False
bool("0")        # True — non-empty string is truthy
bool([])         # False
bool([0])        # True — non-empty list is truthy

# To list
list("hello")    # ['h', 'e', 'l', 'l', 'o']
list((1,2,3))    # [1, 2, 3] — from tuple
list({1,2,3})    # [1, 2, 3] — from set (unordered)
list(range(5))   # [0, 1, 2, 3, 4]

# To tuple
tuple([1,2,3])   # (1, 2, 3)
tuple("abc")     # ('a', 'b', 'c')

# To set
set([1,2,2,3])   # {1, 2, 3} — removes duplicates

# To dict
dict([("a",1), ("b",2)])   # {'a': 1, 'b': 2}
dict(a=1, b=2)             # {'a': 1, 'b': 2}
```

---

## 3. Common Conversion Gotchas

### `int()` truncates, doesn't round

```python
int(3.9)    # 3  — NOT 4
int(-3.9)   # -3 — truncates toward zero (NOT -4)
int(3.0)    # 3

# To round: use round()
round(3.9)  # 4
round(3.5)  # 4 — banker's rounding (rounds to even)
round(2.5)  # 2 — rounds to even (not 3!)
```

### `int("3.14")` fails

```python
int("3.14")     # ValueError: invalid literal for int() with base 10: '3.14'
int(float("3.14"))  # 3 — convert to float first, then to int
```

### `str()` vs `repr()`

```python
s = "hello"
print(str(s))    # hello — human-readable
print(repr(s))   # 'hello' — Python representation with quotes

# repr is useful for debugging — shows the actual type
print(str(None))    # None
print(repr(None))   # None — same here

print(str(""))      # (empty string — nothing printed)
print(repr(""))     # '' — now you can see it's an empty string
```

Use `repr()` in logging and debugging when you need to distinguish `None` from `"None"` or `""` from `" "`.

### `bool("False")` is `True`

```python
bool("False")   # True — it's a non-empty string!
bool("")        # False — only empty string is falsy

# If you need to parse "true"/"false" strings:
def parse_bool(s: str) -> bool:
    return s.lower() in ("true", "1", "yes")
```

---

## 4. Implicit Conversion (Coercion)

Python performs implicit conversion in limited, predictable cases:

### Numeric promotion

```python
3 + 3.0     # 6.0 — int promoted to float
3 * (1+2j)  # (3+6j) — int promoted to complex
True + 1    # 2 — bool (subclass of int) used as int
```

This is intentional design: numeric types form a hierarchy (`int` < `float` < `complex`), and Python promotes to the wider type.

### What Python does NOT coerce

```python
"3" + 3       # TypeError — no implicit str→int or int→str
"3" * 1.5     # TypeError — str repetition requires int multiplier

# Compare to JavaScript:
# "3" + 3     → "33"   (implicit coercion)
# "3" - 3     → 0      (implicit coercion)
# "3" * 3     → 9      (implicit coercion)
```

Python chose not to do this. The explicit, unsurprising approach wins.

---

## 5. The Dunder Methods Behind Conversion

Type constructors call dunder methods:

```python
class Temperature:
    def __init__(self, celsius):
        self.celsius = celsius
    
    def __int__(self):
        return int(self.celsius)
    
    def __float__(self):
        return float(self.celsius)
    
    def __str__(self):
        return f"{self.celsius}°C"
    
    def __repr__(self):
        return f"Temperature({self.celsius})"
    
    def __bool__(self):
        return self.celsius != 0

t = Temperature(37.5)
int(t)     # 37  — calls __int__
float(t)   # 37.5 — calls __float__
str(t)     # "37.5°C" — calls __str__
bool(t)    # True — calls __bool__
repr(t)    # "Temperature(37.5)" — calls __repr__
```

---

## 6. Related Concepts

- [[Basic Data Types]]
- [[Dynamic Typing]]
- [[None]]

---

## 7. Exercises

### Beginner

1. Convert the string `"  42  "` (with spaces) to an integer. What do you need to do first?
2. What does `int(True)` return? What about `int(False)`? What does this tell you about `bool`?
3. What is `bool("0")`? Is it `True` or `False`? Why? What about `bool(0)`?

### Intermediate

1. Write a function `safe_int(value, default=0)` that converts a value to int and returns `default` if conversion fails.
2. Write a class `Celsius` with `__float__`, `__int__`, `__str__`, and `__repr__`. Test all conversions.

---

## 8. Mastery Checklist

- [ ] I know that `int()` truncates toward zero, not rounds
- [ ] I know that `int("3.14")` raises ValueError
- [ ] I know the difference between `str()` and `repr()`
- [ ] I know why `bool("False")` is `True`
- [ ] I know which numeric coercions Python performs implicitly
- [ ] I understand that conversion functions call dunder methods
