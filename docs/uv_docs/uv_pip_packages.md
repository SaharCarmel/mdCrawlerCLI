Skip to content 
# Managing packages
## Installing a package
To install a package into the virtual environment, e.g., Flask:
```
$ uvpipinstallflask

```

To install a package with optional dependencies enabled, e.g., Flask with the "dotenv" extra:
```
$ uvpipinstall"flask[dotenv]"

```

To install multiple packages, e.g., Flask and Ruff:
```
$ uvpipinstallflaskruff

```

To install a package with a constraint, e.g., Ruff v0.2.0 or newer:
```
$ uvpipinstall'ruff>=0.2.0'

```

To install a package at a specific version, e.g., Ruff v0.3.0:
```
$ uvpipinstall'ruff==0.3.0'

```

To install a package from the disk:
```
$ uvpipinstall"ruff @ ./projects/ruff"

```

To install a package from GitHub:
```
$ uvpipinstall"git+https://github.com/astral-sh/ruff"

```

To install a package from GitHub at a specific reference:
```
$ # Install a tag
$ uvpipinstall"git+https://github.com/astral-sh/ruff@v0.2.0"

$ # Install a commit
$ uvpipinstall"git+https://github.com/astral-sh/ruff@1fadefa67b26508cc59cf38e6130bde2243c929d"

$ # Install a branch
$ uvpipinstall"git+https://github.com/astral-sh/ruff@main"

```

See the Git authentication documentation for installation from a private repository.
## Editable packages
Editable packages do not need to be reinstalled for changes to their source code to be active.
To install the current project as an editable package
```
$ uvpipinstall-e.

```

To install a project in another directory as an editable package:
```
$ uvpipinstall-e"ruff @ ./project/ruff"

```

## Installing packages from files
Multiple packages can be installed at once from standard file formats.
Install from a `requirements.txt` file:
```
$ uvpipinstall-rrequirements.txt

```

See the `uv pip compile` documentation for more information on `requirements.txt` files.
Install from a `pyproject.toml` file:
```
$ uvpipinstall-rpyproject.toml

```

Install from a `pyproject.toml` file with optional dependencies enabled, e.g., the "foo" extra:
```
$ uvpipinstall-rpyproject.toml--extrafoo

```

Install from a `pyproject.toml` file with all optional dependencies enabled:
```
$ uvpipinstall-rpyproject.toml--all-extras

```

## Uninstalling a package
To uninstall a package, e.g., Flask:
```
$ uvpipuninstallflask

```

To uninstall multiple packages, e.g., Flask and Ruff:
```
$ uvpipuninstallflaskruff

```

Back to top 
![](https://cdn.usefathom.com/?h=https%3A%2F%2Fdocs.astral.sh&p=%2Fuv%2Fpip%2Fpackages%2F&r=&sid=ESKBRHGN&qs=%7B%7D&cid=56439820)
