---
title: Multiple Assignment
aliases: [chained assignment, tuple unpacking as assignment]
tags: [python, fundamentals, variables, assignment]
category: fundamentals
level: 1
status: not-started
difficulty: beginner
created: 2026-11-23
updated: 2026-11-23
---

# Multiple Assignment

## 1. Definition

Python allows multiple names to be bound in a single statement in several ways: chained assignment (`a = b = value`), tuple unpacking (`a, b = 1, 2`), and starred assignment (`a, *rest = seq`).

---

## 2. Forms of Multiple Assignment

### Chained Assignment

```python
a = b = c = 0
```

This binds all three names to the **same** object. For immutables like integers this is fine. For mutables, it creates a shared reference:

```python
a = b = c = []     # all three point to the SAME list
a.append(1)
print(b)           # [1] — b sees the change
print(c)           # [1] — c sees the change too

# If you want independent lists:
a, b, c = [], [], []
```

### Tuple Unpacking

Python evaluates the right side first, then unpacks:

```python
a, b = 1, 2         # a=1, b=2
a, b = (1, 2)       # same — unpacking a tuple
a, b = [1, 2]       # same — unpacking a list
a, b = "hi"         # a='h', b='i' — unpacking a string

# Must match exactly
a, b = 1, 2, 3      # ValueError: too many values to unpack
a, b, c = 1, 2      # ValueError: not enough values to unpack
```

### Starred Assignment (Python 3+)

```python
first, *rest = [1, 2, 3, 4, 5]
# first = 1, rest = [2, 3, 4, 5]

*init, last = [1, 2, 3, 4, 5]
# init = [1, 2, 3, 4], last = 5

first, *middle, last = [1, 2, 3, 4, 5]
# first = 1, middle = [2, 3, 4], last = 5

# Star always captures a list, even for zero items
a, *b = [1]
# a = 1, b = []

# Only one star allowed per assignment
# *a, *b = [1, 2, 3]   # SyntaxError
```

### The Swap Idiom

```python
a, b = 10, 20
a, b = b, a      # swap — works because right side is fully evaluated first
print(a, b)      # 20, 10
```

Python evaluates the right side completely before binding. This means:
1. `b, a` creates a tuple `(20, 10)`
2. That tuple is unpacked into `a=20, b=10`

In C/Java you need a temp variable for a swap. In Python you don't — but only because of this evaluation order guarantee.

---

## 3. Unpacking in For Loops

```python
pairs = [(1, 'a'), (2, 'b'), (3, 'c')]
for num, letter in pairs:         # unpack each tuple
    print(f"{num}: {letter}")

# With enumerate — unpacking the (index, value) tuple
items = ['x', 'y', 'z']
for i, item in enumerate(items):
    print(f"{i}: {item}")

# With zip
names = ['Alice', 'Bob']
scores = [95, 87]
for name, score in zip(names, scores):
    print(f"{name}: {score}")
```

---

## 4. Common Mistakes

### Chained assignment with mutable objects

```python
# Looks like three separate empty lists
row1 = row2 = row3 = []

row1.append(1)
print(row2)   # [1] — not what you expected
```

This trips up anyone coming from a language where `a = b = value` creates copies.

### Forgetting evaluation order in swap

```python
# This works fine in Python
a, b = b, a   # right side is (b, a) tuple, evaluated before binding

# Common mistake from other languages:
temp = a
a = b
b = temp   # correct but verbose — unnecessary in Python
```

---

## 5. Related Concepts

- [[Variables and Assignment]]
- [[Unpacking]]
- [[Object References]]

---

## 6. Exercises

### Beginner

1. Using chained assignment, create three names bound to `0`. Then show that reassigning one doesn't affect the others.
2. Swap two variables `x = "hello"` and `y = "world"` without a temp variable.
3. Unpack `data = [10, 20, 30, 40, 50]` so that `first=10`, `middle=[20,30,40]`, `last=50`.

### Intermediate

1. Using chained assignment, create three names bound to the same list. Demonstrate the shared reference issue, then fix it.
2. Write a for loop that iterates over a list of `(name, age, city)` tuples and prints each field separately.

---

## 7. Mastery Checklist

- [ ] I know that chained assignment creates shared references to the same object
- [ ] I can use tuple unpacking in assignments and for loops
- [ ] I can use starred assignment to capture variable-length sequences
- [ ] I know why the swap idiom `a, b = b, a` works
