---
title: Learning Roadmap
tags: [roadmap, navigation]
updated: 2026-11-23
---

# Python Mastery Learning Roadmap

This roadmap takes you from environment setup to production-grade backend engineering. Work through levels in order — each level builds on the previous. Mark topics complete as you master them (not just read them).

---

## Level 0 — Environment and Workflow
**Outcome:** You can install, configure, and run Python professionally. You have a working development environment.
**Estimated effort:** 1–2 days

- [ ] Install Python 3.12+ via pyenv (or official installer on Windows)
- [ ] Understand Python version management with pyenv
- [ ] Create and activate virtual environments (`python -m venv`)
- [ ] Install packages with pip
- [ ] Understand `requirements.txt` and `pyproject.toml`
- [ ] Try `uv` as a faster pip/venv alternative
- [ ] Configure VS Code with Python extension
- [ ] Install Ruff for linting and formatting
- [ ] Run Python interactively (REPL)
- [ ] Run Python scripts (`python script.py`, `-m` flag)
- [ ] Basic Git workflow (init, add, commit, branch, push)

**Start here:** [[Python Introduction]], [[Python Environment Setup]]

---

## Level 1 — Python Fundamentals
**Outcome:** You understand Python's core syntax and—crucially—its object/name binding model. You can write correct, idiomatic Python for basic tasks.
**Estimated effort:** 1–2 weeks

- [ ] [[Variables and Assignment]] — name binding, not memory boxes
- [ ] [[Naming Conventions]] — PEP 8, snake_case, PascalCase, constants
- [ ] [[Multiple Assignment]] — chained assignment, what it means for mutables
- [ ] [[Unpacking]] — sequence, starred, nested, in for loops
- [ ] [[Dynamic Typing]] — dynamically typed but strongly typed, duck typing
- [ ] [[Object References]] — id(), is vs ==, int caching, string interning
- [ ] [[None]] — singleton, `is None` vs `== None`, sentinel pattern
- [ ] [[Basic Data Types]] — int, float, bool, str, bytes, NoneType internals
- [ ] [[Type Conversion]] — explicit cast, implicit coercion, common gotchas
- [ ] Input and output (`input()`, `print()`, f-strings)

---

## Level 2 — Control Flow
**Outcome:** You write clean, Pythonic control flow. You understand truthiness deeply.
**Estimated effort:** 3–5 days

- [ ] [[Conditional Statements]] — if/elif/else, truthiness, guard clauses
- [ ] [[Conditional Expressions]] — ternary syntax, when to use vs avoid
- [ ] [[Basic Operators]] — arithmetic, comparison, logical (return values!), bitwise
- [ ] `for` loops — iterating over sequences, `enumerate()`, `zip()`
- [ ] `while` loops — when to use, avoiding infinite loops
- [ ] `break`, `continue`, `pass`
- [ ] Loop `else` clause — rarely known, occasionally useful
- [ ] Truthiness deep dive — falsy values, `__bool__`, `__len__`

---

## Level 3 — Python Collections
**Outcome:** You understand Python's core data structures deeply, including mutability implications and time complexity.
**Estimated effort:** 1–2 weeks

- [ ] Lists — creation, indexing, slicing, methods, complexity
- [ ] Tuples — immutability, named tuples, when to prefer over lists
- [ ] Sets — hash-based, O(1) lookup, set operations
- [ ] Dictionaries — hash maps, O(1) access, ordering (Python 3.7+), methods
- [ ] `defaultdict`, `Counter`, `OrderedDict` from `collections`
- [ ] List/dict/set comprehensions — readable one-liners, when to stop
- [ ] Generator expressions — lazy evaluation
- [ ] Nested collections — common patterns and pitfalls
- [ ] Mutability implications — shared references in nested structures

---

## Level 4 — Functions
**Outcome:** You write clean, flexible Python functions. You understand how Python passes arguments and how scope works.
**Estimated effort:** 1 week

- [ ] Function definitions, return values, `None` returns
- [ ] Positional vs keyword arguments
- [ ] Default argument values — the mutable default trap
- [ ] `*args` — variadic positional arguments
- [ ] `**kwargs` — variadic keyword arguments
- [ ] Keyword-only arguments (`def f(a, *, b)`)
- [ ] Positional-only arguments (`def f(a, b, /)` — Python 3.8+)
- [ ] [[Scope and Namespace]] — LEGB rule
- [ ] `global` and `nonlocal` statements
- [ ] Recursion — base case, stack depth, tail recursion in Python
- [ ] Lambda functions — anonymous, limited, appropriate use
- [ ] First-class functions — functions as arguments, return values

