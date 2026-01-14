# Python Template

A minimalistic template for Python projects that use [Cursor](https://cursor.com/en)/[VSCode](https://code.visualstudio.com/) and [Claude Code](https://code.claude.com/docs/en/overview).
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

### Claude Code

- [Claude Code](https://code.claude.com/docs/en/overview)

## Basic Set Up

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

## Advanced Set Up

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

### CI/CD

This template includes GitHub Actions workflows for continuous integration and release automation.

#### Workflows

| Workflow           | Trigger           | Description                               |
| ------------------ | ----------------- | ----------------------------------------- |
| **CI**             | Push/PR to main   | Runs ruff, mypy, and pytest with coverage |
| **Release Please** | Push to main      | Creates release PRs with changelogs       |
| **Publish**        | Release published | Publishes to PyPI (optional)              |

#### Conventional Commits

This template uses [release-please](https://github.com/googleapis/release-please) for automated releases. Use [conventional commits](https://www.conventionalcommits.org/) to trigger version bumps.

| Commit prefix | Version bump  | Description                                         |
| ------------- | ------------- | --------------------------------------------------- |
| `feat!:`      | Major (x.0.0) | Breaking changes that require major version bump    |
| `feat:`       | Minor (0.x.0) | New features that add functionality                 |
| `fix:`        | Patch (0.0.x) | Bug fixes                                           |
| `perf:`       | Patch (0.0.x) | Performance improvements                            |
| `build:`      | None          | Build system or dependency changes                  |
| `chore:`      | None          | Maintenance tasks that don't affect production code |
| `ci:`         | None          | CI/CD configuration changes                         |
| `docs:`       | None          | Documentation updates                               |
| `refactor:`   | None          | Code restructuring without behavior changes         |
| `revert:`     | None          | Reverting previous commits                          |
| `style:`      | None          | Code formatting changes                             |
| `test:`       | None          | Adding or updating tests                            |

#### Releases

1. Make commits using conventional commit format
2. Push to `main` branch
3. Release Please automatically creates/updates a Release PR
4. When ready, merge the Release PR
5. A GitHub Release is created automatically
6. If configured, the package is published to PyPI

#### Publishing

##### Enable Publishing

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
   - Under "Deployment branches and tags":
     - Select "Selected branches and tags"
     - Click "Add deployment branch or tag rule"
     - Set "Ref type" to "Tag" and "Name pattern" to `*-v*` (matches `package-name-v1.0.0`)

The first release will register your package on PyPI using the pending publisher you created.

##### Disable Publishing

If you don't want to publish to PyPI, simply delete:

- `.github/workflows/publish.yml`

The CI and release-please workflows will continue to work independently.

#### GitHub Repository Settings

##### Branch Protection Rules

Protect your `main` branch to prevent accidental pushes and ensure code quality:

1. Go to Settings → Branches → Add classic branch protection rule
2. Branch name pattern: `main`
3. Enable the following:
   - **Require a pull request before merging**
     - Require approvals: 1
     - Dismiss stale pull request approvals when new commits are pushed
   - **Require status checks to pass before merging**
     - Require branches to be up to date before merging
     - Add status checks (after your first CI run):
       - `Lint & Format`
       - `Type Check`
       - `Test`
   - **Require linear history**

These rules ensure all code goes through PR review and passes CI checks before merging to `main`.

##### Pull Request Settings

Enforce linear history and clean commits:

1. Go to Settings → General → Pull Requests
2. Check the following:
   - Allow squash merging
   - Automatically delete head branches
3. Uncheck the following:
   - Allow merge commits
   - Allow rebase merging

This ensures every PR becomes a single, clean commit on `main` with a proper conventional commit message.

##### Workflow Permissions

Configure GitHub Actions permissions to allow Release Please to create pull requests:

**For personal repositories:**

1. Go to repository Settings → Actions → General
2. Scroll down to **Workflow permissions**
3. Select **"Read and write permissions"**
4. Check **"Allow GitHub Actions to create and approve pull requests"**
5. Click **Save**

**For organization repositories:**

If the workflow permissions option is greyed out in your repository settings, you need to configure this at the organization level:

1. Go to your **organization** Settings → Actions → General (requires organization owner permissions)
2. Scroll down to **Workflow permissions**
3. Select **"Read and write permissions"**
4. Check **"Allow GitHub Actions to create and approve pull requests"**
5. Click **Save**

Without these settings, the Release Please workflow will fail with: `GitHub Actions is not permitted to create or approve pull requests`

#### Claude Code GitHub Actions

Enable Claude to respond to `@claude` mentions in PRs and issues:

```bash
claude
/install-github-app
```

This installs the Claude GitHub App and configures the workflow. Once set up, mention `@claude` in any PR or issue comment to get AI assistance with code reviews, bug fixes, and feature implementation.

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
