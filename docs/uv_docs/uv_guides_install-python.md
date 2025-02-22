Skip to content 
# Installing Python
If Python is already installed on your system, uv will detect and use it without configuration. However, uv can also install and manage Python versions. uv automatically installs missing Python versions as needed — you don't need to install Python to get started.
## Getting started
To install the latest Python version:
```
$ uvpythoninstall

```

Note
Python does not publish official distributable binaries. As such, uv uses distributions from the Astral `python-build-standalone` project. See the Python distributions documentation for more details.
Once Python is installed, it will be used by `uv` commands automatically.
Important
When Python is installed by uv, it will not be available globally (i.e. via the `python` command). Support for this feature is in _preview_. See Installing Python executables for details.
You can still use `uv run` or create and activate a virtual environment to use `python` directly.
## Installing a specific version
To install a specific Python version:
```
$ uvpythoninstall3.12

```

To install multiple Python versions:
```
$ uvpythoninstall3.113.12

```

To install an alternative Python implementation, e.g. PyPy:
```
$ uvpythoninstallpypy@3.10

```

See the `python install` documentation for more details.
## Reinstalling Python
To reinstall uv-managed Python versions, use `--reinstall`, e.g.:
```
$ uvpythoninstall--reinstall

```

This will reinstall all previously installed Python versions. Improvements are constantly being added to the Python distributions, so reinstalling may resolve bugs even if the Python version does not change.
## Viewing Python installations
To view available and installed Python versions:
```
$ uvpythonlist

```

See the `python list` documentation for more details.
## Automatic Python downloads
Python does not need to be explicitly installed to use uv. By default, uv will automatically download Python versions when they are required. For example, the following would download Python 3.12 if it was not installed:
```
$ uvxpython@3.12-c"print('hello world')"

```

Even if a specific Python version is not requested, uv will download the latest version on demand. For example, if there are no Python versions on your system, the following will install Python before creating a new virtual environment:
```
$ uvvenv

```

Tip
Automatic Python downloads can be easily disabled if you want more control over when Python is downloaded.
## Using existing Python versions
uv will use existing Python installations if present on your system. There is no configuration necessary for this behavior: uv will use the system Python if it satisfies the requirements of the command invocation. See the Python discovery documentation for details.
To force uv to use the system Python, provide the `--python-preference only-system` option. See the Python version preference documentation for more details.
## Next steps
To learn more about `uv python`, see the Python version concept page and the command reference.
Or, read on to learn how to run scripts and invoke Python with uv.
Back to top 
![](https://cdn.usefathom.com/?h=https%3A%2F%2Fdocs.astral.sh&p=%2Fuv%2Fguides%2Finstall-python%2F&r=&sid=ESKBRHGN&qs=%7B%7D&cid=83198999)
