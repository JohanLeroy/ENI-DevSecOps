# CI/CD Documentation

## Pre-commit Hooks

Pre-commit hooks automatically check and fix code quality issues before commits.

### Installation

```bash
pip install pre-commit
pre-commit install
```

### Manual Execution

```bash
# Run all hooks on changed files
pre-commit run

# Run all hooks on all files
pre-commit run --all-files

# Run a specific hook
pre-commit run black --all-files
pre-commit run flake8 --all-files
```

### Hooks Configured

- **trailing-whitespace**: Remove trailing whitespace
- **end-of-file-fixer**: Ensure files end with newline
- **check-yaml**: Validate YAML syntax
- **check-json**: Validate JSON syntax
- **check-added-large-files**: Prevent large files (>1MB)
- **check-case-conflict**: Detect case conflicts
- **mixed-line-ending**: Fix mixed line endings
- **black**: Format Python code
- **isort**: Sort imports
- **flake8**: Lint Python code (max 100 chars/line)
- **bandit**: Security checks
- **yamllint**: Lint YAML files

## GitHub Actions CI/CD

### Workflow: Task Manager CI/CD

**Triggers:**
- Push to `main` or `develop` branches
- Pull requests to `main` branch

**Jobs:**

#### 1. **Lint & Format Check**
- Checks black formatting
- Checks isort import order
- Runs flake8 linting
- Runs bandit security checks

#### 2. **Run Tests**
- Depends on lint job passing
- Installs dependencies from `backend/requirements.txt`
- Runs pytest with coverage
- Requires tests in `backend/tests/` (optional)

#### 3. **Security Scan**
- Runs Trivy vulnerability scanner on filesystem
- Uploads results to GitHub Security tab

### Quick Fix

To automatically fix formatting issues locally:

```bash
black backend/
isort backend/
```

## Configuration Files

- **.pre-commit-config.yaml**: Pre-commit hooks configuration
- **setup.cfg**: isort, flake8, and pytest configuration
- **.flake8**: Flake8 linting rules
- **.bandit**: Bandit security configuration
- **.github/workflows/ci.yml**: GitHub Actions workflow

## Development Workflow

1. **Local Development**: Use pre-commit hooks to catch issues early
2. **Commit**: Pre-commit hooks run before commit
3. **Push**: GitHub Actions runs lint, test, and security checks
4. **PR**: Review status checks before merge
