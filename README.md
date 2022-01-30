mypy mirror
===========

Mirror of mypy for pre-commit.

For pre-commit: see https://github.com/pre-commit/pre-commit
For mypy: see https://github.com/python/mypy

### Using mypy with pre-commit:

Add this to your `.pre-commit-config.yaml`

```yaml
-   repo: https://github.com/pre-commit/mirrors-mypy
    rev: ''  # Use the sha / tag you want to point at
    hooks:
    -   id: mypy
```


By default, mypy will run with `mypy --ignore-missing-imports`, `pre-commit`
runs `mypy` from an isolated virtualenv so it won't have access to those.
To change the arguments, override the `args` as follows:

```yaml
    hooks:
    -   id: mypy
        args: [--no-strict-optional, --ignore-missing-imports]
```

Because `pre-commit` runs `mypy` from an isolated virtualenv (without your
dependencies) you may also find it useful to add the typed dependencies to
`additional_dependencies` so `mypy` can better perform dynamic analysis:

```yaml
    hooks:
    -   id: mypy
        additional_dependencies: [tokenize-rt==3.2.0]
```

Note that using the `--install-types` is problematic. Mutating the pre-commit
environment at runtime breaks cache and will break parallel builds.

#### Excluding Files
pre-commit runs tools [by passing files](https://pre-commit.com/#arguments-pattern-in-hooks) to them. `mypy .` is not the equivalent call but rather `mypy $(git ls-files -- '*.py')` (or similar). 

As such, to exclude files from being checked by mypy in pre-commit you must exclude the folders using a regular expression. The `mypy.ini` or similar files are not checked. For example, to exclude everything in `db/migations/` and `dev_tools/`:
```
    hooks:
      - id: mypy
        exclude: db/migrations/|dev_tools/
```