---

## Level 5 — Python Programming Model
**Outcome:** You understand how Python works under the hood. This level transforms how you reason about Python code.
**Estimated effort:** 1–2 weeks

- [ ] [[Python Object Model]] — everything is an object
- [ ] Identity, type, value — the three properties of every object
- [ ] [[Memory and References]] — reference counting, the heap
- [ ] Mutable vs immutable objects — behavioral implications
- [ ] Shallow copy vs deep copy — `copy.copy()` vs `copy.deepcopy()`
- [ ] Equality vs identity — `==` vs `is`, when each is appropriate
- [ ] Garbage collection — reference counting + cyclic GC
- [ ] Python execution model — bytecode, CPython VM
- [ ] The significance for function arguments (pass-by-object-reference)

---

## Level 6 — OOP and Design
**Outcome:** You write clean, well-structured object-oriented Python. You understand Python's object model deeply.
**Estimated effort:** 2–3 weeks

- [ ] Classes, objects, `__init__`, `self`
- [ ] Instance attributes vs class attributes
- [ ] Instance methods, class methods (`@classmethod`), static methods (`@staticmethod`)
- [ ] Encapsulation — `_private`, `__name_mangled`, properties
- [ ] Inheritance — `super()`, MRO (C3 linearization)
- [ ] Multiple inheritance — when it's appropriate, `super()` with MRO
- [ ] Polymorphism — duck typing, `isinstance()`, `issubclass()`
- [ ] Abstraction — `abc.ABC`, `@abstractmethod`
- [ ] Composition over inheritance — when and why
- [ ] Dunder/magic methods — `__repr__`, `__str__`, `__eq__`, `__hash__`, `__len__`, `__getitem__`, etc.
- [ ] SOLID principles in Python
- [ ] Common design patterns — Factory, Strategy, Observer, Decorator

---

## Level 7 — Advanced Python
**Outcome:** You write sophisticated, Pythonic code using advanced language features.
**Estimated effort:** 2–3 weeks

- [ ] Closures — free variables, `__closure__`, the late binding trap
- [ ] Decorators — function decorators, class decorators, `functools.wraps`
- [ ] Iterators — `__iter__`, `__next__`, `StopIteration`
- [ ] Generators — `yield`, generator expressions, `send()`, `throw()`
- [ ] Context managers — `__enter__`, `__exit__`, `contextlib`
- [ ] Descriptors — `__get__`, `__set__`, `__delete__`, how `property` works
- [ ] Dataclasses — `@dataclass`, `field()`, `__post_init__`, comparison
- [ ] `Enum` — type-safe constants
- [ ] Type hints — `typing` module, generics, `Protocol`, `TypeVar`
- [ ] Structural typing and Protocols
- [ ] Functional programming — `map`, `filter`, `functools.reduce`, `partial`
- [ ] `itertools` — combinatorics, chaining, grouping
- [ ] Metaclasses — `type`, `__new__`, `__init_subclass__`

---

## Level 8 — Production Python
**Outcome:** You write code that behaves correctly in production — with proper error handling, logging, tests, and tooling.
**Estimated effort:** 2–3 weeks

- [ ] Exception hierarchy — `BaseException`, `Exception`, built-in exceptions
- [ ] `try`/`except`/`else`/`finally` — correct usage
- [ ] Custom exceptions — when and how to create them
- [ ] Exception chaining — `raise X from Y`
- [ ] Logging — `logging` module, handlers, formatters, levels
- [ ] Structured logging — JSON logs for production
- [ ] Testing with pytest — fixtures, parametrize, marks
- [ ] Mocking — `unittest.mock`, `pytest-mock`
- [ ] Test coverage — `pytest-cov`
- [ ] Type checking — `mypy` or `pyright`
- [ ] Ruff — linting and formatting
- [ ] Code quality — `pre-commit`, CI/CD integration
- [ ] Packaging — `pyproject.toml`, building distributions
- [ ] Configuration management — env vars, `pydantic-settings`
- [ ] Security basics — secrets, hashing, injection prevention

---

