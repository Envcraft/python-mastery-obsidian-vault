---
title: Unpacking
aliases: [sequence unpacking, iterable unpacking, destructuring]
tags: [python, fundamentals, unpacking, assignment]
category: fundamentals
level: 1
status: not-started
difficulty: beginner
created: 2026-11-23
updated: 2026-11-23
---

# Unpacking

## 1. Definition

Unpacking binds multiple names from a single iterable or mapping in one expression. Python can unpack any iterable — lists, tuples, strings, generators, ranges — not just those with fixed length.

---

## 2. Sequence Unpacking

```python
# From a list
x, y, z = [1, 2, 3]

# From a tuple
a, b = (10, 20)

# From a string — each character
first, second, third = "abc"
# first='a', second='b', third='c'

# From a range
p, q, r = range(3)
# p=0, q=1, r=2

# Must have exact count or use star
a, b = 1, 2, 3   # ValueError: too many values
a, b, c = 1, 2   # ValueError: not enough values
```

---

## 3. Starred Unpacking

The `*` captures everything not assigned to a named target:

```python
first, *rest = [1, 2, 3, 4, 5]
# first = 1
# rest = [2, 3, 4, 5]  ← always a list

*init, last = [1, 2, 3, 4, 5]
# init = [1, 2, 3, 4]
# last = 5

head, *middle, tail = [1, 2, 3, 4, 5]
# head = 1, middle = [2, 3, 4], tail = 5

# Star can capture zero items
a, *b = [1]
# a = 1, b = []

# Only one starred target per assignment
# *x, *y = [1, 2, 3]  # SyntaxError
```

---

## 4. Nested Unpacking

```python
(a, b), c = (1, 2), 3
# a=1, b=2, c=3

(x, y), (z, w) = (1, 2), (3, 4)
# x=1, y=2, z=3, w=4

# Practical: unpacking nested data
records = [("Alice", (90, 85, 92)), ("Bob", (75, 88, 70))]
for name, (s1, s2, s3) in records:
    print(f"{name}: avg={sum([s1,s2,s3])/3:.1f}")
```

---

## 5. Unpacking in Function Calls

```python
# * unpacks an iterable into positional arguments
args = [1, 2, 3]
print(*args)              # print(1, 2, 3)

def add(a, b, c):
    return a + b + c

values = [10, 20, 30]
result = add(*values)     # add(10, 20, 30) → 60

# ** unpacks a dict into keyword arguments
def greet(name, greeting="Hello"):
    return f"{greeting}, {name}!"

params = {"name": "Alice", "greeting": "Hi"}
print(greet(**params))    # greet(name="Alice", greeting="Hi")
```

---

## 6. Unpacking in For Loops

```python
# Classic — iterating pairs
pairs = [(1, 'a'), (2, 'b'), (3, 'c')]
for num, letter in pairs:
    print(f"{num}: {letter}")

# With enumerate
items = ['x', 'y', 'z']
for index, value in enumerate(items):
    print(index, value)

# With zip
keys = ['a', 'b', 'c']
values = [1, 2, 3]
for k, v in zip(keys, values):
    print(f"{k}: {v}")

# Ignoring values with _
for _, important in pairs:
    print(important)
```

---

## 7. Dictionary Unpacking

```python
# Merge dicts (Python 3.9+)
defaults = {"color": "blue", "size": "medium"}
overrides = {"color": "red", "weight": 1.5}
merged = {**defaults, **overrides}
# {'color': 'red', 'size': 'medium', 'weight': 1.5}
# Later dict's values win on conflict

# Extend a dict inline
extra = {"debug": True}
config = {"host": "localhost", "port": 8080, **extra}
```

---

## 8. Practical Patterns

```python
# Return multiple values and unpack
def min_max(numbers):
    return min(numbers), max(numbers)

low, high = min_max([3, 1, 4, 1, 5, 9])

# Skip parts of a tuple you don't need
_, second, _ = (10, 20, 30)   # only want second element

# Parse a header line
line = "2026-11-23 ERROR Connection refused"
date, level, *message = line.split()
# message = ['Connection', 'refused'] — joins back with ' '.join(message)

# Swap without temp
a, b = b, a

# Unpack function return in comprehension
data = [(1, 2), (3, 4), (5, 6)]
sums = [a + b for a, b in data]
```

---

## 9. Common Mistakes

### Forgetting star gives a list

```python
first, *rest = [1, 2, 3]
print(type(rest))   # <class 'list'>
# rest = [2, 3], not (2, 3) even though the source was a tuple
```

### Trying to use two stars in assignment

```python
*a, *b = [1, 2, 3, 4]   # SyntaxError
```

### Nested tuple unpack mismatch

```python
(a, b), c = [1, 2], 3    # ok
(a, b), c = [1, 2, 3]    # ValueError — unpacks to (1,2,3), can't match (a,b),c structure
```

---

## 10. Related Concepts

- [[Variables and Assignment]]
- [[Multiple Assignment]]
- [[Functions]] (for `*args`, `**kwargs`)

---

## 11. Exercises

### Beginner

1. Unpack `point = (3, 7)` into `x` and `y`.
2. From `data = [1, 2, 3, 4, 5]`, capture the first element as `head` and the rest as `tail`.
3. Use `**` to merge `{"a": 1, "b": 2}` and `{"b": 3, "c": 4}` into one dict. What wins when keys conflict?

### Intermediate

1. Write a function that returns `(min, max, mean)` of a list. Unpack the result in one line.
2. Given `records = [("Alice", 95), ("Bob", 87), ("Carol", 92)]`, use unpacking in a for loop to print each name and score. Find the name with the highest score using unpacking.

### Challenging

1. Write a function `flatten_one_level(nested)` that takes `[[1,2], [3,4], [5]]` and returns `[1,2,3,4,5]` using unpacking in a list comprehension.

---

## 12. Mastery Checklist

- [ ] I can unpack any iterable into named variables
- [ ] I can use starred unpacking to capture variable-length sequences
- [ ] I can unpack nested structures
- [ ] I know how to unpack in function call arguments with `*` and `**`
- [ ] I use unpacking naturally in for loops with `enumerate` and `zip`
