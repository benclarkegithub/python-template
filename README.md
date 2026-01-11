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

1. Copy (not clone) this template to your new project

2. Rename the package directory:

```bash
mv src/package_name src/new_package_name
```

3. Update the following files with your package name:

   - `pyproject.toml`: Update `name`, `authors`, `project.urls`, and `tool.hatch.version.path`
   - `release-please-config.json`: Update `package-name` and `extra-files` path
   - `src/new_package_name/__init__.py`: Update import
   - `tests/test_version.py`: Update import

4. Initialise the project:

```bash
uv sync
```

5. Run the checks:

```bash
uv run ruff check .
uv run ruff format --check .
uv run mypy .
uv run pytest
```

## CI/CD

This template includes GitHub Actions workflows for continuous integration and release automation.

### Workflows

| Workflow           | Trigger           | Description                               |
| ------------------ | ----------------- | ----------------------------------------- |
| **CI**             | Push/PR to main   | Runs ruff, mypy, and pytest with coverage |
| **Release Please** | Push to main      | Creates release PRs with changelogs       |
| **Publish**        | Release published | Publishes to PyPI (optional)              |

### Conventional Commits

This template uses [release-please](https://github.com/googleapis/release-please) for automated releases. Use [conventional commits](https://www.conventionalcommits.org/) to trigger version bumps:

| Commit prefix | Version bump  | Description                                              |
| ------------- | ------------- | -------------------------------------------------------- |
| `feat!:`      | Major (x.0.0) | Breaking changes that require major version bump         |
| `feat:`       | Minor (0.x.0) | New features that add functionality                      |
| `fix:`        | Patch (0.0.x) | Bug fixes                                                |
| `perf:`       | Patch (0.0.x) | Performance improvements                                 |
| `build:`      | None          | Build system or dependency changes                       |
| `chore:`      | None          | Maintenance tasks that don't affect production code      |
| `ci:`         | None          | CI/CD configuration changes                              |
| `docs:`       | None          | Documentation updates                                    |
| `refactor:`   | None          | Code restructuring without behavior changes              |
| `revert:`     | None          | Reverting previous commits                               |
| `style:`      | None          | Code formatting changes                                  |
| `test:`       | None          | Adding or updating tests                                 |

### Releases

1. Make commits using conventional commit format
2. Push to `main` branch
3. Release Please automatically creates/updates a Release PR
4. When ready, merge the Release PR
5. A GitHub Release is created automatically
6. If configured, the package is published to PyPI

### Publishing

#### Enable Publishing

The `publish.yml` workflow is included but requires setup.

To enable PyPI publishing:

1. **Create a PyPI account** at https://pypi.org

2. **Add Trusted Publisher on PyPI**:

   - Go to https://pypi.org/manage/account/publishing/
   - Add a new Pending Trusted Publisher:
     - **PyPI project name**: your-package-name
     - **Owner**: your-github-username
     - **Repository**: your-repo-name
     - **Workflow name**: `publish.yml`
     - **Environment name**: `pypi`

3. **Create GitHub Environment**:

   - Go to your repo Settings → Environments
   - Create a new environment named `pypi`
   - Optionally add protection rules (required reviewers, etc.)

4. **First publish**: The first release will register your package on PyPI using the pending publisher you created.

#### Disable Publishing

If you don't want to publish to PyPI, simply delete:

- `.github/workflows/publish.yml`

The CI and release-please workflows will continue to work independently.

## Project Structure

```
your-project/
├── .github/
│   └── workflows/
│       ├── ci.yml              # Lint, typecheck, test
│       ├── release-please.yml  # Automated releases
│       └── publish.yml         # PyPI publishing (optional)
├── src/
│   └── package_name/
│       ├── __init__.py
│       └── __about__.py        # Version (updated by release-please)
├── tests/
│   ├── __init__.py
│   └── test_version.py
├── .vscode/
│   └── settings.json
├── pyproject.toml
├── release-please-config.json
├── .release-please-manifest.json
└── README.md
```
