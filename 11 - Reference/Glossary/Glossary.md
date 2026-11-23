---
title: Python Glossary
tags: [reference, glossary]
updated: 2026-11-23
---

# Python Glossary

Key terms with Python-specific precision. These are not generic programming definitions — they describe how Python specifically works.

---

**Object**
Everything in Python is an object. An object has three properties: identity (a unique integer returned by `id()`), type (determines what operations are valid), and value (the data it holds). Unlike some languages, even integers and functions are objects in Python.

---

**Reference**
A name or container slot that points to an object in memory. Python does not store objects in variables — it stores references to objects. Multiple references can point to the same object.

---

**Name Binding**
The act of associating a name with an object. `x = 5` does not "store 5 in x" — it binds the name `x` to the integer object `5`. Assignment, `import`, `def`, `class`, `for`, and `with` all create name bindings.

---

**Identity**
A unique integer that identifies an object for its lifetime. Retrieved with `id(obj)`. Two objects with the same value may have different identities. The `is` operator compares identity, not value.

---

**Mutability**
Whether an object's value can be changed after creation. Lists, dicts, and sets are mutable. Strings, tuples, and ints are immutable. Mutability affects how objects behave when shared between names or passed to functions.

---

**Immutability**
The property of an object whose value cannot change after creation. Immutable objects are safe to share — no reference can alter their value. Strings, integers, floats, booleans, tuples, and `frozenset` are immutable in Python.

---

**Namespace**
A mapping from names to objects. Python has several: built-in, global (module-level), enclosing (closure), and local (function). Every module, class, and function creates its own namespace.

---

**Scope**
The region of code where a name binding is visible. Python resolves names by searching scopes in LEGB order. Scope is determined statically (by where code is written), not dynamically.

---

**LEGB Rule**
The name resolution order Python uses: **L**ocal → **E**nclosing → **G**lobal → **B**uilt-in. Python searches each scope in this order and uses the first match found. See [[Scope and Namespace]].

---

**Closure**
A function that retains access to names from its enclosing scope, even after that scope has returned. The retained names are called free variables and are stored in the function's `__closure__` attribute.

---

**Decorator**
A callable that takes a function (or class) and returns a modified version of it. Syntactic sugar for `func = decorator(func)`. Used for cross-cutting concerns: logging, auth checks, caching, retry logic. See [[Decorators]].

---

**Generator**
A function that uses `yield` to produce values lazily, one at a time. Generators are iterators — they maintain state between `next()` calls. Memory-efficient for large sequences. See [[Iterators and Generators]].

---

**Iterator**
An object implementing `__iter__()` and `__next__()`. `__next__()` returns the next value or raises `StopIteration` when exhausted. All generators are iterators; not all iterators are generators.

---

**Coroutine**
A function defined with `async def`. Coroutines can suspend execution at `await` points, allowing other coroutines to run. They are the building block of `asyncio`. Not the same as threads — coroutines cooperate, threads preempt.

---

**GIL (Global Interpreter Lock)**
A mutex in CPython that allows only one thread to execute Python bytecode at a time. Prevents true parallelism for CPU-bound Python threads. Does not affect I/O-bound concurrent code (threads release the GIL during I/O) or multiprocessing. Removed in Python 3.13 as an optional build flag (PEP 703).

---

**CPython**
The reference implementation of Python, written in C. When someone says "Python," they usually mean CPython. Other implementations exist: PyPy (JIT compiler), Jython (JVM), IronPython (.NET). CPython compiles source to bytecode and runs it on the Python Virtual Machine (PVM).

---

**Bytecode**
An intermediate representation of Python code that CPython compiles source code into. Stored in `.pyc` files in `__pycache__/`. Not machine code — it runs on the Python Virtual Machine. Can be inspected with the `dis` module.

---

**Dunder / Magic Method**
Methods with double leading and trailing underscores (`__init__`, `__repr__`, `__len__`, etc.). They are called by Python's operators and built-in functions rather than directly. `len(x)` calls `x.__len__()`. They implement the Python Data Model.

---

**Descriptor**
An object that implements `__get__`, `__set__`, or `__delete__`, and is assigned as a class attribute. Descriptors power Python's `property`, `classmethod`, `staticmethod`, and much of the ORM layer in frameworks like SQLAlchemy and Django.

---

**Metaclass**
The class of a class. Just as objects are instances of classes, classes are instances of metaclasses. `type` is the default metaclass. Custom metaclasses can intercept class creation, modify class attributes, and enforce invariants.

---

**Comprehension**
A concise syntax for constructing lists, dicts, sets, or generators from iterables: `[x*2 for x in range(10) if x % 2 == 0]`. More Pythonic than equivalent `for` loops with `.append()`. Generator expressions use `()` instead of `[]` and are lazy.

---

**Unpacking**
Binding multiple names from a sequence or mapping in a single assignment: `a, b, c = [1, 2, 3]`. Starred unpacking (`a, *rest = seq`) captures remaining elements. Used in function calls (`f(*args, **kwargs)`) and for loop targets.

---

**Truthiness**
Whether an object evaluates to `True` or `False` in a boolean context. In Python, the following are falsy: `False`, `None`, `0`, `0.0`, `""`, `[]`, `{}`, `set()`, and objects where `__bool__` returns `False` or `__len__` returns `0`. Everything else is truthy.

---

**Type Hint**
An annotation declaring the expected type of a variable, parameter, or return value. Syntax: `def greet(name: str) -> str`. Not enforced at runtime by default — checked by static type checkers like `mypy` or `pyright`. See [[Type Hints]].

---

**Protocol**
A structural typing mechanism (`typing.Protocol`) that defines an interface by the methods and attributes an object must have, without requiring inheritance. If an object has the required methods, it satisfies the protocol — this is "structural subtyping" or "static duck typing."

---

**Dataclass**
A class decorated with `@dataclass` that auto-generates `__init__`, `__repr__`, `__eq__`, and optionally `__hash__`, `__lt__` etc., based on annotated class-level fields. Reduces boilerplate for data-holding classes. See [[Dataclasses]].

---

**Context Manager**
An object implementing `__enter__` and `__exit__`, used with the `with` statement. Guarantees cleanup code runs even if an exception occurs. Used for file handles, database connections, locks, and transactions.

---

**Garbage Collection**
CPython's memory management uses reference counting as the primary mechanism — when an object's reference count drops to zero, it is immediately deallocated. A cyclic garbage collector supplements this to handle reference cycles (objects that reference each other).

---

**Reference Counting**
CPython tracks how many references point to each object. The count increments when a reference is created and decrements when it is deleted. When the count reaches zero, the object is deallocated. `sys.getrefcount(obj)` returns the current count (always at least 1 due to the argument itself).

---

**Duck Typing**
"If it walks like a duck and quacks like a duck, it is a duck." Python checks whether an object supports the required operations at runtime, not whether it is an instance of a specific class. This enables flexible, composable code without rigid inheritance hierarchies.
