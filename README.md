##added by lauraand doara at 0812
pre-commit-hooks
================

How to work with .pre-commit-config.yaml ?
1. Install [pre-commit](http://pre-commit.com/) in the brew/brew_packages. E.g. `brew install pre-commit`.
2. Run ./git/_init.sh to symlin .gitconfig and .gitignore files aslo setup git pre-commit hooks
3. Modify the code
4. Run git add .
5. Run git commit -m "**" which auto do the pre-commit chekcings according to .pre-commit-config.yaml

## General Usage

### Using pre-commit-hooks(https://github.com/gruntwork-io/pre-commit) with pre-commit

```yaml
repos:
  - repo: https://github.com/gruntwork-io/pre-commit
    rev: <VERSION> # Get the latest from: https://github.com/gruntwork-io/pre-commit/releases
    hooks:
      - id: terraform-fmt
      - id: gofmt
      - id: golint
```
### Hooks available
- `terraform-fmt` -  Automatically run `terraform fmt` on all Terraform code (`*.tf` files).
- `gofmt` - Automatically run `gofmt` on all Golang code (`*.go` files).
- `golint` - Automatically run `golint` on all Golang code (`*.go` files)
- `check-json` - Attempts to load all json files to verify syntax.


### Using pre-commit-hooks(https://github.com/pre-commit/pre-commit-hooks) with pre-commit

```yaml
repos:
    -   repo: https://github.com/pre-commit/pre-commit-hooks
        rev: v2.2.3  # Use the ref you want to point at
        hooks:
        -   id: trailing-whitespace
        -   id: end-of-file-fixer
        -   id: check-docstring-first
        -   id: check-json
        -   id: check-yaml
        -   id: debug-statements
        -   id: name-tests-test
        -   id: requirements-txt-fixer
        -   id: double-quote-string-fixer
```
### Hooks available
- `check-json` - Attempts to load all json files to verify syntax.
- `check-yaml` - Attempts to load all yaml files to verify syntax.
    - `--allow-multiple-documents` - allow yaml files which use the
      [multi-document syntax](http://www.yaml.org/spec/1.2/spec.html#YAML)
    - `--unsafe` - Instead of loading the files, simply parse them for syntax.
      A syntax-only check enables extensions and unsafe constructs which would
      otherwise be forbidden.  Using this option removes all guarantees of
      portability to other yaml implementations.
      Implies `--allow-multiple-documents`.
- `debug-statements` - Check for debugger imports and py37+ `breakpoint()`
  calls in python source.
- `double-quote-string-fixer` - This hook replaces double quoted strings
  with single quoted strings.
- `end-of-file-fixer` - Makes sure files end in a newline and only a newline.
- `name-tests-test` - Assert that files in tests/ end in `_test.py`.
    - Use `args: ['--django']` to match `test*.py` instead.
- `requirements-txt-fixer` - Sorts entries in requirements.txt and removes incorrect entry for `pkg-resources==0.0.0`
- `trailing-whitespace` - Trims trailing whitespace.
    - To preserve Markdown [hard linebreaks](https://github.github.com/gfm/#hard-line-break)
      use `args: [--markdown-linebreak-ext=md]` (or other extensions used
      by your markdownfiles).  If for some reason you want to treat all files
      as markdown, use `--markdown-linebreak-ext=*`.


### Using pre-commit-hooks(https://github.com/asottile/reorder_python_imports) with pre-commit

```yaml
-   repo: https://github.com/asottile/reorder_python_imports
    rev: v1.5.0
    hooks:
    -   id: reorder-python-imports
```

## What does it do?

### One Example: Separates imports into three sections

```python
import sys
import pyramid
import reorder_python_imports
```

becomes

```python
import sys

import pyramid

import reorder_python_imports
```

## why this style?

The style chosen by `reorder-python-imports` has a single aim: reduce merge
conflicts.
