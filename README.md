pre-commit-hooks
================

Some out-of-the-box hooks for pre-commit.

See also: https://github.com/pre-commit/pre-commit


### Using pre-commit-hooks with pre-commit

Add this to your `.pre-commit-config.yaml`

    -   repo: https://github.com/pre-commit/pre-commit-hooks
        rev: v2.2.3  # Use the ref you want to point at
        hooks:
        -   id: trailing-whitespace
        # -   id: ...


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


Sample `.pre-commit-config.yaml`

```yaml
-   repo: https://github.com/asottile/reorder_python_imports
    rev: v1.5.0
    hooks:
    -   id: reorder-python-imports
```

## What does it do?

### Separates imports into three sections

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
