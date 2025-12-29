# Python Template

A minimalistic template for [Cursor](https://cursor.com/en)/[VSCode](https://code.visualstudio.com/) Python projects.
Uses [uv](https://docs.astral.sh/uv/), [ruff](https://docs.astral.sh/ruff/), and [mypy](https://mypy-lang.org/).

## Prerequisites

- [uv](https://docs.astral.sh/uv/) installed

### Cursor

- [Cursor](https://cursor.com/en)
- Prettier
- Ruff
- Mypy (by matangover NOT ms-python)

### VSCode

- [VSCode](https://code.visualstudio.com/)
- [Prettier](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode)
- [Ruff](https://marketplace.visualstudio.com/items?itemName=charliermarsh.ruff)
- [Mypy](https://marketplace.visualstudio.com/items?itemName=matangover.mypy)

## Set up

1. Initialise uv

```bash
uv init
```

2. Add development packages

```bash
uv add --dev ruff mypy
```

3. Add the below sections to your `pyproject.toml`:

```
[tool.ruff]
target-version = "py313"
line-length = 100

[tool.mypy]
python_version = "3.13"
strict = true
```

4. Add `.vscode/settings` to your folder

That's it!

## Optional: Set up for PyPI Publishing

If you want to publish your package to PyPI, follow these additional steps:

### 1. Add build system to `pyproject.toml`

Add this at the top of your `pyproject.toml`:

```toml
[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"
```

### 2. Update project metadata

Update your `[project]` section with publishing metadata:

```toml
[project]
name = "your-package-name"
dynamic = ["version"]  # Changed from static version
description = "Your package description"
readme = "README.md"
requires-python = ">=3.13"
license = "Apache-2.0"  # Or your chosen license
keywords = ["keyword1", "keyword2", "keyword3"]
authors = [
  { name = "Your Name", email = "your.email@example.com" },
]
classifiers = [
  "Development Status :: 4 - Beta",
  "Programming Language :: Python",
  "Programming Language :: Python :: 3.13",
  "Programming Language :: Python :: Implementation :: CPython",
  "Programming Language :: Python :: Implementation :: PyPy",
]
dependencies = [
  # Your runtime dependencies
]

[dependency-groups]
dev = [
    "mypy>=1.19.1",
    "ruff>=0.14.10",
]
```

### 3. Create package structure

Create a `src/` directory with your package:

```bash
mkdir -p src/your_package_name
```

### 4. Add version file

Create `src/your_package_name/__about__.py`:

```python
# SPDX-FileCopyrightText: 2025-present Your Name <your.email@example.com>
#
# SPDX-License-Identifier: Apache-2.0
__version__ = "0.1.0"
```

### 5. Add package init file

Create `src/your_package_name/__init__.py`:

```python
# SPDX-FileCopyrightText: 2025-present Your Name <your.email@example.com>
#
# SPDX-License-Identifier: Apache-2.0
from your_package_name.__about__ import __version__

__all__ = ["__version__"]
```

### 6. Configure hatch version and environments

Add these sections to `pyproject.toml`:

```toml
[project.urls]
Documentation = "https://github.com/yourusername/your-repo#readme"
Issues = "https://github.com/yourusername/your-repo/issues"
Source = "https://github.com/yourusername/your-repo"

[tool.hatch.version]
path = "src/your_package_name/__about__.py"

[tool.hatch.envs.types]
extra-dependencies = [
  "mypy>=1.0.0",
]
[tool.hatch.envs.types.scripts]
check = "mypy --install-types --non-interactive {args:src/your_package_name}"

[tool.hatch.envs.lint]
extra-dependencies = [
  "ruff"
]
[tool.hatch.envs.lint.scripts]
check = "ruff check {args:src/your_package_name}"
format = "ruff format {args:src/your_package_name}"
```

### 7. Build and publish

Build your package:

```bash
uv build
```

Publish to PyPI (requires PyPI account and token):

```bash
uv publish
```

Or publish to TestPyPI first:

```bash
uv publish --publish-url https://test.pypi.org/legacy/
```
