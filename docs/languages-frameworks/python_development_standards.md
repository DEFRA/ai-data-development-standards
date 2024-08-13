# Python Development Standards Guide

This manual is designed to aid developers in writing clear and consistent Python code across projects at GDS.

## Table of Contents

1. [Code Formatting](#code-formatting)
2. [Maximum Line Length](#maximum-line-length)
3. [Linting](#linting)
4. [Environments](#environments)
5. [Dependencies](#dependencies)

## Code Formatting

We use `Black` to format our code. `Black` is an opinionated formatter that follows PEP 8. Where `Black` and PEP 8 don't express a view, we defer to the Google Python style guide. Use these as references unless something is explicitly mentioned here.

To add a new rule or exception, please create a pull request against this repo.

## Maximum Line Length

The maximum line length is 120 characters. This deviates from PEP 8's preferred 79 characters due to:

- Most developers now use IDEs that can handle longer lines
- GDS developers often work with nested JSON objects, ORM model definitions/queries, and long error/URL strings

## Linting

### Ruff

We recommend using the Ruff command-line checker as an all-in-one lint, code style, and complexity checker.

#### How to use Ruff

1. Add the `Ruff` module to your 'dev' or 'test' requirements/dependencies.
2. Run it alongside your unit tests.

#### Ruff Ignores

Ruff can ignore particular lines or files. You can specify rule exemptions per directory or file.

### Common Configuration

An example `pyproject.toml` configuration:
``` toml title="pyproject.toml" linenums="1"
[tool.black]
line-length = 120

[tool.ruff]
line-length = 120
target-version = "py311"
select = [
    "E",  # pycodestyle
    "W",  # pycodestyle
    "F",  # pyflakes
    "I",  # isort
    "B",  # flake8-bugbear
    "C90",  # mccabe cyclomatic complexity
    "G",  # flake8-logging-format
]
ignore = []
exclude = [
    "migrations/versions/"
]
```
## Environments

- Use `Pyenv` to manage different Python versions
- Use `pyenv-virtualenv` to manage virtual Python environments
- Use `direnv` to manage environment variables

## Dependencies
### Applications
For applications or script bundles:

1. Pin specific versions of dependencies for reproducibility
2. Keep dependencies up-to-date for security
3. Document the upgrade process in your `README`
4. Use `requirements.txt` for pinned dependencies
5. Consider using `pip-tools` for larger projects
6. Use `requirements-dev.txt` for development/testing dependencies

### Libraries
For installable Python repositories:

1. Use `setup.py` to specify dependencies and version ranges
2. Update `setup.py` when testing new versions outside the existing range
3. Use PEP 440 git references for dependencies not available on PyPI
4. Specify testing dependencies in `tox.ini` (if using Tox) or `requirements-dev.txt`

