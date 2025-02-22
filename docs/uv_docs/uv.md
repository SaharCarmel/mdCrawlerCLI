Skip to content 
# uv
An extremely fast Python package and project manager, written in Rust.
![Shows a bar chart with benchmark results.](https://github.com/astral-sh/uv/assets/1309177/629e59c0-9c6e-4013-9ad4-adb2bcf5080d#only-light)
![Shows a bar chart with benchmark results.](https://github.com/astral-sh/uv/assets/1309177/03aa9163-1c79-4a87-a31d-7a9311ed9310#only-dark)
_InstallingTrio's dependencies with a warm cache._
## Highlights
  * 🚀 A single tool to replace `pip`, `pip-tools`, `pipx`, `poetry`, `pyenv`, `twine`, `virtualenv`, and more.
  * ⚡️ 10-100x faster than `pip`.
  * 🗂️ Provides comprehensive project management, with a universal lockfile.
  * ❇️ Runs scripts, with support for inline dependency metadata.
  * 🐍 Installs and manages Python versions.
  * 🛠️ Runs and installs tools published as Python packages.
  * 🔩 Includes a pip-compatible interface for a performance boost with a familiar CLI.
  * 🏢 Supports Cargo-style workspaces for scalable projects.
  * 💾 Disk-space efficient, with a global cache for dependency deduplication.
  * ⏬ Installable without Rust or Python via `curl` or `pip`.
  * 🖥️ Supports macOS, Linux, and Windows.


uv is backed by Astral, the creators of Ruff.
## Installation
Install uv with our official standalone installer:
macOS and LinuxWindows
```
$ curl-LsSfhttps://astral.sh/uv/install.sh|sh

```

```
$ powershell-ExecutionPolicyByPass-c"irm https://astral.sh/uv/install.ps1 | iex"

```

Then, check out the first steps or read on for a brief overview.
Tip
uv may also be installed with pip, Homebrew, and more. See all of the methods on the installation page.
## Projects
uv manages project dependencies and environments, with support for lockfiles, workspaces, and more, similar to `rye` or `poetry`:
```
$ uvinitexample
Initialized project `example` at `/home/user/example`

$ cdexample

$ uvaddruff
Creating virtual environment at: .venv
Resolved 2 packages in 170ms
  Built example @ file:///home/user/example
Prepared 2 packages in 627ms
Installed 2 packages in 1ms
 + example==0.1.0 (from file:///home/user/example)
 + ruff==0.5.4

$ uvrunruffcheck
All checks passed!

$ uvlock
Resolved 2 packages in 0.33ms

$ uvsync
Resolved 2 packages in 0.70ms
Audited 1 package in 0.02ms

```

See the project guide to get started.
uv also supports building and publishing projects, even if they're not managed with uv. See the packaging guide to learn more.
## Scripts
uv manages dependencies and environments for single-file scripts.
Create a new script and add inline metadata declaring its dependencies:
```
$ echo'import requests; print(requests.get("https://astral.sh"))'>example.py

$ uvadd--scriptexample.pyrequests
Updated `example.py`

```

Then, run the script in an isolated virtual environment:
```
$ uvrunexample.py
Reading inline script metadata from: example.py
Installed 5 packages in 12ms
<Response [200]>

```

See the scripts guide to get started.
## Tools
uv executes and installs command-line tools provided by Python packages, similar to `pipx`.
Run a tool in an ephemeral environment using `uvx` (an alias for `uv tool run`):
```
$ uvxpycowsay'hello world!'
Resolved 1 package in 167ms
Installed 1 package in 9ms
 + pycowsay==0.0.0.2
 """

 ------------
< hello world! >
 ------------
  \  ^__^
  \ (oo)\_______
    (__)\    )\/\
      ||----w |
      ||   ||

```

Install a tool with `uv tool install`:
```
$ uvtoolinstallruff
Resolved 1 package in 6ms
Installed 1 package in 2ms
 + ruff==0.5.4
Installed 1 executable: ruff

$ ruff--version
ruff 0.5.4

```

See the tools guide to get started.
## Python versions
uv installs Python and allows quickly switching between versions.
Install multiple Python versions:
```
$ uvpythoninstall3.103.113.12
Searching for Python versions matching: Python 3.10
Searching for Python versions matching: Python 3.11
Searching for Python versions matching: Python 3.12
Installed 3 versions in 3.42s
 + cpython-3.10.14-macos-aarch64-none
 + cpython-3.11.9-macos-aarch64-none
 + cpython-3.12.4-macos-aarch64-none

```

Download Python versions as needed:
```
$ uvvenv--python3.12.0
Using CPython 3.12.0
Creating virtual environment at: .venv
Activate with: source .venv/bin/activate

$ uvrun--pythonpypy@3.8--python
Python 3.8.16 (a9dbdca6fc3286b0addd2240f11d97d8e8de187a, Dec 29 2022, 11:45:30)
[PyPy 7.3.11 with GCC Apple LLVM 13.1.6 (clang-1316.0.21.2.5)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>>>

```

Use a specific Python version in the current directory:
```
$ uvpythonpin3.11
Pinned `.python-version` to `3.11`

```

See the installing Python guide to get started.
## The pip interface
uv provides a drop-in replacement for common `pip`, `pip-tools`, and `virtualenv` commands.
uv extends their interfaces with advanced features, such as dependency version overrides, platform-independent resolutions, reproducible resolutions, alternative resolution strategies, and more.
Migrate to uv without changing your existing workflows — and experience a 10-100x speedup — with the `uv pip` interface.
Compile requirements into a platform-independent requirements file:
```
$ uvpipcompiledocs/requirements.in\
--universal\
--output-filedocs/requirements.txt
Resolved 43 packages in 12ms

```

Create a virtual environment:
```
$ uvvenv
Using CPython 3.12.3
Creating virtual environment at: .venv
Activate with: source .venv/bin/activate

```

Install the locked requirements:
```
$ uvpipsyncdocs/requirements.txt
Resolved 43 packages in 11ms
Installed 43 packages in 208ms
 + babel==2.15.0
 + black==24.4.2
 + certifi==2024.7.4
 ...

```

See the pip interface documentation to get started.
## Learn more
See the first steps or jump straight to the guides to start using uv.
Back to top 
![](https://cdn.usefathom.com/?h=https%3A%2F%2Fdocs.astral.sh&p=%2Fuv%2F&r=&sid=ESKBRHGN&qs=%7B%7D&cid=204628)
