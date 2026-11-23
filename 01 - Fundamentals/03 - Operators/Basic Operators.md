---
title: Basic Operators
aliases: [operators, arithmetic, comparison, logical, bitwise]
tags: [python, fundamentals, operators]
category: fundamentals
level: 2
status: not-started
difficulty: beginner
created: 2026-11-23
updated: 2026-11-23
---

# Basic Operators

## 1. Arithmetic Operators

```python
10 + 3    # 13 — addition
10 - 3    # 7  — subtraction
10 * 3    # 30 — multiplication
10 / 3    # 3.333... — true division (always float in Python 3)
10 // 3   # 3  — floor division (rounds toward negative infinity)
10 % 3    # 1  — modulo (remainder)
10 ** 3   # 1000 — exponentiation

# Floor division behavior with negatives
-10 // 3  # -4  — rounds toward negative infinity, NOT toward zero
-10 % 3   # 2   — always non-negative when divisor is positive

# This satisfies: (a // b) * b + (a % b) == a
(-10 // 3) * 3 + (-10 % 3)   # -4 * 3 + 2 = -12 + 2 = -10 ✓

# String and list repetition
"ha" * 3        # "hahaha"
[0] * 5         # [0, 0, 0, 0, 0]
```

---

## 2. Comparison Operators

```python
x, y = 5, 10

x == y    # False — equal value?
x != y    # True  — not equal?
x < y     # True  — less than?
x > y     # False — greater than?
x <= y    # True  — less than or equal?
x >= y    # False — greater than or equal?

# Chained comparisons — Python supports these natively
1 < x < 10         # True — equivalent to 1 < x and x < 10
0 <= x <= 100      # True

# Works for all comparison operators
a = 5
1 < a < 10 != 7   # True — chains work with mixed operators
```

Comparison operators return `True` or `False` (bool objects).

---

## 3. Logical Operators — Important: They Return Values

Python's logical operators (`and`, `or`, `not`) do **not** simply return `True` or `False`. They return one of their operands:

### `and`

Returns the **first falsy** operand, or the **last operand** if all are truthy:

```python
1 and 2         # 2    — both truthy, returns last
0 and 2         # 0    — first is falsy, returns it
"" and "hello"  # ""   — first is falsy, returns it
"hi" and "bye"  # "bye" — both truthy, returns last
```

### `or`

Returns the **first truthy** operand, or the **last operand** if all are falsy:

```python
1 or 2          # 1    — first is truthy, returns it
0 or 2          # 2    — first is falsy, tries next, returns it
"" or "hello"   # "hello" — first falsy, second truthy
0 or "" or []   # []   — all falsy, returns last
```

### `not`

Returns `True` or `False` — the exception:

```python
not True    # False
not False   # True
not 0       # True
not ""      # True
not [1,2]   # False
```

### Why this matters

The `or` behavior enables default value patterns:

```python
# "if config_port is falsy, use 8080"
port = config.get("port") or 8080

# But this has a bug if 0 is a valid port!
port = 0
result = port or 8080   # 8080 — wrong! 0 is a valid port

# Correct: use explicit None check
port = config.get("port")
if port is None:
    port = 8080
# or:
port = 8080 if config.get("port") is None else config.get("port")
```

---

## 4. Identity Operators

```python
x is y      # True if x and y are the same object (same id())
x is not y  # True if they are different objects
```

Use `is` only for:
- `is None` / `is not None`
- `is True` / `is False` (rarely needed)
- Explicit identity testing

Never use `is` to compare integers, strings, or other values — small int caching and string interning make it unreliable.

---

## 5. Membership Operators

```python
"hello" in "hello world"    # True — substring check
"xyz" not in "hello"        # True

3 in [1, 2, 3]              # True — list containment (O(n))
3 in {1, 2, 3}              # True — set containment (O(1))
"key" in {"key": "val"}     # True — checks dict keys
"val" in {"key": "val"}.values()  # True — check values explicitly
```

