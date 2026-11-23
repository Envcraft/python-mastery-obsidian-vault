---
title: Python Environment Setup
aliases: []
tags: [python, environment, setup, tools]
category: fundamentals
level: 0
status: not-started
difficulty: beginner
created: 2026-11-23
updated: 2026-11-23
---

# Python Environment Setup

## 1. Version Management with pyenv

Never install Python directly from python.org for development work if you can avoid it. Use **pyenv** to manage multiple Python versions side by side.

### Install pyenv

**macOS / Linux:**
```bash
curl https://pyenv.run | bash
# Then add to your shell profile (~/.bashrc, ~/.zshrc):
export PYENV_ROOT="$HOME/.pyenv"
export PATH="$PYENV_ROOT/bin:$PATH"
eval "$(pyenv init -)"
```

**Windows:** Use [pyenv-win](https://github.com/pyenv-win/pyenv-win):
```powershell
Invoke-WebRequest -UseBasicParsing -Uri "https://raw.githubusercontent.com/pyenv-win/pyenv-win/master/pyenv-win/install-pyenv-win.ps1" -OutFile "./install-pyenv-win.ps1"; &"./install-pyenv-win.ps1"
```

### Use pyenv

```bash
pyenv install --list          # list available versions
pyenv install 3.12.7          # install a specific version
pyenv global 3.12.7           # set global default
pyenv local 3.12.7            # set version for current directory (creates .python-version)
pyenv version                 # show current version
python --version              # verify
```

---

## 2. Virtual Environments

**Always use a virtual environment.** Never install project dependencies into the global Python installation.

### Create and activate

```bash
# Create a venv in .venv directory
python -m venv .venv

# Activate — macOS/Linux
source .venv/bin/activate

# Activate — Windows (PowerShell)
.venv\Scripts\Activate.ps1

# Activate — Windows (Command Prompt)
.venv\Scripts\activate.bat

# Deactivate (any OS)
deactivate
```

When activated, your shell prompt shows `(.venv)` and `python`/`pip` refer to the venv's copies.

### What a venv does

A venv is an isolated Python environment with its own:
- Python interpreter binary (or symlink)
- `site-packages/` directory for installed packages
- `pip` binary

Packages installed in a venv don't affect other venvs or the global Python.

---

## 3. pip

```bash
pip install requests                   # install latest
pip install requests==2.31.0           # install specific version
pip install "requests>=2.28,<3"        # version constraint
pip install -r requirements.txt        # install from requirements file
pip install -e .                       # install current project in editable mode
pip list                               # list installed packages
pip freeze > requirements.txt          # capture current state
pip show requests                      # show package info
pip uninstall requests                 # remove
pip install --upgrade pip              # upgrade pip itself
```

**Always upgrade pip first** in a fresh venv:
```bash
python -m pip install --upgrade pip
```

---

## 4. Modern Alternatives: uv

**uv** is a next-generation Python package manager written in Rust. It's dramatically faster than pip and handles both venv creation and package installation.

```bash
# Install uv
pip install uv  # or: curl -LsSf https://astral.sh/uv/install.sh | sh

# Create venv and install packages
uv venv                           # create .venv
uv pip install requests           # install package
uv pip install -r requirements.txt
uv pip sync requirements.txt      # sync exactly (removes extra packages)

# Or use uv's project workflow
uv init myproject                 # create new project with pyproject.toml
uv add requests                   # add dependency to pyproject.toml
uv run python script.py           # run with project's venv
```

uv is the preferred tool for new projects in 2024+.

---

## 5. Dependency Files

### `requirements.txt` (traditional)

```text
requests==2.31.0
fastapi==0.104.1
pydantic==2.5.0
```

Generated with `pip freeze > requirements.txt`. Locks exact versions.

### `pyproject.toml` (modern standard)

```toml
[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

[project]
name = "my-project"
version = "0.1.0"
description = "Description"
requires-python = ">=3.12"
dependencies = [
    "requests>=2.28",
    "fastapi>=0.104",
    "pydantic>=2.0",
]

[project.optional-dependencies]
dev = [
    "pytest>=7.4",
    "ruff>=0.1",
    "mypy>=1.5",
]
```

`pyproject.toml` is the modern, PEP 517/518/621-compliant approach. Use it for all new projects.

---

## 6. VS Code Setup

1. Install [VS Code](https://code.visualstudio.com/)
2. Install the **Python** extension (ms-python.python)
3. Install the **Pylance** extension (ms-python.vscode-pylance) — language server
4. Install the **Ruff** extension (charliermarsh.ruff) — linting and formatting

### Select interpreter

`Ctrl+Shift+P` → "Python: Select Interpreter" → choose `.venv/bin/python` or `.venv\Scripts\python.exe`

### `.vscode/settings.json`

```json
{
    "[python]": {
        "editor.defaultFormatter": "charliermarsh.ruff",
        "editor.formatOnSave": true,
        "editor.codeActionsOnSave": {
            "source.fixAll.ruff": "explicit",
            "source.organizeImports.ruff": "explicit"
        }
    },
    "python.analysis.typeCheckingMode": "basic"
}
```

---

## 7. Ruff

Ruff is a fast Python linter and formatter written in Rust. It replaces flake8, isort, pyupgrade, and black in one tool.

```bash
pip install ruff  # or: uv add ruff --dev

ruff check .              # lint current directory
ruff check --fix .        # lint and auto-fix
ruff format .             # format (black-compatible)
ruff check --select I .   # check imports only
```

### `ruff.toml` or in `pyproject.toml`:

```toml
[tool.ruff]
line-length = 88
target-version = "py312"

[tool.ruff.lint]
select = ["E", "F", "I", "UP", "B"]
# E: pycodestyle errors
# F: pyflakes
# I: isort (import sorting)
# UP: pyupgrade (modern syntax)
# B: flake8-bugbear (common bugs)
```

---

## 8. Verifying Your Setup

Create `verify_setup.py` and run it:

```python
# Python 3.12+
import sys
import platform

print(f"Python version: {sys.version}")
print(f"Python executable: {sys.executable}")
print(f"Platform: {platform.platform()}")

# Verify you're in a venv
import os
in_venv = sys.prefix != sys.base_prefix
print(f"Virtual environment active: {in_venv}")
if not in_venv:
    print("WARNING: Not in a virtual environment!")

# Quick sanity checks
x: int = 42
name: str = "Python"
print(f"Hello from {name} {sys.version_info.major}.{sys.version_info.minor}!")
```

```bash
python verify_setup.py
```

Expected output:
```
Python version: 3.12.x (...)
Python executable: /path/to/.venv/bin/python
Virtual environment active: True
Hello from Python 3.12!
```

---

## 9. Recommended Project Structure

```
my-project/
├── .venv/                  # virtual environment (not committed)
├── src/
│   └── my_project/
│       ├── __init__.py
│       └── main.py
├── tests/
│   └── test_main.py
├── .gitignore
├── pyproject.toml
└── README.md
```

`.gitignore` should include:
```
.venv/
__pycache__/
*.pyc
.ruff_cache/
.mypy_cache/
dist/
*.egg-info/
```

---

## Related Concepts

- [[Python Introduction]]
- [[Variables and Assignment]]

---

## Mastery Checklist

- [ ] I can install a specific Python version with pyenv
- [ ] I can create and activate a virtual environment
- [ ] I understand what a virtual environment does and why it matters
- [ ] I can install packages with pip and uv
- [ ] I know the difference between `requirements.txt` and `pyproject.toml`
- [ ] I have VS Code configured with Ruff
- [ ] I can verify my setup is correct