## Level 9 — Concurrency and Async
**Outcome:** You can write concurrent and asynchronous Python programs and understand the trade-offs.
**Estimated effort:** 2–3 weeks

- [ ] Processes vs threads — OS-level vs Python-level concurrency
- [ ] The GIL — what it is, what it blocks, what it doesn't
- [ ] `threading` — `Thread`, `Lock`, `Event`, `Queue`
- [ ] `multiprocessing` — `Process`, `Pool`, shared memory
- [ ] `concurrent.futures` — `ThreadPoolExecutor`, `ProcessPoolExecutor`
- [ ] Async programming fundamentals — event loop, non-blocking I/O
- [ ] `asyncio` — `async def`, `await`, `asyncio.run()`
- [ ] Coroutines — how they differ from threads
- [ ] `asyncio.Task` and `asyncio.gather()`
- [ ] Async generators and async context managers
- [ ] Race conditions and synchronization — `asyncio.Lock`
- [ ] Cancellation and timeouts — `asyncio.wait_for()`
- [ ] When to use threads vs processes vs async

---

## Level 10 — Backend Python
**Outcome:** You can build, test, and deploy production-grade Python backend services.
**Estimated effort:** 4–6 weeks

- [ ] HTTP fundamentals — methods, status codes, headers, body
- [ ] REST API design — resources, verbs, status codes, versioning
- [ ] FastAPI — routing, path/query params, request/response models
- [ ] Pydantic — validation, serialization, settings
- [ ] Django — MVT, ORM, migrations, admin
- [ ] Django REST Framework
- [ ] Request lifecycle and middleware
- [ ] Authentication — JWT, sessions, OAuth2
- [ ] Authorization — RBAC, permissions
- [ ] SQLAlchemy — Core vs ORM, sessions, queries, relationships
- [ ] Database migrations — Alembic
- [ ] Database indexing and query optimization
- [ ] Caching — Redis, cache invalidation strategies
- [ ] Background jobs — Celery, Redis as broker
- [ ] Observability — structured logging, metrics, tracing
- [ ] Deployment — Docker, environment config, health checks

---

## Level 11 — Data Structures and Algorithms
**Outcome:** You can analyze algorithm complexity and implement common data structures. You are prepared for technical interviews.
**Estimated effort:** 4–6 weeks (ongoing alongside other levels)

- [ ] Big O notation — time and space complexity
- [ ] Arrays — Python lists, complexity of operations
- [ ] Linked lists — singly, doubly, `collections.deque`
- [ ] Stacks — LIFO, Python list as stack
- [ ] Queues — FIFO, `collections.deque`, `queue.Queue`
- [ ] Hash maps — Python dict internals, collision handling
- [ ] Sets — hash set internals
- [ ] Binary trees — traversal (in/pre/post order), BST
- [ ] Graphs — adjacency list/matrix, BFS, DFS
- [ ] Heaps — `heapq`, priority queues
- [ ] Sorting — built-in sort (Timsort), merge sort, quicksort
- [ ] Searching — binary search
- [ ] Recursion and backtracking
- [ ] Dynamic programming — memoization, tabulation
- [ ] Greedy algorithms

---

## Level 12 — Projects
**Outcome:** You have built real Python projects demonstrating backend engineering skills.
**Estimated effort:** Ongoing

### Project Progression

1. **CLI Utility** — argument parsing with `argparse`, file I/O, error handling
2. **File Processing Tool** — read/transform/write data files, logging, type hints
3. **Expense Tracker CLI** — data persistence (JSON/SQLite), input validation
4. **Task Management CLI** — CRUD operations, file/DB backend, clean architecture
5. **REST API (FastAPI)** — routing, Pydantic models, database, tests
6. **Authentication System** — JWT, hashing, middleware, security best practices
7. **Background Job System** — Celery + Redis, task queuing, monitoring
8. **Database-backed Application** — SQLAlchemy ORM, migrations, queries
9. **Caching Layer** — Redis integration, cache invalidation, TTL
10. **Production Backend Service** — all of the above integrated, Docker, CI/CD

Each project should include: type hints, tests, logging, error handling, documentation.

---

## Revision Schedule

After mastering each concept, revise at these intervals:
- Day 1 (next day)
- Day 3
- Day 7
- Day 14
- Day 30

Use [[Revision Template]] for each session. See `10 - Revision/` folder.