**Performance note:** `in` on a list is O(n). `in` on a set or dict is O(1). For repeated membership tests over large collections, convert to a set:

```python
# Slow for large data
allowed = ["admin", "editor", "viewer"]
if role in allowed:   # O(n) each check
    ...

# Fast
allowed = {"admin", "editor", "viewer"}
if role in allowed:   # O(1) each check
    ...
```

---

## 6. Augmented Assignment Operators

```python
x = 10
x += 5    # x = x + 5  → 15
x -= 3    # x = x - 3  → 12
x *= 2    # x = x * 2  → 24
x /= 4    # x = x / 4  → 6.0 (always float for /)
x //= 2   # x = x // 2 → 3
x **= 2   # x = x ** 2 → 9
x %= 4    # x = x % 4  → 1

# For mutable objects: may operate in-place
lst = [1, 2]
lst += [3]   # calls __iadd__ — modifies in place
lst *= 2     # calls __imul__ — [1, 2, 3, 1, 2, 3]
```

---

## 7. Bitwise Operators

```python
# Binary operations on integers
a = 0b1010   # 10
b = 0b1100   # 12

a & b    # 0b1000  =  8  — AND
a | b    # 0b1110  = 14  — OR
a ^ b    # 0b0110  =  6  — XOR
~a       # -11 (bitwise NOT, two's complement)
a << 1   # 0b10100 = 20  — left shift
a >> 1   # 0b0101  =  5  — right shift

# Practical use: flags
READ  = 0b001  # 1
WRITE = 0b010  # 2
EXEC  = 0b100  # 4

permissions = READ | WRITE    # 3
has_read = bool(permissions & READ)   # True
```

---

## 8. Operator Precedence (highest to lowest, simplified)

| Priority | Operators |
|----------|-----------|
| Highest | `**` |
| | `+x`, `-x`, `~x` (unary) |
| | `*`, `/`, `//`, `%` |
| | `+`, `-` |
| | `<<`, `>>` |
| | `&` |
| | `^` |
| | `\|` |
| | `==`, `!=`, `<`, `>`, `<=`, `>=`, `is`, `in` |
| | `not` |
| | `and` |
| Lowest | `or` |

When in doubt, use parentheses. Code is read more than written.

---

## 9. Short-Circuit Evaluation

Python evaluates `and` and `or` lazily — it stops as soon as the result is determined:

```python
def side_effect():
    print("called!")
    return True

False and side_effect()   # never calls side_effect() — short-circuits at False
True  or  side_effect()   # never calls side_effect() — short-circuits at True
```

This is used deliberately:

```python
# Only access dict key if it exists
if data and data["key"] == "value":
    ...

# Guard against None before calling method
if obj is not None and obj.is_valid():
    ...
```

---

## 10. Related Concepts

- [[Conditional Statements]] — operators used in conditions
- [[Basic Data Types]] — how types affect operator behavior
- [[Dynamic Typing]] — operator overloading via dunder methods

---

## 11. Exercises

### Beginner

1. What is `10 // -3`? What is `-10 // 3`? Verify your answer.
2. What does `"" or "default"` return? What does `"value" or "default"` return? Explain.
3. Write a one-liner that returns the absolute value of a number using only the ternary operator (no `abs()`).

### Intermediate

1. Explain why `port = config_port or 8080` is buggy when `0` is a valid port. Write the correct version.
2. Write a function that checks if a string is non-empty using only `not` and `bool()`. Verify it handles `None`, `""`, `" "`, and `"hello"` correctly.

### Challenging

1. Use only bitwise operators to check if a number is odd, without using `%`.

---

## 12. Mastery Checklist

- [ ] I know what `//` and `/` return and their difference
- [ ] I know that `and`/`or` return values, not booleans
- [ ] I can explain short-circuit evaluation with an example
- [ ] I know when to use `is` vs `==`
- [ ] I know why `in` on a list is O(n) but on a set is O(1)
- [ ] I can read operator precedence correctly (or use parentheses when unsure)
