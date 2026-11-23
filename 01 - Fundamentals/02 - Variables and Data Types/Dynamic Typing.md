---
title: Dynamic Typing
aliases: [dynamically typed, strongly typed, duck typing]
tags: [python, fundamentals, typing, type-system]
category: fundamentals
level: 1
status: not-started
difficulty: beginner
created: 2026-11-23
updated: 2026-11-23
---

# Dynamic Typing

## 1. Definition

Python is **dynamically typed**: types are checked at runtime, not at compile time. Python is also **strongly typed**: it does not silently coerce values between incompatible types.

These two properties are independent and often confused.

---

## 2. The Precise Model

In Python, **names have no type. Objects have type.**

```python
x = 42        # x refers to an int object
x = "hello"   # x now refers to a str object — this is fine
x = [1, 2]    # x now refers to a list object
```

This isn't "x changed type" — `x` never had a type. What changed is what object `x` refers to.

---

## 3. Dynamically Typed vs Statically Typed

| Property | Python | Java / C# / Go |
|----------|--------|----------------|
| Type checking | At runtime | At compile time |
| Name declaration | No type annotation required | Type is part of the declaration |
| Type errors | Raised as `TypeError` when operation runs | Compile error — won't build |
| Flexibility | High | Lower, but caught earlier |

```python
# Python — no compile error, TypeError at runtime
x = "hello"
result = x + 1   # TypeError raised when this line executes

# Java — compile error before the program ever runs
// String x = "hello";
// int result = x + 1;  // compile error
```

---

## 4. Strongly Typed vs Weakly Typed

Python is **strongly typed**: it refuses implicit coercions between incompatible types.

```python
# Python — TypeError, refuses to guess
"3" + 3    # TypeError: can only concatenate str (not "int") to str

# JavaScript (weakly typed) — silently coerces
"3" + 3    // "33" — surprise!

# Python requires explicit conversion
int("3") + 3    # 6 — explicit and clear
```

Python does have some implicit coercions:
```python
True + 1    # 2  — bool is a subclass of int, coercion is intentional
3 + 3.0     # 6.0 — int promoted to float, standard numeric coercion
```

But these are intentional design decisions, not "anything goes."

---

## 5. Duck Typing

Python uses structural typing informally: "if it walks like a duck and quacks like a duck, it's a duck."

```python
def process(obj):
    return obj.upper()   # works on any object with an .upper() method

process("hello")                      # works
process(some_custom_string_wrapper)   # works if it has .upper()
# No isinstance() check required
```

This is powerful for composability. You don't need an interface hierarchy — just implement the methods that matter.

```python
# Anything with __len__ can be passed to len()
len("hello")       # 5
len([1, 2, 3])     # 3
len({"a": 1})      # 1

class MyCollection:
    def __len__(self):
        return 42

len(MyCollection())  # 42 — it quacks like a len-able
```

---

## 6. Type Hints

Dynamic typing doesn't mean you can't annotate types. Python 3.5+ supports type hints:

```python
def greet(name: str) -> str:
    return f"Hello, {name}!"

count: int = 0
items: list[str] = []
```

Type hints are **not enforced at runtime** by default. They are:
- Documentation
- Input for static type checkers (`mypy`, `pyright`)
- Enabling IDE autocompletion and error detection

```python
def add(a: int, b: int) -> int:
    return a + b

add("x", "y")   # No runtime error — type hints ignored at runtime
                 # mypy/pyright will flag this as an error
```

For production code, use type hints + `mypy` or `pyright`. This gives you the safety of static typing without losing Python's flexibility.

---

## 7. Common Mistakes

### Assuming Python checks types at runtime for hints

```python
def greet(name: str) -> str:
    return f"Hello, {name}!"

greet(42)   # No TypeError at runtime — the hint is advisory only
```

If you need runtime validation, use `pydantic` or `isinstance()`.

### Confusing "dynamically typed" with "untyped"

Python code is not untyped. Every object has a definite type at runtime. `type(42)` is `int`. You just don't declare the type of names upfront.

### Thinking duck typing means "no types"

Duck typing means "checked by capability, not inheritance." Types still matter — a list is not a dict. `{"key": "val"}[0]` raises `KeyError`, not "it'll work because it's flexible."

---

## 8. Practical Implications for Backend Dev

- **FastAPI** uses `pydantic` to enforce types at the API boundary — runtime validation
- **SQLAlchemy** models use type annotations for column definitions
- **Large codebases** run `mypy` in CI to catch type errors before deployment
- **Type hints + pyright in VS Code** catches bugs as you type them

The pattern: use duck typing internally for flexibility; enforce types at system boundaries (API endpoints, database I/O, configuration).

---

## 9. Related Concepts

- [[Variables and Assignment]] — why names have no type
- [[Object References]] — objects always have a type
- [[Basic Data Types]] — Python's built-in types
- [[Type Hints]] (Advanced Python) — annotations and static checking

---

## 10. Exercises

### Beginner

1. Demonstrate that Python raises a `TypeError` for `"3" + 3` but not for `3 + 3.0`. Explain why each behaves the way it does.
2. Write a function `double(x)` that works on both strings (`"ha"` → `"haha"`) and integers (`5` → `10`) using Python's duck typing (the `*` operator works on both).

### Intermediate

1. Write a function `get_length(obj)` that calls `len(obj)` on its argument. What types can you pass to it? Create a custom class that works with it.
2. Add type hints to three functions you've written. Run `mypy` on the file. Intentionally pass the wrong type and observe what `mypy` says vs what Python does at runtime.

---

## 11. Mastery Checklist

- [ ] I can explain the difference between dynamically typed and statically typed
- [ ] I can explain the difference between strongly typed and weakly typed, with examples
- [ ] I know why `"3" + 3` raises TypeError in Python but not in JavaScript
- [ ] I understand duck typing and can write code that uses it deliberately
- [ ] I know that type hints are not enforced at runtime
- [ ] I know when to use type hints and static checking in production code
