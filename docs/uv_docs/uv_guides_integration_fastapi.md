Skip to content 
# Using uv with FastAPI
FastAPI is a modern, high-performance Python web framework. You can use uv to manage your FastAPI project, including installing dependencies, managing environments, running FastAPI applications, and more.
Note
You can view the source code for this guide in the uv-fastapi-example repository.
## Migrating an existing FastAPI project
As an example, consider the sample application defined in the FastAPI documentation, structured as follows:
```
project
└── app
  ├── __init__.py
  ├── main.py
  ├── dependencies.py
  ├── routers
  │  ├── __init__.py
  │  ├── items.py
  │  └── users.py
  └── internal
    ├── __init__.py
    └── admin.py

```

To use uv with this application, inside the `project` directory run:
```
$ uvinit--app

```

This creates an project with an application layout and a `pyproject.toml` file.
Then, add a dependency on FastAPI:
```
$ uvaddfastapi--extrastandard

```

You should now have the following structure:
```
project
├── pyproject.toml
└── app
  ├── __init__.py
  ├── main.py
  ├── dependencies.py
  ├── routers
  │  ├── __init__.py
  │  ├── items.py
  │  └── users.py
  └── internal
    ├── __init__.py
    └── admin.py

```

And the contents of the `pyproject.toml` file should look something like this:
pyproject.toml```
[project]
name="uv-fastapi-example"
version="0.1.0"
description="FastAPI project"
readme="README.md"
requires-python=">=3.12"
dependencies=[
"fastapi[standard]",
]

```

From there, you can run the FastAPI application with:
```
$ uvrunfastapidev

```

`uv run` will automatically resolve and lock the project dependencies (i.e., create a `uv.lock` alongside the `pyproject.toml`), create a virtual environment, and run the command in that environment.
Test the app by opening http://127.0.0.1:8000/?token=jessica in a web browser.
## Deployment
To deploy the FastAPI application with Docker, you can use the following `Dockerfile`:
Dockerfile```
FROMpython:3.12-slim

# Install uv.
COPY--from=ghcr.io/astral-sh/uv:latest/uv/uvx/bin/

# Copy the application into the container.
COPY./app

# Install the application dependencies.
WORKDIR/app
RUNuvsync--frozen--no-cache

# Run the application.
CMD["/app/.venv/bin/fastapi","run","app/main.py","--port","80","--host","0.0.0.0"]

```

Build the Docker image with:
```
$ dockerbuild-tfastapi-app.

```

Run the Docker container locally with:
```
$ dockerrun-p8000:80fastapi-app

```

Navigate to http://127.0.0.1:8000/?token=jessica in your browser to verify that the app is running correctly.
Tip
For more on using uv with Docker, see the Docker guide.
Back to top 
![](https://cdn.usefathom.com/?h=https%3A%2F%2Fdocs.astral.sh&p=%2Fuv%2Fguides%2Fintegration%2Ffastapi%2F&r=&sid=ESKBRHGN&qs=%7B%7D&cid=8493907)
