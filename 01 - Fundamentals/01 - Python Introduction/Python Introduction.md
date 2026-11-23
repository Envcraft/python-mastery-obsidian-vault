---
title: Python Introduction
aliases: []
tags: [python, fundamentals, introduction]
category: fundamentals
level: 0
status: not-started
difficulty: beginner
created: 2026-11-23
updated: 2026-11-23
---

# Python Introduction

## 1. What Python Is

Python is a high-level, interpreted, dynamically typed, garbage-collected programming language. It emphasizes readability, expressiveness, and developer productivity.

For backend development, Python is a first-class choice. FastAPI, Django, SQLAlchemy, Celery, and most of the modern data infrastructure stack are built in or for Python. If you're targeting backend engineering, Python is not a detour — it's the destination.

---

## 2. CPython: The Reference Implementation

When someone says "Python," they almost always mean **CPython** — the official implementation written in C, maintained at python.org.

There are other implementations:
- **PyPy** — Python with a JIT compiler, often 3–10x faster for CPU-bound code
- **Jython** — Python on the JVM (largely abandoned)
- **IronPython** — Python on .NET

Unless you have a specific reason, you are using CPython. This matters because some behavior — the GIL, reference counting, specific performance characteristics — is CPython-specific, not a Python language guarantee.

---

## 3. How Python Code Executes

Python does not compile to native machine code (by default). The execution pipeline:

```
source.py
    ↓
Python compiler (at import or run time)
    ↓
Bytecode (.pyc in __pycache__/)
    ↓
Python Virtual Machine (PVM / CPython interpreter)
    ↓
Result
```

You can inspect bytecode:

```python
import dis

def add(a, b):
    return a + b

dis.dis(add)
# LOAD_FAST 0 (a)
# LOAD_FAST 1 (b)
# BINARY_OP 0 (+)
# RETURN_VALUE
```

This is why Python is slower than compiled languages for CPU-bound tasks — the PVM interprets bytecode instructions, it doesn't execute native machine code. For I/O-bound work (web servers, APIs, database queries), this overhead is irrelevant.

---

## 4. Python's Design Philosophy

```python
import this
```

The Zen of Python (Tim Peters):

```
Beautiful is better than ugly.
Explicit is better than implicit.
Simple is better than complex.
Complex is better than complicated.
Flat is better than nested.
Sparse is better than dense.
Readability counts.
Special cases aren't special enough to break the rules.
Although practicality beats purity.
Errors should never pass silently.
Unless explicitly silenced.
In the face of ambiguity, refuse the temptation to guess.
There should be one-- and preferably only one --obvious way to do it.
Although that way may not be obvious at first unless you're Dutch.
Now is better than never.
Although never is often better than *right* now.
If the implementation is hard to explain, it's a bad idea.
If the implementation is easy to explain, it may be a good idea.
Namespaces are one honking great idea -- let's do more of those!
```

**What this means in practice:**
- Explicit over implicit: `import os` not `from os import *`
- Errors should never pass silently: don't swallow exceptions with bare `except:`
- One obvious way: Python has strong idioms — learn them and use them
- Readability counts: write for the reader, not the writer

---

## 5. Python Versions That Matter

You should be on **Python 3.12** or **3.13** for new projects. Never use Python 2.

| Version | Key Features for Backend Dev |
|---------|------------------------------|
| 3.10 | Structural pattern matching (`match`/`case`), better error messages |
| 3.11 | 10–60% faster than 3.10, `tomllib`, `ExceptionGroup` |
| 3.12 | f-string improvements, `@override`, better error messages, `type` statement |
| 3.13 | Free-threaded mode (no GIL, experimental), JIT compiler (experimental), `locals()` changes |

**Minimum for new projects:** Python 3.11+. Prefer 3.12.

Always specify your minimum Python version in `pyproject.toml`:

```toml
[project]
requires-python = ">=3.12"
```

---

## 6. Python's Type System

This is worth understanding precisely. Python is:

- **Dynamically typed**: Type checking happens at runtime, not compile time
- **Strongly typed**: Python does not silently coerce incompatible types

```python
# Dynamic: no type declaration needed, checked at runtime
x = 42        # x is bound to an int
x = "hello"   # x is now bound to a str — this is fine

# Strong: incompatible operations raise TypeError
"1" + 1       # TypeError: can only concatenate str (not "int") to str
# JavaScript would return "11"; Python refuses
```

The key insight: **names have no type. Objects have type.**

`x = 42` doesn't make `x` an integer variable. It makes the name `x` refer to the integer object `42`. When you do `x = "hello"`, you're rebinding `x` to a string object — not changing a variable's type.

This distinction matters enormously for understanding Python's behavior with function arguments, mutation, and references. See [[Variables and Assignment]] and [[Object References]].

---

## 7. The Interactive Interpreter

```bash
python  # or python3
```

The REPL (Read-Eval-Print Loop) is useful for:
- Exploring behavior quickly
- Checking what a function does
- Inspecting objects with `type()`, `dir()`, `help()`
- Prototyping a small algorithm

```python
>>> type(42)
<class 'int'>
>>> dir([])  # all methods on a list
>>> help(str.split)  # documentation for str.split
>>> import this   # Zen of Python
```

**Don't use the REPL as your primary development environment.** Write scripts.

---

## 8. Running Python

```bash
# Run a script
python script.py

# Run a module as a script (-m flag)
python -m pytest          # run pytest as a module
python -m http.server     # start a local HTTP server
python -m venv .venv      # create a virtual environment

# One-liner (-c flag)
python -c "print('hello')"

# Interactive after script (-i flag)
python -i script.py       # run script then drop into REPL with its namespace
```

**Shebang line** (Unix/macOS):
```python
#!/usr/bin/env python3
# This line tells the OS to use the system python3 to run this file
```

The `-m` flag is important: it runs a module as a script, using the module's directory as the working directory and ensuring imports work correctly. Prefer `python -m mypackage.cli` over `python src/mypackage/cli.py` in project scripts.

---

## 9. Python vs Languages You May Know

If you're coming from Java, C#, or JavaScript, these Python-specific behaviors are common trip points:

| Concept | Java/C# | Python |
|---------|---------|--------|
| Variables | Boxes that hold values | Names that refer to objects |
| Assignment | Copies the value into the box | Binds a name to an object |
| `==` | Value equality (usually) | Calls `__eq__`, which compares values |
| `is` | Reference equality (rarely used) | Identity check — same object in memory |
| Null | `null` / `None` | `None` — a singleton object |
| Interfaces | Explicit `implements` | Duck typing and Protocols |
| Private | `private` keyword | Convention: `_name` or `__name` |
| Semicolons | Required | Not used |
| Braces | Required for blocks | Indentation defines blocks |
| Increment | `i++` | `i += 1` (no `++` in Python) |
| Type system | Static (usually) | Dynamic with optional hints |

**The deepest difference:** in Java/C#, variables are typed containers. In Python, variables are names, and objects carry their own type. This distinction ripples through everything: function arguments, mutation, comparison, and memory management.

---

## 10. Related Concepts

- [[Variables and Assignment]]
- [[Object References]]
- [[Dynamic Typing]]
- [[Python Environment Setup]]
- [[Python Object Model]]

---

## 11. Mastery Checklist

- [ ] I can explain how Python code executes (source → bytecode → PVM)
- [ ] I understand the difference between CPython and "Python"
- [ ] I can explain why Python is "strongly typed" despite being "dynamically typed"
- [ ] I can run Python interactively and as a script
- [ ] I know what the `-m` flag does and why it matters
- [ ] I understand the key difference between Python and statically typed languages
