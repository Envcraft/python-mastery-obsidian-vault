---
title: Variables and Assignment
aliases: [name binding, variable binding]
tags: [python, fundamentals, variables, assignment]
category: fundamentals
level: 1
status: not-started
difficulty: beginner
created: 2026-11-23
updated: 2026-11-23
---

# Variables and Assignment

## 1. Definition

In Python, a "variable" is a name bound to an object. Assignment (`=`) does not create a storage location and copy a value into it — it creates (or rebinds) a name that refers to an existing object.

---

## 2. Why It Exists

Python uses a uniform object model: everything is an object. Names are lightweight references into this object space. This design enables garbage collection, flexible data sharing, and Python's consistent behavior around mutation and function arguments.

---

## 3. Mental Model

**Wrong model (from C/Java):**
```
x = 42
[ x | 42 ]   ← x is a box, 42 is inside it
```

**Correct Python model:**
```
x = 42
x ──────→ [ int object | value=42 | id=... ]
```

A name is a label that points to an object. The object exists independently. Multiple labels can point to the same object.

---

## 4. How Python Implements It

When Python executes `x = 42`:
1. The integer object `42` is created (or retrieved from the small int cache)
2. The name `x` is added to the current namespace (a dictionary)
3. The namespace entry maps `"x"` → the integer object

You can see the namespace:
```python
x = 42
print(locals())   # {'x': 42, ...}
```

The object has three properties you can inspect:

```python
x = 42
print(id(x))      # unique identity (memory address in CPython)
print(type(x))    # <class 'int'>
print(x)          # 42 (value)
```

---

## 5. Assignment Variants

```python
# Simple assignment
x = 42

# Multiple targets in one statement
a = b = c = []     # ALL three names refer to the SAME list object

# Augmented assignment
x += 1             # for immutables: creates a new object
lst = [1, 2]
lst += [3]         # for mutables: may modify in place (calls __iadd__)

# Annotated assignment (type hint, no enforcement at runtime)
count: int = 0

# Walrus operator (assignment expression, Python 3.8+)
if (n := len(data)) > 10:
    print(f"Too many items: {n}")
```

---

## 6. Examples

### Basic name binding

```python
x = 42
y = x          # y and x refer to the same int object

print(id(x))   # e.g. 140234567890
print(id(y))   # same id — same object

x = 100        # rebind x to a new int object
print(x)       # 100
print(y)       # 42 — y still points to the original object
```

### The critical difference with mutable objects

```python
a = [1, 2, 3]
b = a          # b and a refer to the SAME list

b.append(4)    # modifies the object both names point to
print(a)       # [1, 2, 3, 4] — a sees the change!
print(b)       # [1, 2, 3, 4]

# Compare: rebinding doesn't affect the other name
b = [9, 9, 9]  # rebind b to a new list
print(a)       # [1, 2, 3, 4] — a unaffected
print(b)       # [9, 9, 9]
```

### Augmented assignment behaves differently for mutables vs immutables

```python
# Immutable: += creates a new object
x = 10
print(id(x))   # some id
x += 1
print(id(x))   # different id — x points to a new int

# Mutable: += modifies in place (calls __iadd__)
lst = [1, 2]
print(id(lst))
lst += [3]
print(id(lst))  # same id — the list was modified in place
```

---

## 7. Code Walkthrough

```python
a = b = c = []
```

This line:
1. Creates one empty list object: `[]`
2. Binds `c` to that list
3. Binds `b` to that same list
4. Binds `a` to that same list

All three names point to the same object. If you do `a.append(1)`, then `b` and `c` both see `[1]`.

This is NOT the same as:
```python
a = []
b = []
c = []  # three separate list objects
```

---

## 8. Common Mistakes

### Mistake 1: Assuming assignment copies a list

```python
original = [1, 2, 3]
copy = original       # NOT a copy — both names point to the same list

copy.append(4)
print(original)       # [1, 2, 3, 4] — surprise!
```

**Fix:** Use `copy = original[:]` or `copy = list(original)` or `import copy; copy.deepcopy(original)`.

### Mistake 2: Mutable default argument

```python
def add_item(item, lst=[]):   # THE LIST IS CREATED ONCE AT FUNCTION DEFINITION
    lst.append(item)
    return lst

print(add_item(1))    # [1]
print(add_item(2))    # [1, 2] — not [2]! Same list reused across calls
```

**Fix:**
```python
def add_item(item, lst=None):
    if lst is None:
        lst = []
    lst.append(item)
    return lst
```

### Mistake 3: Chained assignment with mutables

```python
a = b = []     # a and b are the SAME list
a.append(1)
print(b)       # [1] — usually not intended
```

---

## 9. Edge Cases

### Small integer caching

CPython caches integers from -5 to 256. These are always the same object:

```python
x = 100
y = 100
print(x is y)   # True — same cached object

x = 1000
y = 1000
print(x is y)   # False in some contexts — different objects
                 # (may be True within the same code block due to compile-time optimization)
```

**Don't rely on this behavior.** Use `==` for value comparison, `is` for identity.

### Pass-by-object-reference in functions

```python
def modify(lst):
    lst.append(99)   # modifies the object the caller's name also points to

def rebind(lst):
    lst = [0, 0, 0]  # rebinds local name only — caller unaffected

data = [1, 2, 3]
modify(data)
print(data)    # [1, 2, 3, 99] — the object was mutated

rebind(data)
print(data)    # [1, 2, 3, 99] — unchanged, rebind was local
```

---

## 10. Real-World Applications

- **FastAPI/Pydantic**: Models use assignment validation, not copying. Understanding references prevents unexpected mutation of shared model instances.
- **Django ORM**: QuerySets and model instances are objects; assigning them shares the reference.
- **Configuration objects**: Shared mutable config dicts can cause subtle bugs if modified in multiple places — prefer immutable configs or explicit copying.

---

## 11. Related Concepts

- [[Object References]]
- [[Dynamic Typing]]
- [[Multiple Assignment]]
- [[None]]
- [[Python Object Model]]
- [[Mutability and Immutability]]

---

## 12. Exercises

### Beginner

1. Create two names pointing to the same list. Append an item via one name and verify both names see the change.
2. Demonstrate that rebinding one name does not affect the other.
3. Write a function that takes a list and an integer. The function should append the integer to the list. Call it and verify the caller's list changed.

### Intermediate

1. Write a function that takes a list and returns a modified copy, leaving the original unchanged. Use at least two different approaches.
2. Explain what happens with `a = b = c = [1, 2, 3]`. Then show how to create three independent lists with the same initial values.

### Challenging

1. Write a function with a mutable default argument bug. Then fix it. Explain why default arguments are evaluated once at function definition, not at each call.

---

## 13. Revision Questions

1. What does `x = 42` actually do in Python? Does Python create a "variable" with a type?
2. After `a = [1,2,3]; b = a; a.append(4)`, what does `b` contain and why?
3. After `a = [1,2,3]; b = a; a = [9,9,9]`, what does `b` contain and why?
4. Why is `def f(lst=[])` dangerous? What is the correct pattern?
5. What is the difference between `lst += [1]` and `lst = lst + [1]` for a list?

---

## 14. Mastery Checklist

- [ ] I can explain that Python names are references, not typed containers
- [ ] I can predict what happens when multiple names point to the same mutable object
- [ ] I can distinguish between mutation and rebinding
- [ ] I know the mutable default argument trap and can fix it
- [ ] I understand pass-by-object-reference in function arguments
- [ ] I can write functions that safely copy their arguments when needed
