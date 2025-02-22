Skip to content 
# Declaring dependencies
It is best practice to declare dependencies in a static file instead of modifying environments with ad-hoc installations. Once dependencies are defined, they can be locked to create a consistent, reproducible environment.
## Using `pyproject.toml`
The `pyproject.toml` file is the Python standard for defining configuration for a project.
To define project dependencies in a `pyproject.toml` file:
pyproject.toml```
[project]
dependencies=[
"httpx",
"ruff>=0.3.0"
]

```

To define optional dependencies in a `pyproject.toml` file:
pyproject.toml```
[project.optional-dependencies]
cli=[
"rich",
"click",
]

```

Each of the keys defines an "extra", which can be installed using the `--extra` and `--all-extras` flags or `package[<extra>]` syntax. See the documentation on installing packages for more details.
See the official `pyproject.toml` guide for more details on getting started with a `pyproject.toml`.
## Using `requirements.in`
It is also common to use a lightweight `requirements.txt` format to declare the dependencies for the project. Each requirement is defined on its own line. Commonly, this file is called `requirements.in` to distinguish it from `requirements.txt` which is used for the locked dependencies.
To define dependencies in a `requirements.in` file:
requirements.in```
httpx
ruff>=0.3.0

```

Optional dependencies groups are not supported in this format.
Back to top 
![](https://cdn.usefathom.com/?h=https%3A%2F%2Fdocs.astral.sh&p=%2Fuv%2Fpip%2Fdependencies%2F&r=&sid=ESKBRHGN&qs=%7B%7D&cid=80035606)
