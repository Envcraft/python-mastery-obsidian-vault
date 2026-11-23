---
title: Conditional Expressions
aliases: [ternary operator, ternary expression, inline if]
tags: [python, fundamentals, conditionals, ternary]
category: fundamentals
level: 2
status: not-started
difficulty: beginner
created: 2026-11-23
updated: 2026-11-23
---

# Conditional Expressions

## 1. Definition

A conditional expression (Python's "ternary operator") evaluates to one of two values based on a condition, in a single expression. It is an expression, not a statement — it produces a value.

---

## 2. Syntax

```python
value_if_true if condition else value_if_false
```

Note the order: **true value comes first**, condition in the middle. This is different from C/Java's `condition ? true_value : false_value`.

---

## 3. Examples

```python
age = 20
status = "adult" if age >= 18 else "minor"

# Equivalent if/else statement:
if age >= 18:
    status = "adult"
else:
    status = "minor"

# In function calls
print("even" if n % 2 == 0 else "odd")

# In list comprehensions
values = [abs(x) if x >= 0 else -x for x in [-1, 2, -3, 4]]

# As a return value
def classify(n: int) -> str:
    return "positive" if n > 0 else ("negative" if n < 0 else "zero")
```

---

## 4. When to Use

**Use conditional expressions when:**
- The condition and both values fit clearly on one line
- Both branches are simple values or simple expressions
- You're assigning a value or returning from a function

```python
# Good — clear and concise
label = "enabled" if feature_flag else "disabled"
display = value if value is not None else "N/A"
max_val = a if a > b else b   # though max(a, b) is clearer here
```

---

## 5. When NOT to Use

**Avoid conditional expressions when:**
- Either branch requires multiple operations
- The expression exceeds one readable line
- Nesting ternaries (almost always wrong)

```python
# Bad — hard to read
result = (process_a(x) if condition_a else process_b(x)) if outer_cond else fallback(x)

# Better — use if/else statements
if outer_cond:
    if condition_a:
        result = process_a(x)
    else:
        result = process_b(x)
else:
    result = fallback(x)
```

---

## 6. Lazy Evaluation

Both branches are expressions, but only the matching one is evaluated:

```python
def expensive():
    print("expensive called!")
    return 42

x = expensive() if True else 0     # "expensive called!" — truthy branch evaluated
x = expensive() if False else 0    # nothing printed — falsy branch evaluated, 0 returned
```

---

## 7. Common Mistakes

### Confusing the order

```python
# Python
value_if_true if condition else value_if_false

# C/Java/JavaScript
condition ? value_if_true : value_if_false

# Common mistake from other languages:
x > 0 ? "positive" : "negative"   # SyntaxError in Python
```

### Using when a function should handle the logic

```python
# Overly clever
result = (lambda x: x * 2 if x > 0 else x * -1)(value)

# Just write a function
def process(x):
    return x * 2 if x > 0 else x * -1

result = process(value)
```

---

## 8. Related Concepts

- [[Conditional Statements]] — the full if/elif/else statement
- [[Basic Operators]] — logical operators that often appear in conditions

---

## 9. Exercises

### Beginner

1. Rewrite `if n % 2 == 0: label = "even" else: label = "odd"` as a conditional expression.
2. Write a one-liner function `clamp(x, low, high)` that returns `low` if `x < low`, `high` if `x > high`, and `x` otherwise. Use nested ternaries (then rewrite it readably).

### Intermediate

1. Use a conditional expression inside a list comprehension to map each number in a list to `"pos"` if positive, `"neg"` if negative, or `"zero"` if zero.

---

## 10. Mastery Checklist

- [ ] I know the syntax: `true_val if condition else false_val`
- [ ] I use conditional expressions for simple assignments and returns
- [ ] I avoid nested ternaries
- [ ] I recognize when if/else statements are clearer than a ternary
