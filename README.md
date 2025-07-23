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
