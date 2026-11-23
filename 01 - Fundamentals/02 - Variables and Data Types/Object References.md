---
title: Object References
aliases: [references, identity, is vs ==]
tags: [python, fundamentals, references, identity, objects]
category: fundamentals
level: 1
status: not-started
difficulty: beginner
created: 2026-11-23
updated: 2026-11-23
---

# Object References

## 1. Definition

Every Python name is a reference — a pointer to an object in memory. Objects exist on the heap; names are entries in namespaces that point to them. Assignment creates or rebinds references, not copies.

---

## 2. Everything Is an Object

In Python, everything is an object: integers, strings, lists, functions, classes, modules, `None`. Every object has:

- **Identity**: a unique integer (`id(obj)`) — in CPython, this is the memory address
- **Type**: what kind of object it is (`type(obj)`)
- **Value**: the data it holds

```python
x = [1, 2, 3]
print(id(x))      # 140234567890 (some memory address)
print(type(x))    # <class 'list'>
print(x)          # [1, 2, 3]
```

---

## 3. `is` vs `==`

This is one of the most important distinctions in Python:

| Operator | Checks | Calls |
|----------|--------|-------|
| `is` | Identity — are these the **same object**? | Nothing — compares `id()` |
| `==` | Equality — do these objects have the **same value**? | `__eq__` method |

```python
a = [1, 2, 3]
b = [1, 2, 3]   # same value, different object

print(a == b)   # True — same value
print(a is b)   # False — different objects in memory

c = a
print(a is c)   # True — same object
```

**Rule of thumb:**
- Use `==` for value comparison (almost always what you want)
- Use `is` only for `None`, `True`, `False` checks, and object identity tests

```python
# Correct
if value is None:
    ...

if result is True:
    ...

# Wrong — might work by coincidence but semantically incorrect
if value == None:   # works but style violation
    ...
```

---

## 4. Small Integer Caching

CPython caches integer objects in the range **-5 to 256**. Integers in this range are always the same object:

```python
a = 100
b = 100
print(a is b)    # True — both point to the cached int object

a = 1000
b = 1000
print(a is b)    # False — different objects (may be True in REPL due to compile-time optimization)
```

**Don't write code that relies on this.** It's an implementation detail of CPython, not a language guarantee. Use `==` for value comparison.

---

## 5. String Interning

CPython interns (reuses the same object for) strings that look like identifiers:

```python
a = "hello"
b = "hello"
print(a is b)   # True — interned

a = "hello world"
b = "hello world"
print(a is b)   # False in some contexts — not interned (contains a space)
```

Again: implementation detail. Never use `is` to compare strings. Always use `==`.

---

## 6. Reference Counting Basics

CPython tracks how many references point to each object. When the count reaches zero, the object is immediately deallocated:

```python
import sys

x = [1, 2, 3]
print(sys.getrefcount(x))   # 2 — x + the getrefcount argument

y = x
print(sys.getrefcount(x))   # 3 — x, y, and the argument

del y
print(sys.getrefcount(x))   # 2 again — y no longer contributes

x = None   # original list's refcount → 0 → deallocated
```

You don't manage memory manually in Python. This is automatic.

---

## 7. What Happens When You Pass an Object to a Function

Python passes references, not values and not copies:

```python
def show_id(obj):
    print(id(obj))   # same id as the caller's object

x = [1, 2, 3]
print(id(x))
show_id(x)    # same id — the function receives a reference to the same list
```

This means:
- Mutating the object inside the function affects the caller's object
- Rebinding the local name inside the function doesn't affect the caller

See [[Variables and Assignment]] for the mutation vs rebinding distinction.

---

## 8. Common Mistakes

### Using `is` to compare values

```python
# Wrong
if name is "alice":    # may fail for non-interned strings
    ...

# Correct
if name == "alice":
    ...
```

### Using `is None` correctly (this one is correct — unlike the above)

```python
# This is the right way to check for None
if result is None:
    ...

# This works but is style-incorrect
if result == None:
    ...
```

### Assuming `id()` is always a memory address

`id()` returns a unique integer for the object's lifetime. In CPython this happens to be the memory address, but other Python implementations may differ.

---

## 9. Related Concepts

- [[Variables and Assignment]] — assignment creates references
- [[None]] — the None singleton
- [[Mutability and Immutability]] — why mutable objects need extra care with references
- [[Python Object Model]]

---

## 10. Exercises

### Beginner

1. Create two lists with the same values. Use `is` and `==` to compare them. Explain the results.
2. Create a name `a`, then `b = a`. Modify the object via `a`. Check if `b` sees the change.
3. Use `sys.getrefcount()` to watch reference count change as you create and delete references.

### Intermediate

1. Write a function that demonstrates the difference between mutation and rebinding inside a function.
2. Explain why you should never use `is` to compare strings or integers (beyond None/True/False).

---

## 11. Mastery Checklist

- [ ] I can explain the difference between `is` and `==` precisely
- [ ] I know when to use `is` (None, True, False, identity tests only)
- [ ] I understand small integer caching and string interning — and why not to rely on them
- [ ] I understand reference counting at a conceptual level
- [ ] I know what happens to an object's reference count when a function is called with it
