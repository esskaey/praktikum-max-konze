# Max Konze - Internship plan

- [Max Konze - Internship plan](#max-konze---internship-plan)
  - [Journey of a developer](#journey-of-a-developer)
  - [Creating a github account](#creating-a-github-account)
  - [Creating a github profile ( landing page )](#creating-a-github-profile--landing-page-)
    - [Resources](#resources)
  - [Python project - Objective](#python-project---objective)
  - [Creating a web based to-do app using django framework](#creating-a-web-based-to-do-app-using-django-framework)
    - [Folder structure](#folder-structure)
    - [pyproject.toml](#pyprojecttoml)
    - [Linting](#linting)
      - [Black](#black)
      - [isort](#isort)
      - [ruff](#ruff)
      - [Pre-commit config](#pre-commit-config)
      - [Pre-commit running](#pre-commit-running)
      - [Git hooks (via pre-commit)](#git-hooks-via-pre-commit)
    - [Testing](#testing)
  - [Useful links](#useful-links)

## Journey of a developer

## Creating a github account

## Creating a github profile ( landing page )

One of the first things to do as a passionate and enthusiastic developer, is creating a cool and catchy 😎 github profile.

### Resources
[Profile generator](https://rahuldkjain.github.io/gh-profile-readme-generator/)

## Python project - Objective

What do we want to achieve with the new python project ? Which technology and framework do we want to focus on ? 

## Creating a web based to-do app using django framework

### Folder structure

> Tip: Use tools like [figma](https://www.figma.com/) to create a rough frame to preview what are the interaction points of the user 

```
  backend/
  ├── README.md
  ├── requirements
  ├── ├── requirements_dev.txt  # contains linting, local development and other aspects
  ├── ├── requiremens_prod.txt  # contains requirements needed for deployments
  ├── config/                   # entry point to main app -> contains django related setup
  │   ├── settings.py
  │   ├── urls.py 
  │   ├── 
  │   ├── 
  │   └── 
  │       
  ├── to-do/                   # entry point to app
  │   ├── package1/
  │   │   ├── __init__.py
  │   │   ├── module1.py
  │   │   └── module2.py
  │   ├── package2/
  │   └── tests/
  │       └── test_module1.py
  └── scripts/
      └── utility_script.py
```

### [pyproject.toml](https://packaging.python.org/en/latest/guides/writing-pyproject-toml/)

 A full example is present in the link. Use it as a base

### Linting

Linting is one aspect of static code analyis: [static code analysis explained](https://luminousmen.com/post/python-static-analysis-tools)


#### Black

Black is the uncompromising Python code formatter. By using it, you agree to cede control over minutiae of hand-formatting. In return, Black gives you speed, determinism, and freedom from pycodestyle nagging about formatting. You will save time and mental energy for more important matters.

[Black docs](https://pypi.org/project/black/)

#### isort

isort your imports, so you don't have to.

isort is a Python utility / library to sort imports alphabetically, and automatically separated into sections and by type. It provides a command line utility, Python library and plugins for various editors to quickly sort all your imports. It requires Python 3.7+ to run but supports formatting Python 2 code too

[isort](https://pycqa.github.io/isort/)


#### ruff

An extremely fast Python linter and code formatter, written in Rust.

[ruff docs](https://github.com/astral-sh/ruff)



#### Pre-commit config

Git hook scripts are useful for identifying simple issues before submission to code review. We run our hooks on every commit to automatically point out issues in code such as missing semicolons, trailing whitespace, and debug statements. By pointing these issues out before code review, this allows a code reviewer to focus on the architecture of a change while not wasting time with trivial style nitpicks.

[pre-commit](https://pre-commit.com/)

```yaml
# .pre-commit-config.yaml file stored in the root of backend
# you find the full pre-commit-tools docu under:
# https://pre-commit.com/
repos:
  - repo: https://github.com/ambv/black
    rev: 23.3.0
    hooks:
      - id: black
        args: [--check, --diff, --config=./backend/pyproject.toml]
        language_version: python3.10
        stages: [push]

  - repo: https://github.com/pycqa/isort
    rev: 5.12.0
    hooks:
      - id: isort
        name: isort
        args: [--profile, black, --check, --diff, --sp=./backend/]
        language_version: python3.10
        stages: [push]

  - repo: https://github.com/charliermarsh/ruff-pre-commit
    # Ruff version.
    rev: 'v0.0.282'
    hooks:
      - id: ruff
        args: [--fix, --exit-non-zero-on-fix]

  - repo: https://github.com/asottile/pyupgrade
    rev: v3.3.2
    hooks:
      - id: pyupgrade
        args: [ --py310-plus ]
        language_version: python3.10
        stages: [ push ]

  - repo: https://github.com/adamchainz/django-upgrade
    rev: 1.13.0
    hooks:
      - id: django-upgrade
        args: [--target-version, "4.1"]
        language_version: python3.10
        stages: [ push ]

  - repo: https://github.com/rtts/djhtml
    rev: 3.0.6
    hooks:
      - id: djhtml
        # Indent only HTML files in template directories
        files: .*/templates/.*\.html$
        stages: [ push ]
        exclude: |
          (?x)^(
              .*/node_modules/.*|
          )$
      - id: djcss
        entry: djcss --tabwidth 2
        exclude: |
          (?x)^(
              .*/node_modules/.*|
          )$
        # Run this hook only on SCSS files (CSS and SCSS is the default)
        types: [ scss ]
        stages: [ push ]
      - id: djjs
        # Exclude JavaScript files in vendor directories
        exclude: |
          (?x)^(
              .*/node_modules/.*|
          )$
        stages: [ push ]
```

```yaml
# pyproject.toml -> add following parts to your pyproject.toml
[tool.black]
line-length = 120
target-version = ['py310']
multi_line_output = 3
include_trailing_comma = true

[tool.isort]
profile = "black"
line_length = 120
known_first_party = 'webapp' # change with name of your app

[tool.ruff]
# Enable pycodestyle (`E`) and Pyflakes (`F`) codes by default.
select = ["E", "F", "W", "N", "I", "B", "A", "DTZ", "DJ"]
ignore = [
    'N999',     # Project name contains underscore, not fixable
    'A003',     # Django attributes shadow python builtins
    'DJ001',    # Django model text-based fields shouldn't be nullable
    'DJ012',    # Odd ordering of Django model methods
]

# Allow autofix for all enabled rules (when `--fix`) is provided.
fixable =["E", "F", "W", "N", "I", "B", "A", "DTZ", "DJ"]
unfixable = []

exclude = [
    ".bzr",
    ".direnv",
    ".eggs",
    ".git",
    ".hg",
    ".mypy_cache",
    ".nox",
    ".pants.d",
    ".pytype",
    ".ruff_cache",
    ".svn",
    ".tox",
    ".venv",
    "__pypackages__",
    "_build",
    "buck-out",
    "build",
    "dist",
    "node_modules",
    "venv",
    "*/migrations/*"
]

# Same as Black.
line-length = 120

# Allow unused variables when underscore-prefixed.
dummy-variable-rgx = "^(_+|(_+[a-zA-Z0-9_]*[a-zA-Z0-9]+?))$"

# Assume Python 3.10.
target-version = ["py310","py311","py312"]
```
#### Pre-commit running

#### Git hooks (via pre-commit)

We use pre-push hooks to ensure that only linted code reaches our remote repository and pipelines aren't triggered in
vain.

To enable the configured pre-push hooks, you need to [install](https://pre-commit.com/) pre-commit and run once:

    pre-commit install -t pre-push -t pre-commit --install-hooks

This will permanently install the git hooks for both, front-end and backend, in your local
[`.git/hooks`](.git/hooks) folder.
The hooks are configured in the [`.pre-commit-config.yaml`](.pre-commit-config.yaml).

You can check whether hooks work as intended using the [run](https://pre-commit.com/#pre-commit-run) command:

    pre-commit run [hook-id] [options]

Example: run the single hook

    pre-commit run flake8 --all-files --hook-stage push

Example: run all hooks of pre-push stage

    pre-commit run --all-files --hook-stage push
    
### Testing
    
## Useful links

- [Django Templates](django_templates_intro.md)
- [Tips and tricks](tips_tricks.md)
