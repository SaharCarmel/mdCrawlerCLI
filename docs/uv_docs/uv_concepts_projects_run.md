Skip to content 
# Running commands in projects
When working on a project, it is installed into the virtual environment at `.venv`. This environment is isolated from the current shell by default, so invocations that require the project, e.g., `python -c "import example"`, will fail. Instead, use `uv run` to run commands in the project environment:
```
$ uvrunpython-c"import example"

```

When using `run`, uv will ensure that the project environment is up-to-date before running the given command.
The given command can be provided by the project environment or exist outside of it, e.g.:
```
$ # Presuming the project provides `example-cli`
$ uvrunexample-clifoo

$ # Running a `bash` script that requires the project to be available
$ uvrunbashscripts/foo.sh

```

## Requesting additional dependencies
Additional dependencies or different versions of dependencies can be requested per invocation.
The `--with` option is used to include a dependency for the invocation, e.g., to request a different version of `httpx`:
```
$ uvrun--withhttpx==0.26.0python-c"import httpx; print(httpx.__version__)"
0.26.0
$ uvrun--withhttpx==0.25.0python-c"import httpx; print(httpx.__version__)"
0.25.0

```

The requested version will be respected regardless of the project's requirements. For example, even if the project requires `httpx==0.24.0`, the output above would be the same.
## Running scripts
Scripts that declare inline metadata are automatically executed in environments isolated from the project. See the scripts guide for more details.
For example, given a script:
example.py```
# /// script
# dependencies = [
#  "httpx",
# ]
# ///

import httpx

resp = httpx.get("https://peps.python.org/api/peps.json")
data = resp.json()
print([(k, v["title"]) for k, v in data.items()][:10])

```

The invocation `uv run example.py` would run _isolated_ from the project with only the given dependencies listed.
## Signal handling
uv does not cede control of the process to the spawned command in order to provide better error messages on failure. Consequently, uv is responsible for forwarding some signals to the child process the requested command runs in.
On Unix systems, uv will forward SIGINT and SIGTERM to the child process. Since shells send SIGINT to the foreground process group on Ctrl-C, uv will only forward a SIGINT to the child process if it is seen more than once or the child process group differs from uv's.
On Windows, these concepts do not apply and uv ignores Ctrl-C events, deferring handling to the child process so it can exit cleanly.
Back to top 
![](https://cdn.usefathom.com/?h=https%3A%2F%2Fdocs.astral.sh&p=%2Fuv%2Fconcepts%2Fprojects%2Frun%2F&r=&sid=ESKBRHGN&qs=%7B%7D&cid=25734627)
