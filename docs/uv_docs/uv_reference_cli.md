Skip to content 
# CLI Reference
## uv
An extremely fast Python package manager.
### Usage
```
uv [OPTIONS] <COMMAND>

```

### Commands
`uv run`
    
Run a command or script
`uv init`
    
Create a new project
`uv add`
    
Add dependencies to the project
`uv remove`
    
Remove dependencies from the project
`uv sync`
    
Update the project’s environment
`uv lock`
    
Update the project’s lockfile
`uv export`
    
Export the project’s lockfile to an alternate format
`uv tree`
    
Display the project’s dependency tree
`uv tool`
    
Run and install commands provided by Python packages
`uv python`
    
Manage Python versions and installations
`uv pip`
    
Manage Python packages with a pip-compatible interface
`uv venv`
    
Create a virtual environment
`uv build`
    
Build Python packages into source distributions and wheels
`uv publish`
    
Upload distributions to an index
`uv cache`
    
Manage uv’s cache
`uv self`
    
Manage the uv executable
`uv version`
    
Display uv’s version
`uv help`
    
Display documentation for a command
## uv run
Run a command or script.
Ensures that the command runs in a Python environment.
When used with a file ending in `.py` or an HTTP(S) URL, the file will be treated as a script and run with a Python interpreter, i.e., `uv run file.py` is equivalent to `uv run python file.py`. For URLs, the script is temporarily downloaded before execution. If the script contains inline dependency metadata, it will be installed into an isolated, ephemeral environment. When used with `-`, the input will be read from stdin, and treated as a Python script.
When used in a project, the project environment will be created and updated before invoking the command.
When used outside a project, if a virtual environment can be found in the current directory or a parent directory, the command will be run in that environment. Otherwise, the command will be run in the environment of the discovered interpreter.
Arguments following the command (or script) are not interpreted as arguments to uv. All options to uv must be provided before the command, e.g., `uv run --verbose foo`. A `--` can be used to separate the command from uv options for clarity, e.g., `uv run --python 3.12 -- python`.
### Usage
```
uv run [OPTIONS] [COMMAND]

```

### Options
`--active`
    
Prefer the active virtual environment over the project’s virtual environment.
If the project virtual environment is active or no virtual environment is active, this has no effect.
`--all-extras`
    
Include all optional dependencies.
Optional dependencies are defined via `project.optional-dependencies` in a `pyproject.toml`.
This option is only available when running in a project.
`--all-groups`
    
Include dependencies from all dependency groups.
`--no-group` can be used to exclude specific groups.
`--all-packages`
    
Run the command with all workspace members installed.
The workspace’s environment (`.venv`) is updated to include all workspace members.
Any extras or groups specified via `--extra`, `--group`, or related options will be applied to all workspace members.
`--allow-insecure-host` _allow-insecure-host_
    
Allow insecure connections to a host.
Can be provided multiple times.
Expects to receive either a hostname (e.g., `localhost`), a host-port pair (e.g., `localhost:8080`), or a URL (e.g., `https://localhost`).
WARNING: Hosts included in this list will not be verified against the system’s certificate store. Only use `--allow-insecure-host` in a secure network with verified sources, as it bypasses SSL verification and could expose you to MITM attacks.
May also be set with the `UV_INSECURE_HOST` environment variable.
`--cache-dir` _cache-dir_
    
Path to the cache directory.
Defaults to `$XDG_CACHE_HOME/uv` or `$HOME/.cache/uv` on macOS and Linux, and `%LOCALAPPDATA%\uv\cache` on Windows.
To view the location of the cache directory, run `uv cache dir`.
May also be set with the `UV_CACHE_DIR` environment variable.
`--color` _color-choice_
    
Control the use of color in output.
By default, uv will automatically detect support for colors when writing to a terminal.
Possible values:
  * `auto`: Enables colored output only when the output is going to a terminal or TTY with support
  * `always`: Enables colored output regardless of the detected environment
  * `never`: Disables colored output


`--compile-bytecode`
    
Compile Python files to bytecode after installation.
By default, uv does not compile Python (`.py`) files to bytecode (`__pycache__/*.pyc`); instead, compilation is performed lazily the first time a module is imported. For use-cases in which start time is critical, such as CLI applications and Docker containers, this option can be enabled to trade longer installation times for faster start times.
When enabled, uv will process the entire site-packages directory (including packages that are not being modified by the current operation) for consistency. Like pip, it will also ignore errors.
May also be set with the `UV_COMPILE_BYTECODE` environment variable.
`--config-file` _config-file_
    
The path to a `uv.toml` file to use for configuration.
While uv configuration can be included in a `pyproject.toml` file, it is not allowed in this context.
May also be set with the `UV_CONFIG_FILE` environment variable.
`--config-setting`, `-C` _config-setting_
    
Settings to pass to the PEP 517 build backend, specified as `KEY=VALUE` pairs
`--default-index` _default-index_
    
The URL of the default package index (by default: <https://pypi.org/simple>).
Accepts either a repository compliant with PEP 503 (the simple repository API), or a local directory laid out in the same format.
The index given by this flag is given lower priority than all other indexes specified via the `--index` flag.
May also be set with the `UV_DEFAULT_INDEX` environment variable.
`--directory` _directory_
    
Change to the given directory prior to running the command.
Relative paths are resolved with the given directory as the base.
See `--project` to only change the project root directory.
`--env-file` _env-file_
    
Load environment variables from a `.env` file.
Can be provided multiple times, with subsequent files overriding values defined in previous files.
May also be set with the `UV_ENV_FILE` environment variable.
`--exact`
    
Perform an exact sync, removing extraneous packages.
When enabled, uv will remove any extraneous packages from the environment. By default, `uv run` will make the minimum necessary changes to satisfy the requirements.
`--exclude-newer` _exclude-newer_
    
Limit candidate packages to those that were uploaded prior to the given date.
Accepts both RFC 3339 timestamps (e.g., `2006-12-02T02:07:43Z`) and local dates in the same format (e.g., `2006-12-02`) in your system’s configured time zone.
May also be set with the `UV_EXCLUDE_NEWER` environment variable.
`--extra` _extra_
    
Include optional dependencies from the specified extra name.
May be provided more than once.
Optional dependencies are defined via `project.optional-dependencies` in a `pyproject.toml`.
This option is only available when running in a project.
`--extra-index-url` _extra-index-url_
    
(Deprecated: use `--index` instead) Extra URLs of package indexes to use, in addition to `--index-url`.
Accepts either a repository compliant with PEP 503 (the simple repository API), or a local directory laid out in the same format.
All indexes provided via this flag take priority over the index specified by `--index-url` (which defaults to PyPI). When multiple `--extra-index-url` flags are provided, earlier values take priority.
May also be set with the `UV_EXTRA_INDEX_URL` environment variable.
`--find-links`, `-f` _find-links_
    
Locations to search for candidate distributions, in addition to those found in the registry indexes.
If a path, the target must be a directory that contains packages as wheel files (`.whl`) or source distributions (e.g., `.tar.gz` or `.zip`) at the top level.
If a URL, the page must contain a flat list of links to package files adhering to the formats described above.
May also be set with the `UV_FIND_LINKS` environment variable.
`--fork-strategy` _fork-strategy_
    
The strategy to use when selecting multiple versions of a given package across Python versions and platforms.
By default, uv will optimize for selecting the latest version of each package for each supported Python version (`requires-python`), while minimizing the number of selected versions across platforms.
Under `fewest`, uv will minimize the number of selected versions for each package, preferring older versions that are compatible with a wider range of supported Python versions or platforms.
May also be set with the `UV_FORK_STRATEGY` environment variable.
Possible values:
  * `fewest`: Optimize for selecting the fewest number of versions for each package. Older versions may be preferred if they are compatible with a wider range of supported Python versions or platforms
  * `requires-python`: Optimize for selecting latest supported version of each package, for each supported Python version


`--frozen`
    
Run without updating the `uv.lock` file.
Instead of checking if the lockfile is up-to-date, uses the versions in the lockfile as the source of truth. If the lockfile is missing, uv will exit with an error. If the `pyproject.toml` includes changes to dependencies that have not been included in the lockfile yet, they will not be present in the environment.
May also be set with the `UV_FROZEN` environment variable.
`--group` _group_
    
Include dependencies from the specified dependency group.
May be provided multiple times.
`--gui-script`
    
Run the given path as a Python GUI script.
Using `--gui-script` will attempt to parse the path as a PEP 723 script and run it with `pythonw.exe`, irrespective of its extension. Only available on Windows.
`--help`, `-h`
    
Display the concise help for this command
`--index` _index_
    
The URLs to use when resolving dependencies, in addition to the default index.
Accepts either a repository compliant with PEP 503 (the simple repository API), or a local directory laid out in the same format.
All indexes provided via this flag take priority over the index specified by `--default-index` (which defaults to PyPI). When multiple `--index` flags are provided, earlier values take priority.
May also be set with the `UV_INDEX` environment variable.
`--index-strategy` _index-strategy_
    
The strategy to use when resolving against multiple index URLs.
By default, uv will stop at the first index on which a given package is available, and limit resolutions to those present on that first index (`first-index`). This prevents "dependency confusion" attacks, whereby an attacker can upload a malicious package under the same name to an alternate index.
May also be set with the `UV_INDEX_STRATEGY` environment variable.
Possible values:
  * `first-index`: Only use results from the first index that returns a match for a given package name
  * `unsafe-first-match`: Search for every package name across all indexes, exhausting the versions from the first index before moving on to the next
  * `unsafe-best-match`: Search for every package name across all indexes, preferring the "best" version found. If a package version is in multiple indexes, only look at the entry for the first index


`--index-url`, `-i` _index-url_
    
(Deprecated: use `--default-index` instead) The URL of the Python package index (by default: <https://pypi.org/simple>).
Accepts either a repository compliant with PEP 503 (the simple repository API), or a local directory laid out in the same format.
The index given by this flag is given lower priority than all other indexes specified via the `--extra-index-url` flag.
May also be set with the `UV_INDEX_URL` environment variable.
`--isolated`
    
Run the command in an isolated virtual environment.
Usually, the project environment is reused for performance. This option forces a fresh environment to be used for the project, enforcing strict isolation between dependencies and declaration of requirements.
An editable installation is still used for the project.
When used with `--with` or `--with-requirements`, the additional dependencies will still be layered in a second environment.
`--keyring-provider` _keyring-provider_
    
Attempt to use `keyring` for authentication for index URLs.
At present, only `--keyring-provider subprocess` is supported, which configures uv to use the `keyring` CLI to handle authentication.
Defaults to `disabled`.
May also be set with the `UV_KEYRING_PROVIDER` environment variable.
Possible values:
  * `disabled`: Do not use keyring for credential lookup
  * `subprocess`: Use the `keyring` command for credential lookup


`--link-mode` _link-mode_
    
The method to use when installing packages from the global cache.
Defaults to `clone` (also known as Copy-on-Write) on macOS, and `hardlink` on Linux and Windows.
May also be set with the `UV_LINK_MODE` environment variable.
Possible values:
  * `clone`: Clone (i.e., copy-on-write) packages from the wheel into the `site-packages` directory
  * `copy`: Copy packages from the wheel into the `site-packages` directory
  * `hardlink`: Hard link packages from the wheel into the `site-packages` directory
  * `symlink`: Symbolically link packages from the wheel into the `site-packages` directory


`--locked`
    
Assert that the `uv.lock` will remain unchanged.
Requires that the lockfile is up-to-date. If the lockfile is missing or needs to be updated, uv will exit with an error.
May also be set with the `UV_LOCKED` environment variable.
`--module`, `-m`
    
Run a Python module.
Equivalent to `python -m <module>`.
`--native-tls`
    
Whether to load TLS certificates from the platform’s native certificate store.
By default, uv loads certificates from the bundled `webpki-roots` crate. The `webpki-roots` are a reliable set of trust roots from Mozilla, and including them in uv improves portability and performance (especially on macOS).
However, in some cases, you may want to use the platform’s native certificate store, especially if you’re relying on a corporate trust root (e.g., for a mandatory proxy) that’s included in your system’s certificate store.
May also be set with the `UV_NATIVE_TLS` environment variable.
`--no-binary`
    
Don’t install pre-built wheels.
The given packages will be built and installed from source. The resolver will still use pre-built wheels to extract package metadata, if available.
May also be set with the `UV_NO_BINARY` environment variable.
`--no-binary-package` _no-binary-package_
    
Don’t install pre-built wheels for a specific package
May also be set with the `UV_NO_BINARY_PACKAGE` environment variable.
`--no-build`
    
Don’t build source distributions.
When enabled, resolving will not run arbitrary Python code. The cached wheels of already-built source distributions will be reused, but operations that require building distributions will exit with an error.
`--no-build-isolation`
    
Disable isolation when building source distributions.
Assumes that build dependencies specified by PEP 518 are already installed.
May also be set with the `UV_NO_BUILD_ISOLATION` environment variable.
`--no-build-isolation-package` _no-build-isolation-package_
    
Disable isolation when building source distributions for a specific package.
Assumes that the packages’ build dependencies specified by PEP 518 are already installed.
`--no-build-package` _no-build-package_
    
Don’t build source distributions for a specific package
`--no-cache`, `-n`
    
Avoid reading from or writing to the cache, instead using a temporary directory for the duration of the operation
May also be set with the `UV_NO_CACHE` environment variable.
`--no-config`
    
Avoid discovering configuration files (`pyproject.toml`, `uv.toml`).
Normally, configuration files are discovered in the current directory, parent directories, or user configuration directories.
May also be set with the `UV_NO_CONFIG` environment variable.
`--no-default-groups`
    
Ignore the the default dependency groups.
uv includes the groups defined in `tool.uv.default-groups` by default. This disables that option, however, specific groups can still be included with `--group`.
`--no-dev`
    
Disable the development dependency group.
This option is an alias of `--no-group dev`. See `--no-default-groups` to disable all default groups instead.
This option is only available when running in a project.
`--no-editable`
    
Install any editable dependencies, including the project and any workspace members, as non-editable
`--no-env-file`
    
Avoid reading environment variables from a `.env` file
May also be set with the `UV_NO_ENV_FILE` environment variable.
`--no-extra` _no-extra_
    
Exclude the specified optional dependencies, if `--all-extras` is supplied.
May be provided multiple times.
`--no-group` _no-group_
    
Disable the specified dependency group.
This options always takes precedence over default groups, `--all-groups`, and `--group`.
May be provided multiple times.
`--no-index`
    
Ignore the registry index (e.g., PyPI), instead relying on direct URL dependencies and those provided via `--find-links`
`--no-progress`
    
Hide all progress outputs.
For example, spinners or progress bars.
May also be set with the `UV_NO_PROGRESS` environment variable.
`--no-project`
    
Avoid discovering the project or workspace.
Instead of searching for projects in the current directory and parent directories, run in an isolated, ephemeral environment populated by the `--with` requirements.
If a virtual environment is active or found in a current or parent directory, it will be used as if there was no project or workspace.
`--no-python-downloads`
    
Disable automatic downloads of Python.
`--no-sources`
    
Ignore the `tool.uv.sources` table when resolving dependencies. Used to lock against the standards-compliant, publishable package metadata, as opposed to using any workspace, Git, URL, or local path sources
`--no-sync`
    
Avoid syncing the virtual environment.
Implies `--frozen`, as the project dependencies will be ignored (i.e., the lockfile will not be updated, since the environment will not be synced regardless).
May also be set with the `UV_NO_SYNC` environment variable.
`--offline`
    
Disable network access.
When disabled, uv will only use locally cached data and locally available files.
May also be set with the `UV_OFFLINE` environment variable.
`--only-dev`
    
Only include the development dependency group.
The project and its dependencies will be omitted.
This option is an alias for `--only-group dev`. Implies `--no-default-groups`.
`--only-group` _only-group_
    
Only include dependencies from the specified dependency group.
The project and its dependencies will be omitted.
May be provided multiple times. Implies `--no-default-groups`.
`--package` _package_
    
Run the command in a specific package in the workspace.
If the workspace member does not exist, uv will exit with an error.
`--prerelease` _prerelease_
    
The strategy to use when considering pre-release versions.
By default, uv will accept pre-releases for packages that _only_ publish pre-releases, along with first-party requirements that contain an explicit pre-release marker in the declared specifiers (`if-necessary-or-explicit`).
May also be set with the `UV_PRERELEASE` environment variable.
Possible values:
  * `disallow`: Disallow all pre-release versions
  * `allow`: Allow all pre-release versions
  * `if-necessary`: Allow pre-release versions if all versions of a package are pre-release
  * `explicit`: Allow pre-release versions for first-party packages with explicit pre-release markers in their version requirements
  * `if-necessary-or-explicit`: Allow pre-release versions if all versions of a package are pre-release, or if the package has an explicit pre-release marker in its version requirements


`--project` _project_
    
Run the command within the given project directory.
All `pyproject.toml`, `uv.toml`, and `.python-version` files will be discovered by walking up the directory tree from the project root, as will the project’s virtual environment (`.venv`).
Other command-line arguments (such as relative paths) will be resolved relative to the current working directory.
See `--directory` to change the working directory entirely.
This setting has no effect when used in the `uv pip` interface.
`--python`, `-p` _python_
    
The Python interpreter to use for the run environment.
If the interpreter request is satisfied by a discovered environment, the environment will be used.
See uv python to view supported request formats.
May also be set with the `UV_PYTHON` environment variable.
`--python-preference` _python-preference_
    
Whether to prefer uv-managed or system Python installations.
By default, uv prefers using Python versions it manages. However, it will use system Python installations if a uv-managed Python is not installed. This option allows prioritizing or ignoring system Python installations.
May also be set with the `UV_PYTHON_PREFERENCE` environment variable.
Possible values:
  * `only-managed`: Only use managed Python installations; never use system Python installations
  * `managed`: Prefer managed Python installations over system Python installations
  * `system`: Prefer system Python installations over managed Python installations
  * `only-system`: Only use system Python installations; never use managed Python installations


`--quiet`, `-q`
    
Do not print any output
`--refresh`
    
Refresh all cached data
`--refresh-package` _refresh-package_
    
Refresh cached data for a specific package
`--reinstall`
    
Reinstall all packages, regardless of whether they’re already installed. Implies `--refresh`
`--reinstall-package` _reinstall-package_
    
Reinstall a specific package, regardless of whether it’s already installed. Implies `--refresh-package`
`--resolution` _resolution_
    
The strategy to use when selecting between the different compatible versions for a given package requirement.
By default, uv will use the latest compatible version of each package (`highest`).
May also be set with the `UV_RESOLUTION` environment variable.
Possible values:
  * `highest`: Resolve the highest compatible version of each package
  * `lowest`: Resolve the lowest compatible version of each package
  * `lowest-direct`: Resolve the lowest compatible version of any direct dependencies, and the highest compatible version of any transitive dependencies


`--script`, `-s`
    
Run the given path as a Python script.
Using `--script` will attempt to parse the path as a PEP 723 script, irrespective of its extension.
`--upgrade`, `-U`
    
Allow package upgrades, ignoring pinned versions in any existing output file. Implies `--refresh`
`--upgrade-package`, `-P` _upgrade-package_
    
Allow upgrades for a specific package, ignoring pinned versions in any existing output file. Implies `--refresh-package`
`--verbose`, `-v`
    
Use verbose output.
You can configure fine-grained logging using the `RUST_LOG` environment variable. (<https://docs.rs/tracing-subscriber/latest/tracing_subscriber/filter/struct.EnvFilter.html#directives>)
`--version`, `-V`
    
Display the uv version
`--with` _with_
    
Run with the given packages installed.
When used in a project, these dependencies will be layered on top of the project environment in a separate, ephemeral environment. These dependencies are allowed to conflict with those specified by the project.
`--with-editable` _with-editable_
    
Run with the given packages installed in editable mode.
When used in a project, these dependencies will be layered on top of the project environment in a separate, ephemeral environment. These dependencies are allowed to conflict with those specified by the project.
`--with-requirements` _with-requirements_
    
Run with all packages listed in the given `requirements.txt` files.
The same environment semantics as `--with` apply.
Using `pyproject.toml`, `setup.py`, or `setup.cfg` files is not allowed.
## uv init
Create a new project.
Follows the `pyproject.toml` specification.
If a `pyproject.toml` already exists at the target, uv will exit with an error.
If a `pyproject.toml` is found in any of the parent directories of the target path, the project will be added as a workspace member of the parent.
Some project state is not created until needed, e.g., the project virtual environment (`.venv`) and lockfile (`uv.lock`) are lazily created during the first sync.
### Usage
```
uv init [OPTIONS] [PATH]

```

### Arguments
`PATH`
    
The path to use for the project/script.
Defaults to the current working directory when initializing an app or library; required when initializing a script. Accepts relative and absolute paths.
If a `pyproject.toml` is found in any of the parent directories of the target path, the project will be added as a workspace member of the parent, unless `--no-workspace` is provided.
### Options
`--allow-insecure-host` _allow-insecure-host_
    
Allow insecure connections to a host.
Can be provided multiple times.
Expects to receive either a hostname (e.g., `localhost`), a host-port pair (e.g., `localhost:8080`), or a URL (e.g., `https://localhost`).
WARNING: Hosts included in this list will not be verified against the system’s certificate store. Only use `--allow-insecure-host` in a secure network with verified sources, as it bypasses SSL verification and could expose you to MITM attacks.
May also be set with the `UV_INSECURE_HOST` environment variable.
`--app`
    
Create a project for an application.
This is the default behavior if `--lib` is not requested.
This project kind is for web servers, scripts, and command-line interfaces.
By default, an application is not intended to be built and distributed as a Python package. The `--package` option can be used to create an application that is distributable, e.g., if you want to distribute a command-line interface via PyPI.
`--author-from` _author-from_
    
Fill in the `authors` field in the `pyproject.toml`.
By default, uv will attempt to infer the author information from some sources (e.g., Git) (`auto`). Use `--author-from git` to only infer from Git configuration. Use `--author-from none` to avoid inferring the author information.
Possible values:
  * `auto`: Fetch the author information from some sources (e.g., Git) automatically
  * `git`: Fetch the author information from Git configuration only
  * `none`: Do not infer the author information


`--bare`
    
Only create a `pyproject.toml`.
Disables creating extra files like `README.md`, the `src/` tree, `.python-version` files, etc.
`--build-backend` _build-backend_
    
Initialize a build-backend of choice for the project.
Implicitly sets `--package`.
Possible values:
  * `hatch`: Use hatchling as the project build backend
  * `flit`: Use flit-core as the project build backend
  * `pdm`: Use pdm-backend as the project build backend
  * `setuptools`: Use setuptools as the project build backend
  * `maturin`: Use maturin as the project build backend
  * `scikit`: Use scikit-build-core as the project build backend


`--cache-dir` _cache-dir_
    
Path to the cache directory.
Defaults to `$XDG_CACHE_HOME/uv` or `$HOME/.cache/uv` on macOS and Linux, and `%LOCALAPPDATA%\uv\cache` on Windows.
To view the location of the cache directory, run `uv cache dir`.
May also be set with the `UV_CACHE_DIR` environment variable.
`--color` _color-choice_
    
Control the use of color in output.
By default, uv will automatically detect support for colors when writing to a terminal.
Possible values:
  * `auto`: Enables colored output only when the output is going to a terminal or TTY with support
  * `always`: Enables colored output regardless of the detected environment
  * `never`: Disables colored output


`--config-file` _config-file_
    
The path to a `uv.toml` file to use for configuration.
While uv configuration can be included in a `pyproject.toml` file, it is not allowed in this context.
May also be set with the `UV_CONFIG_FILE` environment variable.
`--description` _description_
    
Set the project description
`--directory` _directory_
    
Change to the given directory prior to running the command.
Relative paths are resolved with the given directory as the base.
See `--project` to only change the project root directory.
`--help`, `-h`
    
Display the concise help for this command
`--lib`
    
Create a project for a library.
A library is a project that is intended to be built and distributed as a Python package.
`--name` _name_
    
The name of the project.
Defaults to the name of the directory.
`--native-tls`
    
Whether to load TLS certificates from the platform’s native certificate store.
By default, uv loads certificates from the bundled `webpki-roots` crate. The `webpki-roots` are a reliable set of trust roots from Mozilla, and including them in uv improves portability and performance (especially on macOS).
However, in some cases, you may want to use the platform’s native certificate store, especially if you’re relying on a corporate trust root (e.g., for a mandatory proxy) that’s included in your system’s certificate store.
May also be set with the `UV_NATIVE_TLS` environment variable.
`--no-cache`, `-n`
    
Avoid reading from or writing to the cache, instead using a temporary directory for the duration of the operation
May also be set with the `UV_NO_CACHE` environment variable.
`--no-config`
    
Avoid discovering configuration files (`pyproject.toml`, `uv.toml`).
Normally, configuration files are discovered in the current directory, parent directories, or user configuration directories.
May also be set with the `UV_NO_CONFIG` environment variable.
`--no-description`
    
Disable the description for the project
`--no-package`
    
Do not set up the project to be built as a Python package.
Does not include a `[build-system]` for the project.
This is the default behavior when using `--app`.
`--no-pin-python`
    
Do not create a `.python-version` file for the project.
By default, uv will create a `.python-version` file containing the minor version of the discovered Python interpreter, which will cause subsequent uv commands to use that version.
`--no-progress`
    
Hide all progress outputs.
For example, spinners or progress bars.
May also be set with the `UV_NO_PROGRESS` environment variable.
`--no-python-downloads`
    
Disable automatic downloads of Python.
`--no-readme`
    
Do not create a `README.md` file
`--no-workspace`
    
Avoid discovering a workspace and create a standalone project.
By default, uv searches for workspaces in the current directory or any parent directory.
`--offline`
    
Disable network access.
When disabled, uv will only use locally cached data and locally available files.
May also be set with the `UV_OFFLINE` environment variable.
`--package`
    
Set up the project to be built as a Python package.
Defines a `[build-system]` for the project.
This is the default behavior when using `--lib` or `--build-backend`.
When using `--app`, this will include a `[project.scripts]` entrypoint and use a `src/` project structure.
`--project` _project_
    
Run the command within the given project directory.
All `pyproject.toml`, `uv.toml`, and `.python-version` files will be discovered by walking up the directory tree from the project root, as will the project’s virtual environment (`.venv`).
Other command-line arguments (such as relative paths) will be resolved relative to the current working directory.
See `--directory` to change the working directory entirely.
This setting has no effect when used in the `uv pip` interface.
`--python`, `-p` _python_
    
The Python interpreter to use to determine the minimum supported Python version.
See uv python to view supported request formats.
May also be set with the `UV_PYTHON` environment variable.
`--python-preference` _python-preference_
    
Whether to prefer uv-managed or system Python installations.
By default, uv prefers using Python versions it manages. However, it will use system Python installations if a uv-managed Python is not installed. This option allows prioritizing or ignoring system Python installations.
May also be set with the `UV_PYTHON_PREFERENCE` environment variable.
Possible values:
  * `only-managed`: Only use managed Python installations; never use system Python installations
  * `managed`: Prefer managed Python installations over system Python installations
  * `system`: Prefer system Python installations over managed Python installations
  * `only-system`: Only use system Python installations; never use managed Python installations


`--quiet`, `-q`
    
Do not print any output
`--script`
    
Create a script.
A script is a standalone file with embedded metadata enumerating its dependencies, along with any Python version requirements, as defined in the PEP 723 specification.
PEP 723 scripts can be executed directly with `uv run`.
By default, adds a requirement on the system Python version; use `--python` to specify an alternative Python version requirement.
`--vcs` _vcs_
    
Initialize a version control system for the project.
By default, uv will initialize a Git repository (`git`). Use `--vcs none` to explicitly avoid initializing a version control system.
Possible values:
  * `git`: Use Git for version control
  * `none`: Do not use any version control system


`--verbose`, `-v`
    
Use verbose output.
You can configure fine-grained logging using the `RUST_LOG` environment variable. (<https://docs.rs/tracing-subscriber/latest/tracing_subscriber/filter/struct.EnvFilter.html#directives>)
`--version`, `-V`
    
Display the uv version
## uv add
Add dependencies to the project.
Dependencies are added to the project's `pyproject.toml` file.
If a given dependency exists already, it will be updated to the new version specifier unless it includes markers that differ from the existing specifier in which case another entry for the dependency will be added.
If no constraint or URL is provided for a dependency, a lower bound is added equal to the latest compatible version of the package, e.g., `>=1.2.3`, unless `--frozen` is provided, in which case no resolution is performed.
The lockfile and project environment will be updated to reflect the added dependencies. To skip updating the lockfile, use `--frozen`. To skip updating the environment, use `--no-sync`.
If any of the requested dependencies cannot be found, uv will exit with an error, unless the `--frozen` flag is provided, in which case uv will add the dependencies verbatim without checking that they exist or are compatible with the project.
uv will search for a project in the current directory or any parent directory. If a project cannot be found, uv will exit with an error.
### Usage
```
uv add [OPTIONS] <PACKAGES|--requirements <REQUIREMENTS>>

```

### Arguments
`PACKAGES`
    
The packages to add, as PEP 508 requirements (e.g., `ruff==0.5.0`)
### Options
`--active`
    
Prefer the active virtual environment over the project’s virtual environment.
If the project virtual environment is active or no virtual environment is active, this has no effect.
`--allow-insecure-host` _allow-insecure-host_
    
Allow insecure connections to a host.
Can be provided multiple times.
Expects to receive either a hostname (e.g., `localhost`), a host-port pair (e.g., `localhost:8080`), or a URL (e.g., `https://localhost`).
WARNING: Hosts included in this list will not be verified against the system’s certificate store. Only use `--allow-insecure-host` in a secure network with verified sources, as it bypasses SSL verification and could expose you to MITM attacks.
May also be set with the `UV_INSECURE_HOST` environment variable.
`--branch` _branch_
    
Branch to use when adding a dependency from Git
`--cache-dir` _cache-dir_
    
Path to the cache directory.
Defaults to `$XDG_CACHE_HOME/uv` or `$HOME/.cache/uv` on macOS and Linux, and `%LOCALAPPDATA%\uv\cache` on Windows.
To view the location of the cache directory, run `uv cache dir`.
May also be set with the `UV_CACHE_DIR` environment variable.
`--color` _color-choice_
    
Control the use of color in output.
By default, uv will automatically detect support for colors when writing to a terminal.
Possible values:
  * `auto`: Enables colored output only when the output is going to a terminal or TTY with support
  * `always`: Enables colored output regardless of the detected environment
  * `never`: Disables colored output


`--compile-bytecode`
    
Compile Python files to bytecode after installation.
By default, uv does not compile Python (`.py`) files to bytecode (`__pycache__/*.pyc`); instead, compilation is performed lazily the first time a module is imported. For use-cases in which start time is critical, such as CLI applications and Docker containers, this option can be enabled to trade longer installation times for faster start times.
When enabled, uv will process the entire site-packages directory (including packages that are not being modified by the current operation) for consistency. Like pip, it will also ignore errors.
May also be set with the `UV_COMPILE_BYTECODE` environment variable.
`--config-file` _config-file_
    
The path to a `uv.toml` file to use for configuration.
While uv configuration can be included in a `pyproject.toml` file, it is not allowed in this context.
May also be set with the `UV_CONFIG_FILE` environment variable.
`--config-setting`, `-C` _config-setting_
    
Settings to pass to the PEP 517 build backend, specified as `KEY=VALUE` pairs
`--default-index` _default-index_
    
The URL of the default package index (by default: <https://pypi.org/simple>).
Accepts either a repository compliant with PEP 503 (the simple repository API), or a local directory laid out in the same format.
The index given by this flag is given lower priority than all other indexes specified via the `--index` flag.
May also be set with the `UV_DEFAULT_INDEX` environment variable.
`--dev`
    
Add the requirements to the development dependency group.
This option is an alias for `--group dev`.
`--directory` _directory_
    
Change to the given directory prior to running the command.
Relative paths are resolved with the given directory as the base.
See `--project` to only change the project root directory.
`--editable`
    
Add the requirements as editable
`--exclude-newer` _exclude-newer_
    
Limit candidate packages to those that were uploaded prior to the given date.
Accepts both RFC 3339 timestamps (e.g., `2006-12-02T02:07:43Z`) and local dates in the same format (e.g., `2006-12-02`) in your system’s configured time zone.
May also be set with the `UV_EXCLUDE_NEWER` environment variable.
`--extra` _extra_
    
Extras to enable for the dependency.
May be provided more than once.
To add this dependency to an optional extra instead, see `--optional`.
`--extra-index-url` _extra-index-url_
    
(Deprecated: use `--index` instead) Extra URLs of package indexes to use, in addition to `--index-url`.
Accepts either a repository compliant with PEP 503 (the simple repository API), or a local directory laid out in the same format.
All indexes provided via this flag take priority over the index specified by `--index-url` (which defaults to PyPI). When multiple `--extra-index-url` flags are provided, earlier values take priority.
May also be set with the `UV_EXTRA_INDEX_URL` environment variable.
`--find-links`, `-f` _find-links_
    
Locations to search for candidate distributions, in addition to those found in the registry indexes.
If a path, the target must be a directory that contains packages as wheel files (`.whl`) or source distributions (e.g., `.tar.gz` or `.zip`) at the top level.
If a URL, the page must contain a flat list of links to package files adhering to the formats described above.
May also be set with the `UV_FIND_LINKS` environment variable.
`--fork-strategy` _fork-strategy_
    
The strategy to use when selecting multiple versions of a given package across Python versions and platforms.
By default, uv will optimize for selecting the latest version of each package for each supported Python version (`requires-python`), while minimizing the number of selected versions across platforms.
Under `fewest`, uv will minimize the number of selected versions for each package, preferring older versions that are compatible with a wider range of supported Python versions or platforms.
May also be set with the `UV_FORK_STRATEGY` environment variable.
Possible values:
  * `fewest`: Optimize for selecting the fewest number of versions for each package. Older versions may be preferred if they are compatible with a wider range of supported Python versions or platforms
  * `requires-python`: Optimize for selecting latest supported version of each package, for each supported Python version


`--frozen`
    
Add dependencies without re-locking the project.
The project environment will not be synced.
May also be set with the `UV_FROZEN` environment variable.
`--group` _group_
    
Add the requirements to the specified dependency group.
These requirements will not be included in the published metadata for the project.
`--help`, `-h`
    
Display the concise help for this command
`--index` _index_
    
The URLs to use when resolving dependencies, in addition to the default index.
Accepts either a repository compliant with PEP 503 (the simple repository API), or a local directory laid out in the same format.
All indexes provided via this flag take priority over the index specified by `--default-index` (which defaults to PyPI). When multiple `--index` flags are provided, earlier values take priority.
May also be set with the `UV_INDEX` environment variable.
`--index-strategy` _index-strategy_
    
The strategy to use when resolving against multiple index URLs.
By default, uv will stop at the first index on which a given package is available, and limit resolutions to those present on that first index (`first-index`). This prevents "dependency confusion" attacks, whereby an attacker can upload a malicious package under the same name to an alternate index.
May also be set with the `UV_INDEX_STRATEGY` environment variable.
Possible values:
  * `first-index`: Only use results from the first index that returns a match for a given package name
  * `unsafe-first-match`: Search for every package name across all indexes, exhausting the versions from the first index before moving on to the next
  * `unsafe-best-match`: Search for every package name across all indexes, preferring the "best" version found. If a package version is in multiple indexes, only look at the entry for the first index


`--index-url`, `-i` _index-url_
    
(Deprecated: use `--default-index` instead) The URL of the Python package index (by default: <https://pypi.org/simple>).
Accepts either a repository compliant with PEP 503 (the simple repository API), or a local directory laid out in the same format.
The index given by this flag is given lower priority than all other indexes specified via the `--extra-index-url` flag.
May also be set with the `UV_INDEX_URL` environment variable.
`--keyring-provider` _keyring-provider_
    
Attempt to use `keyring` for authentication for index URLs.
At present, only `--keyring-provider subprocess` is supported, which configures uv to use the `keyring` CLI to handle authentication.
Defaults to `disabled`.
May also be set with the `UV_KEYRING_PROVIDER` environment variable.
Possible values:
  * `disabled`: Do not use keyring for credential lookup
  * `subprocess`: Use the `keyring` command for credential lookup


`--link-mode` _link-mode_
    
The method to use when installing packages from the global cache.
Defaults to `clone` (also known as Copy-on-Write) on macOS, and `hardlink` on Linux and Windows.
May also be set with the `UV_LINK_MODE` environment variable.
Possible values:
  * `clone`: Clone (i.e., copy-on-write) packages from the wheel into the `site-packages` directory
  * `copy`: Copy packages from the wheel into the `site-packages` directory
  * `hardlink`: Hard link packages from the wheel into the `site-packages` directory
  * `symlink`: Symbolically link packages from the wheel into the `site-packages` directory


`--locked`
    
Assert that the `uv.lock` will remain unchanged.
Requires that the lockfile is up-to-date. If the lockfile is missing or needs to be updated, uv will exit with an error.
May also be set with the `UV_LOCKED` environment variable.
`--native-tls`
    
Whether to load TLS certificates from the platform’s native certificate store.
By default, uv loads certificates from the bundled `webpki-roots` crate. The `webpki-roots` are a reliable set of trust roots from Mozilla, and including them in uv improves portability and performance (especially on macOS).
However, in some cases, you may want to use the platform’s native certificate store, especially if you’re relying on a corporate trust root (e.g., for a mandatory proxy) that’s included in your system’s certificate store.
May also be set with the `UV_NATIVE_TLS` environment variable.
`--no-binary`
    
Don’t install pre-built wheels.
The given packages will be built and installed from source. The resolver will still use pre-built wheels to extract package metadata, if available.
May also be set with the `UV_NO_BINARY` environment variable.
`--no-binary-package` _no-binary-package_
    
Don’t install pre-built wheels for a specific package
May also be set with the `UV_NO_BINARY_PACKAGE` environment variable.
`--no-build`
    
Don’t build source distributions.
When enabled, resolving will not run arbitrary Python code. The cached wheels of already-built source distributions will be reused, but operations that require building distributions will exit with an error.
`--no-build-isolation`
    
Disable isolation when building source distributions.
Assumes that build dependencies specified by PEP 518 are already installed.
May also be set with the `UV_NO_BUILD_ISOLATION` environment variable.
`--no-build-isolation-package` _no-build-isolation-package_
    
Disable isolation when building source distributions for a specific package.
Assumes that the packages’ build dependencies specified by PEP 518 are already installed.
`--no-build-package` _no-build-package_
    
Don’t build source distributions for a specific package
`--no-cache`, `-n`
    
Avoid reading from or writing to the cache, instead using a temporary directory for the duration of the operation
May also be set with the `UV_NO_CACHE` environment variable.
`--no-config`
    
Avoid discovering configuration files (`pyproject.toml`, `uv.toml`).
Normally, configuration files are discovered in the current directory, parent directories, or user configuration directories.
May also be set with the `UV_NO_CONFIG` environment variable.
`--no-index`
    
Ignore the registry index (e.g., PyPI), instead relying on direct URL dependencies and those provided via `--find-links`
`--no-progress`
    
Hide all progress outputs.
For example, spinners or progress bars.
May also be set with the `UV_NO_PROGRESS` environment variable.
`--no-python-downloads`
    
Disable automatic downloads of Python.
`--no-sources`
    
Ignore the `tool.uv.sources` table when resolving dependencies. Used to lock against the standards-compliant, publishable package metadata, as opposed to using any workspace, Git, URL, or local path sources
`--no-sync`
    
Avoid syncing the virtual environment
May also be set with the `UV_NO_SYNC` environment variable.
`--offline`
    
Disable network access.
When disabled, uv will only use locally cached data and locally available files.
May also be set with the `UV_OFFLINE` environment variable.
`--optional` _optional_
    
Add the requirements to the package’s optional dependencies for the specified extra.
The group may then be activated when installing the project with the `--extra` flag.
To enable an optional extra for this requirement instead, see `--extra`.
`--package` _package_
    
Add the dependency to a specific package in the workspace
`--prerelease` _prerelease_
    
The strategy to use when considering pre-release versions.
By default, uv will accept pre-releases for packages that _only_ publish pre-releases, along with first-party requirements that contain an explicit pre-release marker in the declared specifiers (`if-necessary-or-explicit`).
May also be set with the `UV_PRERELEASE` environment variable.
Possible values:
  * `disallow`: Disallow all pre-release versions
  * `allow`: Allow all pre-release versions
  * `if-necessary`: Allow pre-release versions if all versions of a package are pre-release
  * `explicit`: Allow pre-release versions for first-party packages with explicit pre-release markers in their version requirements
  * `if-necessary-or-explicit`: Allow pre-release versions if all versions of a package are pre-release, or if the package has an explicit pre-release marker in its version requirements


`--project` _project_
    
Run the command within the given project directory.
All `pyproject.toml`, `uv.toml`, and `.python-version` files will be discovered by walking up the directory tree from the project root, as will the project’s virtual environment (`.venv`).
Other command-line arguments (such as relative paths) will be resolved relative to the current working directory.
See `--directory` to change the working directory entirely.
This setting has no effect when used in the `uv pip` interface.
`--python`, `-p` _python_
    
The Python interpreter to use for resolving and syncing.
See uv python for details on Python discovery and supported request formats.
May also be set with the `UV_PYTHON` environment variable.
`--python-preference` _python-preference_
    
Whether to prefer uv-managed or system Python installations.
By default, uv prefers using Python versions it manages. However, it will use system Python installations if a uv-managed Python is not installed. This option allows prioritizing or ignoring system Python installations.
May also be set with the `UV_PYTHON_PREFERENCE` environment variable.
Possible values:
  * `only-managed`: Only use managed Python installations; never use system Python installations
  * `managed`: Prefer managed Python installations over system Python installations
  * `system`: Prefer system Python installations over managed Python installations
  * `only-system`: Only use system Python installations; never use managed Python installations


`--quiet`, `-q`
    
Do not print any output
`--raw-sources`
    
Add source requirements to `project.dependencies`, rather than `tool.uv.sources`.
By default, uv will use the `tool.uv.sources` section to record source information for Git, local, editable, and direct URL requirements.
`--refresh`
    
Refresh all cached data
`--refresh-package` _refresh-package_
    
Refresh cached data for a specific package
`--reinstall`
    
Reinstall all packages, regardless of whether they’re already installed. Implies `--refresh`
`--reinstall-package` _reinstall-package_
    
Reinstall a specific package, regardless of whether it’s already installed. Implies `--refresh-package`
`--requirements`, `-r` _requirements_
    
Add all packages listed in the given `requirements.txt` files
`--resolution` _resolution_
    
The strategy to use when selecting between the different compatible versions for a given package requirement.
By default, uv will use the latest compatible version of each package (`highest`).
May also be set with the `UV_RESOLUTION` environment variable.
Possible values:
  * `highest`: Resolve the highest compatible version of each package
  * `lowest`: Resolve the lowest compatible version of each package
  * `lowest-direct`: Resolve the lowest compatible version of any direct dependencies, and the highest compatible version of any transitive dependencies


`--rev` _rev_
    
Commit to use when adding a dependency from Git
`--script` _script_
    
Add the dependency to the specified Python script, rather than to a project.
If provided, uv will add the dependency to the script’s inline metadata table, in adherence with PEP 723. If no such inline metadata table is present, a new one will be created and added to the script. When executed via `uv run`, uv will create a temporary environment for the script with all inline dependencies installed.
`--tag` _tag_
    
Tag to use when adding a dependency from Git
`--upgrade`, `-U`
    
Allow package upgrades, ignoring pinned versions in any existing output file. Implies `--refresh`
`--upgrade-package`, `-P` _upgrade-package_
    
Allow upgrades for a specific package, ignoring pinned versions in any existing output file. Implies `--refresh-package`
`--verbose`, `-v`
    
Use verbose output.
You can configure fine-grained logging using the `RUST_LOG` environment variable. (<https://docs.rs/tracing-subscriber/latest/tracing_subscriber/filter/struct.EnvFilter.html#directives>)
`--version`, `-V`
    
Display the uv version
## uv remove
Remove dependencies from the project.
Dependencies are removed from the project's `pyproject.toml` file.
If multiple entries exist for a given dependency, i.e., each with different markers, all of the entries will be removed.
The lockfile and project environment will be updated to reflect the removed dependencies. To skip updating the lockfile, use `--frozen`. To skip updating the environment, use `--no-sync`.
If any of the requested dependencies are not present in the project, uv will exit with an error.
If a package has been manually installed in the environment, i.e., with `uv pip install`, it will not be removed by `uv remove`.
uv will search for a project in the current directory or any parent directory. If a project cannot be found, uv will exit with an error.
### Usage
```
uv remove [OPTIONS] <PACKAGES>...

```

### Arguments
`PACKAGES`
    
The names of the dependencies to remove (e.g., `ruff`)
### Options
`--active`
    
Prefer the active virtual environment over the project’s virtual environment.
If the project virtual environment is active or no virtual environment is active, this has no effect.
`--allow-insecure-host` _allow-insecure-host_
    
Allow insecure connections to a host.
Can be provided multiple times.
Expects to receive either a hostname (e.g., `localhost`), a host-port pair (e.g., `localhost:8080`), or a URL (e.g., `https://localhost`).
WARNING: Hosts included in this list will not be verified against the system’s certificate store. Only use `--allow-insecure-host` in a secure network with verified sources, as it bypasses SSL verification and could expose you to MITM attacks.
May also be set with the `UV_INSECURE_HOST` environment variable.
`--cache-dir` _cache-dir_
    
Path to the cache directory.
Defaults to `$XDG_CACHE_HOME/uv` or `$HOME/.cache/uv` on macOS and Linux, and `%LOCALAPPDATA%\uv\cache` on Windows.
To view the location of the cache directory, run `uv cache dir`.
May also be set with the `UV_CACHE_DIR` environment variable.
`--color` _color-choice_
    
Control the use of color in output.
By default, uv will automatically detect support for colors when writing to a terminal.
Possible values:
  * `auto`: Enables colored output only when the output is going to a terminal or TTY with support
  * `always`: Enables colored output regardless of the detected environment
  * `never`: Disables colored output


`--compile-bytecode`
    
Compile Python files to bytecode after installation.
By default, uv does not compile Python (`.py`) files to bytecode (`__pycache__/*.pyc`); instead, compilation is performed lazily the first time a module is imported. For use-cases in which start time is critical, such as CLI applications and Docker containers, this option can be enabled to trade longer installation times for faster start times.
When enabled, uv will process the entire site-packages directory (including packages that are not being modified by the current operation) for consistency. Like pip, it will also ignore errors.
May also be set with the `UV_COMPILE_BYTECODE` environment variable.
`--config-file` _config-file_
    
The path to a `uv.toml` file to use for configuration.
While uv configuration can be included in a `pyproject.toml` file, it is not allowed in this context.
May also be set with the `UV_CONFIG_FILE` environment variable.
`--config-setting`, `-C` _config-setting_
    
Settings to pass to the PEP 517 build backend, specified as `KEY=VALUE` pairs
`--default-index` _default-index_
    
The URL of the default package index (by default: <https://pypi.org/simple>).
Accepts either a repository compliant with PEP 503 (the simple repository API), or a local directory laid out in the same format.
The index given by this flag is given lower priority than all other indexes specified via the `--index` flag.
May also be set with the `UV_DEFAULT_INDEX` environment variable.
`--dev`
    
Remove the packages from the development dependency group.
This option is an alias for `--group dev`.
`--directory` _directory_
    
Change to the given directory prior to running the command.
Relative paths are resolved with the given directory as the base.
See `--project` to only change the project root directory.
`--exclude-newer` _exclude-newer_
    
Limit candidate packages to those that were uploaded prior to the given date.
Accepts both RFC 3339 timestamps (e.g., `2006-12-02T02:07:43Z`) and local dates in the same format (e.g., `2006-12-02`) in your system’s configured time zone.
May also be set with the `UV_EXCLUDE_NEWER` environment variable.
`--extra-index-url` _extra-index-url_
    
(Deprecated: use `--index` instead) Extra URLs of package indexes to use, in addition to `--index-url`.
Accepts either a repository compliant with PEP 503 (the simple repository API), or a local directory laid out in the same format.
All indexes provided via this flag take priority over the index specified by `--index-url` (which defaults to PyPI). When multiple `--extra-index-url` flags are provided, earlier values take priority.
May also be set with the `UV_EXTRA_INDEX_URL` environment variable.
`--find-links`, `-f` _find-links_
    
Locations to search for candidate distributions, in addition to those found in the registry indexes.
If a path, the target must be a directory that contains packages as wheel files (`.whl`) or source distributions (e.g., `.tar.gz` or `.zip`) at the top level.
If a URL, the page must contain a flat list of links to package files adhering to the formats described above.
May also be set with the `UV_FIND_LINKS` environment variable.
`--fork-strategy` _fork-strategy_
    
The strategy to use when selecting multiple versions of a given package across Python versions and platforms.
By default, uv will optimize for selecting the latest version of each package for each supported Python version (`requires-python`), while minimizing the number of selected versions across platforms.
Under `fewest`, uv will minimize the number of selected versions for each package, preferring older versions that are compatible with a wider range of supported Python versions or platforms.
May also be set with the `UV_FORK_STRATEGY` environment variable.
Possible values:
  * `fewest`: Optimize for selecting the fewest number of versions for each package. Older versions may be preferred if they are compatible with a wider range of supported Python versions or platforms
  * `requires-python`: Optimize for selecting latest supported version of each package, for each supported Python version


`--frozen`
    
Remove dependencies without re-locking the project.
The project environment will not be synced.
May also be set with the `UV_FROZEN` environment variable.
`--group` _group_
    
Remove the packages from the specified dependency group
`--help`, `-h`
    
Display the concise help for this command
`--index` _index_
    
The URLs to use when resolving dependencies, in addition to the default index.
Accepts either a repository compliant with PEP 503 (the simple repository API), or a local directory laid out in the same format.
All indexes provided via this flag take priority over the index specified by `--default-index` (which defaults to PyPI). When multiple `--index` flags are provided, earlier values take priority.
May also be set with the `UV_INDEX` environment variable.
`--index-strategy` _index-strategy_
    
The strategy to use when resolving against multiple index URLs.
By default, uv will stop at the first index on which a given package is available, and limit resolutions to those present on that first index (`first-index`). This prevents "dependency confusion" attacks, whereby an attacker can upload a malicious package under the same name to an alternate index.
May also be set with the `UV_INDEX_STRATEGY` environment variable.
Possible values:
  * `first-index`: Only use results from the first index that returns a match for a given package name
  * `unsafe-first-match`: Search for every package name across all indexes, exhausting the versions from the first index before moving on to the next
  * `unsafe-best-match`: Search for every package name across all indexes, preferring the "best" version found. If a package version is in multiple indexes, only look at the entry for the first index


`--index-url`, `-i` _index-url_
    
(Deprecated: use `--default-index` instead) The URL of the Python package index (by default: <https://pypi.org/simple>).
Accepts either a repository compliant with PEP 503 (the simple repository API), or a local directory laid out in the same format.
The index given by this flag is given lower priority than all other indexes specified via the `--extra-index-url` flag.
May also be set with the `UV_INDEX_URL` environment variable.
`--keyring-provider` _keyring-provider_
    
Attempt to use `keyring` for authentication for index URLs.
At present, only `--keyring-provider subprocess` is supported, which configures uv to use the `keyring` CLI to handle authentication.
Defaults to `disabled`.
May also be set with the `UV_KEYRING_PROVIDER` environment variable.
Possible values:
  * `disabled`: Do not use keyring for credential lookup
  * `subprocess`: Use the `keyring` command for credential lookup


`--link-mode` _link-mode_
    
The method to use when installing packages from the global cache.
Defaults to `clone` (also known as Copy-on-Write) on macOS, and `hardlink` on Linux and Windows.
May also be set with the `UV_LINK_MODE` environment variable.
Possible values:
  * `clone`: Clone (i.e., copy-on-write) packages from the wheel into the `site-packages` directory
  * `copy`: Copy packages from the wheel into the `site-packages` directory
  * `hardlink`: Hard link packages from the wheel into the `site-packages` directory
  * `symlink`: Symbolically link packages from the wheel into the `site-packages` directory


`--locked`
    
Assert that the `uv.lock` will remain unchanged.
Requires that the lockfile is up-to-date. If the lockfile is missing or needs to be updated, uv will exit with an error.
May also be set with the `UV_LOCKED` environment variable.
`--native-tls`
    
Whether to load TLS certificates from the platform’s native certificate store.
By default, uv loads certificates from the bundled `webpki-roots` crate. The `webpki-roots` are a reliable set of trust roots from Mozilla, and including them in uv improves portability and performance (especially on macOS).
However, in some cases, you may want to use the platform’s native certificate store, especially if you’re relying on a corporate trust root (e.g., for a mandatory proxy) that’s included in your system’s certificate store.
May also be set with the `UV_NATIVE_TLS` environment variable.
`--no-binary`
    
Don’t install pre-built wheels.
The given packages will be built and installed from source. The resolver will still use pre-built wheels to extract package metadata, if available.
May also be set with the `UV_NO_BINARY` environment variable.
`--no-binary-package` _no-binary-package_
    
Don’t install pre-built wheels for a specific package
May also be set with the `UV_NO_BINARY_PACKAGE` environment variable.
`--no-build`
    
Don’t build source distributions.
When enabled, resolving will not run arbitrary Python code. The cached wheels of already-built source distributions will be reused, but operations that require building distributions will exit with an error.
`--no-build-isolation`
    
Disable isolation when building source distributions.
Assumes that build dependencies specified by PEP 518 are already installed.
May also be set with the `UV_NO_BUILD_ISOLATION` environment variable.
`--no-build-isolation-package` _no-build-isolation-package_
    
Disable isolation when building source distributions for a specific package.
Assumes that the packages’ build dependencies specified by PEP 518 are already installed.
`--no-build-package` _no-build-package_
    
Don’t build source distributions for a specific package
`--no-cache`, `-n`
    
Avoid reading from or writing to the cache, instead using a temporary directory for the duration of the operation
May also be set with the `UV_NO_CACHE` environment variable.
`--no-config`
    
Avoid discovering configuration files (`pyproject.toml`, `uv.toml`).
Normally, configuration files are discovered in the current directory, parent directories, or user configuration directories.
May also be set with the `UV_NO_CONFIG` environment variable.
`--no-index`
    
Ignore the registry index (e.g., PyPI), instead relying on direct URL dependencies and those provided via `--find-links`
`--no-progress`
    
Hide all progress outputs.
For example, spinners or progress bars.
May also be set with the `UV_NO_PROGRESS` environment variable.
`--no-python-downloads`
    
Disable automatic downloads of Python.
`--no-sources`
    
Ignore the `tool.uv.sources` table when resolving dependencies. Used to lock against the standards-compliant, publishable package metadata, as opposed to using any workspace, Git, URL, or local path sources
`--no-sync`
    
Avoid syncing the virtual environment after re-locking the project
May also be set with the `UV_NO_SYNC` environment variable.
`--offline`
    
Disable network access.
When disabled, uv will only use locally cached data and locally available files.
May also be set with the `UV_OFFLINE` environment variable.
`--optional` _optional_
    
Remove the packages from the project’s optional dependencies for the specified extra
`--package` _package_
    
Remove the dependencies from a specific package in the workspace
`--prerelease` _prerelease_
    
The strategy to use when considering pre-release versions.
By default, uv will accept pre-releases for packages that _only_ publish pre-releases, along with first-party requirements that contain an explicit pre-release marker in the declared specifiers (`if-necessary-or-explicit`).
May also be set with the `UV_PRERELEASE` environment variable.
Possible values:
  * `disallow`: Disallow all pre-release versions
  * `allow`: Allow all pre-release versions
  * `if-necessary`: Allow pre-release versions if all versions of a package are pre-release
  * `explicit`: Allow pre-release versions for first-party packages with explicit pre-release markers in their version requirements
  * `if-necessary-or-explicit`: Allow pre-release versions if all versions of a package are pre-release, or if the package has an explicit pre-release marker in its version requirements


`--project` _project_
    
Run the command within the given project directory.
All `pyproject.toml`, `uv.toml`, and `.python-version` files will be discovered by walking up the directory tree from the project root, as will the project’s virtual environment (`.venv`).
Other command-line arguments (such as relative paths) will be resolved relative to the current working directory.
See `--directory` to change the working directory entirely.
This setting has no effect when used in the `uv pip` interface.
`--python`, `-p` _python_
    
The Python interpreter to use for resolving and syncing.
See uv python for details on Python discovery and supported request formats.
May also be set with the `UV_PYTHON` environment variable.
`--python-preference` _python-preference_
    
Whether to prefer uv-managed or system Python installations.
By default, uv prefers using Python versions it manages. However, it will use system Python installations if a uv-managed Python is not installed. This option allows prioritizing or ignoring system Python installations.
May also be set with the `UV_PYTHON_PREFERENCE` environment variable.
Possible values:
  * `only-managed`: Only use managed Python installations; never use system Python installations
  * `managed`: Prefer managed Python installations over system Python installations
  * `system`: Prefer system Python installations over managed Python installations
  * `only-system`: Only use system Python installations; never use managed Python installations


`--quiet`, `-q`
    
Do not print any output
`--refresh`
    
Refresh all cached data
`--refresh-package` _refresh-package_
    
Refresh cached data for a specific package
`--reinstall`
    
Reinstall all packages, regardless of whether they’re already installed. Implies `--refresh`
`--reinstall-package` _reinstall-package_
    
Reinstall a specific package, regardless of whether it’s already installed. Implies `--refresh-package`
`--resolution` _resolution_
    
The strategy to use when selecting between the different compatible versions for a given package requirement.
By default, uv will use the latest compatible version of each package (`highest`).
May also be set with the `UV_RESOLUTION` environment variable.
Possible values:
  * `highest`: Resolve the highest compatible version of each package
  * `lowest`: Resolve the lowest compatible version of each package
  * `lowest-direct`: Resolve the lowest compatible version of any direct dependencies, and the highest compatible version of any transitive dependencies


`--script` _script_
    
Remove the dependency from the specified Python script, rather than from a project.
If provided, uv will remove the dependency from the script’s inline metadata table, in adherence with PEP 723.
`--upgrade`, `-U`
    
Allow package upgrades, ignoring pinned versions in any existing output file. Implies `--refresh`
`--upgrade-package`, `-P` _upgrade-package_
    
Allow upgrades for a specific package, ignoring pinned versions in any existing output file. Implies `--refresh-package`
`--verbose`, `-v`
    
Use verbose output.
You can configure fine-grained logging using the `RUST_LOG` environment variable. (<https://docs.rs/tracing-subscriber/latest/tracing_subscriber/filter/struct.EnvFilter.html#directives>)
`--version`, `-V`
    
Display the uv version
## uv sync
Update the project's environment.
Syncing ensures that all project dependencies are installed and up-to-date with the lockfile.
By default, an exact sync is performed: uv removes packages that are not declared as dependencies of the project. Use the `--inexact` flag to keep extraneous packages. Note that if an extraneous package conflicts with a project dependency, it will still be removed. Additionally, if `--no-build-isolation` is used, uv will not remove extraneous packages to avoid removing possible build dependencies.
If the project virtual environment (`.venv`) does not exist, it will be created.
The project is re-locked before syncing unless the `--locked` or `--frozen` flag is provided.
uv will search for a project in the current directory or any parent directory. If a project cannot be found, uv will exit with an error.
Note that, when installing from a lockfile, uv will not provide warnings for yanked package versions.
### Usage
```
uv sync [OPTIONS]

```

### Options
`--active`
    
Sync dependencies to the active virtual environment.
Instead of creating or updating the virtual environment for the project or script, the active virtual environment will be preferred, if the `VIRTUAL_ENV` environment variable is set.
`--all-extras`
    
Include all optional dependencies.
When two or more extras are declared as conflicting in `tool.uv.conflicts`, using this flag will always result in an error.
Note that all optional dependencies are always included in the resolution; this option only affects the selection of packages to install.
`--all-groups`
    
Include dependencies from all dependency groups.
`--no-group` can be used to exclude specific groups.
`--all-packages`
    
Sync all packages in the workspace.
The workspace’s environment (`.venv`) is updated to include all workspace members.
Any extras or groups specified via `--extra`, `--group`, or related options will be applied to all workspace members.
`--allow-insecure-host` _allow-insecure-host_
    
Allow insecure connections to a host.
Can be provided multiple times.
Expects to receive either a hostname (e.g., `localhost`), a host-port pair (e.g., `localhost:8080`), or a URL (e.g., `https://localhost`).
WARNING: Hosts included in this list will not be verified against the system’s certificate store. Only use `--allow-insecure-host` in a secure network with verified sources, as it bypasses SSL verification and could expose you to MITM attacks.
May also be set with the `UV_INSECURE_HOST` environment variable.
`--cache-dir` _cache-dir_
    
Path to the cache directory.
Defaults to `$XDG_CACHE_HOME/uv` or `$HOME/.cache/uv` on macOS and Linux, and `%LOCALAPPDATA%\uv\cache` on Windows.
To view the location of the cache directory, run `uv cache dir`.
May also be set with the `UV_CACHE_DIR` environment variable.
`--color` _color-choice_
    
Control the use of color in output.
By default, uv will automatically detect support for colors when writing to a terminal.
Possible values:
  * `auto`: Enables colored output only when the output is going to a terminal or TTY with support
  * `always`: Enables colored output regardless of the detected environment
  * `never`: Disables colored output


`--compile-bytecode`
    
Compile Python files to bytecode after installation.
By default, uv does not compile Python (`.py`) files to bytecode (`__pycache__/*.pyc`); instead, compilation is performed lazily the first time a module is imported. For use-cases in which start time is critical, such as CLI applications and Docker containers, this option can be enabled to trade longer installation times for faster start times.
When enabled, uv will process the entire site-packages directory (including packages that are not being modified by the current operation) for consistency. Like pip, it will also ignore errors.
May also be set with the `UV_COMPILE_BYTECODE` environment variable.
`--config-file` _config-file_
    
The path to a `uv.toml` file to use for configuration.
While uv configuration can be included in a `pyproject.toml` file, it is not allowed in this context.
May also be set with the `UV_CONFIG_FILE` environment variable.
`--config-setting`, `-C` _config-setting_
    
Settings to pass to the PEP 517 build backend, specified as `KEY=VALUE` pairs
`--default-index` _default-index_
    
The URL of the default package index (by default: <https://pypi.org/simple>).
Accepts either a repository compliant with PEP 503 (the simple repository API), or a local directory laid out in the same format.
The index given by this flag is given lower priority than all other indexes specified via the `--index` flag.
May also be set with the `UV_DEFAULT_INDEX` environment variable.
`--directory` _directory_
    
Change to the given directory prior to running the command.
Relative paths are resolved with the given directory as the base.
See `--project` to only change the project root directory.
`--dry-run`
    
Perform a dry run, without writing the lockfile or modifying the project environment.
In dry-run mode, uv will resolve the project’s dependencies and report on the resulting changes to both the lockfile and the project environment, but will not modify either.
`--exclude-newer` _exclude-newer_
    
Limit candidate packages to those that were uploaded prior to the given date.
Accepts both RFC 3339 timestamps (e.g., `2006-12-02T02:07:43Z`) and local dates in the same format (e.g., `2006-12-02`) in your system’s configured time zone.
May also be set with the `UV_EXCLUDE_NEWER` environment variable.
`--extra` _extra_
    
Include optional dependencies from the specified extra name.
May be provided more than once.
When multiple extras or groups are specified that appear in `tool.uv.conflicts`, uv will report an error.
Note that all optional dependencies are always included in the resolution; this option only affects the selection of packages to install.
`--extra-index-url` _extra-index-url_
    
(Deprecated: use `--index` instead) Extra URLs of package indexes to use, in addition to `--index-url`.
Accepts either a repository compliant with PEP 503 (the simple repository API), or a local directory laid out in the same format.
All indexes provided via this flag take priority over the index specified by `--index-url` (which defaults to PyPI). When multiple `--extra-index-url` flags are provided, earlier values take priority.
May also be set with the `UV_EXTRA_INDEX_URL` environment variable.
`--find-links`, `-f` _find-links_
    
Locations to search for candidate distributions, in addition to those found in the registry indexes.
If a path, the target must be a directory that contains packages as wheel files (`.whl`) or source distributions (e.g., `.tar.gz` or `.zip`) at the top level.
If a URL, the page must contain a flat list of links to package files adhering to the formats described above.
May also be set with the `UV_FIND_LINKS` environment variable.
`--fork-strategy` _fork-strategy_
    
The strategy to use when selecting multiple versions of a given package across Python versions and platforms.
By default, uv will optimize for selecting the latest version of each package for each supported Python version (`requires-python`), while minimizing the number of selected versions across platforms.
Under `fewest`, uv will minimize the number of selected versions for each package, preferring older versions that are compatible with a wider range of supported Python versions or platforms.
May also be set with the `UV_FORK_STRATEGY` environment variable.
Possible values:
  * `fewest`: Optimize for selecting the fewest number of versions for each package. Older versions may be preferred if they are compatible with a wider range of supported Python versions or platforms
  * `requires-python`: Optimize for selecting latest supported version of each package, for each supported Python version


`--frozen`
    
Sync without updating the `uv.lock` file.
Instead of checking if the lockfile is up-to-date, uses the versions in the lockfile as the source of truth. If the lockfile is missing, uv will exit with an error. If the `pyproject.toml` includes changes to dependencies that have not been included in the lockfile yet, they will not be present in the environment.
May also be set with the `UV_FROZEN` environment variable.
`--group` _group_
    
Include dependencies from the specified dependency group.
When multiple extras or groups are specified that appear in `tool.uv.conflicts`, uv will report an error.
May be provided multiple times.
`--help`, `-h`
    
Display the concise help for this command
`--index` _index_
    
The URLs to use when resolving dependencies, in addition to the default index.
Accepts either a repository compliant with PEP 503 (the simple repository API), or a local directory laid out in the same format.
All indexes provided via this flag take priority over the index specified by `--default-index` (which defaults to PyPI). When multiple `--index` flags are provided, earlier values take priority.
May also be set with the `UV_INDEX` environment variable.
`--index-strategy` _index-strategy_
    
The strategy to use when resolving against multiple index URLs.
By default, uv will stop at the first index on which a given package is available, and limit resolutions to those present on that first index (`first-index`). This prevents "dependency confusion" attacks, whereby an attacker can upload a malicious package under the same name to an alternate index.
May also be set with the `UV_INDEX_STRATEGY` environment variable.
Possible values:
  * `first-index`: Only use results from the first index that returns a match for a given package name
  * `unsafe-first-match`: Search for every package name across all indexes, exhausting the versions from the first index before moving on to the next
  * `unsafe-best-match`: Search for every package name across all indexes, preferring the "best" version found. If a package version is in multiple indexes, only look at the entry for the first index


`--index-url`, `-i` _index-url_
    
(Deprecated: use `--default-index` instead) The URL of the Python package index (by default: <https://pypi.org/simple>).
Accepts either a repository compliant with PEP 503 (the simple repository API), or a local directory laid out in the same format.
The index given by this flag is given lower priority than all other indexes specified via the `--extra-index-url` flag.
May also be set with the `UV_INDEX_URL` environment variable.
`--inexact`
    
Do not remove extraneous packages present in the environment.
When enabled, uv will make the minimum necessary changes to satisfy the requirements. By default, syncing will remove any extraneous packages from the environment
`--keyring-provider` _keyring-provider_
    
Attempt to use `keyring` for authentication for index URLs.
At present, only `--keyring-provider subprocess` is supported, which configures uv to use the `keyring` CLI to handle authentication.
Defaults to `disabled`.
May also be set with the `UV_KEYRING_PROVIDER` environment variable.
Possible values:
  * `disabled`: Do not use keyring for credential lookup
  * `subprocess`: Use the `keyring` command for credential lookup


`--link-mode` _link-mode_
    
The method to use when installing packages from the global cache.
Defaults to `clone` (also known as Copy-on-Write) on macOS, and `hardlink` on Linux and Windows.
May also be set with the `UV_LINK_MODE` environment variable.
Possible values:
  * `clone`: Clone (i.e., copy-on-write) packages from the wheel into the `site-packages` directory
  * `copy`: Copy packages from the wheel into the `site-packages` directory
  * `hardlink`: Hard link packages from the wheel into the `site-packages` directory
  * `symlink`: Symbolically link packages from the wheel into the `site-packages` directory


`--locked`
    
Assert that the `uv.lock` will remain unchanged.
Requires that the lockfile is up-to-date. If the lockfile is missing or needs to be updated, uv will exit with an error.
May also be set with the `UV_LOCKED` environment variable.
`--native-tls`
    
Whether to load TLS certificates from the platform’s native certificate store.
By default, uv loads certificates from the bundled `webpki-roots` crate. The `webpki-roots` are a reliable set of trust roots from Mozilla, and including them in uv improves portability and performance (especially on macOS).
However, in some cases, you may want to use the platform’s native certificate store, especially if you’re relying on a corporate trust root (e.g., for a mandatory proxy) that’s included in your system’s certificate store.
May also be set with the `UV_NATIVE_TLS` environment variable.
`--no-binary`
    
Don’t install pre-built wheels.
The given packages will be built and installed from source. The resolver will still use pre-built wheels to extract package metadata, if available.
May also be set with the `UV_NO_BINARY` environment variable.
`--no-binary-package` _no-binary-package_
    
Don’t install pre-built wheels for a specific package
May also be set with the `UV_NO_BINARY_PACKAGE` environment variable.
`--no-build`
    
Don’t build source distributions.
When enabled, resolving will not run arbitrary Python code. The cached wheels of already-built source distributions will be reused, but operations that require building distributions will exit with an error.
`--no-build-isolation`
    
Disable isolation when building source distributions.
Assumes that build dependencies specified by PEP 518 are already installed.
May also be set with the `UV_NO_BUILD_ISOLATION` environment variable.
`--no-build-isolation-package` _no-build-isolation-package_
    
Disable isolation when building source distributions for a specific package.
Assumes that the packages’ build dependencies specified by PEP 518 are already installed.
`--no-build-package` _no-build-package_
    
Don’t build source distributions for a specific package
`--no-cache`, `-n`
    
Avoid reading from or writing to the cache, instead using a temporary directory for the duration of the operation
May also be set with the `UV_NO_CACHE` environment variable.
`--no-config`
    
Avoid discovering configuration files (`pyproject.toml`, `uv.toml`).
Normally, configuration files are discovered in the current directory, parent directories, or user configuration directories.
May also be set with the `UV_NO_CONFIG` environment variable.
`--no-default-groups`
    
Ignore the the default dependency groups.
uv includes the groups defined in `tool.uv.default-groups` by default. This disables that option, however, specific groups can still be included with `--group`.
`--no-dev`
    
Disable the development dependency group.
This option is an alias of `--no-group dev`. See `--no-default-groups` to disable all default groups instead.
`--no-editable`
    
Install any editable dependencies, including the project and any workspace members, as non-editable
`--no-extra` _no-extra_
    
Exclude the specified optional dependencies, if `--all-extras` is supplied.
May be provided multiple times.
`--no-group` _no-group_
    
Disable the specified dependency group.
This options always takes precedence over default groups, `--all-groups`, and `--group`.
May be provided multiple times.
`--no-index`
    
Ignore the registry index (e.g., PyPI), instead relying on direct URL dependencies and those provided via `--find-links`
`--no-install-package` _no-install-package_
    
Do not install the given package(s).
By default, all of the project’s dependencies are installed into the environment. The `--no-install-package` option allows exclusion of specific packages. Note this can result in a broken environment, and should be used with caution.
`--no-install-project`
    
Do not install the current project.
By default, the current project is installed into the environment with all of its dependencies. The `--no-install-project` option allows the project to be excluded, but all of its dependencies are still installed. This is particularly useful in situations like building Docker images where installing the project separately from its dependencies allows optimal layer caching.
`--no-install-workspace`
    
Do not install any workspace members, including the root project.
By default, all of the workspace members and their dependencies are installed into the environment. The `--no-install-workspace` option allows exclusion of all the workspace members while retaining their dependencies. This is particularly useful in situations like building Docker images where installing the workspace separately from its dependencies allows optimal layer caching.
`--no-progress`
    
Hide all progress outputs.
For example, spinners or progress bars.
May also be set with the `UV_NO_PROGRESS` environment variable.
`--no-python-downloads`
    
Disable automatic downloads of Python.
`--no-sources`
    
Ignore the `tool.uv.sources` table when resolving dependencies. Used to lock against the standards-compliant, publishable package metadata, as opposed to using any workspace, Git, URL, or local path sources
`--offline`
    
Disable network access.
When disabled, uv will only use locally cached data and locally available files.
May also be set with the `UV_OFFLINE` environment variable.
`--only-dev`
    
Only include the development dependency group.
The project and its dependencies will be omitted.
This option is an alias for `--only-group dev`. Implies `--no-default-groups`.
`--only-group` _only-group_
    
Only include dependencies from the specified dependency group.
The project and its dependencies will be omitted.
May be provided multiple times. Implies `--no-default-groups`.
`--package` _package_
    
Sync for a specific package in the workspace.
The workspace’s environment (`.venv`) is updated to reflect the subset of dependencies declared by the specified workspace member package.
If the workspace member does not exist, uv will exit with an error.
`--prerelease` _prerelease_
    
The strategy to use when considering pre-release versions.
By default, uv will accept pre-releases for packages that _only_ publish pre-releases, along with first-party requirements that contain an explicit pre-release marker in the declared specifiers (`if-necessary-or-explicit`).
May also be set with the `UV_PRERELEASE` environment variable.
Possible values:
  * `disallow`: Disallow all pre-release versions
  * `allow`: Allow all pre-release versions
  * `if-necessary`: Allow pre-release versions if all versions of a package are pre-release
  * `explicit`: Allow pre-release versions for first-party packages with explicit pre-release markers in their version requirements
  * `if-necessary-or-explicit`: Allow pre-release versions if all versions of a package are pre-release, or if the package has an explicit pre-release marker in its version requirements


`--project` _project_
    
Run the command within the given project directory.
All `pyproject.toml`, `uv.toml`, and `.python-version` files will be discovered by walking up the directory tree from the project root, as will the project’s virtual environment (`.venv`).
Other command-line arguments (such as relative paths) will be resolved relative to the current working directory.
See `--directory` to change the working directory entirely.
This setting has no effect when used in the `uv pip` interface.
`--python`, `-p` _python_
    
The Python interpreter to use for the project environment.
By default, the first interpreter that meets the project’s `requires-python` constraint is used.
If a Python interpreter in a virtual environment is provided, the packages will not be synced to the given environment. The interpreter will be used to create a virtual environment in the project.
See uv python for details on Python discovery and supported request formats.
May also be set with the `UV_PYTHON` environment variable.
`--python-preference` _python-preference_
    
Whether to prefer uv-managed or system Python installations.
By default, uv prefers using Python versions it manages. However, it will use system Python installations if a uv-managed Python is not installed. This option allows prioritizing or ignoring system Python installations.
May also be set with the `UV_PYTHON_PREFERENCE` environment variable.
Possible values:
  * `only-managed`: Only use managed Python installations; never use system Python installations
  * `managed`: Prefer managed Python installations over system Python installations
  * `system`: Prefer system Python installations over managed Python installations
  * `only-system`: Only use system Python installations; never use managed Python installations


`--quiet`, `-q`
    
Do not print any output
`--refresh`
    
Refresh all cached data
`--refresh-package` _refresh-package_
    
Refresh cached data for a specific package
`--reinstall`
    
Reinstall all packages, regardless of whether they’re already installed. Implies `--refresh`
`--reinstall-package` _reinstall-package_
    
Reinstall a specific package, regardless of whether it’s already installed. Implies `--refresh-package`
`--resolution` _resolution_
    
The strategy to use when selecting between the different compatible versions for a given package requirement.
By default, uv will use the latest compatible version of each package (`highest`).
May also be set with the `UV_RESOLUTION` environment variable.
Possible values:
  * `highest`: Resolve the highest compatible version of each package
  * `lowest`: Resolve the lowest compatible version of each package
  * `lowest-direct`: Resolve the lowest compatible version of any direct dependencies, and the highest compatible version of any transitive dependencies


`--script` _script_
    
Sync the environment for a Python script, rather than the current project.
If provided, uv will sync the dependencies based on the script’s inline metadata table, in adherence with PEP 723.
`--upgrade`, `-U`
    
Allow package upgrades, ignoring pinned versions in any existing output file. Implies `--refresh`
`--upgrade-package`, `-P` _upgrade-package_
    
Allow upgrades for a specific package, ignoring pinned versions in any existing output file. Implies `--refresh-package`
`--verbose`, `-v`
    
Use verbose output.
You can configure fine-grained logging using the `RUST_LOG` environment variable. (<https://docs.rs/tracing-subscriber/latest/tracing_subscriber/filter/struct.EnvFilter.html#directives>)
`--version`, `-V`
    
Display the uv version
## uv lock
Update the project's lockfile.
If the project lockfile (`uv.lock`) does not exist, it will be created. If a lockfile is present, its contents will be used as preferences for the resolution.
If there are no changes to the project's dependencies, locking will have no effect unless the `--upgrade` flag is provided.
### Usage
```
uv lock [OPTIONS]

```

### Options
`--allow-insecure-host` _allow-insecure-host_
    
Allow insecure connections to a host.
Can be provided multiple times.
Expects to receive either a hostname (e.g., `localhost`), a host-port pair (e.g., `localhost:8080`), or a URL (e.g., `https://localhost`).
WARNING: Hosts included in this list will not be verified against the system’s certificate store. Only use `--allow-insecure-host` in a secure network with verified sources, as it bypasses SSL verification and could expose you to MITM attacks.
May also be set with the `UV_INSECURE_HOST` environment variable.
`--cache-dir` _cache-dir_
    
Path to the cache directory.
Defaults to `$XDG_CACHE_HOME/uv` or `$HOME/.cache/uv` on macOS and Linux, and `%LOCALAPPDATA%\uv\cache` on Windows.
To view the location of the cache directory, run `uv cache dir`.
May also be set with the `UV_CACHE_DIR` environment variable.
`--check`
    
Check if the lockfile is up-to-date.
Asserts that the `uv.lock` would remain unchanged after a resolution. If the lockfile is missing or needs to be updated, uv will exit with an error.
Equivalent to `--locked`.
May also be set with the `UV_LOCKED` environment variable.
`--check-exists`
    
Assert that a `uv.lock` exists without checking if it is up-to-date.
Equivalent to `--frozen`.
May also be set with the `UV_FROZEN` environment variable.
`--color` _color-choice_
    
Control the use of color in output.
By default, uv will automatically detect support for colors when writing to a terminal.
Possible values:
  * `auto`: Enables colored output only when the output is going to a terminal or TTY with support
  * `always`: Enables colored output regardless of the detected environment
  * `never`: Disables colored output


`--config-file` _config-file_
    
The path to a `uv.toml` file to use for configuration.
While uv configuration can be included in a `pyproject.toml` file, it is not allowed in this context.
May also be set with the `UV_CONFIG_FILE` environment variable.
`--config-setting`, `-C` _config-setting_
    
Settings to pass to the PEP 517 build backend, specified as `KEY=VALUE` pairs
`--default-index` _default-index_
    
The URL of the default package index (by default: <https://pypi.org/simple>).
Accepts either a repository compliant with PEP 503 (the simple repository API), or a local directory laid out in the same format.
The index given by this flag is given lower priority than all other indexes specified via the `--index` flag.
May also be set with the `UV_DEFAULT_INDEX` environment variable.
`--directory` _directory_
    
Change to the given directory prior to running the command.
Relative paths are resolved with the given directory as the base.
See `--project` to only change the project root directory.
`--dry-run`
    
Perform a dry run, without writing the lockfile.
In dry-run mode, uv will resolve the project’s dependencies and report on the resulting changes, but will not write the lockfile to disk.
`--exclude-newer` _exclude-newer_
    
Limit candidate packages to those that were uploaded prior to the given date.
Accepts both RFC 3339 timestamps (e.g., `2006-12-02T02:07:43Z`) and local dates in the same format (e.g., `2006-12-02`) in your system’s configured time zone.
May also be set with the `UV_EXCLUDE_NEWER` environment variable.
`--extra-index-url` _extra-index-url_
    
(Deprecated: use `--index` instead) Extra URLs of package indexes to use, in addition to `--index-url`.
Accepts either a repository compliant with PEP 503 (the simple repository API), or a local directory laid out in the same format.
All indexes provided via this flag take priority over the index specified by `--index-url` (which defaults to PyPI). When multiple `--extra-index-url` flags are provided, earlier values take priority.
May also be set with the `UV_EXTRA_INDEX_URL` environment variable.
`--find-links`, `-f` _find-links_
    
Locations to search for candidate distributions, in addition to those found in the registry indexes.
If a path, the target must be a directory that contains packages as wheel files (`.whl`) or source distributions (e.g., `.tar.gz` or `.zip`) at the top level.
If a URL, the page must contain a flat list of links to package files adhering to the formats described above.
May also be set with the `UV_FIND_LINKS` environment variable.
`--fork-strategy` _fork-strategy_
    
The strategy to use when selecting multiple versions of a given package across Python versions and platforms.
By default, uv will optimize for selecting the latest version of each package for each supported Python version (`requires-python`), while minimizing the number of selected versions across platforms.
Under `fewest`, uv will minimize the number of selected versions for each package, preferring older versions that are compatible with a wider range of supported Python versions or platforms.
May also be set with the `UV_FORK_STRATEGY` environment variable.
Possible values:
  * `fewest`: Optimize for selecting the fewest number of versions for each package. Older versions may be preferred if they are compatible with a wider range of supported Python versions or platforms
  * `requires-python`: Optimize for selecting latest supported version of each package, for each supported Python version


`--help`, `-h`
    
Display the concise help for this command
`--index` _index_
    
The URLs to use when resolving dependencies, in addition to the default index.
Accepts either a repository compliant with PEP 503 (the simple repository API), or a local directory laid out in the same format.
All indexes provided via this flag take priority over the index specified by `--default-index` (which defaults to PyPI). When multiple `--index` flags are provided, earlier values take priority.
May also be set with the `UV_INDEX` environment variable.
`--index-strategy` _index-strategy_
    
The strategy to use when resolving against multiple index URLs.
By default, uv will stop at the first index on which a given package is available, and limit resolutions to those present on that first index (`first-index`). This prevents "dependency confusion" attacks, whereby an attacker can upload a malicious package under the same name to an alternate index.
May also be set with the `UV_INDEX_STRATEGY` environment variable.
Possible values:
  * `first-index`: Only use results from the first index that returns a match for a given package name
  * `unsafe-first-match`: Search for every package name across all indexes, exhausting the versions from the first index before moving on to the next
  * `unsafe-best-match`: Search for every package name across all indexes, preferring the "best" version found. If a package version is in multiple indexes, only look at the entry for the first index


`--index-url`, `-i` _index-url_
    
(Deprecated: use `--default-index` instead) The URL of the Python package index (by default: <https://pypi.org/simple>).
Accepts either a repository compliant with PEP 503 (the simple repository API), or a local directory laid out in the same format.
The index given by this flag is given lower priority than all other indexes specified via the `--extra-index-url` flag.
May also be set with the `UV_INDEX_URL` environment variable.
`--keyring-provider` _keyring-provider_
    
Attempt to use `keyring` for authentication for index URLs.
At present, only `--keyring-provider subprocess` is supported, which configures uv to use the `keyring` CLI to handle authentication.
Defaults to `disabled`.
May also be set with the `UV_KEYRING_PROVIDER` environment variable.
Possible values:
  * `disabled`: Do not use keyring for credential lookup
  * `subprocess`: Use the `keyring` command for credential lookup


`--link-mode` _link-mode_
    
The method to use when installing packages from the global cache.
This option is only used when building source distributions.
Defaults to `clone` (also known as Copy-on-Write) on macOS, and `hardlink` on Linux and Windows.
May also be set with the `UV_LINK_MODE` environment variable.
Possible values:
  * `clone`: Clone (i.e., copy-on-write) packages from the wheel into the `site-packages` directory
  * `copy`: Copy packages from the wheel into the `site-packages` directory
  * `hardlink`: Hard link packages from the wheel into the `site-packages` directory
  * `symlink`: Symbolically link packages from the wheel into the `site-packages` directory


`--native-tls`
    
Whether to load TLS certificates from the platform’s native certificate store.
By default, uv loads certificates from the bundled `webpki-roots` crate. The `webpki-roots` are a reliable set of trust roots from Mozilla, and including them in uv improves portability and performance (especially on macOS).
However, in some cases, you may want to use the platform’s native certificate store, especially if you’re relying on a corporate trust root (e.g., for a mandatory proxy) that’s included in your system’s certificate store.
May also be set with the `UV_NATIVE_TLS` environment variable.
`--no-binary`
    
Don’t install pre-built wheels.
The given packages will be built and installed from source. The resolver will still use pre-built wheels to extract package metadata, if available.
May also be set with the `UV_NO_BINARY` environment variable.
`--no-binary-package` _no-binary-package_
    
Don’t install pre-built wheels for a specific package
May also be set with the `UV_NO_BINARY_PACKAGE` environment variable.
`--no-build`
    
Don’t build source distributions.
When enabled, resolving will not run arbitrary Python code. The cached wheels of already-built source distributions will be reused, but operations that require building distributions will exit with an error.
`--no-build-isolation`
    
Disable isolation when building source distributions.
Assumes that build dependencies specified by PEP 518 are already installed.
May also be set with the `UV_NO_BUILD_ISOLATION` environment variable.
`--no-build-isolation-package` _no-build-isolation-package_
    
Disable isolation when building source distributions for a specific package.
Assumes that the packages’ build dependencies specified by PEP 518 are already installed.
`--no-build-package` _no-build-package_
    
Don’t build source distributions for a specific package
`--no-cache`, `-n`
    
Avoid reading from or writing to the cache, instead using a temporary directory for the duration of the operation
May also be set with the `UV_NO_CACHE` environment variable.
`--no-config`
    
Avoid discovering configuration files (`pyproject.toml`, `uv.toml`).
Normally, configuration files are discovered in the current directory, parent directories, or user configuration directories.
May also be set with the `UV_NO_CONFIG` environment variable.
`--no-index`
    
Ignore the registry index (e.g., PyPI), instead relying on direct URL dependencies and those provided via `--find-links`
`--no-progress`
    
Hide all progress outputs.
For example, spinners or progress bars.
May also be set with the `UV_NO_PROGRESS` environment variable.
`--no-python-downloads`
    
Disable automatic downloads of Python.
`--no-sources`
    
Ignore the `tool.uv.sources` table when resolving dependencies. Used to lock against the standards-compliant, publishable package metadata, as opposed to using any workspace, Git, URL, or local path sources
`--offline`
    
Disable network access.
When disabled, uv will only use locally cached data and locally available files.
May also be set with the `UV_OFFLINE` environment variable.
`--prerelease` _prerelease_
    
The strategy to use when considering pre-release versions.
By default, uv will accept pre-releases for packages that _only_ publish pre-releases, along with first-party requirements that contain an explicit pre-release marker in the declared specifiers (`if-necessary-or-explicit`).
May also be set with the `UV_PRERELEASE` environment variable.
Possible values:
  * `disallow`: Disallow all pre-release versions
  * `allow`: Allow all pre-release versions
  * `if-necessary`: Allow pre-release versions if all versions of a package are pre-release
  * `explicit`: Allow pre-release versions for first-party packages with explicit pre-release markers in their version requirements
  * `if-necessary-or-explicit`: Allow pre-release versions if all versions of a package are pre-release, or if the package has an explicit pre-release marker in its version requirements


`--project` _project_
    
Run the command within the given project directory.
All `pyproject.toml`, `uv.toml`, and `.python-version` files will be discovered by walking up the directory tree from the project root, as will the project’s virtual environment (`.venv`).
Other command-line arguments (such as relative paths) will be resolved relative to the current working directory.
See `--directory` to change the working directory entirely.
This setting has no effect when used in the `uv pip` interface.
`--python`, `-p` _python_
    
The Python interpreter to use during resolution.
A Python interpreter is required for building source distributions to determine package metadata when there are not wheels.
The interpreter is also used as the fallback value for the minimum Python version if `requires-python` is not set.
See uv python for details on Python discovery and supported request formats.
May also be set with the `UV_PYTHON` environment variable.
`--python-preference` _python-preference_
    
Whether to prefer uv-managed or system Python installations.
By default, uv prefers using Python versions it manages. However, it will use system Python installations if a uv-managed Python is not installed. This option allows prioritizing or ignoring system Python installations.
May also be set with the `UV_PYTHON_PREFERENCE` environment variable.
Possible values:
  * `only-managed`: Only use managed Python installations; never use system Python installations
  * `managed`: Prefer managed Python installations over system Python installations
  * `system`: Prefer system Python installations over managed Python installations
  * `only-system`: Only use system Python installations; never use managed Python installations


`--quiet`, `-q`
    
Do not print any output
`--refresh`
    
Refresh all cached data
`--refresh-package` _refresh-package_
    
Refresh cached data for a specific package
`--resolution` _resolution_
    
The strategy to use when selecting between the different compatible versions for a given package requirement.
By default, uv will use the latest compatible version of each package (`highest`).
May also be set with the `UV_RESOLUTION` environment variable.
Possible values:
  * `highest`: Resolve the highest compatible version of each package
  * `lowest`: Resolve the lowest compatible version of each package
  * `lowest-direct`: Resolve the lowest compatible version of any direct dependencies, and the highest compatible version of any transitive dependencies


`--script` _script_
    
Lock the specified Python script, rather than the current project.
If provided, uv will lock the script (based on its inline metadata table, in adherence with PEP 723) to a `.lock` file adjacent to the script itself.
`--upgrade`, `-U`
    
Allow package upgrades, ignoring pinned versions in any existing output file. Implies `--refresh`
`--upgrade-package`, `-P` _upgrade-package_
    
Allow upgrades for a specific package, ignoring pinned versions in any existing output file. Implies `--refresh-package`
`--verbose`, `-v`
    
Use verbose output.
You can configure fine-grained logging using the `RUST_LOG` environment variable. (<https://docs.rs/tracing-subscriber/latest/tracing_subscriber/filter/struct.EnvFilter.html#directives>)
`--version`, `-V`
    
Display the uv version
## uv export
Export the project's lockfile to an alternate format.
At present, only `requirements-txt` is supported.
The project is re-locked before exporting unless the `--locked` or `--frozen` flag is provided.
uv will search for a project in the current directory or any parent directory. If a project cannot be found, uv will exit with an error.
If operating in a workspace, the root will be exported by default; however, a specific member can be selected using the `--package` option.
### Usage
```
uv export [OPTIONS]

```

### Options
`--all-extras`
    
Include all optional dependencies
`--all-groups`
    
Include dependencies from all dependency groups.
`--no-group` can be used to exclude specific groups.
`--all-packages`
    
Export the entire workspace.
The dependencies for all workspace members will be included in the exported requirements file.
Any extras or groups specified via `--extra`, `--group`, or related options will be applied to all workspace members.
`--allow-insecure-host` _allow-insecure-host_
    
Allow insecure connections to a host.
Can be provided multiple times.
Expects to receive either a hostname (e.g., `localhost`), a host-port pair (e.g., `localhost:8080`), or a URL (e.g., `https://localhost`).
WARNING: Hosts included in this list will not be verified against the system’s certificate store. Only use `--allow-insecure-host` in a secure network with verified sources, as it bypasses SSL verification and could expose you to MITM attacks.
May also be set with the `UV_INSECURE_HOST` environment variable.
`--cache-dir` _cache-dir_
    
Path to the cache directory.
Defaults to `$XDG_CACHE_HOME/uv` or `$HOME/.cache/uv` on macOS and Linux, and `%LOCALAPPDATA%\uv\cache` on Windows.
To view the location of the cache directory, run `uv cache dir`.
May also be set with the `UV_CACHE_DIR` environment variable.
`--color` _color-choice_
    
Control the use of color in output.
By default, uv will automatically detect support for colors when writing to a terminal.
Possible values:
  * `auto`: Enables colored output only when the output is going to a terminal or TTY with support
  * `always`: Enables colored output regardless of the detected environment
  * `never`: Disables colored output


`--config-file` _config-file_
    
The path to a `uv.toml` file to use for configuration.
While uv configuration can be included in a `pyproject.toml` file, it is not allowed in this context.
May also be set with the `UV_CONFIG_FILE` environment variable.
`--config-setting`, `-C` _config-setting_
    
Settings to pass to the PEP 517 build backend, specified as `KEY=VALUE` pairs
`--default-index` _default-index_
    
The URL of the default package index (by default: <https://pypi.org/simple>).
Accepts either a repository compliant with PEP 503 (the simple repository API), or a local directory laid out in the same format.
The index given by this flag is given lower priority than all other indexes specified via the `--index` flag.
May also be set with the `UV_DEFAULT_INDEX` environment variable.
`--directory` _directory_
    
Change to the given directory prior to running the command.
Relative paths are resolved with the given directory as the base.
See `--project` to only change the project root directory.
`--exclude-newer` _exclude-newer_
    
Limit candidate packages to those that were uploaded prior to the given date.
Accepts both RFC 3339 timestamps (e.g., `2006-12-02T02:07:43Z`) and local dates in the same format (e.g., `2006-12-02`) in your system’s configured time zone.
May also be set with the `UV_EXCLUDE_NEWER` environment variable.
`--extra` _extra_
    
Include optional dependencies from the specified extra name.
May be provided more than once.
`--extra-index-url` _extra-index-url_
    
(Deprecated: use `--index` instead) Extra URLs of package indexes to use, in addition to `--index-url`.
Accepts either a repository compliant with PEP 503 (the simple repository API), or a local directory laid out in the same format.
All indexes provided via this flag take priority over the index specified by `--index-url` (which defaults to PyPI). When multiple `--extra-index-url` flags are provided, earlier values take priority.
May also be set with the `UV_EXTRA_INDEX_URL` environment variable.
`--find-links`, `-f` _find-links_
    
Locations to search for candidate distributions, in addition to those found in the registry indexes.
If a path, the target must be a directory that contains packages as wheel files (`.whl`) or source distributions (e.g., `.tar.gz` or `.zip`) at the top level.
If a URL, the page must contain a flat list of links to package files adhering to the formats described above.
May also be set with the `UV_FIND_LINKS` environment variable.
`--fork-strategy` _fork-strategy_
    
The strategy to use when selecting multiple versions of a given package across Python versions and platforms.
By default, uv will optimize for selecting the latest version of each package for each supported Python version (`requires-python`), while minimizing the number of selected versions across platforms.
Under `fewest`, uv will minimize the number of selected versions for each package, preferring older versions that are compatible with a wider range of supported Python versions or platforms.
May also be set with the `UV_FORK_STRATEGY` environment variable.
Possible values:
  * `fewest`: Optimize for selecting the fewest number of versions for each package. Older versions may be preferred if they are compatible with a wider range of supported Python versions or platforms
  * `requires-python`: Optimize for selecting latest supported version of each package, for each supported Python version


`--format` _format_
    
The format to which `uv.lock` should be exported.
At present, only `requirements-txt` is supported.
[default: requirements-txt]
Possible values:
  * `requirements-txt`: Export in `requirements.txt` format


`--frozen`
    
Do not update the `uv.lock` before exporting.
If a `uv.lock` does not exist, uv will exit with an error.
May also be set with the `UV_FROZEN` environment variable.
`--group` _group_
    
Include dependencies from the specified dependency group.
May be provided multiple times.
`--help`, `-h`
    
Display the concise help for this command
`--index` _index_
    
The URLs to use when resolving dependencies, in addition to the default index.
Accepts either a repository compliant with PEP 503 (the simple repository API), or a local directory laid out in the same format.
All indexes provided via this flag take priority over the index specified by `--default-index` (which defaults to PyPI). When multiple `--index` flags are provided, earlier values take priority.
May also be set with the `UV_INDEX` environment variable.
`--index-strategy` _index-strategy_
    
The strategy to use when resolving against multiple index URLs.
By default, uv will stop at the first index on which a given package is available, and limit resolutions to those present on that first index (`first-index`). This prevents "dependency confusion" attacks, whereby an attacker can upload a malicious package under the same name to an alternate index.
May also be set with the `UV_INDEX_STRATEGY` environment variable.
Possible values:
  * `first-index`: Only use results from the first index that returns a match for a given package name
  * `unsafe-first-match`: Search for every package name across all indexes, exhausting the versions from the first index before moving on to the next
  * `unsafe-best-match`: Search for every package name across all indexes, preferring the "best" version found. If a package version is in multiple indexes, only look at the entry for the first index


`--index-url`, `-i` _index-url_
    
(Deprecated: use `--default-index` instead) The URL of the Python package index (by default: <https://pypi.org/simple>).
Accepts either a repository compliant with PEP 503 (the simple repository API), or a local directory laid out in the same format.
The index given by this flag is given lower priority than all other indexes specified via the `--extra-index-url` flag.
May also be set with the `UV_INDEX_URL` environment variable.
`--keyring-provider` _keyring-provider_
    
Attempt to use `keyring` for authentication for index URLs.
At present, only `--keyring-provider subprocess` is supported, which configures uv to use the `keyring` CLI to handle authentication.
Defaults to `disabled`.
May also be set with the `UV_KEYRING_PROVIDER` environment variable.
Possible values:
  * `disabled`: Do not use keyring for credential lookup
  * `subprocess`: Use the `keyring` command for credential lookup


`--link-mode` _link-mode_
    
The method to use when installing packages from the global cache.
This option is only used when building source distributions.
Defaults to `clone` (also known as Copy-on-Write) on macOS, and `hardlink` on Linux and Windows.
May also be set with the `UV_LINK_MODE` environment variable.
Possible values:
  * `clone`: Clone (i.e., copy-on-write) packages from the wheel into the `site-packages` directory
  * `copy`: Copy packages from the wheel into the `site-packages` directory
  * `hardlink`: Hard link packages from the wheel into the `site-packages` directory
  * `symlink`: Symbolically link packages from the wheel into the `site-packages` directory


`--locked`
    
Assert that the `uv.lock` will remain unchanged.
Requires that the lockfile is up-to-date. If the lockfile is missing or needs to be updated, uv will exit with an error.
May also be set with the `UV_LOCKED` environment variable.
`--native-tls`
    
Whether to load TLS certificates from the platform’s native certificate store.
By default, uv loads certificates from the bundled `webpki-roots` crate. The `webpki-roots` are a reliable set of trust roots from Mozilla, and including them in uv improves portability and performance (especially on macOS).
However, in some cases, you may want to use the platform’s native certificate store, especially if you’re relying on a corporate trust root (e.g., for a mandatory proxy) that’s included in your system’s certificate store.
May also be set with the `UV_NATIVE_TLS` environment variable.
`--no-binary`
    
Don’t install pre-built wheels.
The given packages will be built and installed from source. The resolver will still use pre-built wheels to extract package metadata, if available.
May also be set with the `UV_NO_BINARY` environment variable.
`--no-binary-package` _no-binary-package_
    
Don’t install pre-built wheels for a specific package
May also be set with the `UV_NO_BINARY_PACKAGE` environment variable.
`--no-build`
    
Don’t build source distributions.
When enabled, resolving will not run arbitrary Python code. The cached wheels of already-built source distributions will be reused, but operations that require building distributions will exit with an error.
`--no-build-isolation`
    
Disable isolation when building source distributions.
Assumes that build dependencies specified by PEP 518 are already installed.
May also be set with the `UV_NO_BUILD_ISOLATION` environment variable.
`--no-build-isolation-package` _no-build-isolation-package_
    
Disable isolation when building source distributions for a specific package.
Assumes that the packages’ build dependencies specified by PEP 518 are already installed.
`--no-build-package` _no-build-package_
    
Don’t build source distributions for a specific package
`--no-cache`, `-n`
    
Avoid reading from or writing to the cache, instead using a temporary directory for the duration of the operation
May also be set with the `UV_NO_CACHE` environment variable.
`--no-config`
    
Avoid discovering configuration files (`pyproject.toml`, `uv.toml`).
Normally, configuration files are discovered in the current directory, parent directories, or user configuration directories.
May also be set with the `UV_NO_CONFIG` environment variable.
`--no-default-groups`
    
Ignore the the default dependency groups.
uv includes the groups defined in `tool.uv.default-groups` by default. This disables that option, however, specific groups can still be included with `--group`.
`--no-dev`
    
Disable the development dependency group.
This option is an alias of `--no-group dev`. See `--no-default-groups` to disable all default groups instead.
`--no-editable`
    
Install any editable dependencies, including the project and any workspace members, as non-editable
`--no-emit-package` _no-emit-package_
    
Do not emit the given package(s).
By default, all of the project’s dependencies are included in the exported requirements file. The `--no-install-package` option allows exclusion of specific packages.
`--no-emit-project`
    
Do not emit the current project.
By default, the current project is included in the exported requirements file with all of its dependencies. The `--no-emit-project` option allows the project to be excluded, but all of its dependencies to remain included.
`--no-emit-workspace`
    
Do not emit any workspace members, including the root project.
By default, all workspace members and their dependencies are included in the exported requirements file, with all of their dependencies. The `--no-emit-workspace` option allows exclusion of all the workspace members while retaining their dependencies.
`--no-extra` _no-extra_
    
Exclude the specified optional dependencies, if `--all-extras` is supplied.
May be provided multiple times.
`--no-group` _no-group_
    
Disable the specified dependency group.
This options always takes precedence over default groups, `--all-groups`, and `--group`.
May be provided multiple times.
`--no-hashes`
    
Omit hashes in the generated output
`--no-header`
    
Exclude the comment header at the top of the generated output file
`--no-index`
    
Ignore the registry index (e.g., PyPI), instead relying on direct URL dependencies and those provided via `--find-links`
`--no-progress`
    
Hide all progress outputs.
For example, spinners or progress bars.
May also be set with the `UV_NO_PROGRESS` environment variable.
`--no-python-downloads`
    
Disable automatic downloads of Python.
`--no-sources`
    
Ignore the `tool.uv.sources` table when resolving dependencies. Used to lock against the standards-compliant, publishable package metadata, as opposed to using any workspace, Git, URL, or local path sources
`--offline`
    
Disable network access.
When disabled, uv will only use locally cached data and locally available files.
May also be set with the `UV_OFFLINE` environment variable.
`--only-dev`
    
Only include the development dependency group.
The project and its dependencies will be omitted.
This option is an alias for `--only-group dev`. Implies `--no-default-groups`.
`--only-group` _only-group_
    
Only include dependencies from the specified dependency group.
The project and its dependencies will be omitted.
May be provided multiple times. Implies `--no-default-groups`.
`--output-file`, `-o` _output-file_
    
Write the exported requirements to the given file
`--package` _package_
    
Export the dependencies for a specific package in the workspace.
If the workspace member does not exist, uv will exit with an error.
`--prerelease` _prerelease_
    
The strategy to use when considering pre-release versions.
By default, uv will accept pre-releases for packages that _only_ publish pre-releases, along with first-party requirements that contain an explicit pre-release marker in the declared specifiers (`if-necessary-or-explicit`).
May also be set with the `UV_PRERELEASE` environment variable.
Possible values:
  * `disallow`: Disallow all pre-release versions
  * `allow`: Allow all pre-release versions
  * `if-necessary`: Allow pre-release versions if all versions of a package are pre-release
  * `explicit`: Allow pre-release versions for first-party packages with explicit pre-release markers in their version requirements
  * `if-necessary-or-explicit`: Allow pre-release versions if all versions of a package are pre-release, or if the package has an explicit pre-release marker in its version requirements


`--project` _project_
    
Run the command within the given project directory.
All `pyproject.toml`, `uv.toml`, and `.python-version` files will be discovered by walking up the directory tree from the project root, as will the project’s virtual environment (`.venv`).
Other command-line arguments (such as relative paths) will be resolved relative to the current working directory.
See `--directory` to change the working directory entirely.
This setting has no effect when used in the `uv pip` interface.
`--prune` _prune_
    
Prune the given package from the dependency tree.
Pruned packages will be excluded from the exported requirements file, as will any dependencies that are no longer required after the pruned package is removed.
`--python`, `-p` _python_
    
The Python interpreter to use during resolution.
A Python interpreter is required for building source distributions to determine package metadata when there are not wheels.
The interpreter is also used as the fallback value for the minimum Python version if `requires-python` is not set.
See uv python for details on Python discovery and supported request formats.
May also be set with the `UV_PYTHON` environment variable.
`--python-preference` _python-preference_
    
Whether to prefer uv-managed or system Python installations.
By default, uv prefers using Python versions it manages. However, it will use system Python installations if a uv-managed Python is not installed. This option allows prioritizing or ignoring system Python installations.
May also be set with the `UV_PYTHON_PREFERENCE` environment variable.
Possible values:
  * `only-managed`: Only use managed Python installations; never use system Python installations
  * `managed`: Prefer managed Python installations over system Python installations
  * `system`: Prefer system Python installations over managed Python installations
  * `only-system`: Only use system Python installations; never use managed Python installations


`--quiet`, `-q`
    
Do not print any output
`--refresh`
    
Refresh all cached data
`--refresh-package` _refresh-package_
    
Refresh cached data for a specific package
`--resolution` _resolution_
    
The strategy to use when selecting between the different compatible versions for a given package requirement.
By default, uv will use the latest compatible version of each package (`highest`).
May also be set with the `UV_RESOLUTION` environment variable.
Possible values:
  * `highest`: Resolve the highest compatible version of each package
  * `lowest`: Resolve the lowest compatible version of each package
  * `lowest-direct`: Resolve the lowest compatible version of any direct dependencies, and the highest compatible version of any transitive dependencies


`--script` _script_
    
Export the dependencies for the specified PEP 723 Python script, rather than the current project.
If provided, uv will resolve the dependencies based on its inline metadata table, in adherence with PEP 723.
`--upgrade`, `-U`
    
Allow package upgrades, ignoring pinned versions in any existing output file. Implies `--refresh`
`--upgrade-package`, `-P` _upgrade-package_
    
Allow upgrades for a specific package, ignoring pinned versions in any existing output file. Implies `--refresh-package`
`--verbose`, `-v`
    
Use verbose output.
You can configure fine-grained logging using the `RUST_LOG` environment variable. (<https://docs.rs/tracing-subscriber/latest/tracing_subscriber/filter/struct.EnvFilter.html#directives>)
`--version`, `-V`
    
Display the uv version
## uv tree
Display the project's dependency tree
### Usage
```
uv tree [OPTIONS]

```

### Options
`--all-groups`
    
Include dependencies from all dependency groups.
`--no-group` can be used to exclude specific groups.
`--allow-insecure-host` _allow-insecure-host_
    
Allow insecure connections to a host.
Can be provided multiple times.
Expects to receive either a hostname (e.g., `localhost`), a host-port pair (e.g., `localhost:8080`), or a URL (e.g., `https://localhost`).
WARNING: Hosts included in this list will not be verified against the system’s certificate store. Only use `--allow-insecure-host` in a secure network with verified sources, as it bypasses SSL verification and could expose you to MITM attacks.
May also be set with the `UV_INSECURE_HOST` environment variable.
`--cache-dir` _cache-dir_
    
Path to the cache directory.
Defaults to `$XDG_CACHE_HOME/uv` or `$HOME/.cache/uv` on macOS and Linux, and `%LOCALAPPDATA%\uv\cache` on Windows.
To view the location of the cache directory, run `uv cache dir`.
May also be set with the `UV_CACHE_DIR` environment variable.
`--color` _color-choice_
    
Control the use of color in output.
By default, uv will automatically detect support for colors when writing to a terminal.
Possible values:
  * `auto`: Enables colored output only when the output is going to a terminal or TTY with support
  * `always`: Enables colored output regardless of the detected environment
  * `never`: Disables colored output


`--config-file` _config-file_
    
The path to a `uv.toml` file to use for configuration.
While uv configuration can be included in a `pyproject.toml` file, it is not allowed in this context.
May also be set with the `UV_CONFIG_FILE` environment variable.
`--config-setting`, `-C` _config-setting_
    
Settings to pass to the PEP 517 build backend, specified as `KEY=VALUE` pairs
`--default-index` _default-index_
    
The URL of the default package index (by default: <https://pypi.org/simple>).
Accepts either a repository compliant with PEP 503 (the simple repository API), or a local directory laid out in the same format.
The index given by this flag is given lower priority than all other indexes specified via the `--index` flag.
May also be set with the `UV_DEFAULT_INDEX` environment variable.
`--depth`, `-d` _depth_
    
Maximum display depth of the dependency tree
[default: 255]
`--directory` _directory_
    
Change to the given directory prior to running the command.
Relative paths are resolved with the given directory as the base.
See `--project` to only change the project root directory.
`--exclude-newer` _exclude-newer_
    
Limit candidate packages to those that were uploaded prior to the given date.
Accepts both RFC 3339 timestamps (e.g., `2006-12-02T02:07:43Z`) and local dates in the same format (e.g., `2006-12-02`) in your system’s configured time zone.
May also be set with the `UV_EXCLUDE_NEWER` environment variable.
`--extra-index-url` _extra-index-url_
    
(Deprecated: use `--index` instead) Extra URLs of package indexes to use, in addition to `--index-url`.
Accepts either a repository compliant with PEP 503 (the simple repository API), or a local directory laid out in the same format.
All indexes provided via this flag take priority over the index specified by `--index-url` (which defaults to PyPI). When multiple `--extra-index-url` flags are provided, earlier values take priority.
May also be set with the `UV_EXTRA_INDEX_URL` environment variable.
`--find-links`, `-f` _find-links_
    
Locations to search for candidate distributions, in addition to those found in the registry indexes.
If a path, the target must be a directory that contains packages as wheel files (`.whl`) or source distributions (e.g., `.tar.gz` or `.zip`) at the top level.
If a URL, the page must contain a flat list of links to package files adhering to the formats described above.
May also be set with the `UV_FIND_LINKS` environment variable.
`--fork-strategy` _fork-strategy_
    
The strategy to use when selecting multiple versions of a given package across Python versions and platforms.
By default, uv will optimize for selecting the latest version of each package for each supported Python version (`requires-python`), while minimizing the number of selected versions across platforms.
Under `fewest`, uv will minimize the number of selected versions for each package, preferring older versions that are compatible with a wider range of supported Python versions or platforms.
May also be set with the `UV_FORK_STRATEGY` environment variable.
Possible values:
  * `fewest`: Optimize for selecting the fewest number of versions for each package. Older versions may be preferred if they are compatible with a wider range of supported Python versions or platforms
  * `requires-python`: Optimize for selecting latest supported version of each package, for each supported Python version


`--frozen`
    
Display the requirements without locking the project.
If the lockfile is missing, uv will exit with an error.
May also be set with the `UV_FROZEN` environment variable.
`--group` _group_
    
Include dependencies from the specified dependency group.
May be provided multiple times.
`--help`, `-h`
    
Display the concise help for this command
`--index` _index_
    
The URLs to use when resolving dependencies, in addition to the default index.
Accepts either a repository compliant with PEP 503 (the simple repository API), or a local directory laid out in the same format.
All indexes provided via this flag take priority over the index specified by `--default-index` (which defaults to PyPI). When multiple `--index` flags are provided, earlier values take priority.
May also be set with the `UV_INDEX` environment variable.
`--index-strategy` _index-strategy_
    
The strategy to use when resolving against multiple index URLs.
By default, uv will stop at the first index on which a given package is available, and limit resolutions to those present on that first index (`first-index`). This prevents "dependency confusion" attacks, whereby an attacker can upload a malicious package under the same name to an alternate index.
May also be set with the `UV_INDEX_STRATEGY` environment variable.
Possible values:
  * `first-index`: Only use results from the first index that returns a match for a given package name
  * `unsafe-first-match`: Search for every package name across all indexes, exhausting the versions from the first index before moving on to the next
  * `unsafe-best-match`: Search for every package name across all indexes, preferring the "best" version found. If a package version is in multiple indexes, only look at the entry for the first index


`--index-url`, `-i` _index-url_
    
(Deprecated: use `--default-index` instead) The URL of the Python package index (by default: <https://pypi.org/simple>).
Accepts either a repository compliant with PEP 503 (the simple repository API), or a local directory laid out in the same format.
The index given by this flag is given lower priority than all other indexes specified via the `--extra-index-url` flag.
May also be set with the `UV_INDEX_URL` environment variable.
`--invert`
    
Show the reverse dependencies for the given package. This flag will invert the tree and display the packages that depend on the given package
`--keyring-provider` _keyring-provider_
    
Attempt to use `keyring` for authentication for index URLs.
At present, only `--keyring-provider subprocess` is supported, which configures uv to use the `keyring` CLI to handle authentication.
Defaults to `disabled`.
May also be set with the `UV_KEYRING_PROVIDER` environment variable.
Possible values:
  * `disabled`: Do not use keyring for credential lookup
  * `subprocess`: Use the `keyring` command for credential lookup


`--link-mode` _link-mode_
    
The method to use when installing packages from the global cache.
This option is only used when building source distributions.
Defaults to `clone` (also known as Copy-on-Write) on macOS, and `hardlink` on Linux and Windows.
May also be set with the `UV_LINK_MODE` environment variable.
Possible values:
  * `clone`: Clone (i.e., copy-on-write) packages from the wheel into the `site-packages` directory
  * `copy`: Copy packages from the wheel into the `site-packages` directory
  * `hardlink`: Hard link packages from the wheel into the `site-packages` directory
  * `symlink`: Symbolically link packages from the wheel into the `site-packages` directory


`--locked`
    
Assert that the `uv.lock` will remain unchanged.
Requires that the lockfile is up-to-date. If the lockfile is missing or needs to be updated, uv will exit with an error.
May also be set with the `UV_LOCKED` environment variable.
`--native-tls`
    
Whether to load TLS certificates from the platform’s native certificate store.
By default, uv loads certificates from the bundled `webpki-roots` crate. The `webpki-roots` are a reliable set of trust roots from Mozilla, and including them in uv improves portability and performance (especially on macOS).
However, in some cases, you may want to use the platform’s native certificate store, especially if you’re relying on a corporate trust root (e.g., for a mandatory proxy) that’s included in your system’s certificate store.
May also be set with the `UV_NATIVE_TLS` environment variable.
`--no-binary`
    
Don’t install pre-built wheels.
The given packages will be built and installed from source. The resolver will still use pre-built wheels to extract package metadata, if available.
May also be set with the `UV_NO_BINARY` environment variable.
`--no-binary-package` _no-binary-package_
    
Don’t install pre-built wheels for a specific package
May also be set with the `UV_NO_BINARY_PACKAGE` environment variable.
`--no-build`
    
Don’t build source distributions.
When enabled, resolving will not run arbitrary Python code. The cached wheels of already-built source distributions will be reused, but operations that require building distributions will exit with an error.
`--no-build-isolation`
    
Disable isolation when building source distributions.
Assumes that build dependencies specified by PEP 518 are already installed.
May also be set with the `UV_NO_BUILD_ISOLATION` environment variable.
`--no-build-isolation-package` _no-build-isolation-package_
    
Disable isolation when building source distributions for a specific package.
Assumes that the packages’ build dependencies specified by PEP 518 are already installed.
`--no-build-package` _no-build-package_
    
Don’t build source distributions for a specific package
`--no-cache`, `-n`
    
Avoid reading from or writing to the cache, instead using a temporary directory for the duration of the operation
May also be set with the `UV_NO_CACHE` environment variable.
`--no-config`
    
Avoid discovering configuration files (`pyproject.toml`, `uv.toml`).
Normally, configuration files are discovered in the current directory, parent directories, or user configuration directories.
May also be set with the `UV_NO_CONFIG` environment variable.
`--no-dedupe`
    
Do not de-duplicate repeated dependencies. Usually, when a package has already displayed its dependencies, further occurrences will not re-display its dependencies, and will include a (*) to indicate it has already been shown. This flag will cause those duplicates to be repeated
`--no-default-groups`
    
Ignore the the default dependency groups.
uv includes the groups defined in `tool.uv.default-groups` by default. This disables that option, however, specific groups can still be included with `--group`.
`--no-dev`
    
Disable the development dependency group.
This option is an alias of `--no-group dev`. See `--no-default-groups` to disable all default groups instead.
`--no-group` _no-group_
    
Disable the specified dependency group.
This options always takes precedence over default groups, `--all-groups`, and `--group`.
May be provided multiple times.
`--no-index`
    
Ignore the registry index (e.g., PyPI), instead relying on direct URL dependencies and those provided via `--find-links`
`--no-progress`
    
Hide all progress outputs.
For example, spinners or progress bars.
May also be set with the `UV_NO_PROGRESS` environment variable.
`--no-python-downloads`
    
Disable automatic downloads of Python.
`--no-sources`
    
Ignore the `tool.uv.sources` table when resolving dependencies. Used to lock against the standards-compliant, publishable package metadata, as opposed to using any workspace, Git, URL, or local path sources
`--offline`
    
Disable network access.
When disabled, uv will only use locally cached data and locally available files.
May also be set with the `UV_OFFLINE` environment variable.
`--only-dev`
    
Only include the development dependency group.
The project and its dependencies will be omitted.
This option is an alias for `--only-group dev`. Implies `--no-default-groups`.
`--only-group` _only-group_
    
Only include dependencies from the specified dependency group.
The project and its dependencies will be omitted.
May be provided multiple times. Implies `--no-default-groups`.
`--outdated`
    
Show the latest available version of each package in the tree
`--package` _package_
    
Display only the specified packages
`--prerelease` _prerelease_
    
The strategy to use when considering pre-release versions.
By default, uv will accept pre-releases for packages that _only_ publish pre-releases, along with first-party requirements that contain an explicit pre-release marker in the declared specifiers (`if-necessary-or-explicit`).
May also be set with the `UV_PRERELEASE` environment variable.
Possible values:
  * `disallow`: Disallow all pre-release versions
  * `allow`: Allow all pre-release versions
  * `if-necessary`: Allow pre-release versions if all versions of a package are pre-release
  * `explicit`: Allow pre-release versions for first-party packages with explicit pre-release markers in their version requirements
  * `if-necessary-or-explicit`: Allow pre-release versions if all versions of a package are pre-release, or if the package has an explicit pre-release marker in its version requirements


`--project` _project_
    
Run the command within the given project directory.
All `pyproject.toml`, `uv.toml`, and `.python-version` files will be discovered by walking up the directory tree from the project root, as will the project’s virtual environment (`.venv`).
Other command-line arguments (such as relative paths) will be resolved relative to the current working directory.
See `--directory` to change the working directory entirely.
This setting has no effect when used in the `uv pip` interface.
`--prune` _prune_
    
Prune the given package from the display of the dependency tree
`--python`, `-p` _python_
    
The Python interpreter to use for locking and filtering.
By default, the tree is filtered to match the platform as reported by the Python interpreter. Use `--universal` to display the tree for all platforms, or use `--python-version` or `--python-platform` to override a subset of markers.
See uv python for details on Python discovery and supported request formats.
May also be set with the `UV_PYTHON` environment variable.
`--python-platform` _python-platform_
    
The platform to use when filtering the tree.
For example, pass `--platform windows` to display the dependencies that would be included when installing on Windows.
Represented as a "target triple", a string that describes the target platform in terms of its CPU, vendor, and operating system name, like `x86_64-unknown-linux-gnu` or `aarch64-apple-darwin`.
Possible values:
  * `windows`: An alias for `x86_64-pc-windows-msvc`, the default target for Windows
  * `linux`: An alias for `x86_64-unknown-linux-gnu`, the default target for Linux
  * `macos`: An alias for `aarch64-apple-darwin`, the default target for macOS
  * `x86_64-pc-windows-msvc`: A 64-bit x86 Windows target
  * `i686-pc-windows-msvc`: A 32-bit x86 Windows target
  * `x86_64-unknown-linux-gnu`: An x86 Linux target. Equivalent to `x86_64-manylinux_2_17`
  * `aarch64-apple-darwin`: An ARM-based macOS target, as seen on Apple Silicon devices
  * `x86_64-apple-darwin`: An x86 macOS target
  * `aarch64-unknown-linux-gnu`: An ARM64 Linux target. Equivalent to `aarch64-manylinux_2_17`
  * `aarch64-unknown-linux-musl`: An ARM64 Linux target
  * `x86_64-unknown-linux-musl`: An `x86_64` Linux target
  * `x86_64-manylinux2014`: An `x86_64` target for the `manylinux2014` platform. Equivalent to `x86_64-manylinux_2_17`
  * `x86_64-manylinux_2_17`: An `x86_64` target for the `manylinux_2_17` platform
  * `x86_64-manylinux_2_28`: An `x86_64` target for the `manylinux_2_28` platform
  * `x86_64-manylinux_2_31`: An `x86_64` target for the `manylinux_2_31` platform
  * `x86_64-manylinux_2_32`: An `x86_64` target for the `manylinux_2_32` platform
  * `x86_64-manylinux_2_33`: An `x86_64` target for the `manylinux_2_33` platform
  * `x86_64-manylinux_2_34`: An `x86_64` target for the `manylinux_2_34` platform
  * `x86_64-manylinux_2_35`: An `x86_64` target for the `manylinux_2_35` platform
  * `x86_64-manylinux_2_36`: An `x86_64` target for the `manylinux_2_36` platform
  * `x86_64-manylinux_2_37`: An `x86_64` target for the `manylinux_2_37` platform
  * `x86_64-manylinux_2_38`: An `x86_64` target for the `manylinux_2_38` platform
  * `x86_64-manylinux_2_39`: An `x86_64` target for the `manylinux_2_39` platform
  * `x86_64-manylinux_2_40`: An `x86_64` target for the `manylinux_2_40` platform
  * `aarch64-manylinux2014`: An ARM64 target for the `manylinux2014` platform. Equivalent to `aarch64-manylinux_2_17`
  * `aarch64-manylinux_2_17`: An ARM64 target for the `manylinux_2_17` platform
  * `aarch64-manylinux_2_28`: An ARM64 target for the `manylinux_2_28` platform
  * `aarch64-manylinux_2_31`: An ARM64 target for the `manylinux_2_31` platform
  * `aarch64-manylinux_2_32`: An ARM64 target for the `manylinux_2_32` platform
  * `aarch64-manylinux_2_33`: An ARM64 target for the `manylinux_2_33` platform
  * `aarch64-manylinux_2_34`: An ARM64 target for the `manylinux_2_34` platform
  * `aarch64-manylinux_2_35`: An ARM64 target for the `manylinux_2_35` platform
  * `aarch64-manylinux_2_36`: An ARM64 target for the `manylinux_2_36` platform
  * `aarch64-manylinux_2_37`: An ARM64 target for the `manylinux_2_37` platform
  * `aarch64-manylinux_2_38`: An ARM64 target for the `manylinux_2_38` platform
  * `aarch64-manylinux_2_39`: An ARM64 target for the `manylinux_2_39` platform
  * `aarch64-manylinux_2_40`: An ARM64 target for the `manylinux_2_40` platform


`--python-preference` _python-preference_
    
Whether to prefer uv-managed or system Python installations.
By default, uv prefers using Python versions it manages. However, it will use system Python installations if a uv-managed Python is not installed. This option allows prioritizing or ignoring system Python installations.
May also be set with the `UV_PYTHON_PREFERENCE` environment variable.
Possible values:
  * `only-managed`: Only use managed Python installations; never use system Python installations
  * `managed`: Prefer managed Python installations over system Python installations
  * `system`: Prefer system Python installations over managed Python installations
  * `only-system`: Only use system Python installations; never use managed Python installations


`--python-version` _python-version_
    
The Python version to use when filtering the tree.
For example, pass `--python-version 3.10` to display the dependencies that would be included when installing on Python 3.10.
Defaults to the version of the discovered Python interpreter.
`--quiet`, `-q`
    
Do not print any output
`--resolution` _resolution_
    
The strategy to use when selecting between the different compatible versions for a given package requirement.
By default, uv will use the latest compatible version of each package (`highest`).
May also be set with the `UV_RESOLUTION` environment variable.
Possible values:
  * `highest`: Resolve the highest compatible version of each package
  * `lowest`: Resolve the lowest compatible version of each package
  * `lowest-direct`: Resolve the lowest compatible version of any direct dependencies, and the highest compatible version of any transitive dependencies


`--script` _script_
    
Show the dependency tree the specified PEP 723 Python script, rather than the current project.
If provided, uv will resolve the dependencies based on its inline metadata table, in adherence with PEP 723.
`--universal`
    
Show a platform-independent dependency tree.
Shows resolved package versions for all Python versions and platforms, rather than filtering to those that are relevant for the current environment.
Multiple versions may be shown for a each package.
`--upgrade`, `-U`
    
Allow package upgrades, ignoring pinned versions in any existing output file. Implies `--refresh`
`--upgrade-package`, `-P` _upgrade-package_
    
Allow upgrades for a specific package, ignoring pinned versions in any existing output file. Implies `--refresh-package`
`--verbose`, `-v`
    
Use verbose output.
You can configure fine-grained logging using the `RUST_LOG` environment variable. (<https://docs.rs/tracing-subscriber/latest/tracing_subscriber/filter/struct.EnvFilter.html#directives>)
`--version`, `-V`
    
Display the uv version
## uv tool
Run and install commands provided by Python packages
### Usage
```
uv tool [OPTIONS] <COMMAND>

```

### Commands
`uv tool run`
    
Run a command provided by a Python package
`uv tool install`
    
Install commands provided by a Python package
`uv tool upgrade`
    
Upgrade installed tools
`uv tool list`
    
List installed tools
`uv tool uninstall`
    
Uninstall a tool
`uv tool update-shell`
    
Ensure that the tool executable directory is on the `PATH`
`uv tool dir`
    
Show the path to the uv tools directory
### uv tool run
Run a command provided by a Python package.
By default, the package to install is assumed to match the command name.
The name of the command can include an exact version in the format `<package>@<version>`, e.g., `uv tool run ruff@0.3.0`. If more complex version specification is desired or if the command is provided by a different package, use `--from`.
`uvx` can be used to invoke Python, e.g., with `uvx python` or `uvx python@<version>`. A Python interpreter will be started in an isolated virtual environment.
If the tool was previously installed, i.e., via `uv tool install`, the installed version will be used unless a version is requested or the `--isolated` flag is used.
`uvx` is provided as a convenient alias for `uv tool run`, their behavior is identical.
If no command is provided, the installed tools are displayed.
Packages are installed into an ephemeral virtual environment in the uv cache directory.
### Usage
```
uv tool run [OPTIONS] [COMMAND]

```

### Options
`--allow-insecure-host` _allow-insecure-host_
    
Allow insecure connections to a host.
Can be provided multiple times.
Expects to receive either a hostname (e.g., `localhost`), a host-port pair (e.g., `localhost:8080`), or a URL (e.g., `https://localhost`).
WARNING: Hosts included in this list will not be verified against the system’s certificate store. Only use `--allow-insecure-host` in a secure network with verified sources, as it bypasses SSL verification and could expose you to MITM attacks.
May also be set with the `UV_INSECURE_HOST` environment variable.
`--cache-dir` _cache-dir_
    
Path to the cache directory.
Defaults to `$XDG_CACHE_HOME/uv` or `$HOME/.cache/uv` on macOS and Linux, and `%LOCALAPPDATA%\uv\cache` on Windows.
To view the location of the cache directory, run `uv cache dir`.
May also be set with the `UV_CACHE_DIR` environment variable.
`--color` _color-choice_
    
Control the use of color in output.
By default, uv will automatically detect support for colors when writing to a terminal.
Possible values:
  * `auto`: Enables colored output only when the output is going to a terminal or TTY with support
  * `always`: Enables colored output regardless of the detected environment
  * `never`: Disables colored output


`--compile-bytecode`
    
Compile Python files to bytecode after installation.
By default, uv does not compile Python (`.py`) files to bytecode (`__pycache__/*.pyc`); instead, compilation is performed lazily the first time a module is imported. For use-cases in which start time is critical, such as CLI applications and Docker containers, this option can be enabled to trade longer installation times for faster start times.
When enabled, uv will process the entire site-packages directory (including packages that are not being modified by the current operation) for consistency. Like pip, it will also ignore errors.
May also be set with the `UV_COMPILE_BYTECODE` environment variable.
`--config-file` _config-file_
    
The path to a `uv.toml` file to use for configuration.
While uv configuration can be included in a `pyproject.toml` file, it is not allowed in this context.
May also be set with the `UV_CONFIG_FILE` environment variable.
`--config-setting`, `-C` _config-setting_
    
Settings to pass to the PEP 517 build backend, specified as `KEY=VALUE` pairs
`--default-index` _default-index_
    
The URL of the default package index (by default: <https://pypi.org/simple>).
Accepts either a repository compliant with PEP 503 (the simple repository API), or a local directory laid out in the same format.
The index given by this flag is given lower priority than all other indexes specified via the `--index` flag.
May also be set with the `UV_DEFAULT_INDEX` environment variable.
`--directory` _directory_
    
Change to the given directory prior to running the command.
Relative paths are resolved with the given directory as the base.
See `--project` to only change the project root directory.
`--exclude-newer` _exclude-newer_
    
Limit candidate packages to those that were uploaded prior to the given date.
Accepts both RFC 3339 timestamps (e.g., `2006-12-02T02:07:43Z`) and local dates in the same format (e.g., `2006-12-02`) in your system’s configured time zone.
May also be set with the `UV_EXCLUDE_NEWER` environment variable.
`--extra-index-url` _extra-index-url_
    
(Deprecated: use `--index` instead) Extra URLs of package indexes to use, in addition to `--index-url`.
Accepts either a repository compliant with PEP 503 (the simple repository API), or a local directory laid out in the same format.
All indexes provided via this flag take priority over the index specified by `--index-url` (which defaults to PyPI). When multiple `--extra-index-url` flags are provided, earlier values take priority.
May also be set with the `UV_EXTRA_INDEX_URL` environment variable.
`--find-links`, `-f` _find-links_
    
Locations to search for candidate distributions, in addition to those found in the registry indexes.
If a path, the target must be a directory that contains packages as wheel files (`.whl`) or source distributions (e.g., `.tar.gz` or `.zip`) at the top level.
If a URL, the page must contain a flat list of links to package files adhering to the formats described above.
May also be set with the `UV_FIND_LINKS` environment variable.
`--fork-strategy` _fork-strategy_
    
The strategy to use when selecting multiple versions of a given package across Python versions and platforms.
By default, uv will optimize for selecting the latest version of each package for each supported Python version (`requires-python`), while minimizing the number of selected versions across platforms.
Under `fewest`, uv will minimize the number of selected versions for each package, preferring older versions that are compatible with a wider range of supported Python versions or platforms.
May also be set with the `UV_FORK_STRATEGY` environment variable.
Possible values:
  * `fewest`: Optimize for selecting the fewest number of versions for each package. Older versions may be preferred if they are compatible with a wider range of supported Python versions or platforms
  * `requires-python`: Optimize for selecting latest supported version of each package, for each supported Python version


`--from` _from_
    
Use the given package to provide the command.
By default, the package name is assumed to match the command name.
`--help`, `-h`
    
Display the concise help for this command
`--index` _index_
    
The URLs to use when resolving dependencies, in addition to the default index.
Accepts either a repository compliant with PEP 503 (the simple repository API), or a local directory laid out in the same format.
All indexes provided via this flag take priority over the index specified by `--default-index` (which defaults to PyPI). When multiple `--index` flags are provided, earlier values take priority.
May also be set with the `UV_INDEX` environment variable.
`--index-strategy` _index-strategy_
    
The strategy to use when resolving against multiple index URLs.
By default, uv will stop at the first index on which a given package is available, and limit resolutions to those present on that first index (`first-index`). This prevents "dependency confusion" attacks, whereby an attacker can upload a malicious package under the same name to an alternate index.
May also be set with the `UV_INDEX_STRATEGY` environment variable.
Possible values:
  * `first-index`: Only use results from the first index that returns a match for a given package name
  * `unsafe-first-match`: Search for every package name across all indexes, exhausting the versions from the first index before moving on to the next
  * `unsafe-best-match`: Search for every package name across all indexes, preferring the "best" version found. If a package version is in multiple indexes, only look at the entry for the first index


`--index-url`, `-i` _index-url_
    
(Deprecated: use `--default-index` instead) The URL of the Python package index (by default: <https://pypi.org/simple>).
Accepts either a repository compliant with PEP 503 (the simple repository API), or a local directory laid out in the same format.
The index given by this flag is given lower priority than all other indexes specified via the `--extra-index-url` flag.
May also be set with the `UV_INDEX_URL` environment variable.
`--isolated`
    
Run the tool in an isolated virtual environment, ignoring any already-installed tools
`--keyring-provider` _keyring-provider_
    
Attempt to use `keyring` for authentication for index URLs.
At present, only `--keyring-provider subprocess` is supported, which configures uv to use the `keyring` CLI to handle authentication.
Defaults to `disabled`.
May also be set with the `UV_KEYRING_PROVIDER` environment variable.
Possible values:
  * `disabled`: Do not use keyring for credential lookup
  * `subprocess`: Use the `keyring` command for credential lookup


`--link-mode` _link-mode_
    
The method to use when installing packages from the global cache.
Defaults to `clone` (also known as Copy-on-Write) on macOS, and `hardlink` on Linux and Windows.
May also be set with the `UV_LINK_MODE` environment variable.
Possible values:
  * `clone`: Clone (i.e., copy-on-write) packages from the wheel into the `site-packages` directory
  * `copy`: Copy packages from the wheel into the `site-packages` directory
  * `hardlink`: Hard link packages from the wheel into the `site-packages` directory
  * `symlink`: Symbolically link packages from the wheel into the `site-packages` directory


`--native-tls`
    
Whether to load TLS certificates from the platform’s native certificate store.
By default, uv loads certificates from the bundled `webpki-roots` crate. The `webpki-roots` are a reliable set of trust roots from Mozilla, and including them in uv improves portability and performance (especially on macOS).
However, in some cases, you may want to use the platform’s native certificate store, especially if you’re relying on a corporate trust root (e.g., for a mandatory proxy) that’s included in your system’s certificate store.
May also be set with the `UV_NATIVE_TLS` environment variable.
`--no-binary`
    
Don’t install pre-built wheels.
The given packages will be built and installed from source. The resolver will still use pre-built wheels to extract package metadata, if available.
May also be set with the `UV_NO_BINARY` environment variable.
`--no-binary-package` _no-binary-package_
    
Don’t install pre-built wheels for a specific package
May also be set with the `UV_NO_BINARY_PACKAGE` environment variable.
`--no-build`
    
Don’t build source distributions.
When enabled, resolving will not run arbitrary Python code. The cached wheels of already-built source distributions will be reused, but operations that require building distributions will exit with an error.
`--no-build-isolation`
    
Disable isolation when building source distributions.
Assumes that build dependencies specified by PEP 518 are already installed.
May also be set with the `UV_NO_BUILD_ISOLATION` environment variable.
`--no-build-isolation-package` _no-build-isolation-package_
    
Disable isolation when building source distributions for a specific package.
Assumes that the packages’ build dependencies specified by PEP 518 are already installed.
`--no-build-package` _no-build-package_
    
Don’t build source distributions for a specific package
`--no-cache`, `-n`
    
Avoid reading from or writing to the cache, instead using a temporary directory for the duration of the operation
May also be set with the `UV_NO_CACHE` environment variable.
`--no-config`
    
Avoid discovering configuration files (`pyproject.toml`, `uv.toml`).
Normally, configuration files are discovered in the current directory, parent directories, or user configuration directories.
May also be set with the `UV_NO_CONFIG` environment variable.
`--no-index`
    
Ignore the registry index (e.g., PyPI), instead relying on direct URL dependencies and those provided via `--find-links`
`--no-progress`
    
Hide all progress outputs.
For example, spinners or progress bars.
May also be set with the `UV_NO_PROGRESS` environment variable.
`--no-python-downloads`
    
Disable automatic downloads of Python.
`--no-sources`
    
Ignore the `tool.uv.sources` table when resolving dependencies. Used to lock against the standards-compliant, publishable package metadata, as opposed to using any workspace, Git, URL, or local path sources
`--offline`
    
Disable network access.
When disabled, uv will only use locally cached data and locally available files.
May also be set with the `UV_OFFLINE` environment variable.
`--prerelease` _prerelease_
    
The strategy to use when considering pre-release versions.
By default, uv will accept pre-releases for packages that _only_ publish pre-releases, along with first-party requirements that contain an explicit pre-release marker in the declared specifiers (`if-necessary-or-explicit`).
May also be set with the `UV_PRERELEASE` environment variable.
Possible values:
  * `disallow`: Disallow all pre-release versions
  * `allow`: Allow all pre-release versions
  * `if-necessary`: Allow pre-release versions if all versions of a package are pre-release
  * `explicit`: Allow pre-release versions for first-party packages with explicit pre-release markers in their version requirements
  * `if-necessary-or-explicit`: Allow pre-release versions if all versions of a package are pre-release, or if the package has an explicit pre-release marker in its version requirements


`--project` _project_
    
Run the command within the given project directory.
All `pyproject.toml`, `uv.toml`, and `.python-version` files will be discovered by walking up the directory tree from the project root, as will the project’s virtual environment (`.venv`).
Other command-line arguments (such as relative paths) will be resolved relative to the current working directory.
See `--directory` to change the working directory entirely.
This setting has no effect when used in the `uv pip` interface.
`--python`, `-p` _python_
    
The Python interpreter to use to build the run environment.
See uv python for details on Python discovery and supported request formats.
May also be set with the `UV_PYTHON` environment variable.
`--python-preference` _python-preference_
    
Whether to prefer uv-managed or system Python installations.
By default, uv prefers using Python versions it manages. However, it will use system Python installations if a uv-managed Python is not installed. This option allows prioritizing or ignoring system Python installations.
May also be set with the `UV_PYTHON_PREFERENCE` environment variable.
Possible values:
  * `only-managed`: Only use managed Python installations; never use system Python installations
  * `managed`: Prefer managed Python installations over system Python installations
  * `system`: Prefer system Python installations over managed Python installations
  * `only-system`: Only use system Python installations; never use managed Python installations


`--quiet`, `-q`
    
Do not print any output
`--refresh`
    
Refresh all cached data
`--refresh-package` _refresh-package_
    
Refresh cached data for a specific package
`--reinstall`
    
Reinstall all packages, regardless of whether they’re already installed. Implies `--refresh`
`--reinstall-package` _reinstall-package_
    
Reinstall a specific package, regardless of whether it’s already installed. Implies `--refresh-package`
`--resolution` _resolution_
    
The strategy to use when selecting between the different compatible versions for a given package requirement.
By default, uv will use the latest compatible version of each package (`highest`).
May also be set with the `UV_RESOLUTION` environment variable.
Possible values:
  * `highest`: Resolve the highest compatible version of each package
  * `lowest`: Resolve the lowest compatible version of each package
  * `lowest-direct`: Resolve the lowest compatible version of any direct dependencies, and the highest compatible version of any transitive dependencies


`--upgrade`, `-U`
    
Allow package upgrades, ignoring pinned versions in any existing output file. Implies `--refresh`
`--upgrade-package`, `-P` _upgrade-package_
    
Allow upgrades for a specific package, ignoring pinned versions in any existing output file. Implies `--refresh-package`
`--verbose`, `-v`
    
Use verbose output.
You can configure fine-grained logging using the `RUST_LOG` environment variable. (<https://docs.rs/tracing-subscriber/latest/tracing_subscriber/filter/struct.EnvFilter.html#directives>)
`--version`, `-V`
    
Display the uv version
`--with` _with_
    
Run with the given packages installed
`--with-editable` _with-editable_
    
Run with the given packages installed in editable mode
When used in a project, these dependencies will be layered on top of the uv tool’s environment in a separate, ephemeral environment. These dependencies are allowed to conflict with those specified.
`--with-requirements` _with-requirements_
    
Run with all packages listed in the given `requirements.txt` files
### uv tool install
Install commands provided by a Python package.
Packages are installed into an isolated virtual environment in the uv tools directory. The executables are linked the tool executable directory, which is determined according to the XDG standard and can be retrieved with `uv tool dir --bin`.
If the tool was previously installed, the existing tool will generally be replaced.
### Usage
```
uv tool install [OPTIONS] <PACKAGE>

```

### Arguments
`PACKAGE`
    
The package to install commands from
### Options
`--allow-insecure-host` _allow-insecure-host_
    
Allow insecure connections to a host.
Can be provided multiple times.
Expects to receive either a hostname (e.g., `localhost`), a host-port pair (e.g., `localhost:8080`), or a URL (e.g., `https://localhost`).
WARNING: Hosts included in this list will not be verified against the system’s certificate store. Only use `--allow-insecure-host` in a secure network with verified sources, as it bypasses SSL verification and could expose you to MITM attacks.
May also be set with the `UV_INSECURE_HOST` environment variable.
`--cache-dir` _cache-dir_
    
Path to the cache directory.
Defaults to `$XDG_CACHE_HOME/uv` or `$HOME/.cache/uv` on macOS and Linux, and `%LOCALAPPDATA%\uv\cache` on Windows.
To view the location of the cache directory, run `uv cache dir`.
May also be set with the `UV_CACHE_DIR` environment variable.
`--color` _color-choice_
    
Control the use of color in output.
By default, uv will automatically detect support for colors when writing to a terminal.
Possible values:
  * `auto`: Enables colored output only when the output is going to a terminal or TTY with support
  * `always`: Enables colored output regardless of the detected environment
  * `never`: Disables colored output


`--compile-bytecode`
    
Compile Python files to bytecode after installation.
By default, uv does not compile Python (`.py`) files to bytecode (`__pycache__/*.pyc`); instead, compilation is performed lazily the first time a module is imported. For use-cases in which start time is critical, such as CLI applications and Docker containers, this option can be enabled to trade longer installation times for faster start times.
When enabled, uv will process the entire site-packages directory (including packages that are not being modified by the current operation) for consistency. Like pip, it will also ignore errors.
May also be set with the `UV_COMPILE_BYTECODE` environment variable.
`--config-file` _config-file_
    
The path to a `uv.toml` file to use for configuration.
While uv configuration can be included in a `pyproject.toml` file, it is not allowed in this context.
May also be set with the `UV_CONFIG_FILE` environment variable.
`--config-setting`, `-C` _config-setting_
    
Settings to pass to the PEP 517 build backend, specified as `KEY=VALUE` pairs
`--constraints`, `-c` _constraints_
    
Constrain versions using the given requirements files.
Constraints files are `requirements.txt`-like files that only control the _version_ of a requirement that’s installed. However, including a package in a constraints file will _not_ trigger the installation of that package.
This is equivalent to pip’s `--constraint` option.
May also be set with the `UV_CONSTRAINT` environment variable.
`--default-index` _default-index_
    
The URL of the default package index (by default: <https://pypi.org/simple>).
Accepts either a repository compliant with PEP 503 (the simple repository API), or a local directory laid out in the same format.
The index given by this flag is given lower priority than all other indexes specified via the `--index` flag.
May also be set with the `UV_DEFAULT_INDEX` environment variable.
`--directory` _directory_
    
Change to the given directory prior to running the command.
Relative paths are resolved with the given directory as the base.
See `--project` to only change the project root directory.
`--editable`, `-e`
    
Install the target package in editable mode, such that changes in the package’s source directory are reflected without reinstallation
`--exclude-newer` _exclude-newer_
    
Limit candidate packages to those that were uploaded prior to the given date.
Accepts both RFC 3339 timestamps (e.g., `2006-12-02T02:07:43Z`) and local dates in the same format (e.g., `2006-12-02`) in your system’s configured time zone.
May also be set with the `UV_EXCLUDE_NEWER` environment variable.
`--extra-index-url` _extra-index-url_
    
(Deprecated: use `--index` instead) Extra URLs of package indexes to use, in addition to `--index-url`.
Accepts either a repository compliant with PEP 503 (the simple repository API), or a local directory laid out in the same format.
All indexes provided via this flag take priority over the index specified by `--index-url` (which defaults to PyPI). When multiple `--extra-index-url` flags are provided, earlier values take priority.
May also be set with the `UV_EXTRA_INDEX_URL` environment variable.
`--find-links`, `-f` _find-links_
    
Locations to search for candidate distributions, in addition to those found in the registry indexes.
If a path, the target must be a directory that contains packages as wheel files (`.whl`) or source distributions (e.g., `.tar.gz` or `.zip`) at the top level.
If a URL, the page must contain a flat list of links to package files adhering to the formats described above.
May also be set with the `UV_FIND_LINKS` environment variable.
`--force`
    
Force installation of the tool.
Will replace any existing entry points with the same name in the executable directory.
`--fork-strategy` _fork-strategy_
    
The strategy to use when selecting multiple versions of a given package across Python versions and platforms.
By default, uv will optimize for selecting the latest version of each package for each supported Python version (`requires-python`), while minimizing the number of selected versions across platforms.
Under `fewest`, uv will minimize the number of selected versions for each package, preferring older versions that are compatible with a wider range of supported Python versions or platforms.
May also be set with the `UV_FORK_STRATEGY` environment variable.
Possible values:
  * `fewest`: Optimize for selecting the fewest number of versions for each package. Older versions may be preferred if they are compatible with a wider range of supported Python versions or platforms
  * `requires-python`: Optimize for selecting latest supported version of each package, for each supported Python version


`--help`, `-h`
    
Display the concise help for this command
`--index` _index_
    
The URLs to use when resolving dependencies, in addition to the default index.
Accepts either a repository compliant with PEP 503 (the simple repository API), or a local directory laid out in the same format.
All indexes provided via this flag take priority over the index specified by `--default-index` (which defaults to PyPI). When multiple `--index` flags are provided, earlier values take priority.
May also be set with the `UV_INDEX` environment variable.
`--index-strategy` _index-strategy_
    
The strategy to use when resolving against multiple index URLs.
By default, uv will stop at the first index on which a given package is available, and limit resolutions to those present on that first index (`first-index`). This prevents "dependency confusion" attacks, whereby an attacker can upload a malicious package under the same name to an alternate index.
May also be set with the `UV_INDEX_STRATEGY` environment variable.
Possible values:
  * `first-index`: Only use results from the first index that returns a match for a given package name
  * `unsafe-first-match`: Search for every package name across all indexes, exhausting the versions from the first index before moving on to the next
  * `unsafe-best-match`: Search for every package name across all indexes, preferring the "best" version found. If a package version is in multiple indexes, only look at the entry for the first index


`--index-url`, `-i` _index-url_
    
(Deprecated: use `--default-index` instead) The URL of the Python package index (by default: <https://pypi.org/simple>).
Accepts either a repository compliant with PEP 503 (the simple repository API), or a local directory laid out in the same format.
The index given by this flag is given lower priority than all other indexes specified via the `--extra-index-url` flag.
May also be set with the `UV_INDEX_URL` environment variable.
`--keyring-provider` _keyring-provider_
    
Attempt to use `keyring` for authentication for index URLs.
At present, only `--keyring-provider subprocess` is supported, which configures uv to use the `keyring` CLI to handle authentication.
Defaults to `disabled`.
May also be set with the `UV_KEYRING_PROVIDER` environment variable.
Possible values:
  * `disabled`: Do not use keyring for credential lookup
  * `subprocess`: Use the `keyring` command for credential lookup


`--link-mode` _link-mode_
    
The method to use when installing packages from the global cache.
Defaults to `clone` (also known as Copy-on-Write) on macOS, and `hardlink` on Linux and Windows.
May also be set with the `UV_LINK_MODE` environment variable.
Possible values:
  * `clone`: Clone (i.e., copy-on-write) packages from the wheel into the `site-packages` directory
  * `copy`: Copy packages from the wheel into the `site-packages` directory
  * `hardlink`: Hard link packages from the wheel into the `site-packages` directory
  * `symlink`: Symbolically link packages from the wheel into the `site-packages` directory


`--native-tls`
    
Whether to load TLS certificates from the platform’s native certificate store.
By default, uv loads certificates from the bundled `webpki-roots` crate. The `webpki-roots` are a reliable set of trust roots from Mozilla, and including them in uv improves portability and performance (especially on macOS).
However, in some cases, you may want to use the platform’s native certificate store, especially if you’re relying on a corporate trust root (e.g., for a mandatory proxy) that’s included in your system’s certificate store.
May also be set with the `UV_NATIVE_TLS` environment variable.
`--no-binary`
    
Don’t install pre-built wheels.
The given packages will be built and installed from source. The resolver will still use pre-built wheels to extract package metadata, if available.
May also be set with the `UV_NO_BINARY` environment variable.
`--no-binary-package` _no-binary-package_
    
Don’t install pre-built wheels for a specific package
May also be set with the `UV_NO_BINARY_PACKAGE` environment variable.
`--no-build`
    
Don’t build source distributions.
When enabled, resolving will not run arbitrary Python code. The cached wheels of already-built source distributions will be reused, but operations that require building distributions will exit with an error.
`--no-build-isolation`
    
Disable isolation when building source distributions.
Assumes that build dependencies specified by PEP 518 are already installed.
May also be set with the `UV_NO_BUILD_ISOLATION` environment variable.
`--no-build-isolation-package` _no-build-isolation-package_
    
Disable isolation when building source distributions for a specific package.
Assumes that the packages’ build dependencies specified by PEP 518 are already installed.
`--no-build-package` _no-build-package_
    
Don’t build source distributions for a specific package
`--no-cache`, `-n`
    
Avoid reading from or writing to the cache, instead using a temporary directory for the duration of the operation
May also be set with the `UV_NO_CACHE` environment variable.
`--no-config`
    
Avoid discovering configuration files (`pyproject.toml`, `uv.toml`).
Normally, configuration files are discovered in the current directory, parent directories, or user configuration directories.
May also be set with the `UV_NO_CONFIG` environment variable.
`--no-index`
    
Ignore the registry index (e.g., PyPI), instead relying on direct URL dependencies and those provided via `--find-links`
`--no-progress`
    
Hide all progress outputs.
For example, spinners or progress bars.
May also be set with the `UV_NO_PROGRESS` environment variable.
`--no-python-downloads`
    
Disable automatic downloads of Python.
`--no-sources`
    
Ignore the `tool.uv.sources` table when resolving dependencies. Used to lock against the standards-compliant, publishable package metadata, as opposed to using any workspace, Git, URL, or local path sources
`--offline`
    
Disable network access.
When disabled, uv will only use locally cached data and locally available files.
May also be set with the `UV_OFFLINE` environment variable.
`--overrides` _overrides_
    
Override versions using the given requirements files.
Overrides files are `requirements.txt`-like files that force a specific version of a requirement to be installed, regardless of the requirements declared by any constituent package, and regardless of whether this would be considered an invalid resolution.
While constraints are _additive_ , in that they’re combined with the requirements of the constituent packages, overrides are _absolute_ , in that they completely replace the requirements of the constituent packages.
May also be set with the `UV_OVERRIDE` environment variable.
`--prerelease` _prerelease_
    
The strategy to use when considering pre-release versions.
By default, uv will accept pre-releases for packages that _only_ publish pre-releases, along with first-party requirements that contain an explicit pre-release marker in the declared specifiers (`if-necessary-or-explicit`).
May also be set with the `UV_PRERELEASE` environment variable.
Possible values:
  * `disallow`: Disallow all pre-release versions
  * `allow`: Allow all pre-release versions
  * `if-necessary`: Allow pre-release versions if all versions of a package are pre-release
  * `explicit`: Allow pre-release versions for first-party packages with explicit pre-release markers in their version requirements
  * `if-necessary-or-explicit`: Allow pre-release versions if all versions of a package are pre-release, or if the package has an explicit pre-release marker in its version requirements


`--project` _project_
    
Run the command within the given project directory.
All `pyproject.toml`, `uv.toml`, and `.python-version` files will be discovered by walking up the directory tree from the project root, as will the project’s virtual environment (`.venv`).
Other command-line arguments (such as relative paths) will be resolved relative to the current working directory.
See `--directory` to change the working directory entirely.
This setting has no effect when used in the `uv pip` interface.
`--python`, `-p` _python_
    
The Python interpreter to use to build the tool environment.
See uv python for details on Python discovery and supported request formats.
May also be set with the `UV_PYTHON` environment variable.
`--python-preference` _python-preference_
    
Whether to prefer uv-managed or system Python installations.
By default, uv prefers using Python versions it manages. However, it will use system Python installations if a uv-managed Python is not installed. This option allows prioritizing or ignoring system Python installations.
May also be set with the `UV_PYTHON_PREFERENCE` environment variable.
Possible values:
  * `only-managed`: Only use managed Python installations; never use system Python installations
  * `managed`: Prefer managed Python installations over system Python installations
  * `system`: Prefer system Python installations over managed Python installations
  * `only-system`: Only use system Python installations; never use managed Python installations


`--quiet`, `-q`
    
Do not print any output
`--refresh`
    
Refresh all cached data
`--refresh-package` _refresh-package_
    
Refresh cached data for a specific package
`--reinstall`
    
Reinstall all packages, regardless of whether they’re already installed. Implies `--refresh`
`--reinstall-package` _reinstall-package_
    
Reinstall a specific package, regardless of whether it’s already installed. Implies `--refresh-package`
`--resolution` _resolution_
    
The strategy to use when selecting between the different compatible versions for a given package requirement.
By default, uv will use the latest compatible version of each package (`highest`).
May also be set with the `UV_RESOLUTION` environment variable.
Possible values:
  * `highest`: Resolve the highest compatible version of each package
  * `lowest`: Resolve the lowest compatible version of each package
  * `lowest-direct`: Resolve the lowest compatible version of any direct dependencies, and the highest compatible version of any transitive dependencies


`--upgrade`, `-U`
    
Allow package upgrades, ignoring pinned versions in any existing output file. Implies `--refresh`
`--upgrade-package`, `-P` _upgrade-package_
    
Allow upgrades for a specific package, ignoring pinned versions in any existing output file. Implies `--refresh-package`
`--verbose`, `-v`
    
Use verbose output.
You can configure fine-grained logging using the `RUST_LOG` environment variable. (<https://docs.rs/tracing-subscriber/latest/tracing_subscriber/filter/struct.EnvFilter.html#directives>)
`--version`, `-V`
    
Display the uv version
`--with` _with_
    
Include the following extra requirements
`--with-editable` _with-editable_
    
Include the given packages in editable mode
`--with-requirements` _with-requirements_
    
Run all requirements listed in the given `requirements.txt` files
### uv tool upgrade
Upgrade installed tools.
If a tool was installed with version constraints, they will be respected on upgrade — to upgrade a tool beyond the originally provided constraints, use `uv tool install` again.
If a tool was installed with specific settings, they will be respected on upgraded. For example, if `--prereleases allow` was provided during installation, it will continue to be respected in upgrades.
### Usage
```
uv tool upgrade [OPTIONS] <NAME>...

```

### Arguments
`NAME`
    
The name of the tool to upgrade, along with an optional version specifier
### Options
`--all`
    
Upgrade all tools
`--allow-insecure-host` _allow-insecure-host_
    
Allow insecure connections to a host.
Can be provided multiple times.
Expects to receive either a hostname (e.g., `localhost`), a host-port pair (e.g., `localhost:8080`), or a URL (e.g., `https://localhost`).
WARNING: Hosts included in this list will not be verified against the system’s certificate store. Only use `--allow-insecure-host` in a secure network with verified sources, as it bypasses SSL verification and could expose you to MITM attacks.
May also be set with the `UV_INSECURE_HOST` environment variable.
`--cache-dir` _cache-dir_
    
Path to the cache directory.
Defaults to `$XDG_CACHE_HOME/uv` or `$HOME/.cache/uv` on macOS and Linux, and `%LOCALAPPDATA%\uv\cache` on Windows.
To view the location of the cache directory, run `uv cache dir`.
May also be set with the `UV_CACHE_DIR` environment variable.
`--color` _color-choice_
    
Control the use of color in output.
By default, uv will automatically detect support for colors when writing to a terminal.
Possible values:
  * `auto`: Enables colored output only when the output is going to a terminal or TTY with support
  * `always`: Enables colored output regardless of the detected environment
  * `never`: Disables colored output


`--compile-bytecode`
    
Compile Python files to bytecode after installation.
By default, uv does not compile Python (`.py`) files to bytecode (`__pycache__/*.pyc`); instead, compilation is performed lazily the first time a module is imported. For use-cases in which start time is critical, such as CLI applications and Docker containers, this option can be enabled to trade longer installation times for faster start times.
When enabled, uv will process the entire site-packages directory (including packages that are not being modified by the current operation) for consistency. Like pip, it will also ignore errors.
May also be set with the `UV_COMPILE_BYTECODE` environment variable.
`--config-file` _config-file_
    
The path to a `uv.toml` file to use for configuration.
While uv configuration can be included in a `pyproject.toml` file, it is not allowed in this context.
May also be set with the `UV_CONFIG_FILE` environment variable.
`--config-setting`, `-C` _config-setting_
    
Settings to pass to the PEP 517 build backend, specified as `KEY=VALUE` pairs
`--default-index` _default-index_
    
The URL of the default package index (by default: <https://pypi.org/simple>).
Accepts either a repository compliant with PEP 503 (the simple repository API), or a local directory laid out in the same format.
The index given by this flag is given lower priority than all other indexes specified via the `--index` flag.
May also be set with the `UV_DEFAULT_INDEX` environment variable.
`--directory` _directory_
    
Change to the given directory prior to running the command.
Relative paths are resolved with the given directory as the base.
See `--project` to only change the project root directory.
`--exclude-newer` _exclude-newer_
    
Limit candidate packages to those that were uploaded prior to the given date.
Accepts both RFC 3339 timestamps (e.g., `2006-12-02T02:07:43Z`) and local dates in the same format (e.g., `2006-12-02`) in your system’s configured time zone.
May also be set with the `UV_EXCLUDE_NEWER` environment variable.
`--extra-index-url` _extra-index-url_
    
(Deprecated: use `--index` instead) Extra URLs of package indexes to use, in addition to `--index-url`.
Accepts either a repository compliant with PEP 503 (the simple repository API), or a local directory laid out in the same format.
All indexes provided via this flag take priority over the index specified by `--index-url` (which defaults to PyPI). When multiple `--extra-index-url` flags are provided, earlier values take priority.
May also be set with the `UV_EXTRA_INDEX_URL` environment variable.
`--find-links`, `-f` _find-links_
    
Locations to search for candidate distributions, in addition to those found in the registry indexes.
If a path, the target must be a directory that contains packages as wheel files (`.whl`) or source distributions (e.g., `.tar.gz` or `.zip`) at the top level.
If a URL, the page must contain a flat list of links to package files adhering to the formats described above.
May also be set with the `UV_FIND_LINKS` environment variable.
`--fork-strategy` _fork-strategy_
    
The strategy to use when selecting multiple versions of a given package across Python versions and platforms.
By default, uv will optimize for selecting the latest version of each package for each supported Python version (`requires-python`), while minimizing the number of selected versions across platforms.
Under `fewest`, uv will minimize the number of selected versions for each package, preferring older versions that are compatible with a wider range of supported Python versions or platforms.
May also be set with the `UV_FORK_STRATEGY` environment variable.
Possible values:
  * `fewest`: Optimize for selecting the fewest number of versions for each package. Older versions may be preferred if they are compatible with a wider range of supported Python versions or platforms
  * `requires-python`: Optimize for selecting latest supported version of each package, for each supported Python version


`--help`, `-h`
    
Display the concise help for this command
`--index` _index_
    
The URLs to use when resolving dependencies, in addition to the default index.
Accepts either a repository compliant with PEP 503 (the simple repository API), or a local directory laid out in the same format.
All indexes provided via this flag take priority over the index specified by `--default-index` (which defaults to PyPI). When multiple `--index` flags are provided, earlier values take priority.
May also be set with the `UV_INDEX` environment variable.
`--index-strategy` _index-strategy_
    
The strategy to use when resolving against multiple index URLs.
By default, uv will stop at the first index on which a given package is available, and limit resolutions to those present on that first index (`first-index`). This prevents "dependency confusion" attacks, whereby an attacker can upload a malicious package under the same name to an alternate index.
May also be set with the `UV_INDEX_STRATEGY` environment variable.
Possible values:
  * `first-index`: Only use results from the first index that returns a match for a given package name
  * `unsafe-first-match`: Search for every package name across all indexes, exhausting the versions from the first index before moving on to the next
  * `unsafe-best-match`: Search for every package name across all indexes, preferring the "best" version found. If a package version is in multiple indexes, only look at the entry for the first index


`--index-url`, `-i` _index-url_
    
(Deprecated: use `--default-index` instead) The URL of the Python package index (by default: <https://pypi.org/simple>).
Accepts either a repository compliant with PEP 503 (the simple repository API), or a local directory laid out in the same format.
The index given by this flag is given lower priority than all other indexes specified via the `--extra-index-url` flag.
May also be set with the `UV_INDEX_URL` environment variable.
`--keyring-provider` _keyring-provider_
    
Attempt to use `keyring` for authentication for index URLs.
At present, only `--keyring-provider subprocess` is supported, which configures uv to use the `keyring` CLI to handle authentication.
Defaults to `disabled`.
May also be set with the `UV_KEYRING_PROVIDER` environment variable.
Possible values:
  * `disabled`: Do not use keyring for credential lookup
  * `subprocess`: Use the `keyring` command for credential lookup


`--link-mode` _link-mode_
    
The method to use when installing packages from the global cache.
Defaults to `clone` (also known as Copy-on-Write) on macOS, and `hardlink` on Linux and Windows.
May also be set with the `UV_LINK_MODE` environment variable.
Possible values:
  * `clone`: Clone (i.e., copy-on-write) packages from the wheel into the `site-packages` directory
  * `copy`: Copy packages from the wheel into the `site-packages` directory
  * `hardlink`: Hard link packages from the wheel into the `site-packages` directory
  * `symlink`: Symbolically link packages from the wheel into the `site-packages` directory


`--native-tls`
    
Whether to load TLS certificates from the platform’s native certificate store.
By default, uv loads certificates from the bundled `webpki-roots` crate. The `webpki-roots` are a reliable set of trust roots from Mozilla, and including them in uv improves portability and performance (especially on macOS).
However, in some cases, you may want to use the platform’s native certificate store, especially if you’re relying on a corporate trust root (e.g., for a mandatory proxy) that’s included in your system’s certificate store.
May also be set with the `UV_NATIVE_TLS` environment variable.
`--no-binary`
    
Don’t install pre-built wheels.
The given packages will be built and installed from source. The resolver will still use pre-built wheels to extract package metadata, if available.
May also be set with the `UV_NO_BINARY` environment variable.
`--no-binary-package` _no-binary-package_
    
Don’t install pre-built wheels for a specific package
May also be set with the `UV_NO_BINARY_PACKAGE` environment variable.
`--no-build`
    
Don’t build source distributions.
When enabled, resolving will not run arbitrary Python code. The cached wheels of already-built source distributions will be reused, but operations that require building distributions will exit with an error.
`--no-build-isolation`
    
Disable isolation when building source distributions.
Assumes that build dependencies specified by PEP 518 are already installed.
May also be set with the `UV_NO_BUILD_ISOLATION` environment variable.
`--no-build-isolation-package` _no-build-isolation-package_
    
Disable isolation when building source distributions for a specific package.
Assumes that the packages’ build dependencies specified by PEP 518 are already installed.
`--no-build-package` _no-build-package_
    
Don’t build source distributions for a specific package
`--no-cache`, `-n`
    
Avoid reading from or writing to the cache, instead using a temporary directory for the duration of the operation
May also be set with the `UV_NO_CACHE` environment variable.
`--no-config`
    
Avoid discovering configuration files (`pyproject.toml`, `uv.toml`).
Normally, configuration files are discovered in the current directory, parent directories, or user configuration directories.
May also be set with the `UV_NO_CONFIG` environment variable.
`--no-index`
    
Ignore the registry index (e.g., PyPI), instead relying on direct URL dependencies and those provided via `--find-links`
`--no-progress`
    
Hide all progress outputs.
For example, spinners or progress bars.
May also be set with the `UV_NO_PROGRESS` environment variable.
`--no-python-downloads`
    
Disable automatic downloads of Python.
`--no-sources`
    
Ignore the `tool.uv.sources` table when resolving dependencies. Used to lock against the standards-compliant, publishable package metadata, as opposed to using any workspace, Git, URL, or local path sources
`--offline`
    
Disable network access.
When disabled, uv will only use locally cached data and locally available files.
May also be set with the `UV_OFFLINE` environment variable.
`--prerelease` _prerelease_
    
The strategy to use when considering pre-release versions.
By default, uv will accept pre-releases for packages that _only_ publish pre-releases, along with first-party requirements that contain an explicit pre-release marker in the declared specifiers (`if-necessary-or-explicit`).
May also be set with the `UV_PRERELEASE` environment variable.
Possible values:
  * `disallow`: Disallow all pre-release versions
  * `allow`: Allow all pre-release versions
  * `if-necessary`: Allow pre-release versions if all versions of a package are pre-release
  * `explicit`: Allow pre-release versions for first-party packages with explicit pre-release markers in their version requirements
  * `if-necessary-or-explicit`: Allow pre-release versions if all versions of a package are pre-release, or if the package has an explicit pre-release marker in its version requirements


`--project` _project_
    
Run the command within the given project directory.
All `pyproject.toml`, `uv.toml`, and `.python-version` files will be discovered by walking up the directory tree from the project root, as will the project’s virtual environment (`.venv`).
Other command-line arguments (such as relative paths) will be resolved relative to the current working directory.
See `--directory` to change the working directory entirely.
This setting has no effect when used in the `uv pip` interface.
`--python`, `-p` _python_
    
Upgrade a tool, and specify it to use the given Python interpreter to build its environment. Use with `--all` to apply to all tools.
See uv python for details on Python discovery and supported request formats.
May also be set with the `UV_PYTHON` environment variable.
`--python-preference` _python-preference_
    
Whether to prefer uv-managed or system Python installations.
By default, uv prefers using Python versions it manages. However, it will use system Python installations if a uv-managed Python is not installed. This option allows prioritizing or ignoring system Python installations.
May also be set with the `UV_PYTHON_PREFERENCE` environment variable.
Possible values:
  * `only-managed`: Only use managed Python installations; never use system Python installations
  * `managed`: Prefer managed Python installations over system Python installations
  * `system`: Prefer system Python installations over managed Python installations
  * `only-system`: Only use system Python installations; never use managed Python installations


`--quiet`, `-q`
    
Do not print any output
`--reinstall`
    
Reinstall all packages, regardless of whether they’re already installed. Implies `--refresh`
`--reinstall-package` _reinstall-package_
    
Reinstall a specific package, regardless of whether it’s already installed. Implies `--refresh-package`
`--resolution` _resolution_
    
The strategy to use when selecting between the different compatible versions for a given package requirement.
By default, uv will use the latest compatible version of each package (`highest`).
May also be set with the `UV_RESOLUTION` environment variable.
Possible values:
  * `highest`: Resolve the highest compatible version of each package
  * `lowest`: Resolve the lowest compatible version of each package
  * `lowest-direct`: Resolve the lowest compatible version of any direct dependencies, and the highest compatible version of any transitive dependencies


`--verbose`, `-v`
    
Use verbose output.
You can configure fine-grained logging using the `RUST_LOG` environment variable. (<https://docs.rs/tracing-subscriber/latest/tracing_subscriber/filter/struct.EnvFilter.html#directives>)
`--version`, `-V`
    
Display the uv version
### uv tool list
List installed tools
### Usage
```
uv tool list [OPTIONS]

```

### Options
`--allow-insecure-host` _allow-insecure-host_
    
Allow insecure connections to a host.
Can be provided multiple times.
Expects to receive either a hostname (e.g., `localhost`), a host-port pair (e.g., `localhost:8080`), or a URL (e.g., `https://localhost`).
WARNING: Hosts included in this list will not be verified against the system’s certificate store. Only use `--allow-insecure-host` in a secure network with verified sources, as it bypasses SSL verification and could expose you to MITM attacks.
May also be set with the `UV_INSECURE_HOST` environment variable.
`--cache-dir` _cache-dir_
    
Path to the cache directory.
Defaults to `$XDG_CACHE_HOME/uv` or `$HOME/.cache/uv` on macOS and Linux, and `%LOCALAPPDATA%\uv\cache` on Windows.
To view the location of the cache directory, run `uv cache dir`.
May also be set with the `UV_CACHE_DIR` environment variable.
`--color` _color-choice_
    
Control the use of color in output.
By default, uv will automatically detect support for colors when writing to a terminal.
Possible values:
  * `auto`: Enables colored output only when the output is going to a terminal or TTY with support
  * `always`: Enables colored output regardless of the detected environment
  * `never`: Disables colored output


`--config-file` _config-file_
    
The path to a `uv.toml` file to use for configuration.
While uv configuration can be included in a `pyproject.toml` file, it is not allowed in this context.
May also be set with the `UV_CONFIG_FILE` environment variable.
`--directory` _directory_
    
Change to the given directory prior to running the command.
Relative paths are resolved with the given directory as the base.
See `--project` to only change the project root directory.
`--help`, `-h`
    
Display the concise help for this command
`--native-tls`
    
Whether to load TLS certificates from the platform’s native certificate store.
By default, uv loads certificates from the bundled `webpki-roots` crate. The `webpki-roots` are a reliable set of trust roots from Mozilla, and including them in uv improves portability and performance (especially on macOS).
However, in some cases, you may want to use the platform’s native certificate store, especially if you’re relying on a corporate trust root (e.g., for a mandatory proxy) that’s included in your system’s certificate store.
May also be set with the `UV_NATIVE_TLS` environment variable.
`--no-cache`, `-n`
    
Avoid reading from or writing to the cache, instead using a temporary directory for the duration of the operation
May also be set with the `UV_NO_CACHE` environment variable.
`--no-config`
    
Avoid discovering configuration files (`pyproject.toml`, `uv.toml`).
Normally, configuration files are discovered in the current directory, parent directories, or user configuration directories.
May also be set with the `UV_NO_CONFIG` environment variable.
`--no-progress`
    
Hide all progress outputs.
For example, spinners or progress bars.
May also be set with the `UV_NO_PROGRESS` environment variable.
`--offline`
    
Disable network access.
When disabled, uv will only use locally cached data and locally available files.
May also be set with the `UV_OFFLINE` environment variable.
`--project` _project_
    
Run the command within the given project directory.
All `pyproject.toml`, `uv.toml`, and `.python-version` files will be discovered by walking up the directory tree from the project root, as will the project’s virtual environment (`.venv`).
Other command-line arguments (such as relative paths) will be resolved relative to the current working directory.
See `--directory` to change the working directory entirely.
This setting has no effect when used in the `uv pip` interface.
`--quiet`, `-q`
    
Do not print any output
`--show-paths`
    
Whether to display the path to each tool environment and installed executable
`--show-version-specifiers`
    
Whether to display the version specifier(s) used to install each tool
`--verbose`, `-v`
    
Use verbose output.
You can configure fine-grained logging using the `RUST_LOG` environment variable. (<https://docs.rs/tracing-subscriber/latest/tracing_subscriber/filter/struct.EnvFilter.html#directives>)
`--version`, `-V`
    
Display the uv version
### uv tool uninstall
Uninstall a tool
### Usage
```
uv tool uninstall [OPTIONS] <NAME>...

```

### Arguments
`NAME`
    
The name of the tool to uninstall
### Options
`--all`
    
Uninstall all tools
`--allow-insecure-host` _allow-insecure-host_
    
Allow insecure connections to a host.
Can be provided multiple times.
Expects to receive either a hostname (e.g., `localhost`), a host-port pair (e.g., `localhost:8080`), or a URL (e.g., `https://localhost`).
WARNING: Hosts included in this list will not be verified against the system’s certificate store. Only use `--allow-insecure-host` in a secure network with verified sources, as it bypasses SSL verification and could expose you to MITM attacks.
May also be set with the `UV_INSECURE_HOST` environment variable.
`--cache-dir` _cache-dir_
    
Path to the cache directory.
Defaults to `$XDG_CACHE_HOME/uv` or `$HOME/.cache/uv` on macOS and Linux, and `%LOCALAPPDATA%\uv\cache` on Windows.
To view the location of the cache directory, run `uv cache dir`.
May also be set with the `UV_CACHE_DIR` environment variable.
`--color` _color-choice_
    
Control the use of color in output.
By default, uv will automatically detect support for colors when writing to a terminal.
Possible values:
  * `auto`: Enables colored output only when the output is going to a terminal or TTY with support
  * `always`: Enables colored output regardless of the detected environment
  * `never`: Disables colored output


`--config-file` _config-file_
    
The path to a `uv.toml` file to use for configuration.
While uv configuration can be included in a `pyproject.toml` file, it is not allowed in this context.
May also be set with the `UV_CONFIG_FILE` environment variable.
`--directory` _directory_
    
Change to the given directory prior to running the command.
Relative paths are resolved with the given directory as the base.
See `--project` to only change the project root directory.
`--help`, `-h`
    
Display the concise help for this command
`--native-tls`
    
Whether to load TLS certificates from the platform’s native certificate store.
By default, uv loads certificates from the bundled `webpki-roots` crate. The `webpki-roots` are a reliable set of trust roots from Mozilla, and including them in uv improves portability and performance (especially on macOS).
However, in some cases, you may want to use the platform’s native certificate store, especially if you’re relying on a corporate trust root (e.g., for a mandatory proxy) that’s included in your system’s certificate store.
May also be set with the `UV_NATIVE_TLS` environment variable.
`--no-cache`, `-n`
    
Avoid reading from or writing to the cache, instead using a temporary directory for the duration of the operation
May also be set with the `UV_NO_CACHE` environment variable.
`--no-config`
    
Avoid discovering configuration files (`pyproject.toml`, `uv.toml`).
Normally, configuration files are discovered in the current directory, parent directories, or user configuration directories.
May also be set with the `UV_NO_CONFIG` environment variable.
`--no-progress`
    
Hide all progress outputs.
For example, spinners or progress bars.
May also be set with the `UV_NO_PROGRESS` environment variable.
`--no-python-downloads`
    
Disable automatic downloads of Python.
`--offline`
    
Disable network access.
When disabled, uv will only use locally cached data and locally available files.
May also be set with the `UV_OFFLINE` environment variable.
`--project` _project_
    
Run the command within the given project directory.
All `pyproject.toml`, `uv.toml`, and `.python-version` files will be discovered by walking up the directory tree from the project root, as will the project’s virtual environment (`.venv`).
Other command-line arguments (such as relative paths) will be resolved relative to the current working directory.
See `--directory` to change the working directory entirely.
This setting has no effect when used in the `uv pip` interface.
`--python-preference` _python-preference_
    
Whether to prefer uv-managed or system Python installations.
By default, uv prefers using Python versions it manages. However, it will use system Python installations if a uv-managed Python is not installed. This option allows prioritizing or ignoring system Python installations.
May also be set with the `UV_PYTHON_PREFERENCE` environment variable.
Possible values:
  * `only-managed`: Only use managed Python installations; never use system Python installations
  * `managed`: Prefer managed Python installations over system Python installations
  * `system`: Prefer system Python installations over managed Python installations
  * `only-system`: Only use system Python installations; never use managed Python installations


`--quiet`, `-q`
    
Do not print any output
`--verbose`, `-v`
    
Use verbose output.
You can configure fine-grained logging using the `RUST_LOG` environment variable. (<https://docs.rs/tracing-subscriber/latest/tracing_subscriber/filter/struct.EnvFilter.html#directives>)
`--version`, `-V`
    
Display the uv version
### uv tool update-shell
Ensure that the tool executable directory is on the `PATH`.
If the tool executable directory is not present on the `PATH`, uv will attempt to add it to the relevant shell configuration files.
If the shell configuration files already include a blurb to add the executable directory to the path, but the directory is not present on the `PATH`, uv will exit with an error.
The tool executable directory is determined according to the XDG standard and can be retrieved with `uv tool dir --bin`.
### Usage
```
uv tool update-shell [OPTIONS]

```

### Options
`--allow-insecure-host` _allow-insecure-host_
    
Allow insecure connections to a host.
Can be provided multiple times.
Expects to receive either a hostname (e.g., `localhost`), a host-port pair (e.g., `localhost:8080`), or a URL (e.g., `https://localhost`).
WARNING: Hosts included in this list will not be verified against the system’s certificate store. Only use `--allow-insecure-host` in a secure network with verified sources, as it bypasses SSL verification and could expose you to MITM attacks.
May also be set with the `UV_INSECURE_HOST` environment variable.
`--cache-dir` _cache-dir_
    
Path to the cache directory.
Defaults to `$XDG_CACHE_HOME/uv` or `$HOME/.cache/uv` on macOS and Linux, and `%LOCALAPPDATA%\uv\cache` on Windows.
To view the location of the cache directory, run `uv cache dir`.
May also be set with the `UV_CACHE_DIR` environment variable.
`--color` _color-choice_
    
Control the use of color in output.
By default, uv will automatically detect support for colors when writing to a terminal.
Possible values:
  * `auto`: Enables colored output only when the output is going to a terminal or TTY with support
  * `always`: Enables colored output regardless of the detected environment
  * `never`: Disables colored output


`--config-file` _config-file_
    
The path to a `uv.toml` file to use for configuration.
While uv configuration can be included in a `pyproject.toml` file, it is not allowed in this context.
May also be set with the `UV_CONFIG_FILE` environment variable.
`--directory` _directory_
    
Change to the given directory prior to running the command.
Relative paths are resolved with the given directory as the base.
See `--project` to only change the project root directory.
`--help`, `-h`
    
Display the concise help for this command
`--native-tls`
    
Whether to load TLS certificates from the platform’s native certificate store.
By default, uv loads certificates from the bundled `webpki-roots` crate. The `webpki-roots` are a reliable set of trust roots from Mozilla, and including them in uv improves portability and performance (especially on macOS).
However, in some cases, you may want to use the platform’s native certificate store, especially if you’re relying on a corporate trust root (e.g., for a mandatory proxy) that’s included in your system’s certificate store.
May also be set with the `UV_NATIVE_TLS` environment variable.
`--no-cache`, `-n`
    
Avoid reading from or writing to the cache, instead using a temporary directory for the duration of the operation
May also be set with the `UV_NO_CACHE` environment variable.
`--no-config`
    
Avoid discovering configuration files (`pyproject.toml`, `uv.toml`).
Normally, configuration files are discovered in the current directory, parent directories, or user configuration directories.
May also be set with the `UV_NO_CONFIG` environment variable.
`--no-progress`
    
Hide all progress outputs.
For example, spinners or progress bars.
May also be set with the `UV_NO_PROGRESS` environment variable.
`--no-python-downloads`
    
Disable automatic downloads of Python.
`--offline`
    
Disable network access.
When disabled, uv will only use locally cached data and locally available files.
May also be set with the `UV_OFFLINE` environment variable.
`--project` _project_
    
Run the command within the given project directory.
All `pyproject.toml`, `uv.toml`, and `.python-version` files will be discovered by walking up the directory tree from the project root, as will the project’s virtual environment (`.venv`).
Other command-line arguments (such as relative paths) will be resolved relative to the current working directory.
See `--directory` to change the working directory entirely.
This setting has no effect when used in the `uv pip` interface.
`--python-preference` _python-preference_
    
Whether to prefer uv-managed or system Python installations.
By default, uv prefers using Python versions it manages. However, it will use system Python installations if a uv-managed Python is not installed. This option allows prioritizing or ignoring system Python installations.
May also be set with the `UV_PYTHON_PREFERENCE` environment variable.
Possible values:
  * `only-managed`: Only use managed Python installations; never use system Python installations
  * `managed`: Prefer managed Python installations over system Python installations
  * `system`: Prefer system Python installations over managed Python installations
  * `only-system`: Only use system Python installations; never use managed Python installations


`--quiet`, `-q`
    
Do not print any output
`--verbose`, `-v`
    
Use verbose output.
You can configure fine-grained logging using the `RUST_LOG` environment variable. (<https://docs.rs/tracing-subscriber/latest/tracing_subscriber/filter/struct.EnvFilter.html#directives>)
`--version`, `-V`
    
Display the uv version
### uv tool dir
Show the path to the uv tools directory.
The tools directory is used to store environments and metadata for installed tools.
By default, tools are stored in the uv data directory at `$XDG_DATA_HOME/uv/tools` or `$HOME/.local/share/uv/tools` on Unix and `%APPDATA%\uv\data\tools` on Windows.
The tool installation directory may be overridden with `$UV_TOOL_DIR`.
To instead view the directory uv installs executables into, use the `--bin` flag.
### Usage
```
uv tool dir [OPTIONS]

```

### Options
`--allow-insecure-host` _allow-insecure-host_
    
Allow insecure connections to a host.
Can be provided multiple times.
Expects to receive either a hostname (e.g., `localhost`), a host-port pair (e.g., `localhost:8080`), or a URL (e.g., `https://localhost`).
WARNING: Hosts included in this list will not be verified against the system’s certificate store. Only use `--allow-insecure-host` in a secure network with verified sources, as it bypasses SSL verification and could expose you to MITM attacks.
May also be set with the `UV_INSECURE_HOST` environment variable.
`--bin`
    
Show the directory into which `uv tool` will install executables.
By default, `uv tool dir` shows the directory into which the tool Python environments themselves are installed, rather than the directory containing the linked executables.
The tool executable directory is determined according to the XDG standard and is derived from the following environment variables, in order of preference:
  * `$UV_TOOL_BIN_DIR`
  * `$XDG_BIN_HOME`
  * `$XDG_DATA_HOME/../bin`
  * `$HOME/.local/bin`


`--cache-dir` _cache-dir_
    
Path to the cache directory.
Defaults to `$XDG_CACHE_HOME/uv` or `$HOME/.cache/uv` on macOS and Linux, and `%LOCALAPPDATA%\uv\cache` on Windows.
To view the location of the cache directory, run `uv cache dir`.
May also be set with the `UV_CACHE_DIR` environment variable.
`--color` _color-choice_
    
Control the use of color in output.
By default, uv will automatically detect support for colors when writing to a terminal.
Possible values:
  * `auto`: Enables colored output only when the output is going to a terminal or TTY with support
  * `always`: Enables colored output regardless of the detected environment
  * `never`: Disables colored output


`--config-file` _config-file_
    
The path to a `uv.toml` file to use for configuration.
While uv configuration can be included in a `pyproject.toml` file, it is not allowed in this context.
May also be set with the `UV_CONFIG_FILE` environment variable.
`--directory` _directory_
    
Change to the given directory prior to running the command.
Relative paths are resolved with the given directory as the base.
See `--project` to only change the project root directory.
`--help`, `-h`
    
Display the concise help for this command
`--native-tls`
    
Whether to load TLS certificates from the platform’s native certificate store.
By default, uv loads certificates from the bundled `webpki-roots` crate. The `webpki-roots` are a reliable set of trust roots from Mozilla, and including them in uv improves portability and performance (especially on macOS).
However, in some cases, you may want to use the platform’s native certificate store, especially if you’re relying on a corporate trust root (e.g., for a mandatory proxy) that’s included in your system’s certificate store.
May also be set with the `UV_NATIVE_TLS` environment variable.
`--no-cache`, `-n`
    
Avoid reading from or writing to the cache, instead using a temporary directory for the duration of the operation
May also be set with the `UV_NO_CACHE` environment variable.
`--no-config`
    
Avoid discovering configuration files (`pyproject.toml`, `uv.toml`).
Normally, configuration files are discovered in the current directory, parent directories, or user configuration directories.
May also be set with the `UV_NO_CONFIG` environment variable.
`--no-progress`
    
Hide all progress outputs.
For example, spinners or progress bars.
May also be set with the `UV_NO_PROGRESS` environment variable.
`--no-python-downloads`
    
Disable automatic downloads of Python.
`--offline`
    
Disable network access.
When disabled, uv will only use locally cached data and locally available files.
May also be set with the `UV_OFFLINE` environment variable.
`--project` _project_
    
Run the command within the given project directory.
All `pyproject.toml`, `uv.toml`, and `.python-version` files will be discovered by walking up the directory tree from the project root, as will the project’s virtual environment (`.venv`).
Other command-line arguments (such as relative paths) will be resolved relative to the current working directory.
See `--directory` to change the working directory entirely.
This setting has no effect when used in the `uv pip` interface.
`--python-preference` _python-preference_
    
Whether to prefer uv-managed or system Python installations.
By default, uv prefers using Python versions it manages. However, it will use system Python installations if a uv-managed Python is not installed. This option allows prioritizing or ignoring system Python installations.
May also be set with the `UV_PYTHON_PREFERENCE` environment variable.
Possible values:
  * `only-managed`: Only use managed Python installations; never use system Python installations
  * `managed`: Prefer managed Python installations over system Python installations
  * `system`: Prefer system Python installations over managed Python installations
  * `only-system`: Only use system Python installations; never use managed Python installations


`--quiet`, `-q`
    
Do not print any output
`--verbose`, `-v`
    
Use verbose output.
You can configure fine-grained logging using the `RUST_LOG` environment variable. (<https://docs.rs/tracing-subscriber/latest/tracing_subscriber/filter/struct.EnvFilter.html#directives>)
`--version`, `-V`
    
Display the uv version
## uv python
Manage Python versions and installations
Generally, uv first searches for Python in a virtual environment, either active or in a `.venv` directory in the current working directory or any parent directory. If a virtual environment is not required, uv will then search for a Python interpreter. Python interpreters are found by searching for Python executables in the `PATH` environment variable.
On Windows, the registry is also searched for Python executables.
By default, uv will download Python if a version cannot be found. This behavior can be disabled with the `--no-python-downloads` flag or the `python-downloads` setting.
The `--python` option allows requesting a different interpreter.
The following Python version request formats are supported:
  * `<version>` e.g. `3`, `3.12`, `3.12.3`
  * `<version-specifier>` e.g. `>=3.12,<3.13`
  * `<implementation>` e.g. `cpython` or `cp`
  * `<implementation>@<version>` e.g. `cpython@3.12`
  * `<implementation><version>` e.g. `cpython3.12` or `cp312`
  * `<implementation><version-specifier>` e.g. `cpython>=3.12,<3.13`
  * `<implementation>-<version>-<os>-<arch>-<libc>` e.g. `cpython-3.12.3-macos-aarch64-none`


Additionally, a specific system Python interpreter can often be requested with:
  * `<executable-path>` e.g. `/opt/homebrew/bin/python3`
  * `<executable-name>` e.g. `mypython3`
  * `<install-dir>` e.g. `/some/environment/`


When the `--python` option is used, normal discovery rules apply but discovered interpreters are checked for compatibility with the request, e.g., if `pypy` is requested, uv will first check if the virtual environment contains a PyPy interpreter then check if each executable in the path is a PyPy interpreter.
uv supports discovering CPython, PyPy, and GraalPy interpreters. Unsupported interpreters will be skipped during discovery. If an unsupported interpreter implementation is requested, uv will exit with an error.
### Usage
```
uv python [OPTIONS] <COMMAND>

```

### Commands
`uv python list`
    
List the available Python installations
`uv python install`
    
Download and install Python versions
`uv python find`
    
Search for a Python installation
`uv python pin`
    
Pin to a specific Python version
`uv python dir`
    
Show the uv Python installation directory
`uv python uninstall`
    
Uninstall Python versions
### uv python list
List the available Python installations.
By default, installed Python versions and the downloads for latest available patch version of each supported Python major version are shown.
The displayed versions are filtered by the `--python-preference` option, i.e., if using `only-system`, no managed Python versions will be shown.
Use `--all-versions` to view all available patch versions.
Use `--only-installed` to omit available downloads.
### Usage
```
uv python list [OPTIONS]

```

### Options
`--all-arches`
    
List Python downloads for all architectures.
By default, only downloads for the current architecture are shown.
`--all-platforms`
    
List Python downloads for all platforms.
By default, only downloads for the current platform are shown.
`--all-versions`
    
List all Python versions, including old patch versions.
By default, only the latest patch version is shown for each minor version.
`--allow-insecure-host` _allow-insecure-host_
    
Allow insecure connections to a host.
Can be provided multiple times.
Expects to receive either a hostname (e.g., `localhost`), a host-port pair (e.g., `localhost:8080`), or a URL (e.g., `https://localhost`).
WARNING: Hosts included in this list will not be verified against the system’s certificate store. Only use `--allow-insecure-host` in a secure network with verified sources, as it bypasses SSL verification and could expose you to MITM attacks.
May also be set with the `UV_INSECURE_HOST` environment variable.
`--cache-dir` _cache-dir_
    
Path to the cache directory.
Defaults to `$XDG_CACHE_HOME/uv` or `$HOME/.cache/uv` on macOS and Linux, and `%LOCALAPPDATA%\uv\cache` on Windows.
To view the location of the cache directory, run `uv cache dir`.
May also be set with the `UV_CACHE_DIR` environment variable.
`--color` _color-choice_
    
Control the use of color in output.
By default, uv will automatically detect support for colors when writing to a terminal.
Possible values:
  * `auto`: Enables colored output only when the output is going to a terminal or TTY with support
  * `always`: Enables colored output regardless of the detected environment
  * `never`: Disables colored output


`--config-file` _config-file_
    
The path to a `uv.toml` file to use for configuration.
While uv configuration can be included in a `pyproject.toml` file, it is not allowed in this context.
May also be set with the `UV_CONFIG_FILE` environment variable.
`--directory` _directory_
    
Change to the given directory prior to running the command.
Relative paths are resolved with the given directory as the base.
See `--project` to only change the project root directory.
`--help`, `-h`
    
Display the concise help for this command
`--native-tls`
    
Whether to load TLS certificates from the platform’s native certificate store.
By default, uv loads certificates from the bundled `webpki-roots` crate. The `webpki-roots` are a reliable set of trust roots from Mozilla, and including them in uv improves portability and performance (especially on macOS).
However, in some cases, you may want to use the platform’s native certificate store, especially if you’re relying on a corporate trust root (e.g., for a mandatory proxy) that’s included in your system’s certificate store.
May also be set with the `UV_NATIVE_TLS` environment variable.
`--no-cache`, `-n`
    
Avoid reading from or writing to the cache, instead using a temporary directory for the duration of the operation
May also be set with the `UV_NO_CACHE` environment variable.
`--no-config`
    
Avoid discovering configuration files (`pyproject.toml`, `uv.toml`).
Normally, configuration files are discovered in the current directory, parent directories, or user configuration directories.
May also be set with the `UV_NO_CONFIG` environment variable.
`--no-progress`
    
Hide all progress outputs.
For example, spinners or progress bars.
May also be set with the `UV_NO_PROGRESS` environment variable.
`--no-python-downloads`
    
Disable automatic downloads of Python.
`--offline`
    
Disable network access.
When disabled, uv will only use locally cached data and locally available files.
May also be set with the `UV_OFFLINE` environment variable.
`--only-downloads`
    
Only show Python downloads, exclude installed distributions.
By default, available downloads for the current platform are shown.
`--only-installed`
    
Only show installed Python versions, exclude available downloads.
By default, available downloads for the current platform are shown.
`--output-format` _output-format_
    
Select the output format
[default: text]
Possible values:
  * `text`: Plain text (for humans)
  * `json`: JSON (for computers)


`--project` _project_
    
Run the command within the given project directory.
All `pyproject.toml`, `uv.toml`, and `.python-version` files will be discovered by walking up the directory tree from the project root, as will the project’s virtual environment (`.venv`).
Other command-line arguments (such as relative paths) will be resolved relative to the current working directory.
See `--directory` to change the working directory entirely.
This setting has no effect when used in the `uv pip` interface.
`--python-preference` _python-preference_
    
Whether to prefer uv-managed or system Python installations.
By default, uv prefers using Python versions it manages. However, it will use system Python installations if a uv-managed Python is not installed. This option allows prioritizing or ignoring system Python installations.
May also be set with the `UV_PYTHON_PREFERENCE` environment variable.
Possible values:
  * `only-managed`: Only use managed Python installations; never use system Python installations
  * `managed`: Prefer managed Python installations over system Python installations
  * `system`: Prefer system Python installations over managed Python installations
  * `only-system`: Only use system Python installations; never use managed Python installations


`--quiet`, `-q`
    
Do not print any output
`--show-urls`
    
Show the URLs of available Python downloads.
By default, these display as `<download available>`.
`--verbose`, `-v`
    
Use verbose output.
You can configure fine-grained logging using the `RUST_LOG` environment variable. (<https://docs.rs/tracing-subscriber/latest/tracing_subscriber/filter/struct.EnvFilter.html#directives>)
`--version`, `-V`
    
Display the uv version
### uv python install
Download and install Python versions.
Supports CPython and PyPy. CPython distributions are downloaded from the Astral `python-build-standalone` project. PyPy distributions are downloaded from `python.org`. The available Python versions are bundled with each uv release. To install new Python versions, you may need upgrade uv.
Python versions are installed into the uv Python directory, which can be retrieved with `uv python dir`.
A `python` executable is not made globally available, managed Python versions are only used in uv commands or in active virtual environments. There is experimental support for adding Python executables to the `PATH` — use the `--preview` flag to enable this behavior.
Multiple Python versions may be requested.
See `uv help python` to view supported request formats.
### Usage
```
uv python install [OPTIONS] [TARGETS]...

```

### Arguments
`TARGETS`
    
The Python version(s) to install.
If not provided, the requested Python version(s) will be read from the `UV_PYTHON` environment variable then `.python-versions` or `.python-version` files. If none of the above are present, uv will check if it has installed any Python versions. If not, it will install the latest stable version of Python.
See uv python to view supported request formats.
### Options
`--allow-insecure-host` _allow-insecure-host_
    
Allow insecure connections to a host.
Can be provided multiple times.
Expects to receive either a hostname (e.g., `localhost`), a host-port pair (e.g., `localhost:8080`), or a URL (e.g., `https://localhost`).
WARNING: Hosts included in this list will not be verified against the system’s certificate store. Only use `--allow-insecure-host` in a secure network with verified sources, as it bypasses SSL verification and could expose you to MITM attacks.
May also be set with the `UV_INSECURE_HOST` environment variable.
`--cache-dir` _cache-dir_
    
Path to the cache directory.
Defaults to `$XDG_CACHE_HOME/uv` or `$HOME/.cache/uv` on macOS and Linux, and `%LOCALAPPDATA%\uv\cache` on Windows.
To view the location of the cache directory, run `uv cache dir`.
May also be set with the `UV_CACHE_DIR` environment variable.
`--color` _color-choice_
    
Control the use of color in output.
By default, uv will automatically detect support for colors when writing to a terminal.
Possible values:
  * `auto`: Enables colored output only when the output is going to a terminal or TTY with support
  * `always`: Enables colored output regardless of the detected environment
  * `never`: Disables colored output


`--config-file` _config-file_
    
The path to a `uv.toml` file to use for configuration.
While uv configuration can be included in a `pyproject.toml` file, it is not allowed in this context.
May also be set with the `UV_CONFIG_FILE` environment variable.
`--default`
    
Use as the default Python version.
By default, only a `python{major}.{minor}` executable is installed, e.g., `python3.10`. When the `--default` flag is used, `python{major}`, e.g., `python3`, and `python` executables are also installed.
Alternative Python variants will still include their tag. For example, installing 3.13+freethreaded with `--default` will include in `python3t` and `pythont`, not `python3` and `python`.
If multiple Python versions are requested, uv will exit with an error.
`--directory` _directory_
    
Change to the given directory prior to running the command.
Relative paths are resolved with the given directory as the base.
See `--project` to only change the project root directory.
`--force`, `-f`
    
Replace existing Python executables during installation.
By default, uv will refuse to replace executables that it does not manage.
Implies `--reinstall`.
`--help`, `-h`
    
Display the concise help for this command
`--install-dir`, `-i` _install-dir_
    
The directory to store the Python installation in.
If provided, `UV_PYTHON_INSTALL_DIR` will need to be set for subsequent operations for uv to discover the Python installation.
See `uv python dir` to view the current Python installation directory. Defaults to `~/.local/share/uv/python`.
May also be set with the `UV_PYTHON_INSTALL_DIR` environment variable.
`--mirror` _mirror_
    
Set the URL to use as the source for downloading Python installations.
The provided URL will replace `https://github.com/astral-sh/python-build-standalone/releases/download` in, e.g., `https://github.com/astral-sh/python-build-standalone/releases/download/20240713/cpython-3.12.4%2B20240713-aarch64-apple-darwin-install_only.tar.gz`.
Distributions can be read from a local directory by using the `file://` URL scheme.
May also be set with the `UV_PYTHON_INSTALL_MIRROR` environment variable.
`--native-tls`
    
Whether to load TLS certificates from the platform’s native certificate store.
By default, uv loads certificates from the bundled `webpki-roots` crate. The `webpki-roots` are a reliable set of trust roots from Mozilla, and including them in uv improves portability and performance (especially on macOS).
However, in some cases, you may want to use the platform’s native certificate store, especially if you’re relying on a corporate trust root (e.g., for a mandatory proxy) that’s included in your system’s certificate store.
May also be set with the `UV_NATIVE_TLS` environment variable.
`--no-cache`, `-n`
    
Avoid reading from or writing to the cache, instead using a temporary directory for the duration of the operation
May also be set with the `UV_NO_CACHE` environment variable.
`--no-config`
    
Avoid discovering configuration files (`pyproject.toml`, `uv.toml`).
Normally, configuration files are discovered in the current directory, parent directories, or user configuration directories.
May also be set with the `UV_NO_CONFIG` environment variable.
`--no-progress`
    
Hide all progress outputs.
For example, spinners or progress bars.
May also be set with the `UV_NO_PROGRESS` environment variable.
`--no-python-downloads`
    
Disable automatic downloads of Python.
`--offline`
    
Disable network access.
When disabled, uv will only use locally cached data and locally available files.
May also be set with the `UV_OFFLINE` environment variable.
`--project` _project_
    
Run the command within the given project directory.
All `pyproject.toml`, `uv.toml`, and `.python-version` files will be discovered by walking up the directory tree from the project root, as will the project’s virtual environment (`.venv`).
Other command-line arguments (such as relative paths) will be resolved relative to the current working directory.
See `--directory` to change the working directory entirely.
This setting has no effect when used in the `uv pip` interface.
`--pypy-mirror` _pypy-mirror_
    
Set the URL to use as the source for downloading PyPy installations.
The provided URL will replace `https://downloads.python.org/pypy` in, e.g., `https://downloads.python.org/pypy/pypy3.8-v7.3.7-osx64.tar.bz2`.
Distributions can be read from a local directory by using the `file://` URL scheme.
May also be set with the `UV_PYPY_INSTALL_MIRROR` environment variable.
`--python-preference` _python-preference_
    
Whether to prefer uv-managed or system Python installations.
By default, uv prefers using Python versions it manages. However, it will use system Python installations if a uv-managed Python is not installed. This option allows prioritizing or ignoring system Python installations.
May also be set with the `UV_PYTHON_PREFERENCE` environment variable.
Possible values:
  * `only-managed`: Only use managed Python installations; never use system Python installations
  * `managed`: Prefer managed Python installations over system Python installations
  * `system`: Prefer system Python installations over managed Python installations
  * `only-system`: Only use system Python installations; never use managed Python installations


`--quiet`, `-q`
    
Do not print any output
`--reinstall`, `-r`
    
Reinstall the requested Python version, if it’s already installed.
By default, uv will exit successfully if the version is already installed.
`--verbose`, `-v`
    
Use verbose output.
You can configure fine-grained logging using the `RUST_LOG` environment variable. (<https://docs.rs/tracing-subscriber/latest/tracing_subscriber/filter/struct.EnvFilter.html#directives>)
`--version`, `-V`
    
Display the uv version
### uv python find
Search for a Python installation.
Displays the path to the Python executable.
See `uv help python` to view supported request formats and details on discovery behavior.
### Usage
```
uv python find [OPTIONS] [REQUEST]

```

### Arguments
`REQUEST`
    
The Python request.
See uv python to view supported request formats.
### Options
`--allow-insecure-host` _allow-insecure-host_
    
Allow insecure connections to a host.
Can be provided multiple times.
Expects to receive either a hostname (e.g., `localhost`), a host-port pair (e.g., `localhost:8080`), or a URL (e.g., `https://localhost`).
WARNING: Hosts included in this list will not be verified against the system’s certificate store. Only use `--allow-insecure-host` in a secure network with verified sources, as it bypasses SSL verification and could expose you to MITM attacks.
May also be set with the `UV_INSECURE_HOST` environment variable.
`--cache-dir` _cache-dir_
    
Path to the cache directory.
Defaults to `$XDG_CACHE_HOME/uv` or `$HOME/.cache/uv` on macOS and Linux, and `%LOCALAPPDATA%\uv\cache` on Windows.
To view the location of the cache directory, run `uv cache dir`.
May also be set with the `UV_CACHE_DIR` environment variable.
`--color` _color-choice_
    
Control the use of color in output.
By default, uv will automatically detect support for colors when writing to a terminal.
Possible values:
  * `auto`: Enables colored output only when the output is going to a terminal or TTY with support
  * `always`: Enables colored output regardless of the detected environment
  * `never`: Disables colored output


`--config-file` _config-file_
    
The path to a `uv.toml` file to use for configuration.
While uv configuration can be included in a `pyproject.toml` file, it is not allowed in this context.
May also be set with the `UV_CONFIG_FILE` environment variable.
`--directory` _directory_
    
Change to the given directory prior to running the command.
Relative paths are resolved with the given directory as the base.
See `--project` to only change the project root directory.
`--help`, `-h`
    
Display the concise help for this command
`--native-tls`
    
Whether to load TLS certificates from the platform’s native certificate store.
By default, uv loads certificates from the bundled `webpki-roots` crate. The `webpki-roots` are a reliable set of trust roots from Mozilla, and including them in uv improves portability and performance (especially on macOS).
However, in some cases, you may want to use the platform’s native certificate store, especially if you’re relying on a corporate trust root (e.g., for a mandatory proxy) that’s included in your system’s certificate store.
May also be set with the `UV_NATIVE_TLS` environment variable.
`--no-cache`, `-n`
    
Avoid reading from or writing to the cache, instead using a temporary directory for the duration of the operation
May also be set with the `UV_NO_CACHE` environment variable.
`--no-config`
    
Avoid discovering configuration files (`pyproject.toml`, `uv.toml`).
Normally, configuration files are discovered in the current directory, parent directories, or user configuration directories.
May also be set with the `UV_NO_CONFIG` environment variable.
`--no-progress`
    
Hide all progress outputs.
For example, spinners or progress bars.
May also be set with the `UV_NO_PROGRESS` environment variable.
`--no-project`
    
Avoid discovering a project or workspace.
Otherwise, when no request is provided, the Python requirement of a project in the current directory or parent directories will be used.
`--no-python-downloads`
    
Disable automatic downloads of Python.
`--offline`
    
Disable network access.
When disabled, uv will only use locally cached data and locally available files.
May also be set with the `UV_OFFLINE` environment variable.
`--project` _project_
    
Run the command within the given project directory.
All `pyproject.toml`, `uv.toml`, and `.python-version` files will be discovered by walking up the directory tree from the project root, as will the project’s virtual environment (`.venv`).
Other command-line arguments (such as relative paths) will be resolved relative to the current working directory.
See `--directory` to change the working directory entirely.
This setting has no effect when used in the `uv pip` interface.
`--python-preference` _python-preference_
    
Whether to prefer uv-managed or system Python installations.
By default, uv prefers using Python versions it manages. However, it will use system Python installations if a uv-managed Python is not installed. This option allows prioritizing or ignoring system Python installations.
May also be set with the `UV_PYTHON_PREFERENCE` environment variable.
Possible values:
  * `only-managed`: Only use managed Python installations; never use system Python installations
  * `managed`: Prefer managed Python installations over system Python installations
  * `system`: Prefer system Python installations over managed Python installations
  * `only-system`: Only use system Python installations; never use managed Python installations


`--quiet`, `-q`
    
Do not print any output
`--system`
    
Only find system Python interpreters.
By default, uv will report the first Python interpreter it would use, including those in an active virtual environment or a virtual environment in the current working directory or any parent directory.
The `--system` option instructs uv to skip virtual environment Python interpreters and restrict its search to the system path.
May also be set with the `UV_SYSTEM_PYTHON` environment variable.
`--verbose`, `-v`
    
Use verbose output.
You can configure fine-grained logging using the `RUST_LOG` environment variable. (<https://docs.rs/tracing-subscriber/latest/tracing_subscriber/filter/struct.EnvFilter.html#directives>)
`--version`, `-V`
    
Display the uv version
### uv python pin
Pin to a specific Python version.
Writes the pinned Python version to a `.python-version` file, which is used by other uv commands to determine the required Python version.
If no version is provided, uv will look for an existing `.python-version` file and display the currently pinned version. If no `.python-version` file is found, uv will exit with an error.
See `uv help python` to view supported request formats.
### Usage
```
uv python pin [OPTIONS] [REQUEST]

```

### Arguments
`REQUEST`
    
The Python version request.
uv supports more formats than other tools that read `.python-version` files, i.e., `pyenv`. If compatibility with those tools is needed, only use version numbers instead of complex requests such as `cpython@3.10`.
If no request is provided, the currently pinned version will be shown.
See uv python to view supported request formats.
### Options
`--allow-insecure-host` _allow-insecure-host_
    
Allow insecure connections to a host.
Can be provided multiple times.
Expects to receive either a hostname (e.g., `localhost`), a host-port pair (e.g., `localhost:8080`), or a URL (e.g., `https://localhost`).
WARNING: Hosts included in this list will not be verified against the system’s certificate store. Only use `--allow-insecure-host` in a secure network with verified sources, as it bypasses SSL verification and could expose you to MITM attacks.
May also be set with the `UV_INSECURE_HOST` environment variable.
`--cache-dir` _cache-dir_
    
Path to the cache directory.
Defaults to `$XDG_CACHE_HOME/uv` or `$HOME/.cache/uv` on macOS and Linux, and `%LOCALAPPDATA%\uv\cache` on Windows.
To view the location of the cache directory, run `uv cache dir`.
May also be set with the `UV_CACHE_DIR` environment variable.
`--color` _color-choice_
    
Control the use of color in output.
By default, uv will automatically detect support for colors when writing to a terminal.
Possible values:
  * `auto`: Enables colored output only when the output is going to a terminal or TTY with support
  * `always`: Enables colored output regardless of the detected environment
  * `never`: Disables colored output


`--config-file` _config-file_
    
The path to a `uv.toml` file to use for configuration.
While uv configuration can be included in a `pyproject.toml` file, it is not allowed in this context.
May also be set with the `UV_CONFIG_FILE` environment variable.
`--directory` _directory_
    
Change to the given directory prior to running the command.
Relative paths are resolved with the given directory as the base.
See `--project` to only change the project root directory.
`--help`, `-h`
    
Display the concise help for this command
`--native-tls`
    
Whether to load TLS certificates from the platform’s native certificate store.
By default, uv loads certificates from the bundled `webpki-roots` crate. The `webpki-roots` are a reliable set of trust roots from Mozilla, and including them in uv improves portability and performance (especially on macOS).
However, in some cases, you may want to use the platform’s native certificate store, especially if you’re relying on a corporate trust root (e.g., for a mandatory proxy) that’s included in your system’s certificate store.
May also be set with the `UV_NATIVE_TLS` environment variable.
`--no-cache`, `-n`
    
Avoid reading from or writing to the cache, instead using a temporary directory for the duration of the operation
May also be set with the `UV_NO_CACHE` environment variable.
`--no-config`
    
Avoid discovering configuration files (`pyproject.toml`, `uv.toml`).
Normally, configuration files are discovered in the current directory, parent directories, or user configuration directories.
May also be set with the `UV_NO_CONFIG` environment variable.
`--no-progress`
    
Hide all progress outputs.
For example, spinners or progress bars.
May also be set with the `UV_NO_PROGRESS` environment variable.
`--no-project`
    
Avoid validating the Python pin is compatible with the project or workspace.
By default, a project or workspace is discovered in the current directory or any parent directory. If a workspace is found, the Python pin is validated against the workspace’s `requires-python` constraint.
`--no-python-downloads`
    
Disable automatic downloads of Python.
`--offline`
    
Disable network access.
When disabled, uv will only use locally cached data and locally available files.
May also be set with the `UV_OFFLINE` environment variable.
`--project` _project_
    
Run the command within the given project directory.
All `pyproject.toml`, `uv.toml`, and `.python-version` files will be discovered by walking up the directory tree from the project root, as will the project’s virtual environment (`.venv`).
Other command-line arguments (such as relative paths) will be resolved relative to the current working directory.
See `--directory` to change the working directory entirely.
This setting has no effect when used in the `uv pip` interface.
`--python-preference` _python-preference_
    
Whether to prefer uv-managed or system Python installations.
By default, uv prefers using Python versions it manages. However, it will use system Python installations if a uv-managed Python is not installed. This option allows prioritizing or ignoring system Python installations.
May also be set with the `UV_PYTHON_PREFERENCE` environment variable.
Possible values:
  * `only-managed`: Only use managed Python installations; never use system Python installations
  * `managed`: Prefer managed Python installations over system Python installations
  * `system`: Prefer system Python installations over managed Python installations
  * `only-system`: Only use system Python installations; never use managed Python installations


`--quiet`, `-q`
    
Do not print any output
`--resolved`
    
Write the resolved Python interpreter path instead of the request.
Ensures that the exact same interpreter is used.
This option is usually not safe to use when committing the `.python-version` file to version control.
`--verbose`, `-v`
    
Use verbose output.
You can configure fine-grained logging using the `RUST_LOG` environment variable. (<https://docs.rs/tracing-subscriber/latest/tracing_subscriber/filter/struct.EnvFilter.html#directives>)
`--version`, `-V`
    
Display the uv version
### uv python dir
Show the uv Python installation directory.
By default, Python installations are stored in the uv data directory at `$XDG_DATA_HOME/uv/python` or `$HOME/.local/share/uv/python` on Unix and `%APPDATA%\uv\data\python` on Windows.
The Python installation directory may be overridden with `$UV_PYTHON_INSTALL_DIR`.
To view the directory where uv installs Python executables instead, use the `--bin` flag. Note that Python executables are only installed when preview mode is enabled.
### Usage
```
uv python dir [OPTIONS]

```

### Options
`--allow-insecure-host` _allow-insecure-host_
    
Allow insecure connections to a host.
Can be provided multiple times.
Expects to receive either a hostname (e.g., `localhost`), a host-port pair (e.g., `localhost:8080`), or a URL (e.g., `https://localhost`).
WARNING: Hosts included in this list will not be verified against the system’s certificate store. Only use `--allow-insecure-host` in a secure network with verified sources, as it bypasses SSL verification and could expose you to MITM attacks.
May also be set with the `UV_INSECURE_HOST` environment variable.
`--bin`
    
Show the directory into which `uv python` will install Python executables.
Note that this directory is only used when installing Python with preview mode enabled.
The Python executable directory is determined according to the XDG standard and is derived from the following environment variables, in order of preference:
  * `$UV_PYTHON_BIN_DIR`
  * `$XDG_BIN_HOME`
  * `$XDG_DATA_HOME/../bin`
  * `$HOME/.local/bin`


`--cache-dir` _cache-dir_
    
Path to the cache directory.
Defaults to `$XDG_CACHE_HOME/uv` or `$HOME/.cache/uv` on macOS and Linux, and `%LOCALAPPDATA%\uv\cache` on Windows.
To view the location of the cache directory, run `uv cache dir`.
May also be set with the `UV_CACHE_DIR` environment variable.
`--color` _color-choice_
    
Control the use of color in output.
By default, uv will automatically detect support for colors when writing to a terminal.
Possible values:
  * `auto`: Enables colored output only when the output is going to a terminal or TTY with support
  * `always`: Enables colored output regardless of the detected environment
  * `never`: Disables colored output


`--config-file` _config-file_
    
The path to a `uv.toml` file to use for configuration.
While uv configuration can be included in a `pyproject.toml` file, it is not allowed in this context.
May also be set with the `UV_CONFIG_FILE` environment variable.
`--directory` _directory_
    
Change to the given directory prior to running the command.
Relative paths are resolved with the given directory as the base.
See `--project` to only change the project root directory.
`--help`, `-h`
    
Display the concise help for this command
`--native-tls`
    
Whether to load TLS certificates from the platform’s native certificate store.
By default, uv loads certificates from the bundled `webpki-roots` crate. The `webpki-roots` are a reliable set of trust roots from Mozilla, and including them in uv improves portability and performance (especially on macOS).
However, in some cases, you may want to use the platform’s native certificate store, especially if you’re relying on a corporate trust root (e.g., for a mandatory proxy) that’s included in your system’s certificate store.
May also be set with the `UV_NATIVE_TLS` environment variable.
`--no-cache`, `-n`
    
Avoid reading from or writing to the cache, instead using a temporary directory for the duration of the operation
May also be set with the `UV_NO_CACHE` environment variable.
`--no-config`
    
Avoid discovering configuration files (`pyproject.toml`, `uv.toml`).
Normally, configuration files are discovered in the current directory, parent directories, or user configuration directories.
May also be set with the `UV_NO_CONFIG` environment variable.
`--no-progress`
    
Hide all progress outputs.
For example, spinners or progress bars.
May also be set with the `UV_NO_PROGRESS` environment variable.
`--no-python-downloads`
    
Disable automatic downloads of Python.
`--offline`
    
Disable network access.
When disabled, uv will only use locally cached data and locally available files.
May also be set with the `UV_OFFLINE` environment variable.
`--project` _project_
    
Run the command within the given project directory.
All `pyproject.toml`, `uv.toml`, and `.python-version` files will be discovered by walking up the directory tree from the project root, as will the project’s virtual environment (`.venv`).
Other command-line arguments (such as relative paths) will be resolved relative to the current working directory.
See `--directory` to change the working directory entirely.
This setting has no effect when used in the `uv pip` interface.
`--python-preference` _python-preference_
    
Whether to prefer uv-managed or system Python installations.
By default, uv prefers using Python versions it manages. However, it will use system Python installations if a uv-managed Python is not installed. This option allows prioritizing or ignoring system Python installations.
May also be set with the `UV_PYTHON_PREFERENCE` environment variable.
Possible values:
  * `only-managed`: Only use managed Python installations; never use system Python installations
  * `managed`: Prefer managed Python installations over system Python installations
  * `system`: Prefer system Python installations over managed Python installations
  * `only-system`: Only use system Python installations; never use managed Python installations


`--quiet`, `-q`
    
Do not print any output
`--verbose`, `-v`
    
Use verbose output.
You can configure fine-grained logging using the `RUST_LOG` environment variable. (<https://docs.rs/tracing-subscriber/latest/tracing_subscriber/filter/struct.EnvFilter.html#directives>)
`--version`, `-V`
    
Display the uv version
### uv python uninstall
Uninstall Python versions
### Usage
```
uv python uninstall [OPTIONS] <TARGETS>...

```

### Arguments
`TARGETS`
    
The Python version(s) to uninstall.
See uv python to view supported request formats.
### Options
`--all`
    
Uninstall all managed Python versions
`--allow-insecure-host` _allow-insecure-host_
    
Allow insecure connections to a host.
Can be provided multiple times.
Expects to receive either a hostname (e.g., `localhost`), a host-port pair (e.g., `localhost:8080`), or a URL (e.g., `https://localhost`).
WARNING: Hosts included in this list will not be verified against the system’s certificate store. Only use `--allow-insecure-host` in a secure network with verified sources, as it bypasses SSL verification and could expose you to MITM attacks.
May also be set with the `UV_INSECURE_HOST` environment variable.
`--cache-dir` _cache-dir_
    
Path to the cache directory.
Defaults to `$XDG_CACHE_HOME/uv` or `$HOME/.cache/uv` on macOS and Linux, and `%LOCALAPPDATA%\uv\cache` on Windows.
To view the location of the cache directory, run `uv cache dir`.
May also be set with the `UV_CACHE_DIR` environment variable.
`--color` _color-choice_
    
Control the use of color in output.
By default, uv will automatically detect support for colors when writing to a terminal.
Possible values:
  * `auto`: Enables colored output only when the output is going to a terminal or TTY with support
  * `always`: Enables colored output regardless of the detected environment
  * `never`: Disables colored output


`--config-file` _config-file_
    
The path to a `uv.toml` file to use for configuration.
While uv configuration can be included in a `pyproject.toml` file, it is not allowed in this context.
May also be set with the `UV_CONFIG_FILE` environment variable.
`--directory` _directory_
    
Change to the given directory prior to running the command.
Relative paths are resolved with the given directory as the base.
See `--project` to only change the project root directory.
`--help`, `-h`
    
Display the concise help for this command
`--install-dir`, `-i` _install-dir_
    
The directory where the Python was installed
May also be set with the `UV_PYTHON_INSTALL_DIR` environment variable.
`--native-tls`
    
Whether to load TLS certificates from the platform’s native certificate store.
By default, uv loads certificates from the bundled `webpki-roots` crate. The `webpki-roots` are a reliable set of trust roots from Mozilla, and including them in uv improves portability and performance (especially on macOS).
However, in some cases, you may want to use the platform’s native certificate store, especially if you’re relying on a corporate trust root (e.g., for a mandatory proxy) that’s included in your system’s certificate store.
May also be set with the `UV_NATIVE_TLS` environment variable.
`--no-cache`, `-n`
    
Avoid reading from or writing to the cache, instead using a temporary directory for the duration of the operation
May also be set with the `UV_NO_CACHE` environment variable.
`--no-config`
    
Avoid discovering configuration files (`pyproject.toml`, `uv.toml`).
Normally, configuration files are discovered in the current directory, parent directories, or user configuration directories.
May also be set with the `UV_NO_CONFIG` environment variable.
`--no-progress`
    
Hide all progress outputs.
For example, spinners or progress bars.
May also be set with the `UV_NO_PROGRESS` environment variable.
`--no-python-downloads`
    
Disable automatic downloads of Python.
`--offline`
    
Disable network access.
When disabled, uv will only use locally cached data and locally available files.
May also be set with the `UV_OFFLINE` environment variable.
`--project` _project_
    
Run the command within the given project directory.
All `pyproject.toml`, `uv.toml`, and `.python-version` files will be discovered by walking up the directory tree from the project root, as will the project’s virtual environment (`.venv`).
Other command-line arguments (such as relative paths) will be resolved relative to the current working directory.
See `--directory` to change the working directory entirely.
This setting has no effect when used in the `uv pip` interface.
`--python-preference` _python-preference_
    
Whether to prefer uv-managed or system Python installations.
By default, uv prefers using Python versions it manages. However, it will use system Python installations if a uv-managed Python is not installed. This option allows prioritizing or ignoring system Python installations.
May also be set with the `UV_PYTHON_PREFERENCE` environment variable.
Possible values:
  * `only-managed`: Only use managed Python installations; never use system Python installations
  * `managed`: Prefer managed Python installations over system Python installations
  * `system`: Prefer system Python installations over managed Python installations
  * `only-system`: Only use system Python installations; never use managed Python installations


`--quiet`, `-q`
    
Do not print any output
`--verbose`, `-v`
    
Use verbose output.
You can configure fine-grained logging using the `RUST_LOG` environment variable. (<https://docs.rs/tracing-subscriber/latest/tracing_subscriber/filter/struct.EnvFilter.html#directives>)
`--version`, `-V`
    
Display the uv version
## uv pip
Manage Python packages with a pip-compatible interface
### Usage
```
uv pip [OPTIONS] <COMMAND>

```

### Commands
`uv pip compile`
    
Compile a `requirements.in` file to a `requirements.txt` file
`uv pip sync`
    
Sync an environment with a `requirements.txt` file
`uv pip install`
    
Install packages into an environment
`uv pip uninstall`
    
Uninstall packages from an environment
`uv pip freeze`
    
List, in requirements format, packages installed in an environment
`uv pip list`
    
List, in tabular format, packages installed in an environment
`uv pip show`
    
Show information about one or more installed packages
`uv pip tree`
    
Display the dependency tree for an environment
`uv pip check`
    
Verify installed packages have compatible dependencies
### uv pip compile
Compile a `requirements.in` file to a `requirements.txt` file
### Usage
```
uv pip compile [OPTIONS] <SRC_FILE>...

```

### Arguments
`SRC_FILE`
    
Include all packages listed in the given `requirements.in` files.
If a `pyproject.toml`, `setup.py`, or `setup.cfg` file is provided, uv will extract the requirements for the relevant project.
If `-` is provided, then requirements will be read from stdin.
The order of the requirements files and the requirements in them is used to determine priority during resolution.
### Options
`--all-extras`
    
Include all optional dependencies.
Only applies to `pyproject.toml`, `setup.py`, and `setup.cfg` sources.
`--allow-insecure-host` _allow-insecure-host_
    
Allow insecure connections to a host.
Can be provided multiple times.
Expects to receive either a hostname (e.g., `localhost`), a host-port pair (e.g., `localhost:8080`), or a URL (e.g., `https://localhost`).
WARNING: Hosts included in this list will not be verified against the system’s certificate store. Only use `--allow-insecure-host` in a secure network with verified sources, as it bypasses SSL verification and could expose you to MITM attacks.
May also be set with the `UV_INSECURE_HOST` environment variable.
`--annotation-style` _annotation-style_
    
The style of the annotation comments included in the output file, used to indicate the source of each package.
Defaults to `split`.
Possible values:
  * `line`: Render the annotations on a single, comma-separated line
  * `split`: Render each annotation on its own line


`--build-constraints`, `-b` _build-constraints_
    
Constrain build dependencies using the given requirements files when building source distributions.
Constraints files are `requirements.txt`-like files that only control the _version_ of a requirement that’s installed. However, including a package in a constraints file will _not_ trigger the installation of that package.
May also be set with the `UV_BUILD_CONSTRAINT` environment variable.
`--cache-dir` _cache-dir_
    
Path to the cache directory.
Defaults to `$XDG_CACHE_HOME/uv` or `$HOME/.cache/uv` on macOS and Linux, and `%LOCALAPPDATA%\uv\cache` on Windows.
To view the location of the cache directory, run `uv cache dir`.
May also be set with the `UV_CACHE_DIR` environment variable.
`--color` _color-choice_
    
Control the use of color in output.
By default, uv will automatically detect support for colors when writing to a terminal.
Possible values:
  * `auto`: Enables colored output only when the output is going to a terminal or TTY with support
  * `always`: Enables colored output regardless of the detected environment
  * `never`: Disables colored output


`--config-file` _config-file_
    
The path to a `uv.toml` file to use for configuration.
While uv configuration can be included in a `pyproject.toml` file, it is not allowed in this context.
May also be set with the `UV_CONFIG_FILE` environment variable.
`--config-setting`, `-C` _config-setting_
    
Settings to pass to the PEP 517 build backend, specified as `KEY=VALUE` pairs
`--constraints`, `-c` _constraints_
    
Constrain versions using the given requirements files.
Constraints files are `requirements.txt`-like files that only control the _version_ of a requirement that’s installed. However, including a package in a constraints file will _not_ trigger the installation of that package.
This is equivalent to pip’s `--constraint` option.
May also be set with the `UV_CONSTRAINT` environment variable.
`--custom-compile-command` _custom-compile-command_
    
The header comment to include at the top of the output file generated by `uv pip compile`.
Used to reflect custom build scripts and commands that wrap `uv pip compile`.
May also be set with the `UV_CUSTOM_COMPILE_COMMAND` environment variable.
`--default-index` _default-index_
    
The URL of the default package index (by default: <https://pypi.org/simple>).
Accepts either a repository compliant with PEP 503 (the simple repository API), or a local directory laid out in the same format.
The index given by this flag is given lower priority than all other indexes specified via the `--index` flag.
May also be set with the `UV_DEFAULT_INDEX` environment variable.
`--directory` _directory_
    
Change to the given directory prior to running the command.
Relative paths are resolved with the given directory as the base.
See `--project` to only change the project root directory.
`--emit-build-options`
    
Include `--no-binary` and `--only-binary` entries in the generated output file
`--emit-find-links`
    
Include `--find-links` entries in the generated output file
`--emit-index-annotation`
    
Include comment annotations indicating the index used to resolve each package (e.g., `# from https://pypi.org/simple`)
`--emit-index-url`
    
Include `--index-url` and `--extra-index-url` entries in the generated output file
`--exclude-newer` _exclude-newer_
    
Limit candidate packages to those that were uploaded prior to the given date.
Accepts both RFC 3339 timestamps (e.g., `2006-12-02T02:07:43Z`) and local dates in the same format (e.g., `2006-12-02`) in your system’s configured time zone.
May also be set with the `UV_EXCLUDE_NEWER` environment variable.
`--extra` _extra_
    
Include optional dependencies from the specified extra name; may be provided more than once.
Only applies to `pyproject.toml`, `setup.py`, and `setup.cfg` sources.
`--extra-index-url` _extra-index-url_
    
(Deprecated: use `--index` instead) Extra URLs of package indexes to use, in addition to `--index-url`.
Accepts either a repository compliant with PEP 503 (the simple repository API), or a local directory laid out in the same format.
All indexes provided via this flag take priority over the index specified by `--index-url` (which defaults to PyPI). When multiple `--extra-index-url` flags are provided, earlier values take priority.
May also be set with the `UV_EXTRA_INDEX_URL` environment variable.
`--find-links`, `-f` _find-links_
    
Locations to search for candidate distributions, in addition to those found in the registry indexes.
If a path, the target must be a directory that contains packages as wheel files (`.whl`) or source distributions (e.g., `.tar.gz` or `.zip`) at the top level.
If a URL, the page must contain a flat list of links to package files adhering to the formats described above.
May also be set with the `UV_FIND_LINKS` environment variable.
`--fork-strategy` _fork-strategy_
    
The strategy to use when selecting multiple versions of a given package across Python versions and platforms.
By default, uv will optimize for selecting the latest version of each package for each supported Python version (`requires-python`), while minimizing the number of selected versions across platforms.
Under `fewest`, uv will minimize the number of selected versions for each package, preferring older versions that are compatible with a wider range of supported Python versions or platforms.
May also be set with the `UV_FORK_STRATEGY` environment variable.
Possible values:
  * `fewest`: Optimize for selecting the fewest number of versions for each package. Older versions may be preferred if they are compatible with a wider range of supported Python versions or platforms
  * `requires-python`: Optimize for selecting latest supported version of each package, for each supported Python version


`--generate-hashes`
    
Include distribution hashes in the output file
`--help`, `-h`
    
Display the concise help for this command
`--index` _index_
    
The URLs to use when resolving dependencies, in addition to the default index.
Accepts either a repository compliant with PEP 503 (the simple repository API), or a local directory laid out in the same format.
All indexes provided via this flag take priority over the index specified by `--default-index` (which defaults to PyPI). When multiple `--index` flags are provided, earlier values take priority.
May also be set with the `UV_INDEX` environment variable.
`--index-strategy` _index-strategy_
    
The strategy to use when resolving against multiple index URLs.
By default, uv will stop at the first index on which a given package is available, and limit resolutions to those present on that first index (`first-index`). This prevents "dependency confusion" attacks, whereby an attacker can upload a malicious package under the same name to an alternate index.
May also be set with the `UV_INDEX_STRATEGY` environment variable.
Possible values:
  * `first-index`: Only use results from the first index that returns a match for a given package name
  * `unsafe-first-match`: Search for every package name across all indexes, exhausting the versions from the first index before moving on to the next
  * `unsafe-best-match`: Search for every package name across all indexes, preferring the "best" version found. If a package version is in multiple indexes, only look at the entry for the first index


`--index-url`, `-i` _index-url_
    
(Deprecated: use `--default-index` instead) The URL of the Python package index (by default: <https://pypi.org/simple>).
Accepts either a repository compliant with PEP 503 (the simple repository API), or a local directory laid out in the same format.
The index given by this flag is given lower priority than all other indexes specified via the `--extra-index-url` flag.
May also be set with the `UV_INDEX_URL` environment variable.
`--keyring-provider` _keyring-provider_
    
Attempt to use `keyring` for authentication for index URLs.
At present, only `--keyring-provider subprocess` is supported, which configures uv to use the `keyring` CLI to handle authentication.
Defaults to `disabled`.
May also be set with the `UV_KEYRING_PROVIDER` environment variable.
Possible values:
  * `disabled`: Do not use keyring for credential lookup
  * `subprocess`: Use the `keyring` command for credential lookup


`--link-mode` _link-mode_
    
The method to use when installing packages from the global cache.
This option is only used when building source distributions.
Defaults to `clone` (also known as Copy-on-Write) on macOS, and `hardlink` on Linux and Windows.
May also be set with the `UV_LINK_MODE` environment variable.
Possible values:
  * `clone`: Clone (i.e., copy-on-write) packages from the wheel into the `site-packages` directory
  * `copy`: Copy packages from the wheel into the `site-packages` directory
  * `hardlink`: Hard link packages from the wheel into the `site-packages` directory
  * `symlink`: Symbolically link packages from the wheel into the `site-packages` directory


`--native-tls`
    
Whether to load TLS certificates from the platform’s native certificate store.
By default, uv loads certificates from the bundled `webpki-roots` crate. The `webpki-roots` are a reliable set of trust roots from Mozilla, and including them in uv improves portability and performance (especially on macOS).
However, in some cases, you may want to use the platform’s native certificate store, especially if you’re relying on a corporate trust root (e.g., for a mandatory proxy) that’s included in your system’s certificate store.
May also be set with the `UV_NATIVE_TLS` environment variable.
`--no-annotate`
    
Exclude comment annotations indicating the source of each package
`--no-binary` _no-binary_
    
Don’t install pre-built wheels.
The given packages will be built and installed from source. The resolver will still use pre-built wheels to extract package metadata, if available.
Multiple packages may be provided. Disable binaries for all packages with `:all:`. Clear previously specified packages with `:none:`.
`--no-build`
    
Don’t build source distributions.
When enabled, resolving will not run arbitrary Python code. The cached wheels of already-built source distributions will be reused, but operations that require building distributions will exit with an error.
Alias for `--only-binary :all:`.
`--no-build-isolation`
    
Disable isolation when building source distributions.
Assumes that build dependencies specified by PEP 518 are already installed.
May also be set with the `UV_NO_BUILD_ISOLATION` environment variable.
`--no-build-isolation-package` _no-build-isolation-package_
    
Disable isolation when building source distributions for a specific package.
Assumes that the packages’ build dependencies specified by PEP 518 are already installed.
`--no-cache`, `-n`
    
Avoid reading from or writing to the cache, instead using a temporary directory for the duration of the operation
May also be set with the `UV_NO_CACHE` environment variable.
`--no-deps`
    
Ignore package dependencies, instead only add those packages explicitly listed on the command line to the resulting requirements file
`--no-emit-package` _no-emit-package_
    
Specify a package to omit from the output resolution. Its dependencies will still be included in the resolution. Equivalent to pip-compile’s `--unsafe-package` option
`--no-header`
    
Exclude the comment header at the top of the generated output file
`--no-index`
    
Ignore the registry index (e.g., PyPI), instead relying on direct URL dependencies and those provided via `--find-links`
`--no-progress`
    
Hide all progress outputs.
For example, spinners or progress bars.
May also be set with the `UV_NO_PROGRESS` environment variable.
`--no-python-downloads`
    
Disable automatic downloads of Python.
`--no-sources`
    
Ignore the `tool.uv.sources` table when resolving dependencies. Used to lock against the standards-compliant, publishable package metadata, as opposed to using any workspace, Git, URL, or local path sources
`--no-strip-extras`
    
Include extras in the output file.
By default, uv strips extras, as any packages pulled in by the extras are already included as dependencies in the output file directly. Further, output files generated with `--no-strip-extras` cannot be used as constraints files in `install` and `sync` invocations.
`--no-strip-markers`
    
Include environment markers in the output file.
By default, uv strips environment markers, as the resolution generated by `compile` is only guaranteed to be correct for the target environment.
`--offline`
    
Disable network access.
When disabled, uv will only use locally cached data and locally available files.
May also be set with the `UV_OFFLINE` environment variable.
`--only-binary` _only-binary_
    
Only use pre-built wheels; don’t build source distributions.
When enabled, resolving will not run code from the given packages. The cached wheels of already-built source distributions will be reused, but operations that require building distributions will exit with an error.
Multiple packages may be provided. Disable binaries for all packages with `:all:`. Clear previously specified packages with `:none:`.
`--output-file`, `-o` _output-file_
    
Write the compiled requirements to the given `requirements.txt` file.
If the file already exists, the existing versions will be preferred when resolving dependencies, unless `--upgrade` is also specified.
`--overrides` _overrides_
    
Override versions using the given requirements files.
Overrides files are `requirements.txt`-like files that force a specific version of a requirement to be installed, regardless of the requirements declared by any constituent package, and regardless of whether this would be considered an invalid resolution.
While constraints are _additive_ , in that they’re combined with the requirements of the constituent packages, overrides are _absolute_ , in that they completely replace the requirements of the constituent packages.
May also be set with the `UV_OVERRIDE` environment variable.
`--prerelease` _prerelease_
    
The strategy to use when considering pre-release versions.
By default, uv will accept pre-releases for packages that _only_ publish pre-releases, along with first-party requirements that contain an explicit pre-release marker in the declared specifiers (`if-necessary-or-explicit`).
May also be set with the `UV_PRERELEASE` environment variable.
Possible values:
  * `disallow`: Disallow all pre-release versions
  * `allow`: Allow all pre-release versions
  * `if-necessary`: Allow pre-release versions if all versions of a package are pre-release
  * `explicit`: Allow pre-release versions for first-party packages with explicit pre-release markers in their version requirements
  * `if-necessary-or-explicit`: Allow pre-release versions if all versions of a package are pre-release, or if the package has an explicit pre-release marker in its version requirements


`--project` _project_
    
Run the command within the given project directory.
All `pyproject.toml`, `uv.toml`, and `.python-version` files will be discovered by walking up the directory tree from the project root, as will the project’s virtual environment (`.venv`).
Other command-line arguments (such as relative paths) will be resolved relative to the current working directory.
See `--directory` to change the working directory entirely.
This setting has no effect when used in the `uv pip` interface.
`--python`, `-p` _python_
    
The Python interpreter to use during resolution.
A Python interpreter is required for building source distributions to determine package metadata when there are not wheels.
The interpreter is also used to determine the default minimum Python version, unless `--python-version` is provided.
This option respects `UV_PYTHON`, but when set via environment variable, it is overridden by `--python-version`.
See uv python for details on Python discovery and supported request formats.
`--python-platform` _python-platform_
    
The platform for which requirements should be resolved.
Represented as a "target triple", a string that describes the target platform in terms of its CPU, vendor, and operating system name, like `x86_64-unknown-linux-gnu` or `aarch64-apple-darwin`.
When targeting macOS (Darwin), the default minimum version is `12.0`. Use `MACOSX_DEPLOYMENT_TARGET` to specify a different minimum version, e.g., `13.0`.
Possible values:
  * `windows`: An alias for `x86_64-pc-windows-msvc`, the default target for Windows
  * `linux`: An alias for `x86_64-unknown-linux-gnu`, the default target for Linux
  * `macos`: An alias for `aarch64-apple-darwin`, the default target for macOS
  * `x86_64-pc-windows-msvc`: A 64-bit x86 Windows target
  * `i686-pc-windows-msvc`: A 32-bit x86 Windows target
  * `x86_64-unknown-linux-gnu`: An x86 Linux target. Equivalent to `x86_64-manylinux_2_17`
  * `aarch64-apple-darwin`: An ARM-based macOS target, as seen on Apple Silicon devices
  * `x86_64-apple-darwin`: An x86 macOS target
  * `aarch64-unknown-linux-gnu`: An ARM64 Linux target. Equivalent to `aarch64-manylinux_2_17`
  * `aarch64-unknown-linux-musl`: An ARM64 Linux target
  * `x86_64-unknown-linux-musl`: An `x86_64` Linux target
  * `x86_64-manylinux2014`: An `x86_64` target for the `manylinux2014` platform. Equivalent to `x86_64-manylinux_2_17`
  * `x86_64-manylinux_2_17`: An `x86_64` target for the `manylinux_2_17` platform
  * `x86_64-manylinux_2_28`: An `x86_64` target for the `manylinux_2_28` platform
  * `x86_64-manylinux_2_31`: An `x86_64` target for the `manylinux_2_31` platform
  * `x86_64-manylinux_2_32`: An `x86_64` target for the `manylinux_2_32` platform
  * `x86_64-manylinux_2_33`: An `x86_64` target for the `manylinux_2_33` platform
  * `x86_64-manylinux_2_34`: An `x86_64` target for the `manylinux_2_34` platform
  * `x86_64-manylinux_2_35`: An `x86_64` target for the `manylinux_2_35` platform
  * `x86_64-manylinux_2_36`: An `x86_64` target for the `manylinux_2_36` platform
  * `x86_64-manylinux_2_37`: An `x86_64` target for the `manylinux_2_37` platform
  * `x86_64-manylinux_2_38`: An `x86_64` target for the `manylinux_2_38` platform
  * `x86_64-manylinux_2_39`: An `x86_64` target for the `manylinux_2_39` platform
  * `x86_64-manylinux_2_40`: An `x86_64` target for the `manylinux_2_40` platform
  * `aarch64-manylinux2014`: An ARM64 target for the `manylinux2014` platform. Equivalent to `aarch64-manylinux_2_17`
  * `aarch64-manylinux_2_17`: An ARM64 target for the `manylinux_2_17` platform
  * `aarch64-manylinux_2_28`: An ARM64 target for the `manylinux_2_28` platform
  * `aarch64-manylinux_2_31`: An ARM64 target for the `manylinux_2_31` platform
  * `aarch64-manylinux_2_32`: An ARM64 target for the `manylinux_2_32` platform
  * `aarch64-manylinux_2_33`: An ARM64 target for the `manylinux_2_33` platform
  * `aarch64-manylinux_2_34`: An ARM64 target for the `manylinux_2_34` platform
  * `aarch64-manylinux_2_35`: An ARM64 target for the `manylinux_2_35` platform
  * `aarch64-manylinux_2_36`: An ARM64 target for the `manylinux_2_36` platform
  * `aarch64-manylinux_2_37`: An ARM64 target for the `manylinux_2_37` platform
  * `aarch64-manylinux_2_38`: An ARM64 target for the `manylinux_2_38` platform
  * `aarch64-manylinux_2_39`: An ARM64 target for the `manylinux_2_39` platform
  * `aarch64-manylinux_2_40`: An ARM64 target for the `manylinux_2_40` platform


`--python-preference` _python-preference_
    
Whether to prefer uv-managed or system Python installations.
By default, uv prefers using Python versions it manages. However, it will use system Python installations if a uv-managed Python is not installed. This option allows prioritizing or ignoring system Python installations.
May also be set with the `UV_PYTHON_PREFERENCE` environment variable.
Possible values:
  * `only-managed`: Only use managed Python installations; never use system Python installations
  * `managed`: Prefer managed Python installations over system Python installations
  * `system`: Prefer system Python installations over managed Python installations
  * `only-system`: Only use system Python installations; never use managed Python installations


`--python-version` _python-version_
    
The Python version to use for resolution.
For example, `3.8` or `3.8.17`.
Defaults to the version of the Python interpreter used for resolution.
Defines the minimum Python version that must be supported by the resolved requirements.
If a patch version is omitted, the minimum patch version is assumed. For example, `3.8` is mapped to `3.8.0`.
`--quiet`, `-q`
    
Do not print any output
`--refresh`
    
Refresh all cached data
`--refresh-package` _refresh-package_
    
Refresh cached data for a specific package
`--resolution` _resolution_
    
The strategy to use when selecting between the different compatible versions for a given package requirement.
By default, uv will use the latest compatible version of each package (`highest`).
May also be set with the `UV_RESOLUTION` environment variable.
Possible values:
  * `highest`: Resolve the highest compatible version of each package
  * `lowest`: Resolve the lowest compatible version of each package
  * `lowest-direct`: Resolve the lowest compatible version of any direct dependencies, and the highest compatible version of any transitive dependencies


`--system`
    
Install packages into the system Python environment.
By default, uv uses the virtual environment in the current working directory or any parent directory, falling back to searching for a Python executable in `PATH`. The `--system` option instructs uv to avoid using a virtual environment Python and restrict its search to the system path.
May also be set with the `UV_SYSTEM_PYTHON` environment variable.
`--universal`
    
Perform a universal resolution, attempting to generate a single `requirements.txt` output file that is compatible with all operating systems, architectures, and Python implementations.
In universal mode, the current Python version (or user-provided `--python-version`) will be treated as a lower bound. For example, `--universal --python-version 3.7` would produce a universal resolution for Python 3.7 and later.
Implies `--no-strip-markers`.
`--upgrade`, `-U`
    
Allow package upgrades, ignoring pinned versions in any existing output file. Implies `--refresh`
`--upgrade-package`, `-P` _upgrade-package_
    
Allow upgrades for a specific package, ignoring pinned versions in any existing output file. Implies `--refresh-package`
`--verbose`, `-v`
    
Use verbose output.
You can configure fine-grained logging using the `RUST_LOG` environment variable. (<https://docs.rs/tracing-subscriber/latest/tracing_subscriber/filter/struct.EnvFilter.html#directives>)
`--version`, `-V`
    
Display the uv version
### uv pip sync
Sync an environment with a `requirements.txt` file
### Usage
```
uv pip sync [OPTIONS] <SRC_FILE>...

```

### Arguments
`SRC_FILE`
    
Include all packages listed in the given `requirements.txt` files.
If a `pyproject.toml`, `setup.py`, or `setup.cfg` file is provided, uv will extract the requirements for the relevant project.
If `-` is provided, then requirements will be read from stdin.
### Options
`--allow-empty-requirements`
    
Allow sync of empty requirements, which will clear the environment of all packages
`--allow-insecure-host` _allow-insecure-host_
    
Allow insecure connections to a host.
Can be provided multiple times.
Expects to receive either a hostname (e.g., `localhost`), a host-port pair (e.g., `localhost:8080`), or a URL (e.g., `https://localhost`).
WARNING: Hosts included in this list will not be verified against the system’s certificate store. Only use `--allow-insecure-host` in a secure network with verified sources, as it bypasses SSL verification and could expose you to MITM attacks.
May also be set with the `UV_INSECURE_HOST` environment variable.
`--break-system-packages`
    
Allow uv to modify an `EXTERNALLY-MANAGED` Python installation.
WARNING: `--break-system-packages` is intended for use in continuous integration (CI) environments, when installing into Python installations that are managed by an external package manager, like `apt`. It should be used with caution, as such Python installations explicitly recommend against modifications by other package managers (like uv or `pip`).
May also be set with the `UV_BREAK_SYSTEM_PACKAGES` environment variable.
`--build-constraints`, `-b` _build-constraints_
    
Constrain build dependencies using the given requirements files when building source distributions.
Constraints files are `requirements.txt`-like files that only control the _version_ of a requirement that’s installed. However, including a package in a constraints file will _not_ trigger the installation of that package.
May also be set with the `UV_BUILD_CONSTRAINT` environment variable.
`--cache-dir` _cache-dir_
    
Path to the cache directory.
Defaults to `$XDG_CACHE_HOME/uv` or `$HOME/.cache/uv` on macOS and Linux, and `%LOCALAPPDATA%\uv\cache` on Windows.
To view the location of the cache directory, run `uv cache dir`.
May also be set with the `UV_CACHE_DIR` environment variable.
`--color` _color-choice_
    
Control the use of color in output.
By default, uv will automatically detect support for colors when writing to a terminal.
Possible values:
  * `auto`: Enables colored output only when the output is going to a terminal or TTY with support
  * `always`: Enables colored output regardless of the detected environment
  * `never`: Disables colored output


`--compile-bytecode`
    
Compile Python files to bytecode after installation.
By default, uv does not compile Python (`.py`) files to bytecode (`__pycache__/*.pyc`); instead, compilation is performed lazily the first time a module is imported. For use-cases in which start time is critical, such as CLI applications and Docker containers, this option can be enabled to trade longer installation times for faster start times.
When enabled, uv will process the entire site-packages directory (including packages that are not being modified by the current operation) for consistency. Like pip, it will also ignore errors.
May also be set with the `UV_COMPILE_BYTECODE` environment variable.
`--config-file` _config-file_
    
The path to a `uv.toml` file to use for configuration.
While uv configuration can be included in a `pyproject.toml` file, it is not allowed in this context.
May also be set with the `UV_CONFIG_FILE` environment variable.
`--config-setting`, `-C` _config-setting_
    
Settings to pass to the PEP 517 build backend, specified as `KEY=VALUE` pairs
`--constraints`, `-c` _constraints_
    
Constrain versions using the given requirements files.
Constraints files are `requirements.txt`-like files that only control the _version_ of a requirement that’s installed. However, including a package in a constraints file will _not_ trigger the installation of that package.
This is equivalent to pip’s `--constraint` option.
May also be set with the `UV_CONSTRAINT` environment variable.
`--default-index` _default-index_
    
The URL of the default package index (by default: <https://pypi.org/simple>).
Accepts either a repository compliant with PEP 503 (the simple repository API), or a local directory laid out in the same format.
The index given by this flag is given lower priority than all other indexes specified via the `--index` flag.
May also be set with the `UV_DEFAULT_INDEX` environment variable.
`--directory` _directory_
    
Change to the given directory prior to running the command.
Relative paths are resolved with the given directory as the base.
See `--project` to only change the project root directory.
`--dry-run`
    
Perform a dry run, i.e., don’t actually install anything but resolve the dependencies and print the resulting plan
`--exclude-newer` _exclude-newer_
    
Limit candidate packages to those that were uploaded prior to the given date.
Accepts both RFC 3339 timestamps (e.g., `2006-12-02T02:07:43Z`) and local dates in the same format (e.g., `2006-12-02`) in your system’s configured time zone.
May also be set with the `UV_EXCLUDE_NEWER` environment variable.
`--extra-index-url` _extra-index-url_
    
(Deprecated: use `--index` instead) Extra URLs of package indexes to use, in addition to `--index-url`.
Accepts either a repository compliant with PEP 503 (the simple repository API), or a local directory laid out in the same format.
All indexes provided via this flag take priority over the index specified by `--index-url` (which defaults to PyPI). When multiple `--extra-index-url` flags are provided, earlier values take priority.
May also be set with the `UV_EXTRA_INDEX_URL` environment variable.
`--find-links`, `-f` _find-links_
    
Locations to search for candidate distributions, in addition to those found in the registry indexes.
If a path, the target must be a directory that contains packages as wheel files (`.whl`) or source distributions (e.g., `.tar.gz` or `.zip`) at the top level.
If a URL, the page must contain a flat list of links to package files adhering to the formats described above.
May also be set with the `UV_FIND_LINKS` environment variable.
`--help`, `-h`
    
Display the concise help for this command
`--index` _index_
    
The URLs to use when resolving dependencies, in addition to the default index.
Accepts either a repository compliant with PEP 503 (the simple repository API), or a local directory laid out in the same format.
All indexes provided via this flag take priority over the index specified by `--default-index` (which defaults to PyPI). When multiple `--index` flags are provided, earlier values take priority.
May also be set with the `UV_INDEX` environment variable.
`--index-strategy` _index-strategy_
    
The strategy to use when resolving against multiple index URLs.
By default, uv will stop at the first index on which a given package is available, and limit resolutions to those present on that first index (`first-index`). This prevents "dependency confusion" attacks, whereby an attacker can upload a malicious package under the same name to an alternate index.
May also be set with the `UV_INDEX_STRATEGY` environment variable.
Possible values:
  * `first-index`: Only use results from the first index that returns a match for a given package name
  * `unsafe-first-match`: Search for every package name across all indexes, exhausting the versions from the first index before moving on to the next
  * `unsafe-best-match`: Search for every package name across all indexes, preferring the "best" version found. If a package version is in multiple indexes, only look at the entry for the first index


`--index-url`, `-i` _index-url_
    
(Deprecated: use `--default-index` instead) The URL of the Python package index (by default: <https://pypi.org/simple>).
Accepts either a repository compliant with PEP 503 (the simple repository API), or a local directory laid out in the same format.
The index given by this flag is given lower priority than all other indexes specified via the `--extra-index-url` flag.
May also be set with the `UV_INDEX_URL` environment variable.
`--keyring-provider` _keyring-provider_
    
Attempt to use `keyring` for authentication for index URLs.
At present, only `--keyring-provider subprocess` is supported, which configures uv to use the `keyring` CLI to handle authentication.
Defaults to `disabled`.
May also be set with the `UV_KEYRING_PROVIDER` environment variable.
Possible values:
  * `disabled`: Do not use keyring for credential lookup
  * `subprocess`: Use the `keyring` command for credential lookup


`--link-mode` _link-mode_
    
The method to use when installing packages from the global cache.
Defaults to `clone` (also known as Copy-on-Write) on macOS, and `hardlink` on Linux and Windows.
May also be set with the `UV_LINK_MODE` environment variable.
Possible values:
  * `clone`: Clone (i.e., copy-on-write) packages from the wheel into the `site-packages` directory
  * `copy`: Copy packages from the wheel into the `site-packages` directory
  * `hardlink`: Hard link packages from the wheel into the `site-packages` directory
  * `symlink`: Symbolically link packages from the wheel into the `site-packages` directory


`--native-tls`
    
Whether to load TLS certificates from the platform’s native certificate store.
By default, uv loads certificates from the bundled `webpki-roots` crate. The `webpki-roots` are a reliable set of trust roots from Mozilla, and including them in uv improves portability and performance (especially on macOS).
However, in some cases, you may want to use the platform’s native certificate store, especially if you’re relying on a corporate trust root (e.g., for a mandatory proxy) that’s included in your system’s certificate store.
May also be set with the `UV_NATIVE_TLS` environment variable.
`--no-allow-empty-requirements`
`--no-binary` _no-binary_
    
Don’t install pre-built wheels.
The given packages will be built and installed from source. The resolver will still use pre-built wheels to extract package metadata, if available.
Multiple packages may be provided. Disable binaries for all packages with `:all:`. Clear previously specified packages with `:none:`.
`--no-break-system-packages`
`--no-build`
    
Don’t build source distributions.
When enabled, resolving will not run arbitrary Python code. The cached wheels of already-built source distributions will be reused, but operations that require building distributions will exit with an error.
Alias for `--only-binary :all:`.
`--no-build-isolation`
    
Disable isolation when building source distributions.
Assumes that build dependencies specified by PEP 518 are already installed.
May also be set with the `UV_NO_BUILD_ISOLATION` environment variable.
`--no-cache`, `-n`
    
Avoid reading from or writing to the cache, instead using a temporary directory for the duration of the operation
May also be set with the `UV_NO_CACHE` environment variable.
`--no-index`
    
Ignore the registry index (e.g., PyPI), instead relying on direct URL dependencies and those provided via `--find-links`
`--no-progress`
    
Hide all progress outputs.
For example, spinners or progress bars.
May also be set with the `UV_NO_PROGRESS` environment variable.
`--no-python-downloads`
    
Disable automatic downloads of Python.
`--no-sources`
    
Ignore the `tool.uv.sources` table when resolving dependencies. Used to lock against the standards-compliant, publishable package metadata, as opposed to using any workspace, Git, URL, or local path sources
`--no-verify-hashes`
    
Disable validation of hashes in the requirements file.
By default, uv will verify any available hashes in the requirements file, but will not require that all requirements have an associated hash. To enforce hash validation, use `--require-hashes`.
May also be set with the `UV_NO_VERIFY_HASHES` environment variable.
`--offline`
    
Disable network access.
When disabled, uv will only use locally cached data and locally available files.
May also be set with the `UV_OFFLINE` environment variable.
`--only-binary` _only-binary_
    
Only use pre-built wheels; don’t build source distributions.
When enabled, resolving will not run code from the given packages. The cached wheels of already-built source distributions will be reused, but operations that require building distributions will exit with an error.
Multiple packages may be provided. Disable binaries for all packages with `:all:`. Clear previously specified packages with `:none:`.
`--prefix` _prefix_
    
Install packages into `lib`, `bin`, and other top-level folders under the specified directory, as if a virtual environment were present at that location.
In general, prefer the use of `--python` to install into an alternate environment, as scripts and other artifacts installed via `--prefix` will reference the installing interpreter, rather than any interpreter added to the `--prefix` directory, rendering them non-portable.
`--project` _project_
    
Run the command within the given project directory.
All `pyproject.toml`, `uv.toml`, and `.python-version` files will be discovered by walking up the directory tree from the project root, as will the project’s virtual environment (`.venv`).
Other command-line arguments (such as relative paths) will be resolved relative to the current working directory.
See `--directory` to change the working directory entirely.
This setting has no effect when used in the `uv pip` interface.
`--python`, `-p` _python_
    
The Python interpreter into which packages should be installed.
By default, syncing requires a virtual environment. A path to an alternative Python can be provided, but it is only recommended in continuous integration (CI) environments and should be used with caution, as it can modify the system Python installation.
See uv python for details on Python discovery and supported request formats.
May also be set with the `UV_PYTHON` environment variable.
`--python-platform` _python-platform_
    
The platform for which requirements should be installed.
Represented as a "target triple", a string that describes the target platform in terms of its CPU, vendor, and operating system name, like `x86_64-unknown-linux-gnu` or `aarch64-apple-darwin`.
When targeting macOS (Darwin), the default minimum version is `12.0`. Use `MACOSX_DEPLOYMENT_TARGET` to specify a different minimum version, e.g., `13.0`.
WARNING: When specified, uv will select wheels that are compatible with the _target_ platform; as a result, the installed distributions may not be compatible with the _current_ platform. Conversely, any distributions that are built from source may be incompatible with the _target_ platform, as they will be built for the _current_ platform. The `--python-platform` option is intended for advanced use cases.
Possible values:
  * `windows`: An alias for `x86_64-pc-windows-msvc`, the default target for Windows
  * `linux`: An alias for `x86_64-unknown-linux-gnu`, the default target for Linux
  * `macos`: An alias for `aarch64-apple-darwin`, the default target for macOS
  * `x86_64-pc-windows-msvc`: A 64-bit x86 Windows target
  * `i686-pc-windows-msvc`: A 32-bit x86 Windows target
  * `x86_64-unknown-linux-gnu`: An x86 Linux target. Equivalent to `x86_64-manylinux_2_17`
  * `aarch64-apple-darwin`: An ARM-based macOS target, as seen on Apple Silicon devices
  * `x86_64-apple-darwin`: An x86 macOS target
  * `aarch64-unknown-linux-gnu`: An ARM64 Linux target. Equivalent to `aarch64-manylinux_2_17`
  * `aarch64-unknown-linux-musl`: An ARM64 Linux target
  * `x86_64-unknown-linux-musl`: An `x86_64` Linux target
  * `x86_64-manylinux2014`: An `x86_64` target for the `manylinux2014` platform. Equivalent to `x86_64-manylinux_2_17`
  * `x86_64-manylinux_2_17`: An `x86_64` target for the `manylinux_2_17` platform
  * `x86_64-manylinux_2_28`: An `x86_64` target for the `manylinux_2_28` platform
  * `x86_64-manylinux_2_31`: An `x86_64` target for the `manylinux_2_31` platform
  * `x86_64-manylinux_2_32`: An `x86_64` target for the `manylinux_2_32` platform
  * `x86_64-manylinux_2_33`: An `x86_64` target for the `manylinux_2_33` platform
  * `x86_64-manylinux_2_34`: An `x86_64` target for the `manylinux_2_34` platform
  * `x86_64-manylinux_2_35`: An `x86_64` target for the `manylinux_2_35` platform
  * `x86_64-manylinux_2_36`: An `x86_64` target for the `manylinux_2_36` platform
  * `x86_64-manylinux_2_37`: An `x86_64` target for the `manylinux_2_37` platform
  * `x86_64-manylinux_2_38`: An `x86_64` target for the `manylinux_2_38` platform
  * `x86_64-manylinux_2_39`: An `x86_64` target for the `manylinux_2_39` platform
  * `x86_64-manylinux_2_40`: An `x86_64` target for the `manylinux_2_40` platform
  * `aarch64-manylinux2014`: An ARM64 target for the `manylinux2014` platform. Equivalent to `aarch64-manylinux_2_17`
  * `aarch64-manylinux_2_17`: An ARM64 target for the `manylinux_2_17` platform
  * `aarch64-manylinux_2_28`: An ARM64 target for the `manylinux_2_28` platform
  * `aarch64-manylinux_2_31`: An ARM64 target for the `manylinux_2_31` platform
  * `aarch64-manylinux_2_32`: An ARM64 target for the `manylinux_2_32` platform
  * `aarch64-manylinux_2_33`: An ARM64 target for the `manylinux_2_33` platform
  * `aarch64-manylinux_2_34`: An ARM64 target for the `manylinux_2_34` platform
  * `aarch64-manylinux_2_35`: An ARM64 target for the `manylinux_2_35` platform
  * `aarch64-manylinux_2_36`: An ARM64 target for the `manylinux_2_36` platform
  * `aarch64-manylinux_2_37`: An ARM64 target for the `manylinux_2_37` platform
  * `aarch64-manylinux_2_38`: An ARM64 target for the `manylinux_2_38` platform
  * `aarch64-manylinux_2_39`: An ARM64 target for the `manylinux_2_39` platform
  * `aarch64-manylinux_2_40`: An ARM64 target for the `manylinux_2_40` platform


`--python-preference` _python-preference_
    
Whether to prefer uv-managed or system Python installations.
By default, uv prefers using Python versions it manages. However, it will use system Python installations if a uv-managed Python is not installed. This option allows prioritizing or ignoring system Python installations.
May also be set with the `UV_PYTHON_PREFERENCE` environment variable.
Possible values:
  * `only-managed`: Only use managed Python installations; never use system Python installations
  * `managed`: Prefer managed Python installations over system Python installations
  * `system`: Prefer system Python installations over managed Python installations
  * `only-system`: Only use system Python installations; never use managed Python installations


`--python-version` _python-version_
    
The minimum Python version that should be supported by the requirements (e.g., `3.7` or `3.7.9`).
If a patch version is omitted, the minimum patch version is assumed. For example, `3.7` is mapped to `3.7.0`.
`--quiet`, `-q`
    
Do not print any output
`--refresh`
    
Refresh all cached data
`--refresh-package` _refresh-package_
    
Refresh cached data for a specific package
`--reinstall`
    
Reinstall all packages, regardless of whether they’re already installed. Implies `--refresh`
`--reinstall-package` _reinstall-package_
    
Reinstall a specific package, regardless of whether it’s already installed. Implies `--refresh-package`
`--require-hashes`
    
Require a matching hash for each requirement.
By default, uv will verify any available hashes in the requirements file, but will not require that all requirements have an associated hash.
When `--require-hashes` is enabled, _all_ requirements must include a hash or set of hashes, and _all_ requirements must either be pinned to exact versions (e.g., `==1.0.0`), or be specified via direct URL.
Hash-checking mode introduces a number of additional constraints:
  * Git dependencies are not supported. - Editable installs are not supported. - Local dependencies are not supported, unless they point to a specific wheel (`.whl`) or source archive (`.zip`, `.tar.gz`), as opposed to a directory.


May also be set with the `UV_REQUIRE_HASHES` environment variable.
`--strict`
    
Validate the Python environment after completing the installation, to detect packages with missing dependencies or other issues
`--system`
    
Install packages into the system Python environment.
By default, uv installs into the virtual environment in the current working directory or any parent directory. The `--system` option instructs uv to instead use the first Python found in the system `PATH`.
WARNING: `--system` is intended for use in continuous integration (CI) environments and should be used with caution, as it can modify the system Python installation.
May also be set with the `UV_SYSTEM_PYTHON` environment variable.
`--target` _target_
    
Install packages into the specified directory, rather than into the virtual or system Python environment. The packages will be installed at the top-level of the directory
`--verbose`, `-v`
    
Use verbose output.
You can configure fine-grained logging using the `RUST_LOG` environment variable. (<https://docs.rs/tracing-subscriber/latest/tracing_subscriber/filter/struct.EnvFilter.html#directives>)
`--version`, `-V`
    
Display the uv version
### uv pip install
Install packages into an environment
### Usage
```
uv pip install [OPTIONS] <PACKAGE|--requirements <REQUIREMENTS>|--editable <EDITABLE>>

```

### Arguments
`PACKAGE`
    
Install all listed packages.
The order of the packages is used to determine priority during resolution.
### Options
`--all-extras`
    
Include all optional dependencies.
Only applies to `pyproject.toml`, `setup.py`, and `setup.cfg` sources.
`--allow-insecure-host` _allow-insecure-host_
    
Allow insecure connections to a host.
Can be provided multiple times.
Expects to receive either a hostname (e.g., `localhost`), a host-port pair (e.g., `localhost:8080`), or a URL (e.g., `https://localhost`).
WARNING: Hosts included in this list will not be verified against the system’s certificate store. Only use `--allow-insecure-host` in a secure network with verified sources, as it bypasses SSL verification and could expose you to MITM attacks.
May also be set with the `UV_INSECURE_HOST` environment variable.
`--break-system-packages`
    
Allow uv to modify an `EXTERNALLY-MANAGED` Python installation.
WARNING: `--break-system-packages` is intended for use in continuous integration (CI) environments, when installing into Python installations that are managed by an external package manager, like `apt`. It should be used with caution, as such Python installations explicitly recommend against modifications by other package managers (like uv or `pip`).
May also be set with the `UV_BREAK_SYSTEM_PACKAGES` environment variable.
`--build-constraints`, `-b` _build-constraints_
    
Constrain build dependencies using the given requirements files when building source distributions.
Constraints files are `requirements.txt`-like files that only control the _version_ of a requirement that’s installed. However, including a package in a constraints file will _not_ trigger the installation of that package.
May also be set with the `UV_BUILD_CONSTRAINT` environment variable.
`--cache-dir` _cache-dir_
    
Path to the cache directory.
Defaults to `$XDG_CACHE_HOME/uv` or `$HOME/.cache/uv` on macOS and Linux, and `%LOCALAPPDATA%\uv\cache` on Windows.
To view the location of the cache directory, run `uv cache dir`.
May also be set with the `UV_CACHE_DIR` environment variable.
`--color` _color-choice_
    
Control the use of color in output.
By default, uv will automatically detect support for colors when writing to a terminal.
Possible values:
  * `auto`: Enables colored output only when the output is going to a terminal or TTY with support
  * `always`: Enables colored output regardless of the detected environment
  * `never`: Disables colored output


`--compile-bytecode`
    
Compile Python files to bytecode after installation.
By default, uv does not compile Python (`.py`) files to bytecode (`__pycache__/*.pyc`); instead, compilation is performed lazily the first time a module is imported. For use-cases in which start time is critical, such as CLI applications and Docker containers, this option can be enabled to trade longer installation times for faster start times.
When enabled, uv will process the entire site-packages directory (including packages that are not being modified by the current operation) for consistency. Like pip, it will also ignore errors.
May also be set with the `UV_COMPILE_BYTECODE` environment variable.
`--config-file` _config-file_
    
The path to a `uv.toml` file to use for configuration.
While uv configuration can be included in a `pyproject.toml` file, it is not allowed in this context.
May also be set with the `UV_CONFIG_FILE` environment variable.
`--config-setting`, `-C` _config-setting_
    
Settings to pass to the PEP 517 build backend, specified as `KEY=VALUE` pairs
`--constraints`, `-c` _constraints_
    
Constrain versions using the given requirements files.
Constraints files are `requirements.txt`-like files that only control the _version_ of a requirement that’s installed. However, including a package in a constraints file will _not_ trigger the installation of that package.
This is equivalent to pip’s `--constraint` option.
May also be set with the `UV_CONSTRAINT` environment variable.
`--default-index` _default-index_
    
The URL of the default package index (by default: <https://pypi.org/simple>).
Accepts either a repository compliant with PEP 503 (the simple repository API), or a local directory laid out in the same format.
The index given by this flag is given lower priority than all other indexes specified via the `--index` flag.
May also be set with the `UV_DEFAULT_INDEX` environment variable.
`--directory` _directory_
    
Change to the given directory prior to running the command.
Relative paths are resolved with the given directory as the base.
See `--project` to only change the project root directory.
`--dry-run`
    
Perform a dry run, i.e., don’t actually install anything but resolve the dependencies and print the resulting plan
`--editable`, `-e` _editable_
    
Install the editable package based on the provided local file path
`--exact`
    
Perform an exact sync, removing extraneous packages.
By default, installing will make the minimum necessary changes to satisfy the requirements. When enabled, uv will update the environment to exactly match the requirements, removing packages that are not included in the requirements.
`--exclude-newer` _exclude-newer_
    
Limit candidate packages to those that were uploaded prior to the given date.
Accepts both RFC 3339 timestamps (e.g., `2006-12-02T02:07:43Z`) and local dates in the same format (e.g., `2006-12-02`) in your system’s configured time zone.
May also be set with the `UV_EXCLUDE_NEWER` environment variable.
`--extra` _extra_
    
Include optional dependencies from the specified extra name; may be provided more than once.
Only applies to `pyproject.toml`, `setup.py`, and `setup.cfg` sources.
`--extra-index-url` _extra-index-url_
    
(Deprecated: use `--index` instead) Extra URLs of package indexes to use, in addition to `--index-url`.
Accepts either a repository compliant with PEP 503 (the simple repository API), or a local directory laid out in the same format.
All indexes provided via this flag take priority over the index specified by `--index-url` (which defaults to PyPI). When multiple `--extra-index-url` flags are provided, earlier values take priority.
May also be set with the `UV_EXTRA_INDEX_URL` environment variable.
`--find-links`, `-f` _find-links_
    
Locations to search for candidate distributions, in addition to those found in the registry indexes.
If a path, the target must be a directory that contains packages as wheel files (`.whl`) or source distributions (e.g., `.tar.gz` or `.zip`) at the top level.
If a URL, the page must contain a flat list of links to package files adhering to the formats described above.
May also be set with the `UV_FIND_LINKS` environment variable.
`--fork-strategy` _fork-strategy_
    
The strategy to use when selecting multiple versions of a given package across Python versions and platforms.
By default, uv will optimize for selecting the latest version of each package for each supported Python version (`requires-python`), while minimizing the number of selected versions across platforms.
Under `fewest`, uv will minimize the number of selected versions for each package, preferring older versions that are compatible with a wider range of supported Python versions or platforms.
May also be set with the `UV_FORK_STRATEGY` environment variable.
Possible values:
  * `fewest`: Optimize for selecting the fewest number of versions for each package. Older versions may be preferred if they are compatible with a wider range of supported Python versions or platforms
  * `requires-python`: Optimize for selecting latest supported version of each package, for each supported Python version


`--help`, `-h`
    
Display the concise help for this command
`--index` _index_
    
The URLs to use when resolving dependencies, in addition to the default index.
Accepts either a repository compliant with PEP 503 (the simple repository API), or a local directory laid out in the same format.
All indexes provided via this flag take priority over the index specified by `--default-index` (which defaults to PyPI). When multiple `--index` flags are provided, earlier values take priority.
May also be set with the `UV_INDEX` environment variable.
`--index-strategy` _index-strategy_
    
The strategy to use when resolving against multiple index URLs.
By default, uv will stop at the first index on which a given package is available, and limit resolutions to those present on that first index (`first-index`). This prevents "dependency confusion" attacks, whereby an attacker can upload a malicious package under the same name to an alternate index.
May also be set with the `UV_INDEX_STRATEGY` environment variable.
Possible values:
  * `first-index`: Only use results from the first index that returns a match for a given package name
  * `unsafe-first-match`: Search for every package name across all indexes, exhausting the versions from the first index before moving on to the next
  * `unsafe-best-match`: Search for every package name across all indexes, preferring the "best" version found. If a package version is in multiple indexes, only look at the entry for the first index


`--index-url`, `-i` _index-url_
    
(Deprecated: use `--default-index` instead) The URL of the Python package index (by default: <https://pypi.org/simple>).
Accepts either a repository compliant with PEP 503 (the simple repository API), or a local directory laid out in the same format.
The index given by this flag is given lower priority than all other indexes specified via the `--extra-index-url` flag.
May also be set with the `UV_INDEX_URL` environment variable.
`--keyring-provider` _keyring-provider_
    
Attempt to use `keyring` for authentication for index URLs.
At present, only `--keyring-provider subprocess` is supported, which configures uv to use the `keyring` CLI to handle authentication.
Defaults to `disabled`.
May also be set with the `UV_KEYRING_PROVIDER` environment variable.
Possible values:
  * `disabled`: Do not use keyring for credential lookup
  * `subprocess`: Use the `keyring` command for credential lookup


`--link-mode` _link-mode_
    
The method to use when installing packages from the global cache.
Defaults to `clone` (also known as Copy-on-Write) on macOS, and `hardlink` on Linux and Windows.
May also be set with the `UV_LINK_MODE` environment variable.
Possible values:
  * `clone`: Clone (i.e., copy-on-write) packages from the wheel into the `site-packages` directory
  * `copy`: Copy packages from the wheel into the `site-packages` directory
  * `hardlink`: Hard link packages from the wheel into the `site-packages` directory
  * `symlink`: Symbolically link packages from the wheel into the `site-packages` directory


`--native-tls`
    
Whether to load TLS certificates from the platform’s native certificate store.
By default, uv loads certificates from the bundled `webpki-roots` crate. The `webpki-roots` are a reliable set of trust roots from Mozilla, and including them in uv improves portability and performance (especially on macOS).
However, in some cases, you may want to use the platform’s native certificate store, especially if you’re relying on a corporate trust root (e.g., for a mandatory proxy) that’s included in your system’s certificate store.
May also be set with the `UV_NATIVE_TLS` environment variable.
`--no-binary` _no-binary_
    
Don’t install pre-built wheels.
The given packages will be built and installed from source. The resolver will still use pre-built wheels to extract package metadata, if available.
Multiple packages may be provided. Disable binaries for all packages with `:all:`. Clear previously specified packages with `:none:`.
`--no-break-system-packages`
`--no-build`
    
Don’t build source distributions.
When enabled, resolving will not run arbitrary Python code. The cached wheels of already-built source distributions will be reused, but operations that require building distributions will exit with an error.
Alias for `--only-binary :all:`.
`--no-build-isolation`
    
Disable isolation when building source distributions.
Assumes that build dependencies specified by PEP 518 are already installed.
May also be set with the `UV_NO_BUILD_ISOLATION` environment variable.
`--no-build-isolation-package` _no-build-isolation-package_
    
Disable isolation when building source distributions for a specific package.
Assumes that the packages’ build dependencies specified by PEP 518 are already installed.
`--no-cache`, `-n`
    
Avoid reading from or writing to the cache, instead using a temporary directory for the duration of the operation
May also be set with the `UV_NO_CACHE` environment variable.
`--no-config`
    
Avoid discovering configuration files (`pyproject.toml`, `uv.toml`).
Normally, configuration files are discovered in the current directory, parent directories, or user configuration directories.
May also be set with the `UV_NO_CONFIG` environment variable.
`--no-deps`
    
Ignore package dependencies, instead only installing those packages explicitly listed on the command line or in the requirements files
`--no-index`
    
Ignore the registry index (e.g., PyPI), instead relying on direct URL dependencies and those provided via `--find-links`
`--no-progress`
    
Hide all progress outputs.
For example, spinners or progress bars.
May also be set with the `UV_NO_PROGRESS` environment variable.
`--no-python-downloads`
    
Disable automatic downloads of Python.
`--no-sources`
    
Ignore the `tool.uv.sources` table when resolving dependencies. Used to lock against the standards-compliant, publishable package metadata, as opposed to using any workspace, Git, URL, or local path sources
`--no-verify-hashes`
    
Disable validation of hashes in the requirements file.
By default, uv will verify any available hashes in the requirements file, but will not require that all requirements have an associated hash. To enforce hash validation, use `--require-hashes`.
May also be set with the `UV_NO_VERIFY_HASHES` environment variable.
`--offline`
    
Disable network access.
When disabled, uv will only use locally cached data and locally available files.
May also be set with the `UV_OFFLINE` environment variable.
`--only-binary` _only-binary_
    
Only use pre-built wheels; don’t build source distributions.
When enabled, resolving will not run code from the given packages. The cached wheels of already-built source distributions will be reused, but operations that require building distributions will exit with an error.
Multiple packages may be provided. Disable binaries for all packages with `:all:`. Clear previously specified packages with `:none:`.
`--overrides` _overrides_
    
Override versions using the given requirements files.
Overrides files are `requirements.txt`-like files that force a specific version of a requirement to be installed, regardless of the requirements declared by any constituent package, and regardless of whether this would be considered an invalid resolution.
While constraints are _additive_ , in that they’re combined with the requirements of the constituent packages, overrides are _absolute_ , in that they completely replace the requirements of the constituent packages.
May also be set with the `UV_OVERRIDE` environment variable.
`--prefix` _prefix_
    
Install packages into `lib`, `bin`, and other top-level folders under the specified directory, as if a virtual environment were present at that location.
In general, prefer the use of `--python` to install into an alternate environment, as scripts and other artifacts installed via `--prefix` will reference the installing interpreter, rather than any interpreter added to the `--prefix` directory, rendering them non-portable.
`--prerelease` _prerelease_
    
The strategy to use when considering pre-release versions.
By default, uv will accept pre-releases for packages that _only_ publish pre-releases, along with first-party requirements that contain an explicit pre-release marker in the declared specifiers (`if-necessary-or-explicit`).
May also be set with the `UV_PRERELEASE` environment variable.
Possible values:
  * `disallow`: Disallow all pre-release versions
  * `allow`: Allow all pre-release versions
  * `if-necessary`: Allow pre-release versions if all versions of a package are pre-release
  * `explicit`: Allow pre-release versions for first-party packages with explicit pre-release markers in their version requirements
  * `if-necessary-or-explicit`: Allow pre-release versions if all versions of a package are pre-release, or if the package has an explicit pre-release marker in its version requirements


`--project` _project_
    
Run the command within the given project directory.
All `pyproject.toml`, `uv.toml`, and `.python-version` files will be discovered by walking up the directory tree from the project root, as will the project’s virtual environment (`.venv`).
Other command-line arguments (such as relative paths) will be resolved relative to the current working directory.
See `--directory` to change the working directory entirely.
This setting has no effect when used in the `uv pip` interface.
`--python`, `-p` _python_
    
The Python interpreter into which packages should be installed.
By default, installation requires a virtual environment. A path to an alternative Python can be provided, but it is only recommended in continuous integration (CI) environments and should be used with caution, as it can modify the system Python installation.
See uv python for details on Python discovery and supported request formats.
May also be set with the `UV_PYTHON` environment variable.
`--python-platform` _python-platform_
    
The platform for which requirements should be installed.
Represented as a "target triple", a string that describes the target platform in terms of its CPU, vendor, and operating system name, like `x86_64-unknown-linux-gnu` or `aarch64-apple-darwin`.
When targeting macOS (Darwin), the default minimum version is `12.0`. Use `MACOSX_DEPLOYMENT_TARGET` to specify a different minimum version, e.g., `13.0`.
WARNING: When specified, uv will select wheels that are compatible with the _target_ platform; as a result, the installed distributions may not be compatible with the _current_ platform. Conversely, any distributions that are built from source may be incompatible with the _target_ platform, as they will be built for the _current_ platform. The `--python-platform` option is intended for advanced use cases.
Possible values:
  * `windows`: An alias for `x86_64-pc-windows-msvc`, the default target for Windows
  * `linux`: An alias for `x86_64-unknown-linux-gnu`, the default target for Linux
  * `macos`: An alias for `aarch64-apple-darwin`, the default target for macOS
  * `x86_64-pc-windows-msvc`: A 64-bit x86 Windows target
  * `i686-pc-windows-msvc`: A 32-bit x86 Windows target
  * `x86_64-unknown-linux-gnu`: An x86 Linux target. Equivalent to `x86_64-manylinux_2_17`
  * `aarch64-apple-darwin`: An ARM-based macOS target, as seen on Apple Silicon devices
  * `x86_64-apple-darwin`: An x86 macOS target
  * `aarch64-unknown-linux-gnu`: An ARM64 Linux target. Equivalent to `aarch64-manylinux_2_17`
  * `aarch64-unknown-linux-musl`: An ARM64 Linux target
  * `x86_64-unknown-linux-musl`: An `x86_64` Linux target
  * `x86_64-manylinux2014`: An `x86_64` target for the `manylinux2014` platform. Equivalent to `x86_64-manylinux_2_17`
  * `x86_64-manylinux_2_17`: An `x86_64` target for the `manylinux_2_17` platform
  * `x86_64-manylinux_2_28`: An `x86_64` target for the `manylinux_2_28` platform
  * `x86_64-manylinux_2_31`: An `x86_64` target for the `manylinux_2_31` platform
  * `x86_64-manylinux_2_32`: An `x86_64` target for the `manylinux_2_32` platform
  * `x86_64-manylinux_2_33`: An `x86_64` target for the `manylinux_2_33` platform
  * `x86_64-manylinux_2_34`: An `x86_64` target for the `manylinux_2_34` platform
  * `x86_64-manylinux_2_35`: An `x86_64` target for the `manylinux_2_35` platform
  * `x86_64-manylinux_2_36`: An `x86_64` target for the `manylinux_2_36` platform
  * `x86_64-manylinux_2_37`: An `x86_64` target for the `manylinux_2_37` platform
  * `x86_64-manylinux_2_38`: An `x86_64` target for the `manylinux_2_38` platform
  * `x86_64-manylinux_2_39`: An `x86_64` target for the `manylinux_2_39` platform
  * `x86_64-manylinux_2_40`: An `x86_64` target for the `manylinux_2_40` platform
  * `aarch64-manylinux2014`: An ARM64 target for the `manylinux2014` platform. Equivalent to `aarch64-manylinux_2_17`
  * `aarch64-manylinux_2_17`: An ARM64 target for the `manylinux_2_17` platform
  * `aarch64-manylinux_2_28`: An ARM64 target for the `manylinux_2_28` platform
  * `aarch64-manylinux_2_31`: An ARM64 target for the `manylinux_2_31` platform
  * `aarch64-manylinux_2_32`: An ARM64 target for the `manylinux_2_32` platform
  * `aarch64-manylinux_2_33`: An ARM64 target for the `manylinux_2_33` platform
  * `aarch64-manylinux_2_34`: An ARM64 target for the `manylinux_2_34` platform
  * `aarch64-manylinux_2_35`: An ARM64 target for the `manylinux_2_35` platform
  * `aarch64-manylinux_2_36`: An ARM64 target for the `manylinux_2_36` platform
  * `aarch64-manylinux_2_37`: An ARM64 target for the `manylinux_2_37` platform
  * `aarch64-manylinux_2_38`: An ARM64 target for the `manylinux_2_38` platform
  * `aarch64-manylinux_2_39`: An ARM64 target for the `manylinux_2_39` platform
  * `aarch64-manylinux_2_40`: An ARM64 target for the `manylinux_2_40` platform


`--python-preference` _python-preference_
    
Whether to prefer uv-managed or system Python installations.
By default, uv prefers using Python versions it manages. However, it will use system Python installations if a uv-managed Python is not installed. This option allows prioritizing or ignoring system Python installations.
May also be set with the `UV_PYTHON_PREFERENCE` environment variable.
Possible values:
  * `only-managed`: Only use managed Python installations; never use system Python installations
  * `managed`: Prefer managed Python installations over system Python installations
  * `system`: Prefer system Python installations over managed Python installations
  * `only-system`: Only use system Python installations; never use managed Python installations


`--python-version` _python-version_
    
The minimum Python version that should be supported by the requirements (e.g., `3.7` or `3.7.9`).
If a patch version is omitted, the minimum patch version is assumed. For example, `3.7` is mapped to `3.7.0`.
`--quiet`, `-q`
    
Do not print any output
`--refresh`
    
Refresh all cached data
`--refresh-package` _refresh-package_
    
Refresh cached data for a specific package
`--reinstall`
    
Reinstall all packages, regardless of whether they’re already installed. Implies `--refresh`
`--reinstall-package` _reinstall-package_
    
Reinstall a specific package, regardless of whether it’s already installed. Implies `--refresh-package`
`--require-hashes`
    
Require a matching hash for each requirement.
By default, uv will verify any available hashes in the requirements file, but will not require that all requirements have an associated hash.
When `--require-hashes` is enabled, _all_ requirements must include a hash or set of hashes, and _all_ requirements must either be pinned to exact versions (e.g., `==1.0.0`), or be specified via direct URL.
Hash-checking mode introduces a number of additional constraints:
  * Git dependencies are not supported. - Editable installs are not supported. - Local dependencies are not supported, unless they point to a specific wheel (`.whl`) or source archive (`.zip`, `.tar.gz`), as opposed to a directory.


May also be set with the `UV_REQUIRE_HASHES` environment variable.
`--requirements`, `-r` _requirements_
    
Install all packages listed in the given `requirements.txt` files.
If a `pyproject.toml`, `setup.py`, or `setup.cfg` file is provided, uv will extract the requirements for the relevant project.
If `-` is provided, then requirements will be read from stdin.
`--resolution` _resolution_
    
The strategy to use when selecting between the different compatible versions for a given package requirement.
By default, uv will use the latest compatible version of each package (`highest`).
May also be set with the `UV_RESOLUTION` environment variable.
Possible values:
  * `highest`: Resolve the highest compatible version of each package
  * `lowest`: Resolve the lowest compatible version of each package
  * `lowest-direct`: Resolve the lowest compatible version of any direct dependencies, and the highest compatible version of any transitive dependencies


`--strict`
    
Validate the Python environment after completing the installation, to detect packages with missing dependencies or other issues
`--system`
    
Install packages into the system Python environment.
By default, uv installs into the virtual environment in the current working directory or any parent directory. The `--system` option instructs uv to instead use the first Python found in the system `PATH`.
WARNING: `--system` is intended for use in continuous integration (CI) environments and should be used with caution, as it can modify the system Python installation.
May also be set with the `UV_SYSTEM_PYTHON` environment variable.
`--target` _target_
    
Install packages into the specified directory, rather than into the virtual or system Python environment. The packages will be installed at the top-level of the directory
`--upgrade`, `-U`
    
Allow package upgrades, ignoring pinned versions in any existing output file. Implies `--refresh`
`--upgrade-package`, `-P` _upgrade-package_
    
Allow upgrades for a specific package, ignoring pinned versions in any existing output file. Implies `--refresh-package`
`--user`
`--verbose`, `-v`
    
Use verbose output.
You can configure fine-grained logging using the `RUST_LOG` environment variable. (<https://docs.rs/tracing-subscriber/latest/tracing_subscriber/filter/struct.EnvFilter.html#directives>)
`--version`, `-V`
    
Display the uv version
### uv pip uninstall
Uninstall packages from an environment
### Usage
```
uv pip uninstall [OPTIONS] <PACKAGE|--requirements <REQUIREMENTS>>

```

### Arguments
`PACKAGE`
    
Uninstall all listed packages
### Options
`--allow-insecure-host` _allow-insecure-host_
    
Allow insecure connections to a host.
Can be provided multiple times.
Expects to receive either a hostname (e.g., `localhost`), a host-port pair (e.g., `localhost:8080`), or a URL (e.g., `https://localhost`).
WARNING: Hosts included in this list will not be verified against the system’s certificate store. Only use `--allow-insecure-host` in a secure network with verified sources, as it bypasses SSL verification and could expose you to MITM attacks.
May also be set with the `UV_INSECURE_HOST` environment variable.
`--break-system-packages`
    
Allow uv to modify an `EXTERNALLY-MANAGED` Python installation.
WARNING: `--break-system-packages` is intended for use in continuous integration (CI) environments, when installing into Python installations that are managed by an external package manager, like `apt`. It should be used with caution, as such Python installations explicitly recommend against modifications by other package managers (like uv or `pip`).
May also be set with the `UV_BREAK_SYSTEM_PACKAGES` environment variable.
`--cache-dir` _cache-dir_
    
Path to the cache directory.
Defaults to `$XDG_CACHE_HOME/uv` or `$HOME/.cache/uv` on macOS and Linux, and `%LOCALAPPDATA%\uv\cache` on Windows.
To view the location of the cache directory, run `uv cache dir`.
May also be set with the `UV_CACHE_DIR` environment variable.
`--color` _color-choice_
    
Control the use of color in output.
By default, uv will automatically detect support for colors when writing to a terminal.
Possible values:
  * `auto`: Enables colored output only when the output is going to a terminal or TTY with support
  * `always`: Enables colored output regardless of the detected environment
  * `never`: Disables colored output


`--config-file` _config-file_
    
The path to a `uv.toml` file to use for configuration.
While uv configuration can be included in a `pyproject.toml` file, it is not allowed in this context.
May also be set with the `UV_CONFIG_FILE` environment variable.
`--directory` _directory_
    
Change to the given directory prior to running the command.
Relative paths are resolved with the given directory as the base.
See `--project` to only change the project root directory.
`--dry-run`
    
Perform a dry run, i.e., don’t actually uninstall anything but print the resulting plan
`--help`, `-h`
    
Display the concise help for this command
`--keyring-provider` _keyring-provider_
    
Attempt to use `keyring` for authentication for remote requirements files.
At present, only `--keyring-provider subprocess` is supported, which configures uv to use the `keyring` CLI to handle authentication.
Defaults to `disabled`.
May also be set with the `UV_KEYRING_PROVIDER` environment variable.
Possible values:
  * `disabled`: Do not use keyring for credential lookup
  * `subprocess`: Use the `keyring` command for credential lookup


`--native-tls`
    
Whether to load TLS certificates from the platform’s native certificate store.
By default, uv loads certificates from the bundled `webpki-roots` crate. The `webpki-roots` are a reliable set of trust roots from Mozilla, and including them in uv improves portability and performance (especially on macOS).
However, in some cases, you may want to use the platform’s native certificate store, especially if you’re relying on a corporate trust root (e.g., for a mandatory proxy) that’s included in your system’s certificate store.
May also be set with the `UV_NATIVE_TLS` environment variable.
`--no-break-system-packages`
`--no-cache`, `-n`
    
Avoid reading from or writing to the cache, instead using a temporary directory for the duration of the operation
May also be set with the `UV_NO_CACHE` environment variable.
`--no-config`
    
Avoid discovering configuration files (`pyproject.toml`, `uv.toml`).
Normally, configuration files are discovered in the current directory, parent directories, or user configuration directories.
May also be set with the `UV_NO_CONFIG` environment variable.
`--no-progress`
    
Hide all progress outputs.
For example, spinners or progress bars.
May also be set with the `UV_NO_PROGRESS` environment variable.
`--no-python-downloads`
    
Disable automatic downloads of Python.
`--offline`
    
Disable network access.
When disabled, uv will only use locally cached data and locally available files.
May also be set with the `UV_OFFLINE` environment variable.
`--prefix` _prefix_
    
Uninstall packages from the specified `--prefix` directory
`--project` _project_
    
Run the command within the given project directory.
All `pyproject.toml`, `uv.toml`, and `.python-version` files will be discovered by walking up the directory tree from the project root, as will the project’s virtual environment (`.venv`).
Other command-line arguments (such as relative paths) will be resolved relative to the current working directory.
See `--directory` to change the working directory entirely.
This setting has no effect when used in the `uv pip` interface.
`--python`, `-p` _python_
    
The Python interpreter from which packages should be uninstalled.
By default, uninstallation requires a virtual environment. A path to an alternative Python can be provided, but it is only recommended in continuous integration (CI) environments and should be used with caution, as it can modify the system Python installation.
See uv python for details on Python discovery and supported request formats.
May also be set with the `UV_PYTHON` environment variable.
`--python-preference` _python-preference_
    
Whether to prefer uv-managed or system Python installations.
By default, uv prefers using Python versions it manages. However, it will use system Python installations if a uv-managed Python is not installed. This option allows prioritizing or ignoring system Python installations.
May also be set with the `UV_PYTHON_PREFERENCE` environment variable.
Possible values:
  * `only-managed`: Only use managed Python installations; never use system Python installations
  * `managed`: Prefer managed Python installations over system Python installations
  * `system`: Prefer system Python installations over managed Python installations
  * `only-system`: Only use system Python installations; never use managed Python installations


`--quiet`, `-q`
    
Do not print any output
`--requirements`, `-r` _requirements_
    
Uninstall all packages listed in the given requirements files
`--system`
    
Use the system Python to uninstall packages.
By default, uv uninstalls from the virtual environment in the current working directory or any parent directory. The `--system` option instructs uv to instead use the first Python found in the system `PATH`.
WARNING: `--system` is intended for use in continuous integration (CI) environments and should be used with caution, as it can modify the system Python installation.
May also be set with the `UV_SYSTEM_PYTHON` environment variable.
`--target` _target_
    
Uninstall packages from the specified `--target` directory
`--verbose`, `-v`
    
Use verbose output.
You can configure fine-grained logging using the `RUST_LOG` environment variable. (<https://docs.rs/tracing-subscriber/latest/tracing_subscriber/filter/struct.EnvFilter.html#directives>)
`--version`, `-V`
    
Display the uv version
### uv pip freeze
List, in requirements format, packages installed in an environment
### Usage
```
uv pip freeze [OPTIONS]

```

### Options
`--allow-insecure-host` _allow-insecure-host_
    
Allow insecure connections to a host.
Can be provided multiple times.
Expects to receive either a hostname (e.g., `localhost`), a host-port pair (e.g., `localhost:8080`), or a URL (e.g., `https://localhost`).
WARNING: Hosts included in this list will not be verified against the system’s certificate store. Only use `--allow-insecure-host` in a secure network with verified sources, as it bypasses SSL verification and could expose you to MITM attacks.
May also be set with the `UV_INSECURE_HOST` environment variable.
`--cache-dir` _cache-dir_
    
Path to the cache directory.
Defaults to `$XDG_CACHE_HOME/uv` or `$HOME/.cache/uv` on macOS and Linux, and `%LOCALAPPDATA%\uv\cache` on Windows.
To view the location of the cache directory, run `uv cache dir`.
May also be set with the `UV_CACHE_DIR` environment variable.
`--color` _color-choice_
    
Control the use of color in output.
By default, uv will automatically detect support for colors when writing to a terminal.
Possible values:
  * `auto`: Enables colored output only when the output is going to a terminal or TTY with support
  * `always`: Enables colored output regardless of the detected environment
  * `never`: Disables colored output


`--config-file` _config-file_
    
The path to a `uv.toml` file to use for configuration.
While uv configuration can be included in a `pyproject.toml` file, it is not allowed in this context.
May also be set with the `UV_CONFIG_FILE` environment variable.
`--directory` _directory_
    
Change to the given directory prior to running the command.
Relative paths are resolved with the given directory as the base.
See `--project` to only change the project root directory.
`--exclude-editable`
    
Exclude any editable packages from output
`--help`, `-h`
    
Display the concise help for this command
`--native-tls`
    
Whether to load TLS certificates from the platform’s native certificate store.
By default, uv loads certificates from the bundled `webpki-roots` crate. The `webpki-roots` are a reliable set of trust roots from Mozilla, and including them in uv improves portability and performance (especially on macOS).
However, in some cases, you may want to use the platform’s native certificate store, especially if you’re relying on a corporate trust root (e.g., for a mandatory proxy) that’s included in your system’s certificate store.
May also be set with the `UV_NATIVE_TLS` environment variable.
`--no-cache`, `-n`
    
Avoid reading from or writing to the cache, instead using a temporary directory for the duration of the operation
May also be set with the `UV_NO_CACHE` environment variable.
`--no-config`
    
Avoid discovering configuration files (`pyproject.toml`, `uv.toml`).
Normally, configuration files are discovered in the current directory, parent directories, or user configuration directories.
May also be set with the `UV_NO_CONFIG` environment variable.
`--no-progress`
    
Hide all progress outputs.
For example, spinners or progress bars.
May also be set with the `UV_NO_PROGRESS` environment variable.
`--no-python-downloads`
    
Disable automatic downloads of Python.
`--offline`
    
Disable network access.
When disabled, uv will only use locally cached data and locally available files.
May also be set with the `UV_OFFLINE` environment variable.
`--path` _paths_
    
Restrict to the specified installation path for listing packages (can be used multiple times)
`--project` _project_
    
Run the command within the given project directory.
All `pyproject.toml`, `uv.toml`, and `.python-version` files will be discovered by walking up the directory tree from the project root, as will the project’s virtual environment (`.venv`).
Other command-line arguments (such as relative paths) will be resolved relative to the current working directory.
See `--directory` to change the working directory entirely.
This setting has no effect when used in the `uv pip` interface.
`--python`, `-p` _python_
    
The Python interpreter for which packages should be listed.
By default, uv lists packages in a virtual environment but will show packages in a system Python environment if no virtual environment is found.
See uv python for details on Python discovery and supported request formats.
May also be set with the `UV_PYTHON` environment variable.
`--python-preference` _python-preference_
    
Whether to prefer uv-managed or system Python installations.
By default, uv prefers using Python versions it manages. However, it will use system Python installations if a uv-managed Python is not installed. This option allows prioritizing or ignoring system Python installations.
May also be set with the `UV_PYTHON_PREFERENCE` environment variable.
Possible values:
  * `only-managed`: Only use managed Python installations; never use system Python installations
  * `managed`: Prefer managed Python installations over system Python installations
  * `system`: Prefer system Python installations over managed Python installations
  * `only-system`: Only use system Python installations; never use managed Python installations


`--quiet`, `-q`
    
Do not print any output
`--strict`
    
Validate the Python environment, to detect packages with missing dependencies and other issues
`--system`
    
List packages in the system Python environment.
Disables discovery of virtual environments.
See uv python for details on Python discovery.
May also be set with the `UV_SYSTEM_PYTHON` environment variable.
`--verbose`, `-v`
    
Use verbose output.
You can configure fine-grained logging using the `RUST_LOG` environment variable. (<https://docs.rs/tracing-subscriber/latest/tracing_subscriber/filter/struct.EnvFilter.html#directives>)
`--version`, `-V`
    
Display the uv version
### uv pip list
List, in tabular format, packages installed in an environment
### Usage
```
uv pip list [OPTIONS]

```

### Options
`--allow-insecure-host` _allow-insecure-host_
    
Allow insecure connections to a host.
Can be provided multiple times.
Expects to receive either a hostname (e.g., `localhost`), a host-port pair (e.g., `localhost:8080`), or a URL (e.g., `https://localhost`).
WARNING: Hosts included in this list will not be verified against the system’s certificate store. Only use `--allow-insecure-host` in a secure network with verified sources, as it bypasses SSL verification and could expose you to MITM attacks.
May also be set with the `UV_INSECURE_HOST` environment variable.
`--cache-dir` _cache-dir_
    
Path to the cache directory.
Defaults to `$XDG_CACHE_HOME/uv` or `$HOME/.cache/uv` on macOS and Linux, and `%LOCALAPPDATA%\uv\cache` on Windows.
To view the location of the cache directory, run `uv cache dir`.
May also be set with the `UV_CACHE_DIR` environment variable.
`--color` _color-choice_
    
Control the use of color in output.
By default, uv will automatically detect support for colors when writing to a terminal.
Possible values:
  * `auto`: Enables colored output only when the output is going to a terminal or TTY with support
  * `always`: Enables colored output regardless of the detected environment
  * `never`: Disables colored output


`--config-file` _config-file_
    
The path to a `uv.toml` file to use for configuration.
While uv configuration can be included in a `pyproject.toml` file, it is not allowed in this context.
May also be set with the `UV_CONFIG_FILE` environment variable.
`--default-index` _default-index_
    
The URL of the default package index (by default: <https://pypi.org/simple>).
Accepts either a repository compliant with PEP 503 (the simple repository API), or a local directory laid out in the same format.
The index given by this flag is given lower priority than all other indexes specified via the `--index` flag.
May also be set with the `UV_DEFAULT_INDEX` environment variable.
`--directory` _directory_
    
Change to the given directory prior to running the command.
Relative paths are resolved with the given directory as the base.
See `--project` to only change the project root directory.
`--editable`, `-e`
    
Only include editable projects
`--exclude` _exclude_
    
Exclude the specified package(s) from the output
`--exclude-editable`
    
Exclude any editable packages from output
`--exclude-newer` _exclude-newer_
    
Limit candidate packages to those that were uploaded prior to the given date.
Accepts both RFC 3339 timestamps (e.g., `2006-12-02T02:07:43Z`) and local dates in the same format (e.g., `2006-12-02`) in your system’s configured time zone.
May also be set with the `UV_EXCLUDE_NEWER` environment variable.
`--extra-index-url` _extra-index-url_
    
(Deprecated: use `--index` instead) Extra URLs of package indexes to use, in addition to `--index-url`.
Accepts either a repository compliant with PEP 503 (the simple repository API), or a local directory laid out in the same format.
All indexes provided via this flag take priority over the index specified by `--index-url` (which defaults to PyPI). When multiple `--extra-index-url` flags are provided, earlier values take priority.
May also be set with the `UV_EXTRA_INDEX_URL` environment variable.
`--find-links`, `-f` _find-links_
    
Locations to search for candidate distributions, in addition to those found in the registry indexes.
If a path, the target must be a directory that contains packages as wheel files (`.whl`) or source distributions (e.g., `.tar.gz` or `.zip`) at the top level.
If a URL, the page must contain a flat list of links to package files adhering to the formats described above.
May also be set with the `UV_FIND_LINKS` environment variable.
`--format` _format_
    
Select the output format
[default: columns]
Possible values:
  * `columns`: Display the list of packages in a human-readable table
  * `freeze`: Display the list of packages in a `pip freeze`-like format, with one package per line alongside its version
  * `json`: Display the list of packages in a machine-readable JSON format


`--help`, `-h`
    
Display the concise help for this command
`--index` _index_
    
The URLs to use when resolving dependencies, in addition to the default index.
Accepts either a repository compliant with PEP 503 (the simple repository API), or a local directory laid out in the same format.
All indexes provided via this flag take priority over the index specified by `--default-index` (which defaults to PyPI). When multiple `--index` flags are provided, earlier values take priority.
May also be set with the `UV_INDEX` environment variable.
`--index-strategy` _index-strategy_
    
The strategy to use when resolving against multiple index URLs.
By default, uv will stop at the first index on which a given package is available, and limit resolutions to those present on that first index (`first-index`). This prevents "dependency confusion" attacks, whereby an attacker can upload a malicious package under the same name to an alternate index.
May also be set with the `UV_INDEX_STRATEGY` environment variable.
Possible values:
  * `first-index`: Only use results from the first index that returns a match for a given package name
  * `unsafe-first-match`: Search for every package name across all indexes, exhausting the versions from the first index before moving on to the next
  * `unsafe-best-match`: Search for every package name across all indexes, preferring the "best" version found. If a package version is in multiple indexes, only look at the entry for the first index


`--index-url`, `-i` _index-url_
    
(Deprecated: use `--default-index` instead) The URL of the Python package index (by default: <https://pypi.org/simple>).
Accepts either a repository compliant with PEP 503 (the simple repository API), or a local directory laid out in the same format.
The index given by this flag is given lower priority than all other indexes specified via the `--extra-index-url` flag.
May also be set with the `UV_INDEX_URL` environment variable.
`--keyring-provider` _keyring-provider_
    
Attempt to use `keyring` for authentication for index URLs.
At present, only `--keyring-provider subprocess` is supported, which configures uv to use the `keyring` CLI to handle authentication.
Defaults to `disabled`.
May also be set with the `UV_KEYRING_PROVIDER` environment variable.
Possible values:
  * `disabled`: Do not use keyring for credential lookup
  * `subprocess`: Use the `keyring` command for credential lookup


`--native-tls`
    
Whether to load TLS certificates from the platform’s native certificate store.
By default, uv loads certificates from the bundled `webpki-roots` crate. The `webpki-roots` are a reliable set of trust roots from Mozilla, and including them in uv improves portability and performance (especially on macOS).
However, in some cases, you may want to use the platform’s native certificate store, especially if you’re relying on a corporate trust root (e.g., for a mandatory proxy) that’s included in your system’s certificate store.
May also be set with the `UV_NATIVE_TLS` environment variable.
`--no-cache`, `-n`
    
Avoid reading from or writing to the cache, instead using a temporary directory for the duration of the operation
May also be set with the `UV_NO_CACHE` environment variable.
`--no-config`
    
Avoid discovering configuration files (`pyproject.toml`, `uv.toml`).
Normally, configuration files are discovered in the current directory, parent directories, or user configuration directories.
May also be set with the `UV_NO_CONFIG` environment variable.
`--no-index`
    
Ignore the registry index (e.g., PyPI), instead relying on direct URL dependencies and those provided via `--find-links`
`--no-progress`
    
Hide all progress outputs.
For example, spinners or progress bars.
May also be set with the `UV_NO_PROGRESS` environment variable.
`--no-python-downloads`
    
Disable automatic downloads of Python.
`--offline`
    
Disable network access.
When disabled, uv will only use locally cached data and locally available files.
May also be set with the `UV_OFFLINE` environment variable.
`--outdated`
    
List outdated packages.
The latest version of each package will be shown alongside the installed version. Up-to-date packages will be omitted from the output.
`--project` _project_
    
Run the command within the given project directory.
All `pyproject.toml`, `uv.toml`, and `.python-version` files will be discovered by walking up the directory tree from the project root, as will the project’s virtual environment (`.venv`).
Other command-line arguments (such as relative paths) will be resolved relative to the current working directory.
See `--directory` to change the working directory entirely.
This setting has no effect when used in the `uv pip` interface.
`--python`, `-p` _python_
    
The Python interpreter for which packages should be listed.
By default, uv lists packages in a virtual environment but will show packages in a system Python environment if no virtual environment is found.
See uv python for details on Python discovery and supported request formats.
May also be set with the `UV_PYTHON` environment variable.
`--python-preference` _python-preference_
    
Whether to prefer uv-managed or system Python installations.
By default, uv prefers using Python versions it manages. However, it will use system Python installations if a uv-managed Python is not installed. This option allows prioritizing or ignoring system Python installations.
May also be set with the `UV_PYTHON_PREFERENCE` environment variable.
Possible values:
  * `only-managed`: Only use managed Python installations; never use system Python installations
  * `managed`: Prefer managed Python installations over system Python installations
  * `system`: Prefer system Python installations over managed Python installations
  * `only-system`: Only use system Python installations; never use managed Python installations


`--quiet`, `-q`
    
Do not print any output
`--strict`
    
Validate the Python environment, to detect packages with missing dependencies and other issues
`--system`
    
List packages in the system Python environment.
Disables discovery of virtual environments.
See uv python for details on Python discovery.
May also be set with the `UV_SYSTEM_PYTHON` environment variable.
`--verbose`, `-v`
    
Use verbose output.
You can configure fine-grained logging using the `RUST_LOG` environment variable. (<https://docs.rs/tracing-subscriber/latest/tracing_subscriber/filter/struct.EnvFilter.html#directives>)
`--version`, `-V`
    
Display the uv version
### uv pip show
Show information about one or more installed packages
### Usage
```
uv pip show [OPTIONS] [PACKAGE]...

```

### Arguments
`PACKAGE`
    
The package(s) to display
### Options
`--allow-insecure-host` _allow-insecure-host_
    
Allow insecure connections to a host.
Can be provided multiple times.
Expects to receive either a hostname (e.g., `localhost`), a host-port pair (e.g., `localhost:8080`), or a URL (e.g., `https://localhost`).
WARNING: Hosts included in this list will not be verified against the system’s certificate store. Only use `--allow-insecure-host` in a secure network with verified sources, as it bypasses SSL verification and could expose you to MITM attacks.
May also be set with the `UV_INSECURE_HOST` environment variable.
`--cache-dir` _cache-dir_
    
Path to the cache directory.
Defaults to `$XDG_CACHE_HOME/uv` or `$HOME/.cache/uv` on macOS and Linux, and `%LOCALAPPDATA%\uv\cache` on Windows.
To view the location of the cache directory, run `uv cache dir`.
May also be set with the `UV_CACHE_DIR` environment variable.
`--color` _color-choice_
    
Control the use of color in output.
By default, uv will automatically detect support for colors when writing to a terminal.
Possible values:
  * `auto`: Enables colored output only when the output is going to a terminal or TTY with support
  * `always`: Enables colored output regardless of the detected environment
  * `never`: Disables colored output


`--config-file` _config-file_
    
The path to a `uv.toml` file to use for configuration.
While uv configuration can be included in a `pyproject.toml` file, it is not allowed in this context.
May also be set with the `UV_CONFIG_FILE` environment variable.
`--directory` _directory_
    
Change to the given directory prior to running the command.
Relative paths are resolved with the given directory as the base.
See `--project` to only change the project root directory.
`--files`, `-f`
    
Show the full list of installed files for each package
`--help`, `-h`
    
Display the concise help for this command
`--native-tls`
    
Whether to load TLS certificates from the platform’s native certificate store.
By default, uv loads certificates from the bundled `webpki-roots` crate. The `webpki-roots` are a reliable set of trust roots from Mozilla, and including them in uv improves portability and performance (especially on macOS).
However, in some cases, you may want to use the platform’s native certificate store, especially if you’re relying on a corporate trust root (e.g., for a mandatory proxy) that’s included in your system’s certificate store.
May also be set with the `UV_NATIVE_TLS` environment variable.
`--no-cache`, `-n`
    
Avoid reading from or writing to the cache, instead using a temporary directory for the duration of the operation
May also be set with the `UV_NO_CACHE` environment variable.
`--no-config`
    
Avoid discovering configuration files (`pyproject.toml`, `uv.toml`).
Normally, configuration files are discovered in the current directory, parent directories, or user configuration directories.
May also be set with the `UV_NO_CONFIG` environment variable.
`--no-progress`
    
Hide all progress outputs.
For example, spinners or progress bars.
May also be set with the `UV_NO_PROGRESS` environment variable.
`--no-python-downloads`
    
Disable automatic downloads of Python.
`--offline`
    
Disable network access.
When disabled, uv will only use locally cached data and locally available files.
May also be set with the `UV_OFFLINE` environment variable.
`--project` _project_
    
Run the command within the given project directory.
All `pyproject.toml`, `uv.toml`, and `.python-version` files will be discovered by walking up the directory tree from the project root, as will the project’s virtual environment (`.venv`).
Other command-line arguments (such as relative paths) will be resolved relative to the current working directory.
See `--directory` to change the working directory entirely.
This setting has no effect when used in the `uv pip` interface.
`--python`, `-p` _python_
    
The Python interpreter to find the package in.
By default, uv looks for packages in a virtual environment but will look for packages in a system Python environment if no virtual environment is found.
See uv python for details on Python discovery and supported request formats.
May also be set with the `UV_PYTHON` environment variable.
`--python-preference` _python-preference_
    
Whether to prefer uv-managed or system Python installations.
By default, uv prefers using Python versions it manages. However, it will use system Python installations if a uv-managed Python is not installed. This option allows prioritizing or ignoring system Python installations.
May also be set with the `UV_PYTHON_PREFERENCE` environment variable.
Possible values:
  * `only-managed`: Only use managed Python installations; never use system Python installations
  * `managed`: Prefer managed Python installations over system Python installations
  * `system`: Prefer system Python installations over managed Python installations
  * `only-system`: Only use system Python installations; never use managed Python installations


`--quiet`, `-q`
    
Do not print any output
`--strict`
    
Validate the Python environment, to detect packages with missing dependencies and other issues
`--system`
    
Show a package in the system Python environment.
Disables discovery of virtual environments.
See uv python for details on Python discovery.
May also be set with the `UV_SYSTEM_PYTHON` environment variable.
`--verbose`, `-v`
    
Use verbose output.
You can configure fine-grained logging using the `RUST_LOG` environment variable. (<https://docs.rs/tracing-subscriber/latest/tracing_subscriber/filter/struct.EnvFilter.html#directives>)
`--version`, `-V`
    
Display the uv version
### uv pip tree
Display the dependency tree for an environment
### Usage
```
uv pip tree [OPTIONS]

```

### Options
`--allow-insecure-host` _allow-insecure-host_
    
Allow insecure connections to a host.
Can be provided multiple times.
Expects to receive either a hostname (e.g., `localhost`), a host-port pair (e.g., `localhost:8080`), or a URL (e.g., `https://localhost`).
WARNING: Hosts included in this list will not be verified against the system’s certificate store. Only use `--allow-insecure-host` in a secure network with verified sources, as it bypasses SSL verification and could expose you to MITM attacks.
May also be set with the `UV_INSECURE_HOST` environment variable.
`--cache-dir` _cache-dir_
    
Path to the cache directory.
Defaults to `$XDG_CACHE_HOME/uv` or `$HOME/.cache/uv` on macOS and Linux, and `%LOCALAPPDATA%\uv\cache` on Windows.
To view the location of the cache directory, run `uv cache dir`.
May also be set with the `UV_CACHE_DIR` environment variable.
`--color` _color-choice_
    
Control the use of color in output.
By default, uv will automatically detect support for colors when writing to a terminal.
Possible values:
  * `auto`: Enables colored output only when the output is going to a terminal or TTY with support
  * `always`: Enables colored output regardless of the detected environment
  * `never`: Disables colored output


`--config-file` _config-file_
    
The path to a `uv.toml` file to use for configuration.
While uv configuration can be included in a `pyproject.toml` file, it is not allowed in this context.
May also be set with the `UV_CONFIG_FILE` environment variable.
`--default-index` _default-index_
    
The URL of the default package index (by default: <https://pypi.org/simple>).
Accepts either a repository compliant with PEP 503 (the simple repository API), or a local directory laid out in the same format.
The index given by this flag is given lower priority than all other indexes specified via the `--index` flag.
May also be set with the `UV_DEFAULT_INDEX` environment variable.
`--depth`, `-d` _depth_
    
Maximum display depth of the dependency tree
[default: 255]
`--directory` _directory_
    
Change to the given directory prior to running the command.
Relative paths are resolved with the given directory as the base.
See `--project` to only change the project root directory.
`--exclude-newer` _exclude-newer_
    
Limit candidate packages to those that were uploaded prior to the given date.
Accepts both RFC 3339 timestamps (e.g., `2006-12-02T02:07:43Z`) and local dates in the same format (e.g., `2006-12-02`) in your system’s configured time zone.
May also be set with the `UV_EXCLUDE_NEWER` environment variable.
`--extra-index-url` _extra-index-url_
    
(Deprecated: use `--index` instead) Extra URLs of package indexes to use, in addition to `--index-url`.
Accepts either a repository compliant with PEP 503 (the simple repository API), or a local directory laid out in the same format.
All indexes provided via this flag take priority over the index specified by `--index-url` (which defaults to PyPI). When multiple `--extra-index-url` flags are provided, earlier values take priority.
May also be set with the `UV_EXTRA_INDEX_URL` environment variable.
`--find-links`, `-f` _find-links_
    
Locations to search for candidate distributions, in addition to those found in the registry indexes.
If a path, the target must be a directory that contains packages as wheel files (`.whl`) or source distributions (e.g., `.tar.gz` or `.zip`) at the top level.
If a URL, the page must contain a flat list of links to package files adhering to the formats described above.
May also be set with the `UV_FIND_LINKS` environment variable.
`--help`, `-h`
    
Display the concise help for this command
`--index` _index_
    
The URLs to use when resolving dependencies, in addition to the default index.
Accepts either a repository compliant with PEP 503 (the simple repository API), or a local directory laid out in the same format.
All indexes provided via this flag take priority over the index specified by `--default-index` (which defaults to PyPI). When multiple `--index` flags are provided, earlier values take priority.
May also be set with the `UV_INDEX` environment variable.
`--index-strategy` _index-strategy_
    
The strategy to use when resolving against multiple index URLs.
By default, uv will stop at the first index on which a given package is available, and limit resolutions to those present on that first index (`first-index`). This prevents "dependency confusion" attacks, whereby an attacker can upload a malicious package under the same name to an alternate index.
May also be set with the `UV_INDEX_STRATEGY` environment variable.
Possible values:
  * `first-index`: Only use results from the first index that returns a match for a given package name
  * `unsafe-first-match`: Search for every package name across all indexes, exhausting the versions from the first index before moving on to the next
  * `unsafe-best-match`: Search for every package name across all indexes, preferring the "best" version found. If a package version is in multiple indexes, only look at the entry for the first index


`--index-url`, `-i` _index-url_
    
(Deprecated: use `--default-index` instead) The URL of the Python package index (by default: <https://pypi.org/simple>).
Accepts either a repository compliant with PEP 503 (the simple repository API), or a local directory laid out in the same format.
The index given by this flag is given lower priority than all other indexes specified via the `--extra-index-url` flag.
May also be set with the `UV_INDEX_URL` environment variable.
`--invert`
    
Show the reverse dependencies for the given package. This flag will invert the tree and display the packages that depend on the given package
`--keyring-provider` _keyring-provider_
    
Attempt to use `keyring` for authentication for index URLs.
At present, only `--keyring-provider subprocess` is supported, which configures uv to use the `keyring` CLI to handle authentication.
Defaults to `disabled`.
May also be set with the `UV_KEYRING_PROVIDER` environment variable.
Possible values:
  * `disabled`: Do not use keyring for credential lookup
  * `subprocess`: Use the `keyring` command for credential lookup


`--native-tls`
    
Whether to load TLS certificates from the platform’s native certificate store.
By default, uv loads certificates from the bundled `webpki-roots` crate. The `webpki-roots` are a reliable set of trust roots from Mozilla, and including them in uv improves portability and performance (especially on macOS).
However, in some cases, you may want to use the platform’s native certificate store, especially if you’re relying on a corporate trust root (e.g., for a mandatory proxy) that’s included in your system’s certificate store.
May also be set with the `UV_NATIVE_TLS` environment variable.
`--no-cache`, `-n`
    
Avoid reading from or writing to the cache, instead using a temporary directory for the duration of the operation
May also be set with the `UV_NO_CACHE` environment variable.
`--no-config`
    
Avoid discovering configuration files (`pyproject.toml`, `uv.toml`).
Normally, configuration files are discovered in the current directory, parent directories, or user configuration directories.
May also be set with the `UV_NO_CONFIG` environment variable.
`--no-dedupe`
    
Do not de-duplicate repeated dependencies. Usually, when a package has already displayed its dependencies, further occurrences will not re-display its dependencies, and will include a (*) to indicate it has already been shown. This flag will cause those duplicates to be repeated
`--no-index`
    
Ignore the registry index (e.g., PyPI), instead relying on direct URL dependencies and those provided via `--find-links`
`--no-progress`
    
Hide all progress outputs.
For example, spinners or progress bars.
May also be set with the `UV_NO_PROGRESS` environment variable.
`--no-python-downloads`
    
Disable automatic downloads of Python.
`--offline`
    
Disable network access.
When disabled, uv will only use locally cached data and locally available files.
May also be set with the `UV_OFFLINE` environment variable.
`--outdated`
    
Show the latest available version of each package in the tree
`--package` _package_
    
Display only the specified packages
`--project` _project_
    
Run the command within the given project directory.
All `pyproject.toml`, `uv.toml`, and `.python-version` files will be discovered by walking up the directory tree from the project root, as will the project’s virtual environment (`.venv`).
Other command-line arguments (such as relative paths) will be resolved relative to the current working directory.
See `--directory` to change the working directory entirely.
This setting has no effect when used in the `uv pip` interface.
`--prune` _prune_
    
Prune the given package from the display of the dependency tree
`--python`, `-p` _python_
    
The Python interpreter for which packages should be listed.
By default, uv lists packages in a virtual environment but will show packages in a system Python environment if no virtual environment is found.
See uv python for details on Python discovery and supported request formats.
May also be set with the `UV_PYTHON` environment variable.
`--python-preference` _python-preference_
    
Whether to prefer uv-managed or system Python installations.
By default, uv prefers using Python versions it manages. However, it will use system Python installations if a uv-managed Python is not installed. This option allows prioritizing or ignoring system Python installations.
May also be set with the `UV_PYTHON_PREFERENCE` environment variable.
Possible values:
  * `only-managed`: Only use managed Python installations; never use system Python installations
  * `managed`: Prefer managed Python installations over system Python installations
  * `system`: Prefer system Python installations over managed Python installations
  * `only-system`: Only use system Python installations; never use managed Python installations


`--quiet`, `-q`
    
Do not print any output
`--show-version-specifiers`
    
Show the version constraint(s) imposed on each package
`--strict`
    
Validate the Python environment, to detect packages with missing dependencies and other issues
`--system`
    
List packages in the system Python environment.
Disables discovery of virtual environments.
See uv python for details on Python discovery.
May also be set with the `UV_SYSTEM_PYTHON` environment variable.
`--verbose`, `-v`
    
Use verbose output.
You can configure fine-grained logging using the `RUST_LOG` environment variable. (<https://docs.rs/tracing-subscriber/latest/tracing_subscriber/filter/struct.EnvFilter.html#directives>)
`--version`, `-V`
    
Display the uv version
### uv pip check
Verify installed packages have compatible dependencies
### Usage
```
uv pip check [OPTIONS]

```

### Options
`--allow-insecure-host` _allow-insecure-host_
    
Allow insecure connections to a host.
Can be provided multiple times.
Expects to receive either a hostname (e.g., `localhost`), a host-port pair (e.g., `localhost:8080`), or a URL (e.g., `https://localhost`).
WARNING: Hosts included in this list will not be verified against the system’s certificate store. Only use `--allow-insecure-host` in a secure network with verified sources, as it bypasses SSL verification and could expose you to MITM attacks.
May also be set with the `UV_INSECURE_HOST` environment variable.
`--cache-dir` _cache-dir_
    
Path to the cache directory.
Defaults to `$XDG_CACHE_HOME/uv` or `$HOME/.cache/uv` on macOS and Linux, and `%LOCALAPPDATA%\uv\cache` on Windows.
To view the location of the cache directory, run `uv cache dir`.
May also be set with the `UV_CACHE_DIR` environment variable.
`--color` _color-choice_
    
Control the use of color in output.
By default, uv will automatically detect support for colors when writing to a terminal.
Possible values:
  * `auto`: Enables colored output only when the output is going to a terminal or TTY with support
  * `always`: Enables colored output regardless of the detected environment
  * `never`: Disables colored output


`--config-file` _config-file_
    
The path to a `uv.toml` file to use for configuration.
While uv configuration can be included in a `pyproject.toml` file, it is not allowed in this context.
May also be set with the `UV_CONFIG_FILE` environment variable.
`--directory` _directory_
    
Change to the given directory prior to running the command.
Relative paths are resolved with the given directory as the base.
See `--project` to only change the project root directory.
`--help`, `-h`
    
Display the concise help for this command
`--native-tls`
    
Whether to load TLS certificates from the platform’s native certificate store.
By default, uv loads certificates from the bundled `webpki-roots` crate. The `webpki-roots` are a reliable set of trust roots from Mozilla, and including them in uv improves portability and performance (especially on macOS).
However, in some cases, you may want to use the platform’s native certificate store, especially if you’re relying on a corporate trust root (e.g., for a mandatory proxy) that’s included in your system’s certificate store.
May also be set with the `UV_NATIVE_TLS` environment variable.
`--no-cache`, `-n`
    
Avoid reading from or writing to the cache, instead using a temporary directory for the duration of the operation
May also be set with the `UV_NO_CACHE` environment variable.
`--no-config`
    
Avoid discovering configuration files (`pyproject.toml`, `uv.toml`).
Normally, configuration files are discovered in the current directory, parent directories, or user configuration directories.
May also be set with the `UV_NO_CONFIG` environment variable.
`--no-progress`
    
Hide all progress outputs.
For example, spinners or progress bars.
May also be set with the `UV_NO_PROGRESS` environment variable.
`--no-python-downloads`
    
Disable automatic downloads of Python.
`--offline`
    
Disable network access.
When disabled, uv will only use locally cached data and locally available files.
May also be set with the `UV_OFFLINE` environment variable.
`--project` _project_
    
Run the command within the given project directory.
All `pyproject.toml`, `uv.toml`, and `.python-version` files will be discovered by walking up the directory tree from the project root, as will the project’s virtual environment (`.venv`).
Other command-line arguments (such as relative paths) will be resolved relative to the current working directory.
See `--directory` to change the working directory entirely.
This setting has no effect when used in the `uv pip` interface.
`--python`, `-p` _python_
    
The Python interpreter for which packages should be checked.
By default, uv checks packages in a virtual environment but will check packages in a system Python environment if no virtual environment is found.
See uv python for details on Python discovery and supported request formats.
May also be set with the `UV_PYTHON` environment variable.
`--python-preference` _python-preference_
    
Whether to prefer uv-managed or system Python installations.
By default, uv prefers using Python versions it manages. However, it will use system Python installations if a uv-managed Python is not installed. This option allows prioritizing or ignoring system Python installations.
May also be set with the `UV_PYTHON_PREFERENCE` environment variable.
Possible values:
  * `only-managed`: Only use managed Python installations; never use system Python installations
  * `managed`: Prefer managed Python installations over system Python installations
  * `system`: Prefer system Python installations over managed Python installations
  * `only-system`: Only use system Python installations; never use managed Python installations


`--quiet`, `-q`
    
Do not print any output
`--system`
    
Check packages in the system Python environment.
Disables discovery of virtual environments.
See uv python for details on Python discovery.
May also be set with the `UV_SYSTEM_PYTHON` environment variable.
`--verbose`, `-v`
    
Use verbose output.
You can configure fine-grained logging using the `RUST_LOG` environment variable. (<https://docs.rs/tracing-subscriber/latest/tracing_subscriber/filter/struct.EnvFilter.html#directives>)
`--version`, `-V`
    
Display the uv version
## uv venv
Create a virtual environment.
By default, creates a virtual environment named `.venv` in the working directory. An alternative path may be provided positionally.
If in a project, the default environment name can be changed with the `UV_PROJECT_ENVIRONMENT` environment variable; this only applies when run from the project root directory.
If a virtual environment exists at the target path, it will be removed and a new, empty virtual environment will be created.
When using uv, the virtual environment does not need to be activated. uv will find a virtual environment (named `.venv`) in the working directory or any parent directories.
### Usage
```
uv venv [OPTIONS] [PATH]

```

### Arguments
`PATH`
    
The path to the virtual environment to create.
Default to `.venv` in the working directory.
Relative paths are resolved relative to the working directory.
### Options
`--allow-existing`
    
Preserve any existing files or directories at the target path.
By default, `uv venv` will remove an existing virtual environment at the given path, and exit with an error if the path is non-empty but _not_ a virtual environment. The `--allow-existing` option will instead write to the given path, regardless of its contents, and without clearing it beforehand.
WARNING: This option can lead to unexpected behavior if the existing virtual environment and the newly-created virtual environment are linked to different Python interpreters.
`--allow-insecure-host` _allow-insecure-host_
    
Allow insecure connections to a host.
Can be provided multiple times.
Expects to receive either a hostname (e.g., `localhost`), a host-port pair (e.g., `localhost:8080`), or a URL (e.g., `https://localhost`).
WARNING: Hosts included in this list will not be verified against the system’s certificate store. Only use `--allow-insecure-host` in a secure network with verified sources, as it bypasses SSL verification and could expose you to MITM attacks.
May also be set with the `UV_INSECURE_HOST` environment variable.
`--cache-dir` _cache-dir_
    
Path to the cache directory.
Defaults to `$XDG_CACHE_HOME/uv` or `$HOME/.cache/uv` on macOS and Linux, and `%LOCALAPPDATA%\uv\cache` on Windows.
To view the location of the cache directory, run `uv cache dir`.
May also be set with the `UV_CACHE_DIR` environment variable.
`--color` _color-choice_
    
Control the use of color in output.
By default, uv will automatically detect support for colors when writing to a terminal.
Possible values:
  * `auto`: Enables colored output only when the output is going to a terminal or TTY with support
  * `always`: Enables colored output regardless of the detected environment
  * `never`: Disables colored output


`--config-file` _config-file_
    
The path to a `uv.toml` file to use for configuration.
While uv configuration can be included in a `pyproject.toml` file, it is not allowed in this context.
May also be set with the `UV_CONFIG_FILE` environment variable.
`--default-index` _default-index_
    
The URL of the default package index (by default: <https://pypi.org/simple>).
Accepts either a repository compliant with PEP 503 (the simple repository API), or a local directory laid out in the same format.
The index given by this flag is given lower priority than all other indexes specified via the `--index` flag.
May also be set with the `UV_DEFAULT_INDEX` environment variable.
`--directory` _directory_
    
Change to the given directory prior to running the command.
Relative paths are resolved with the given directory as the base.
See `--project` to only change the project root directory.
`--exclude-newer` _exclude-newer_
    
Limit candidate packages to those that were uploaded prior to the given date.
Accepts both RFC 3339 timestamps (e.g., `2006-12-02T02:07:43Z`) and local dates in the same format (e.g., `2006-12-02`) in your system’s configured time zone.
May also be set with the `UV_EXCLUDE_NEWER` environment variable.
`--extra-index-url` _extra-index-url_
    
(Deprecated: use `--index` instead) Extra URLs of package indexes to use, in addition to `--index-url`.
Accepts either a repository compliant with PEP 503 (the simple repository API), or a local directory laid out in the same format.
All indexes provided via this flag take priority over the index specified by `--index-url` (which defaults to PyPI). When multiple `--extra-index-url` flags are provided, earlier values take priority.
May also be set with the `UV_EXTRA_INDEX_URL` environment variable.
`--find-links`, `-f` _find-links_
    
Locations to search for candidate distributions, in addition to those found in the registry indexes.
If a path, the target must be a directory that contains packages as wheel files (`.whl`) or source distributions (e.g., `.tar.gz` or `.zip`) at the top level.
If a URL, the page must contain a flat list of links to package files adhering to the formats described above.
May also be set with the `UV_FIND_LINKS` environment variable.
`--help`, `-h`
    
Display the concise help for this command
`--index` _index_
    
The URLs to use when resolving dependencies, in addition to the default index.
Accepts either a repository compliant with PEP 503 (the simple repository API), or a local directory laid out in the same format.
All indexes provided via this flag take priority over the index specified by `--default-index` (which defaults to PyPI). When multiple `--index` flags are provided, earlier values take priority.
May also be set with the `UV_INDEX` environment variable.
`--index-strategy` _index-strategy_
    
The strategy to use when resolving against multiple index URLs.
By default, uv will stop at the first index on which a given package is available, and limit resolutions to those present on that first index (`first-index`). This prevents "dependency confusion" attacks, whereby an attacker can upload a malicious package under the same name to an alternate index.
May also be set with the `UV_INDEX_STRATEGY` environment variable.
Possible values:
  * `first-index`: Only use results from the first index that returns a match for a given package name
  * `unsafe-first-match`: Search for every package name across all indexes, exhausting the versions from the first index before moving on to the next
  * `unsafe-best-match`: Search for every package name across all indexes, preferring the "best" version found. If a package version is in multiple indexes, only look at the entry for the first index


`--index-url`, `-i` _index-url_
    
(Deprecated: use `--default-index` instead) The URL of the Python package index (by default: <https://pypi.org/simple>).
Accepts either a repository compliant with PEP 503 (the simple repository API), or a local directory laid out in the same format.
The index given by this flag is given lower priority than all other indexes specified via the `--extra-index-url` flag.
May also be set with the `UV_INDEX_URL` environment variable.
`--keyring-provider` _keyring-provider_
    
Attempt to use `keyring` for authentication for index URLs.
At present, only `--keyring-provider subprocess` is supported, which configures uv to use the `keyring` CLI to handle authentication.
Defaults to `disabled`.
May also be set with the `UV_KEYRING_PROVIDER` environment variable.
Possible values:
  * `disabled`: Do not use keyring for credential lookup
  * `subprocess`: Use the `keyring` command for credential lookup


`--link-mode` _link-mode_
    
The method to use when installing packages from the global cache.
This option is only used for installing seed packages.
Defaults to `clone` (also known as Copy-on-Write) on macOS, and `hardlink` on Linux and Windows.
May also be set with the `UV_LINK_MODE` environment variable.
Possible values:
  * `clone`: Clone (i.e., copy-on-write) packages from the wheel into the `site-packages` directory
  * `copy`: Copy packages from the wheel into the `site-packages` directory
  * `hardlink`: Hard link packages from the wheel into the `site-packages` directory
  * `symlink`: Symbolically link packages from the wheel into the `site-packages` directory


`--native-tls`
    
Whether to load TLS certificates from the platform’s native certificate store.
By default, uv loads certificates from the bundled `webpki-roots` crate. The `webpki-roots` are a reliable set of trust roots from Mozilla, and including them in uv improves portability and performance (especially on macOS).
However, in some cases, you may want to use the platform’s native certificate store, especially if you’re relying on a corporate trust root (e.g., for a mandatory proxy) that’s included in your system’s certificate store.
May also be set with the `UV_NATIVE_TLS` environment variable.
`--no-cache`, `-n`
    
Avoid reading from or writing to the cache, instead using a temporary directory for the duration of the operation
May also be set with the `UV_NO_CACHE` environment variable.
`--no-config`
    
Avoid discovering configuration files (`pyproject.toml`, `uv.toml`).
Normally, configuration files are discovered in the current directory, parent directories, or user configuration directories.
May also be set with the `UV_NO_CONFIG` environment variable.
`--no-index`
    
Ignore the registry index (e.g., PyPI), instead relying on direct URL dependencies and those provided via `--find-links`
`--no-progress`
    
Hide all progress outputs.
For example, spinners or progress bars.
May also be set with the `UV_NO_PROGRESS` environment variable.
`--no-project`
    
Avoid discovering a project or workspace.
By default, uv searches for projects in the current directory or any parent directory to determine the default path of the virtual environment and check for Python version constraints, if any.
`--no-python-downloads`
    
Disable automatic downloads of Python.
`--offline`
    
Disable network access.
When disabled, uv will only use locally cached data and locally available files.
May also be set with the `UV_OFFLINE` environment variable.
`--project` _project_
    
Run the command within the given project directory.
All `pyproject.toml`, `uv.toml`, and `.python-version` files will be discovered by walking up the directory tree from the project root, as will the project’s virtual environment (`.venv`).
Other command-line arguments (such as relative paths) will be resolved relative to the current working directory.
See `--directory` to change the working directory entirely.
This setting has no effect when used in the `uv pip` interface.
`--prompt` _prompt_
    
Provide an alternative prompt prefix for the virtual environment.
By default, the prompt is dependent on whether a path was provided to `uv venv`. If provided (e.g, `uv venv project`), the prompt is set to the directory name. If not provided (`uv venv`), the prompt is set to the current directory’s name.
If "." is provided, the current directory name will be used regardless of whether a path was provided to `uv venv`.
`--python`, `-p` _python_
    
The Python interpreter to use for the virtual environment.
During virtual environment creation, uv will not look for Python interpreters in virtual environments.
See uv python for details on Python discovery and supported request formats.
May also be set with the `UV_PYTHON` environment variable.
`--python-preference` _python-preference_
    
Whether to prefer uv-managed or system Python installations.
By default, uv prefers using Python versions it manages. However, it will use system Python installations if a uv-managed Python is not installed. This option allows prioritizing or ignoring system Python installations.
May also be set with the `UV_PYTHON_PREFERENCE` environment variable.
Possible values:
  * `only-managed`: Only use managed Python installations; never use system Python installations
  * `managed`: Prefer managed Python installations over system Python installations
  * `system`: Prefer system Python installations over managed Python installations
  * `only-system`: Only use system Python installations; never use managed Python installations


`--quiet`, `-q`
    
Do not print any output
`--refresh`
    
Refresh all cached data
`--refresh-package` _refresh-package_
    
Refresh cached data for a specific package
`--relocatable`
    
Make the virtual environment relocatable.
A relocatable virtual environment can be moved around and redistributed without invalidating its associated entrypoint and activation scripts.
Note that this can only be guaranteed for standard `console_scripts` and `gui_scripts`. Other scripts may be adjusted if they ship with a generic `#!python[w]` shebang, and binaries are left as-is.
As a result of making the environment relocatable (by way of writing relative, rather than absolute paths), the entrypoints and scripts themselves will _not_ be relocatable. In other words, copying those entrypoints and scripts to a location outside the environment will not work, as they reference paths relative to the environment itself.
`--seed`
    
Install seed packages (one or more of: `pip`, `setuptools`, and `wheel`) into the virtual environment.
Note that `setuptools` and `wheel` are not included in Python 3.12+ environments.
May also be set with the `UV_VENV_SEED` environment variable.
`--system-site-packages`
    
Give the virtual environment access to the system site packages directory.
Unlike `pip`, when a virtual environment is created with `--system-site-packages`, uv will _not_ take system site packages into account when running commands like `uv pip list` or `uv pip install`. The `--system-site-packages` flag will provide the virtual environment with access to the system site packages directory at runtime, but will not affect the behavior of uv commands.
`--verbose`, `-v`
    
Use verbose output.
You can configure fine-grained logging using the `RUST_LOG` environment variable. (<https://docs.rs/tracing-subscriber/latest/tracing_subscriber/filter/struct.EnvFilter.html#directives>)
`--version`, `-V`
    
Display the uv version
## uv build
Build Python packages into source distributions and wheels.
`uv build` accepts a path to a directory or source distribution, which defaults to the current working directory.
By default, if passed a directory, `uv build` will build a source distribution ("sdist") from the source directory, and a binary distribution ("wheel") from the source distribution.
`uv build --sdist` can be used to build only the source distribution, `uv build --wheel` can be used to build only the binary distribution, and `uv build --sdist --wheel` can be used to build both distributions from source.
If passed a source distribution, `uv build --wheel` will build a wheel from the source distribution.
### Usage
```
uv build [OPTIONS] [SRC]

```

### Arguments
`SRC`
    
The directory from which distributions should be built, or a source distribution archive to build into a wheel.
Defaults to the current working directory.
### Options
`--all-packages`
    
Builds all packages in the workspace.
The workspace will be discovered from the provided source directory, or the current directory if no source directory is provided.
If the workspace member does not exist, uv will exit with an error.
`--allow-insecure-host` _allow-insecure-host_
    
Allow insecure connections to a host.
Can be provided multiple times.
Expects to receive either a hostname (e.g., `localhost`), a host-port pair (e.g., `localhost:8080`), or a URL (e.g., `https://localhost`).
WARNING: Hosts included in this list will not be verified against the system’s certificate store. Only use `--allow-insecure-host` in a secure network with verified sources, as it bypasses SSL verification and could expose you to MITM attacks.
May also be set with the `UV_INSECURE_HOST` environment variable.
`--build-constraints`, `-b` _build-constraints_
    
Constrain build dependencies using the given requirements files when building distributions.
Constraints files are `requirements.txt`-like files that only control the _version_ of a build dependency that’s installed. However, including a package in a constraints file will _not_ trigger the inclusion of that package on its own.
May also be set with the `UV_BUILD_CONSTRAINT` environment variable.
`--cache-dir` _cache-dir_
    
Path to the cache directory.
Defaults to `$XDG_CACHE_HOME/uv` or `$HOME/.cache/uv` on macOS and Linux, and `%LOCALAPPDATA%\uv\cache` on Windows.
To view the location of the cache directory, run `uv cache dir`.
May also be set with the `UV_CACHE_DIR` environment variable.
`--color` _color-choice_
    
Control the use of color in output.
By default, uv will automatically detect support for colors when writing to a terminal.
Possible values:
  * `auto`: Enables colored output only when the output is going to a terminal or TTY with support
  * `always`: Enables colored output regardless of the detected environment
  * `never`: Disables colored output


`--config-file` _config-file_
    
The path to a `uv.toml` file to use for configuration.
While uv configuration can be included in a `pyproject.toml` file, it is not allowed in this context.
May also be set with the `UV_CONFIG_FILE` environment variable.
`--config-setting`, `-C` _config-setting_
    
Settings to pass to the PEP 517 build backend, specified as `KEY=VALUE` pairs
`--default-index` _default-index_
    
The URL of the default package index (by default: <https://pypi.org/simple>).
Accepts either a repository compliant with PEP 503 (the simple repository API), or a local directory laid out in the same format.
The index given by this flag is given lower priority than all other indexes specified via the `--index` flag.
May also be set with the `UV_DEFAULT_INDEX` environment variable.
`--directory` _directory_
    
Change to the given directory prior to running the command.
Relative paths are resolved with the given directory as the base.
See `--project` to only change the project root directory.
`--exclude-newer` _exclude-newer_
    
Limit candidate packages to those that were uploaded prior to the given date.
Accepts both RFC 3339 timestamps (e.g., `2006-12-02T02:07:43Z`) and local dates in the same format (e.g., `2006-12-02`) in your system’s configured time zone.
May also be set with the `UV_EXCLUDE_NEWER` environment variable.
`--extra-index-url` _extra-index-url_
    
(Deprecated: use `--index` instead) Extra URLs of package indexes to use, in addition to `--index-url`.
Accepts either a repository compliant with PEP 503 (the simple repository API), or a local directory laid out in the same format.
All indexes provided via this flag take priority over the index specified by `--index-url` (which defaults to PyPI). When multiple `--extra-index-url` flags are provided, earlier values take priority.
May also be set with the `UV_EXTRA_INDEX_URL` environment variable.
`--find-links`, `-f` _find-links_
    
Locations to search for candidate distributions, in addition to those found in the registry indexes.
If a path, the target must be a directory that contains packages as wheel files (`.whl`) or source distributions (e.g., `.tar.gz` or `.zip`) at the top level.
If a URL, the page must contain a flat list of links to package files adhering to the formats described above.
May also be set with the `UV_FIND_LINKS` environment variable.
`--force-pep517`
    
Always build through PEP 517, don’t use the fast path for the uv build backend.
By default, uv won’t create a PEP 517 build environment for packages using the uv build backend, but use a fast path that calls into the build backend directly. This option forces always using PEP 517.
`--fork-strategy` _fork-strategy_
    
The strategy to use when selecting multiple versions of a given package across Python versions and platforms.
By default, uv will optimize for selecting the latest version of each package for each supported Python version (`requires-python`), while minimizing the number of selected versions across platforms.
Under `fewest`, uv will minimize the number of selected versions for each package, preferring older versions that are compatible with a wider range of supported Python versions or platforms.
May also be set with the `UV_FORK_STRATEGY` environment variable.
Possible values:
  * `fewest`: Optimize for selecting the fewest number of versions for each package. Older versions may be preferred if they are compatible with a wider range of supported Python versions or platforms
  * `requires-python`: Optimize for selecting latest supported version of each package, for each supported Python version


`--help`, `-h`
    
Display the concise help for this command
`--index` _index_
    
The URLs to use when resolving dependencies, in addition to the default index.
Accepts either a repository compliant with PEP 503 (the simple repository API), or a local directory laid out in the same format.
All indexes provided via this flag take priority over the index specified by `--default-index` (which defaults to PyPI). When multiple `--index` flags are provided, earlier values take priority.
May also be set with the `UV_INDEX` environment variable.
`--index-strategy` _index-strategy_
    
The strategy to use when resolving against multiple index URLs.
By default, uv will stop at the first index on which a given package is available, and limit resolutions to those present on that first index (`first-index`). This prevents "dependency confusion" attacks, whereby an attacker can upload a malicious package under the same name to an alternate index.
May also be set with the `UV_INDEX_STRATEGY` environment variable.
Possible values:
  * `first-index`: Only use results from the first index that returns a match for a given package name
  * `unsafe-first-match`: Search for every package name across all indexes, exhausting the versions from the first index before moving on to the next
  * `unsafe-best-match`: Search for every package name across all indexes, preferring the "best" version found. If a package version is in multiple indexes, only look at the entry for the first index


`--index-url`, `-i` _index-url_
    
(Deprecated: use `--default-index` instead) The URL of the Python package index (by default: <https://pypi.org/simple>).
Accepts either a repository compliant with PEP 503 (the simple repository API), or a local directory laid out in the same format.
The index given by this flag is given lower priority than all other indexes specified via the `--extra-index-url` flag.
May also be set with the `UV_INDEX_URL` environment variable.
`--keyring-provider` _keyring-provider_
    
Attempt to use `keyring` for authentication for index URLs.
At present, only `--keyring-provider subprocess` is supported, which configures uv to use the `keyring` CLI to handle authentication.
Defaults to `disabled`.
May also be set with the `UV_KEYRING_PROVIDER` environment variable.
Possible values:
  * `disabled`: Do not use keyring for credential lookup
  * `subprocess`: Use the `keyring` command for credential lookup


`--link-mode` _link-mode_
    
The method to use when installing packages from the global cache.
This option is only used when building source distributions.
Defaults to `clone` (also known as Copy-on-Write) on macOS, and `hardlink` on Linux and Windows.
May also be set with the `UV_LINK_MODE` environment variable.
Possible values:
  * `clone`: Clone (i.e., copy-on-write) packages from the wheel into the `site-packages` directory
  * `copy`: Copy packages from the wheel into the `site-packages` directory
  * `hardlink`: Hard link packages from the wheel into the `site-packages` directory
  * `symlink`: Symbolically link packages from the wheel into the `site-packages` directory


`--native-tls`
    
Whether to load TLS certificates from the platform’s native certificate store.
By default, uv loads certificates from the bundled `webpki-roots` crate. The `webpki-roots` are a reliable set of trust roots from Mozilla, and including them in uv improves portability and performance (especially on macOS).
However, in some cases, you may want to use the platform’s native certificate store, especially if you’re relying on a corporate trust root (e.g., for a mandatory proxy) that’s included in your system’s certificate store.
May also be set with the `UV_NATIVE_TLS` environment variable.
`--no-binary`
    
Don’t install pre-built wheels.
The given packages will be built and installed from source. The resolver will still use pre-built wheels to extract package metadata, if available.
May also be set with the `UV_NO_BINARY` environment variable.
`--no-binary-package` _no-binary-package_
    
Don’t install pre-built wheels for a specific package
May also be set with the `UV_NO_BINARY_PACKAGE` environment variable.
`--no-build`
    
Don’t build source distributions.
When enabled, resolving will not run arbitrary Python code. The cached wheels of already-built source distributions will be reused, but operations that require building distributions will exit with an error.
`--no-build-isolation`
    
Disable isolation when building source distributions.
Assumes that build dependencies specified by PEP 518 are already installed.
May also be set with the `UV_NO_BUILD_ISOLATION` environment variable.
`--no-build-isolation-package` _no-build-isolation-package_
    
Disable isolation when building source distributions for a specific package.
Assumes that the packages’ build dependencies specified by PEP 518 are already installed.
`--no-build-logs`
    
Hide logs from the build backend
`--no-build-package` _no-build-package_
    
Don’t build source distributions for a specific package
`--no-cache`, `-n`
    
Avoid reading from or writing to the cache, instead using a temporary directory for the duration of the operation
May also be set with the `UV_NO_CACHE` environment variable.
`--no-config`
    
Avoid discovering configuration files (`pyproject.toml`, `uv.toml`).
Normally, configuration files are discovered in the current directory, parent directories, or user configuration directories.
May also be set with the `UV_NO_CONFIG` environment variable.
`--no-index`
    
Ignore the registry index (e.g., PyPI), instead relying on direct URL dependencies and those provided via `--find-links`
`--no-progress`
    
Hide all progress outputs.
For example, spinners or progress bars.
May also be set with the `UV_NO_PROGRESS` environment variable.
`--no-python-downloads`
    
Disable automatic downloads of Python.
`--no-sources`
    
Ignore the `tool.uv.sources` table when resolving dependencies. Used to lock against the standards-compliant, publishable package metadata, as opposed to using any workspace, Git, URL, or local path sources
`--no-verify-hashes`
    
Disable validation of hashes in the requirements file.
By default, uv will verify any available hashes in the requirements file, but will not require that all requirements have an associated hash. To enforce hash validation, use `--require-hashes`.
May also be set with the `UV_NO_VERIFY_HASHES` environment variable.
`--offline`
    
Disable network access.
When disabled, uv will only use locally cached data and locally available files.
May also be set with the `UV_OFFLINE` environment variable.
`--out-dir`, `-o` _out-dir_
    
The output directory to which distributions should be written.
Defaults to the `dist` subdirectory within the source directory, or the directory containing the source distribution archive.
`--package` _package_
    
Build a specific package in the workspace.
The workspace will be discovered from the provided source directory, or the current directory if no source directory is provided.
If the workspace member does not exist, uv will exit with an error.
`--prerelease` _prerelease_
    
The strategy to use when considering pre-release versions.
By default, uv will accept pre-releases for packages that _only_ publish pre-releases, along with first-party requirements that contain an explicit pre-release marker in the declared specifiers (`if-necessary-or-explicit`).
May also be set with the `UV_PRERELEASE` environment variable.
Possible values:
  * `disallow`: Disallow all pre-release versions
  * `allow`: Allow all pre-release versions
  * `if-necessary`: Allow pre-release versions if all versions of a package are pre-release
  * `explicit`: Allow pre-release versions for first-party packages with explicit pre-release markers in their version requirements
  * `if-necessary-or-explicit`: Allow pre-release versions if all versions of a package are pre-release, or if the package has an explicit pre-release marker in its version requirements


`--project` _project_
    
Run the command within the given project directory.
All `pyproject.toml`, `uv.toml`, and `.python-version` files will be discovered by walking up the directory tree from the project root, as will the project’s virtual environment (`.venv`).
Other command-line arguments (such as relative paths) will be resolved relative to the current working directory.
See `--directory` to change the working directory entirely.
This setting has no effect when used in the `uv pip` interface.
`--python`, `-p` _python_
    
The Python interpreter to use for the build environment.
By default, builds are executed in isolated virtual environments. The discovered interpreter will be used to create those environments, and will be symlinked or copied in depending on the platform.
See uv python to view supported request formats.
May also be set with the `UV_PYTHON` environment variable.
`--python-preference` _python-preference_
    
Whether to prefer uv-managed or system Python installations.
By default, uv prefers using Python versions it manages. However, it will use system Python installations if a uv-managed Python is not installed. This option allows prioritizing or ignoring system Python installations.
May also be set with the `UV_PYTHON_PREFERENCE` environment variable.
Possible values:
  * `only-managed`: Only use managed Python installations; never use system Python installations
  * `managed`: Prefer managed Python installations over system Python installations
  * `system`: Prefer system Python installations over managed Python installations
  * `only-system`: Only use system Python installations; never use managed Python installations


`--quiet`, `-q`
    
Do not print any output
`--refresh`
    
Refresh all cached data
`--refresh-package` _refresh-package_
    
Refresh cached data for a specific package
`--require-hashes`
    
Require a matching hash for each requirement.
By default, uv will verify any available hashes in the requirements file, but will not require that all requirements have an associated hash.
When `--require-hashes` is enabled, _all_ requirements must include a hash or set of hashes, and _all_ requirements must either be pinned to exact versions (e.g., `==1.0.0`), or be specified via direct URL.
Hash-checking mode introduces a number of additional constraints:
  * Git dependencies are not supported. - Editable installs are not supported. - Local dependencies are not supported, unless they point to a specific wheel (`.whl`) or source archive (`.zip`, `.tar.gz`), as opposed to a directory.


May also be set with the `UV_REQUIRE_HASHES` environment variable.
`--resolution` _resolution_
    
The strategy to use when selecting between the different compatible versions for a given package requirement.
By default, uv will use the latest compatible version of each package (`highest`).
May also be set with the `UV_RESOLUTION` environment variable.
Possible values:
  * `highest`: Resolve the highest compatible version of each package
  * `lowest`: Resolve the lowest compatible version of each package
  * `lowest-direct`: Resolve the lowest compatible version of any direct dependencies, and the highest compatible version of any transitive dependencies


`--sdist`
    
Build a source distribution ("sdist") from the given directory
`--upgrade`, `-U`
    
Allow package upgrades, ignoring pinned versions in any existing output file. Implies `--refresh`
`--upgrade-package`, `-P` _upgrade-package_
    
Allow upgrades for a specific package, ignoring pinned versions in any existing output file. Implies `--refresh-package`
`--verbose`, `-v`
    
Use verbose output.
You can configure fine-grained logging using the `RUST_LOG` environment variable. (<https://docs.rs/tracing-subscriber/latest/tracing_subscriber/filter/struct.EnvFilter.html#directives>)
`--version`, `-V`
    
Display the uv version
`--wheel`
    
Build a binary distribution ("wheel") from the given directory
## uv publish
Upload distributions to an index
### Usage
```
uv publish [OPTIONS] [FILES]...

```

### Arguments
`FILES`
    
Paths to the files to upload. Accepts glob expressions.
Defaults to the `dist` directory. Selects only wheels and source distributions, while ignoring other files.
### Options
`--allow-insecure-host` _allow-insecure-host_
    
Allow insecure connections to a host.
Can be provided multiple times.
Expects to receive either a hostname (e.g., `localhost`), a host-port pair (e.g., `localhost:8080`), or a URL (e.g., `https://localhost`).
WARNING: Hosts included in this list will not be verified against the system’s certificate store. Only use `--allow-insecure-host` in a secure network with verified sources, as it bypasses SSL verification and could expose you to MITM attacks.
May also be set with the `UV_INSECURE_HOST` environment variable.
`--cache-dir` _cache-dir_
    
Path to the cache directory.
Defaults to `$XDG_CACHE_HOME/uv` or `$HOME/.cache/uv` on macOS and Linux, and `%LOCALAPPDATA%\uv\cache` on Windows.
To view the location of the cache directory, run `uv cache dir`.
May also be set with the `UV_CACHE_DIR` environment variable.
`--check-url` _check-url_
    
Check an index URL for existing files to skip duplicate uploads.
This option allows retrying publishing that failed after only some, but not all files have been uploaded, and handles error due to parallel uploads of the same file.
Before uploading, the index is checked. If the exact same file already exists in the index, the file will not be uploaded. If an error occurred during the upload, the index is checked again, to handle cases where the identical file was uploaded twice in parallel.
The exact behavior will vary based on the index. When uploading to PyPI, uploading the same file succeeds even without `--check-url`, while most other indexes error.
The index must provide one of the supported hashes (SHA-256, SHA-384, or SHA-512).
May also be set with the `UV_PUBLISH_CHECK_URL` environment variable.
`--color` _color-choice_
    
Control the use of color in output.
By default, uv will automatically detect support for colors when writing to a terminal.
Possible values:
  * `auto`: Enables colored output only when the output is going to a terminal or TTY with support
  * `always`: Enables colored output regardless of the detected environment
  * `never`: Disables colored output


`--config-file` _config-file_
    
The path to a `uv.toml` file to use for configuration.
While uv configuration can be included in a `pyproject.toml` file, it is not allowed in this context.
May also be set with the `UV_CONFIG_FILE` environment variable.
`--directory` _directory_
    
Change to the given directory prior to running the command.
Relative paths are resolved with the given directory as the base.
See `--project` to only change the project root directory.
`--help`, `-h`
    
Display the concise help for this command
`--index` _index_
    
The name of an index in the configuration to use for publishing.
The index must have a `publish-url` setting, for example:
```
The index `url` will be used to check for existing files to skip duplicate uploads.
With these settings, the following two calls are equivalent:
```

May also be set with the `UV_PUBLISH_INDEX` environment variable.
`--keyring-provider` _keyring-provider_
    
Attempt to use `keyring` for authentication for remote requirements files.
At present, only `--keyring-provider subprocess` is supported, which configures uv to use the `keyring` CLI to handle authentication.
Defaults to `disabled`.
May also be set with the `UV_KEYRING_PROVIDER` environment variable.
Possible values:
  * `disabled`: Do not use keyring for credential lookup
  * `subprocess`: Use the `keyring` command for credential lookup


`--native-tls`
    
Whether to load TLS certificates from the platform’s native certificate store.
By default, uv loads certificates from the bundled `webpki-roots` crate. The `webpki-roots` are a reliable set of trust roots from Mozilla, and including them in uv improves portability and performance (especially on macOS).
However, in some cases, you may want to use the platform’s native certificate store, especially if you’re relying on a corporate trust root (e.g., for a mandatory proxy) that’s included in your system’s certificate store.
May also be set with the `UV_NATIVE_TLS` environment variable.
`--no-cache`, `-n`
    
Avoid reading from or writing to the cache, instead using a temporary directory for the duration of the operation
May also be set with the `UV_NO_CACHE` environment variable.
`--no-config`
    
Avoid discovering configuration files (`pyproject.toml`, `uv.toml`).
Normally, configuration files are discovered in the current directory, parent directories, or user configuration directories.
May also be set with the `UV_NO_CONFIG` environment variable.
`--no-progress`
    
Hide all progress outputs.
For example, spinners or progress bars.
May also be set with the `UV_NO_PROGRESS` environment variable.
`--no-python-downloads`
    
Disable automatic downloads of Python.
`--offline`
    
Disable network access.
When disabled, uv will only use locally cached data and locally available files.
May also be set with the `UV_OFFLINE` environment variable.
`--password`, `-p` _password_
    
The password for the upload
May also be set with the `UV_PUBLISH_PASSWORD` environment variable.
`--project` _project_
    
Run the command within the given project directory.
All `pyproject.toml`, `uv.toml`, and `.python-version` files will be discovered by walking up the directory tree from the project root, as will the project’s virtual environment (`.venv`).
Other command-line arguments (such as relative paths) will be resolved relative to the current working directory.
See `--directory` to change the working directory entirely.
This setting has no effect when used in the `uv pip` interface.
`--publish-url` _publish-url_
    
The URL of the upload endpoint (not the index URL).
Note that there are typically different URLs for index access (e.g., `https:://.../simple`) and index upload.
Defaults to PyPI’s publish URL (<https://upload.pypi.org/legacy/>).
May also be set with the `UV_PUBLISH_URL` environment variable.
`--python-preference` _python-preference_
    
Whether to prefer uv-managed or system Python installations.
By default, uv prefers using Python versions it manages. However, it will use system Python installations if a uv-managed Python is not installed. This option allows prioritizing or ignoring system Python installations.
May also be set with the `UV_PYTHON_PREFERENCE` environment variable.
Possible values:
  * `only-managed`: Only use managed Python installations; never use system Python installations
  * `managed`: Prefer managed Python installations over system Python installations
  * `system`: Prefer system Python installations over managed Python installations
  * `only-system`: Only use system Python installations; never use managed Python installations


`--quiet`, `-q`
    
Do not print any output
`--token`, `-t` _token_
    
The token for the upload.
Using a token is equivalent to passing `__token__` as `--username` and the token as `--password` password.
May also be set with the `UV_PUBLISH_TOKEN` environment variable.
`--trusted-publishing` _trusted-publishing_
    
Configure using trusted publishing through GitHub Actions.
By default, uv checks for trusted publishing when running in GitHub Actions, but ignores it if it isn’t configured or the workflow doesn’t have enough permissions (e.g., a pull request from a fork).
Possible values:
  * `automatic`: Try trusted publishing when we’re already in GitHub Actions, continue if that fails
  * `always`
  * `never`


`--username`, `-u` _username_
    
The username for the upload
May also be set with the `UV_PUBLISH_USERNAME` environment variable.
`--verbose`, `-v`
    
Use verbose output.
You can configure fine-grained logging using the `RUST_LOG` environment variable. (<https://docs.rs/tracing-subscriber/latest/tracing_subscriber/filter/struct.EnvFilter.html#directives>)
`--version`, `-V`
    
Display the uv version
## uv cache
Manage uv's cache
### Usage
```
uv cache [OPTIONS] <COMMAND>

```

### Commands
`uv cache clean`
    
Clear the cache, removing all entries or those linked to specific packages
`uv cache prune`
    
Prune all unreachable objects from the cache
`uv cache dir`
    
Show the cache directory
### uv cache clean
Clear the cache, removing all entries or those linked to specific packages
### Usage
```
uv cache clean [OPTIONS] [PACKAGE]...

```

### Arguments
`PACKAGE`
    
The packages to remove from the cache
### Options
`--allow-insecure-host` _allow-insecure-host_
    
Allow insecure connections to a host.
Can be provided multiple times.
Expects to receive either a hostname (e.g., `localhost`), a host-port pair (e.g., `localhost:8080`), or a URL (e.g., `https://localhost`).
WARNING: Hosts included in this list will not be verified against the system’s certificate store. Only use `--allow-insecure-host` in a secure network with verified sources, as it bypasses SSL verification and could expose you to MITM attacks.
May also be set with the `UV_INSECURE_HOST` environment variable.
`--cache-dir` _cache-dir_
    
Path to the cache directory.
Defaults to `$XDG_CACHE_HOME/uv` or `$HOME/.cache/uv` on macOS and Linux, and `%LOCALAPPDATA%\uv\cache` on Windows.
To view the location of the cache directory, run `uv cache dir`.
May also be set with the `UV_CACHE_DIR` environment variable.
`--color` _color-choice_
    
Control the use of color in output.
By default, uv will automatically detect support for colors when writing to a terminal.
Possible values:
  * `auto`: Enables colored output only when the output is going to a terminal or TTY with support
  * `always`: Enables colored output regardless of the detected environment
  * `never`: Disables colored output


`--config-file` _config-file_
    
The path to a `uv.toml` file to use for configuration.
While uv configuration can be included in a `pyproject.toml` file, it is not allowed in this context.
May also be set with the `UV_CONFIG_FILE` environment variable.
`--directory` _directory_
    
Change to the given directory prior to running the command.
Relative paths are resolved with the given directory as the base.
See `--project` to only change the project root directory.
`--help`, `-h`
    
Display the concise help for this command
`--native-tls`
    
Whether to load TLS certificates from the platform’s native certificate store.
By default, uv loads certificates from the bundled `webpki-roots` crate. The `webpki-roots` are a reliable set of trust roots from Mozilla, and including them in uv improves portability and performance (especially on macOS).
However, in some cases, you may want to use the platform’s native certificate store, especially if you’re relying on a corporate trust root (e.g., for a mandatory proxy) that’s included in your system’s certificate store.
May also be set with the `UV_NATIVE_TLS` environment variable.
`--no-cache`, `-n`
    
Avoid reading from or writing to the cache, instead using a temporary directory for the duration of the operation
May also be set with the `UV_NO_CACHE` environment variable.
`--no-config`
    
Avoid discovering configuration files (`pyproject.toml`, `uv.toml`).
Normally, configuration files are discovered in the current directory, parent directories, or user configuration directories.
May also be set with the `UV_NO_CONFIG` environment variable.
`--no-progress`
    
Hide all progress outputs.
For example, spinners or progress bars.
May also be set with the `UV_NO_PROGRESS` environment variable.
`--no-python-downloads`
    
Disable automatic downloads of Python.
`--offline`
    
Disable network access.
When disabled, uv will only use locally cached data and locally available files.
May also be set with the `UV_OFFLINE` environment variable.
`--project` _project_
    
Run the command within the given project directory.
All `pyproject.toml`, `uv.toml`, and `.python-version` files will be discovered by walking up the directory tree from the project root, as will the project’s virtual environment (`.venv`).
Other command-line arguments (such as relative paths) will be resolved relative to the current working directory.
See `--directory` to change the working directory entirely.
This setting has no effect when used in the `uv pip` interface.
`--python-preference` _python-preference_
    
Whether to prefer uv-managed or system Python installations.
By default, uv prefers using Python versions it manages. However, it will use system Python installations if a uv-managed Python is not installed. This option allows prioritizing or ignoring system Python installations.
May also be set with the `UV_PYTHON_PREFERENCE` environment variable.
Possible values:
  * `only-managed`: Only use managed Python installations; never use system Python installations
  * `managed`: Prefer managed Python installations over system Python installations
  * `system`: Prefer system Python installations over managed Python installations
  * `only-system`: Only use system Python installations; never use managed Python installations


`--quiet`, `-q`
    
Do not print any output
`--verbose`, `-v`
    
Use verbose output.
You can configure fine-grained logging using the `RUST_LOG` environment variable. (<https://docs.rs/tracing-subscriber/latest/tracing_subscriber/filter/struct.EnvFilter.html#directives>)
`--version`, `-V`
    
Display the uv version
### uv cache prune
Prune all unreachable objects from the cache
### Usage
```
uv cache prune [OPTIONS]

```

### Options
`--allow-insecure-host` _allow-insecure-host_
    
Allow insecure connections to a host.
Can be provided multiple times.
Expects to receive either a hostname (e.g., `localhost`), a host-port pair (e.g., `localhost:8080`), or a URL (e.g., `https://localhost`).
WARNING: Hosts included in this list will not be verified against the system’s certificate store. Only use `--allow-insecure-host` in a secure network with verified sources, as it bypasses SSL verification and could expose you to MITM attacks.
May also be set with the `UV_INSECURE_HOST` environment variable.
`--cache-dir` _cache-dir_
    
Path to the cache directory.
Defaults to `$XDG_CACHE_HOME/uv` or `$HOME/.cache/uv` on macOS and Linux, and `%LOCALAPPDATA%\uv\cache` on Windows.
To view the location of the cache directory, run `uv cache dir`.
May also be set with the `UV_CACHE_DIR` environment variable.
`--ci`
    
Optimize the cache for persistence in a continuous integration environment, like GitHub Actions.
By default, uv caches both the wheels that it builds from source and the pre-built wheels that it downloads directly, to enable high-performance package installation. In some scenarios, though, persisting pre-built wheels may be undesirable. For example, in GitHub Actions, it’s faster to omit pre-built wheels from the cache and instead have re-download them on each run. However, it typically _is_ faster to cache wheels that are built from source, since the wheel building process can be expensive, especially for extension modules.
In `--ci` mode, uv will prune any pre-built wheels from the cache, but retain any wheels that were built from source.
`--color` _color-choice_
    
Control the use of color in output.
By default, uv will automatically detect support for colors when writing to a terminal.
Possible values:
  * `auto`: Enables colored output only when the output is going to a terminal or TTY with support
  * `always`: Enables colored output regardless of the detected environment
  * `never`: Disables colored output


`--config-file` _config-file_
    
The path to a `uv.toml` file to use for configuration.
While uv configuration can be included in a `pyproject.toml` file, it is not allowed in this context.
May also be set with the `UV_CONFIG_FILE` environment variable.
`--directory` _directory_
    
Change to the given directory prior to running the command.
Relative paths are resolved with the given directory as the base.
See `--project` to only change the project root directory.
`--help`, `-h`
    
Display the concise help for this command
`--native-tls`
    
Whether to load TLS certificates from the platform’s native certificate store.
By default, uv loads certificates from the bundled `webpki-roots` crate. The `webpki-roots` are a reliable set of trust roots from Mozilla, and including them in uv improves portability and performance (especially on macOS).
However, in some cases, you may want to use the platform’s native certificate store, especially if you’re relying on a corporate trust root (e.g., for a mandatory proxy) that’s included in your system’s certificate store.
May also be set with the `UV_NATIVE_TLS` environment variable.
`--no-cache`, `-n`
    
Avoid reading from or writing to the cache, instead using a temporary directory for the duration of the operation
May also be set with the `UV_NO_CACHE` environment variable.
`--no-config`
    
Avoid discovering configuration files (`pyproject.toml`, `uv.toml`).
Normally, configuration files are discovered in the current directory, parent directories, or user configuration directories.
May also be set with the `UV_NO_CONFIG` environment variable.
`--no-progress`
    
Hide all progress outputs.
For example, spinners or progress bars.
May also be set with the `UV_NO_PROGRESS` environment variable.
`--no-python-downloads`
    
Disable automatic downloads of Python.
`--offline`
    
Disable network access.
When disabled, uv will only use locally cached data and locally available files.
May also be set with the `UV_OFFLINE` environment variable.
`--project` _project_
    
Run the command within the given project directory.
All `pyproject.toml`, `uv.toml`, and `.python-version` files will be discovered by walking up the directory tree from the project root, as will the project’s virtual environment (`.venv`).
Other command-line arguments (such as relative paths) will be resolved relative to the current working directory.
See `--directory` to change the working directory entirely.
This setting has no effect when used in the `uv pip` interface.
`--python-preference` _python-preference_
    
Whether to prefer uv-managed or system Python installations.
By default, uv prefers using Python versions it manages. However, it will use system Python installations if a uv-managed Python is not installed. This option allows prioritizing or ignoring system Python installations.
May also be set with the `UV_PYTHON_PREFERENCE` environment variable.
Possible values:
  * `only-managed`: Only use managed Python installations; never use system Python installations
  * `managed`: Prefer managed Python installations over system Python installations
  * `system`: Prefer system Python installations over managed Python installations
  * `only-system`: Only use system Python installations; never use managed Python installations


`--quiet`, `-q`
    
Do not print any output
`--verbose`, `-v`
    
Use verbose output.
You can configure fine-grained logging using the `RUST_LOG` environment variable. (<https://docs.rs/tracing-subscriber/latest/tracing_subscriber/filter/struct.EnvFilter.html#directives>)
`--version`, `-V`
    
Display the uv version
### uv cache dir
Show the cache directory.
By default, the cache is stored in `$XDG_CACHE_HOME/uv` or `$HOME/.cache/uv` on Unix and `%LOCALAPPDATA%\uv\cache` on Windows.
When `--no-cache` is used, the cache is stored in a temporary directory and discarded when the process exits.
An alternative cache directory may be specified via the `cache-dir` setting, the `--cache-dir` option, or the `$UV_CACHE_DIR` environment variable.
Note that it is important for performance for the cache directory to be located on the same file system as the Python environment uv is operating on.
### Usage
```
uv cache dir [OPTIONS]

```

### Options
`--allow-insecure-host` _allow-insecure-host_
    
Allow insecure connections to a host.
Can be provided multiple times.
Expects to receive either a hostname (e.g., `localhost`), a host-port pair (e.g., `localhost:8080`), or a URL (e.g., `https://localhost`).
WARNING: Hosts included in this list will not be verified against the system’s certificate store. Only use `--allow-insecure-host` in a secure network with verified sources, as it bypasses SSL verification and could expose you to MITM attacks.
May also be set with the `UV_INSECURE_HOST` environment variable.
`--cache-dir` _cache-dir_
    
Path to the cache directory.
Defaults to `$XDG_CACHE_HOME/uv` or `$HOME/.cache/uv` on macOS and Linux, and `%LOCALAPPDATA%\uv\cache` on Windows.
To view the location of the cache directory, run `uv cache dir`.
May also be set with the `UV_CACHE_DIR` environment variable.
`--color` _color-choice_
    
Control the use of color in output.
By default, uv will automatically detect support for colors when writing to a terminal.
Possible values:
  * `auto`: Enables colored output only when the output is going to a terminal or TTY with support
  * `always`: Enables colored output regardless of the detected environment
  * `never`: Disables colored output


`--config-file` _config-file_
    
The path to a `uv.toml` file to use for configuration.
While uv configuration can be included in a `pyproject.toml` file, it is not allowed in this context.
May also be set with the `UV_CONFIG_FILE` environment variable.
`--directory` _directory_
    
Change to the given directory prior to running the command.
Relative paths are resolved with the given directory as the base.
See `--project` to only change the project root directory.
`--help`, `-h`
    
Display the concise help for this command
`--native-tls`
    
Whether to load TLS certificates from the platform’s native certificate store.
By default, uv loads certificates from the bundled `webpki-roots` crate. The `webpki-roots` are a reliable set of trust roots from Mozilla, and including them in uv improves portability and performance (especially on macOS).
However, in some cases, you may want to use the platform’s native certificate store, especially if you’re relying on a corporate trust root (e.g., for a mandatory proxy) that’s included in your system’s certificate store.
May also be set with the `UV_NATIVE_TLS` environment variable.
`--no-cache`, `-n`
    
Avoid reading from or writing to the cache, instead using a temporary directory for the duration of the operation
May also be set with the `UV_NO_CACHE` environment variable.
`--no-config`
    
Avoid discovering configuration files (`pyproject.toml`, `uv.toml`).
Normally, configuration files are discovered in the current directory, parent directories, or user configuration directories.
May also be set with the `UV_NO_CONFIG` environment variable.
`--no-progress`
    
Hide all progress outputs.
For example, spinners or progress bars.
May also be set with the `UV_NO_PROGRESS` environment variable.
`--no-python-downloads`
    
Disable automatic downloads of Python.
`--offline`
    
Disable network access.
When disabled, uv will only use locally cached data and locally available files.
May also be set with the `UV_OFFLINE` environment variable.
`--project` _project_
    
Run the command within the given project directory.
All `pyproject.toml`, `uv.toml`, and `.python-version` files will be discovered by walking up the directory tree from the project root, as will the project’s virtual environment (`.venv`).
Other command-line arguments (such as relative paths) will be resolved relative to the current working directory.
See `--directory` to change the working directory entirely.
This setting has no effect when used in the `uv pip` interface.
`--python-preference` _python-preference_
    
Whether to prefer uv-managed or system Python installations.
By default, uv prefers using Python versions it manages. However, it will use system Python installations if a uv-managed Python is not installed. This option allows prioritizing or ignoring system Python installations.
May also be set with the `UV_PYTHON_PREFERENCE` environment variable.
Possible values:
  * `only-managed`: Only use managed Python installations; never use system Python installations
  * `managed`: Prefer managed Python installations over system Python installations
  * `system`: Prefer system Python installations over managed Python installations
  * `only-system`: Only use system Python installations; never use managed Python installations


`--quiet`, `-q`
    
Do not print any output
`--verbose`, `-v`
    
Use verbose output.
You can configure fine-grained logging using the `RUST_LOG` environment variable. (<https://docs.rs/tracing-subscriber/latest/tracing_subscriber/filter/struct.EnvFilter.html#directives>)
`--version`, `-V`
    
Display the uv version
## uv self
Manage the uv executable
### Usage
```
uv self [OPTIONS] <COMMAND>

```

### Commands
`uv self update`
    
Update uv
### uv self update
Update uv
### Usage
```
uv self update [OPTIONS] [TARGET_VERSION]

```

### Arguments
`TARGET_VERSION`
    
Update to the specified version. If not provided, uv will update to the latest version
### Options
`--allow-insecure-host` _allow-insecure-host_
    
Allow insecure connections to a host.
Can be provided multiple times.
Expects to receive either a hostname (e.g., `localhost`), a host-port pair (e.g., `localhost:8080`), or a URL (e.g., `https://localhost`).
WARNING: Hosts included in this list will not be verified against the system’s certificate store. Only use `--allow-insecure-host` in a secure network with verified sources, as it bypasses SSL verification and could expose you to MITM attacks.
May also be set with the `UV_INSECURE_HOST` environment variable.
`--cache-dir` _cache-dir_
    
Path to the cache directory.
Defaults to `$XDG_CACHE_HOME/uv` or `$HOME/.cache/uv` on macOS and Linux, and `%LOCALAPPDATA%\uv\cache` on Windows.
To view the location of the cache directory, run `uv cache dir`.
May also be set with the `UV_CACHE_DIR` environment variable.
`--color` _color-choice_
    
Control the use of color in output.
By default, uv will automatically detect support for colors when writing to a terminal.
Possible values:
  * `auto`: Enables colored output only when the output is going to a terminal or TTY with support
  * `always`: Enables colored output regardless of the detected environment
  * `never`: Disables colored output


`--config-file` _config-file_
    
The path to a `uv.toml` file to use for configuration.
While uv configuration can be included in a `pyproject.toml` file, it is not allowed in this context.
May also be set with the `UV_CONFIG_FILE` environment variable.
`--directory` _directory_
    
Change to the given directory prior to running the command.
Relative paths are resolved with the given directory as the base.
See `--project` to only change the project root directory.
`--help`, `-h`
    
Display the concise help for this command
`--native-tls`
    
Whether to load TLS certificates from the platform’s native certificate store.
By default, uv loads certificates from the bundled `webpki-roots` crate. The `webpki-roots` are a reliable set of trust roots from Mozilla, and including them in uv improves portability and performance (especially on macOS).
However, in some cases, you may want to use the platform’s native certificate store, especially if you’re relying on a corporate trust root (e.g., for a mandatory proxy) that’s included in your system’s certificate store.
May also be set with the `UV_NATIVE_TLS` environment variable.
`--no-cache`, `-n`
    
Avoid reading from or writing to the cache, instead using a temporary directory for the duration of the operation
May also be set with the `UV_NO_CACHE` environment variable.
`--no-config`
    
Avoid discovering configuration files (`pyproject.toml`, `uv.toml`).
Normally, configuration files are discovered in the current directory, parent directories, or user configuration directories.
May also be set with the `UV_NO_CONFIG` environment variable.
`--no-progress`
    
Hide all progress outputs.
For example, spinners or progress bars.
May also be set with the `UV_NO_PROGRESS` environment variable.
`--no-python-downloads`
    
Disable automatic downloads of Python.
`--offline`
    
Disable network access.
When disabled, uv will only use locally cached data and locally available files.
May also be set with the `UV_OFFLINE` environment variable.
`--project` _project_
    
Run the command within the given project directory.
All `pyproject.toml`, `uv.toml`, and `.python-version` files will be discovered by walking up the directory tree from the project root, as will the project’s virtual environment (`.venv`).
Other command-line arguments (such as relative paths) will be resolved relative to the current working directory.
See `--directory` to change the working directory entirely.
This setting has no effect when used in the `uv pip` interface.
`--python-preference` _python-preference_
    
Whether to prefer uv-managed or system Python installations.
By default, uv prefers using Python versions it manages. However, it will use system Python installations if a uv-managed Python is not installed. This option allows prioritizing or ignoring system Python installations.
May also be set with the `UV_PYTHON_PREFERENCE` environment variable.
Possible values:
  * `only-managed`: Only use managed Python installations; never use system Python installations
  * `managed`: Prefer managed Python installations over system Python installations
  * `system`: Prefer system Python installations over managed Python installations
  * `only-system`: Only use system Python installations; never use managed Python installations


`--quiet`, `-q`
    
Do not print any output
`--token` _token_
    
A GitHub token for authentication. A token is not required but can be used to reduce the chance of encountering rate limits
May also be set with the `UV_GITHUB_TOKEN` environment variable.
`--verbose`, `-v`
    
Use verbose output.
You can configure fine-grained logging using the `RUST_LOG` environment variable. (<https://docs.rs/tracing-subscriber/latest/tracing_subscriber/filter/struct.EnvFilter.html#directives>)
`--version`, `-V`
    
Display the uv version
## uv version
Display uv's version
### Usage
```
uv version [OPTIONS]

```

### Options
`--allow-insecure-host` _allow-insecure-host_
    
Allow insecure connections to a host.
Can be provided multiple times.
Expects to receive either a hostname (e.g., `localhost`), a host-port pair (e.g., `localhost:8080`), or a URL (e.g., `https://localhost`).
WARNING: Hosts included in this list will not be verified against the system’s certificate store. Only use `--allow-insecure-host` in a secure network with verified sources, as it bypasses SSL verification and could expose you to MITM attacks.
May also be set with the `UV_INSECURE_HOST` environment variable.
`--cache-dir` _cache-dir_
    
Path to the cache directory.
Defaults to `$XDG_CACHE_HOME/uv` or `$HOME/.cache/uv` on macOS and Linux, and `%LOCALAPPDATA%\uv\cache` on Windows.
To view the location of the cache directory, run `uv cache dir`.
May also be set with the `UV_CACHE_DIR` environment variable.
`--color` _color-choice_
    
Control the use of color in output.
By default, uv will automatically detect support for colors when writing to a terminal.
Possible values:
  * `auto`: Enables colored output only when the output is going to a terminal or TTY with support
  * `always`: Enables colored output regardless of the detected environment
  * `never`: Disables colored output


`--config-file` _config-file_
    
The path to a `uv.toml` file to use for configuration.
While uv configuration can be included in a `pyproject.toml` file, it is not allowed in this context.
May also be set with the `UV_CONFIG_FILE` environment variable.
`--directory` _directory_
    
Change to the given directory prior to running the command.
Relative paths are resolved with the given directory as the base.
See `--project` to only change the project root directory.
`--help`, `-h`
    
Display the concise help for this command
`--native-tls`
    
Whether to load TLS certificates from the platform’s native certificate store.
By default, uv loads certificates from the bundled `webpki-roots` crate. The `webpki-roots` are a reliable set of trust roots from Mozilla, and including them in uv improves portability and performance (especially on macOS).
However, in some cases, you may want to use the platform’s native certificate store, especially if you’re relying on a corporate trust root (e.g., for a mandatory proxy) that’s included in your system’s certificate store.
May also be set with the `UV_NATIVE_TLS` environment variable.
`--no-cache`, `-n`
    
Avoid reading from or writing to the cache, instead using a temporary directory for the duration of the operation
May also be set with the `UV_NO_CACHE` environment variable.
`--no-config`
    
Avoid discovering configuration files (`pyproject.toml`, `uv.toml`).
Normally, configuration files are discovered in the current directory, parent directories, or user configuration directories.
May also be set with the `UV_NO_CONFIG` environment variable.
`--no-progress`
    
Hide all progress outputs.
For example, spinners or progress bars.
May also be set with the `UV_NO_PROGRESS` environment variable.
`--no-python-downloads`
    
Disable automatic downloads of Python.
`--offline`
    
Disable network access.
When disabled, uv will only use locally cached data and locally available files.
May also be set with the `UV_OFFLINE` environment variable.
`--output-format` _output-format_
`--project` _project_
    
Run the command within the given project directory.
All `pyproject.toml`, `uv.toml`, and `.python-version` files will be discovered by walking up the directory tree from the project root, as will the project’s virtual environment (`.venv`).
Other command-line arguments (such as relative paths) will be resolved relative to the current working directory.
See `--directory` to change the working directory entirely.
This setting has no effect when used in the `uv pip` interface.
`--python-preference` _python-preference_
    
Whether to prefer uv-managed or system Python installations.
By default, uv prefers using Python versions it manages. However, it will use system Python installations if a uv-managed Python is not installed. This option allows prioritizing or ignoring system Python installations.
May also be set with the `UV_PYTHON_PREFERENCE` environment variable.
Possible values:
  * `only-managed`: Only use managed Python installations; never use system Python installations
  * `managed`: Prefer managed Python installations over system Python installations
  * `system`: Prefer system Python installations over managed Python installations
  * `only-system`: Only use system Python installations; never use managed Python installations


`--quiet`, `-q`
    
Do not print any output
`--verbose`, `-v`
    
Use verbose output.
You can configure fine-grained logging using the `RUST_LOG` environment variable. (<https://docs.rs/tracing-subscriber/latest/tracing_subscriber/filter/struct.EnvFilter.html#directives>)
`--version`, `-V`
    
Display the uv version
## uv generate-shell-completion
Generate shell completion
### Usage
```
uv generate-shell-completion [OPTIONS] <SHELL>

```

### Arguments
`SHELL`
    
The shell to generate the completion script for
### Options
`--allow-insecure-host` _allow-insecure-host_
    
Allow insecure connections to a host.
Can be provided multiple times.
Expects to receive either a hostname (e.g., `localhost`), a host-port pair (e.g., `localhost:8080`), or a URL (e.g., `https://localhost`).
WARNING: Hosts included in this list will not be verified against the system’s certificate store. Only use `--allow-insecure-host` in a secure network with verified sources, as it bypasses SSL verification and could expose you to MITM attacks.
May also be set with the `UV_INSECURE_HOST` environment variable.
`--directory` _directory_
    
Change to the given directory prior to running the command.
Relative paths are resolved with the given directory as the base.
See `--project` to only change the project root directory.
`--project` _project_
    
Run the command within the given project directory.
All `pyproject.toml`, `uv.toml`, and `.python-version` files will be discovered by walking up the directory tree from the project root, as will the project’s virtual environment (`.venv`).
Other command-line arguments (such as relative paths) will be resolved relative to the current working directory.
See `--directory` to change the working directory entirely.
This setting has no effect when used in the `uv pip` interface.
## uv help
Display documentation for a command
### Usage
```
uv help [OPTIONS] [COMMAND]...

```

### Arguments
`COMMAND`
### Options
`--allow-insecure-host` _allow-insecure-host_
    
Allow insecure connections to a host.
Can be provided multiple times.
Expects to receive either a hostname (e.g., `localhost`), a host-port pair (e.g., `localhost:8080`), or a URL (e.g., `https://localhost`).
WARNING: Hosts included in this list will not be verified against the system’s certificate store. Only use `--allow-insecure-host` in a secure network with verified sources, as it bypasses SSL verification and could expose you to MITM attacks.
May also be set with the `UV_INSECURE_HOST` environment variable.
`--cache-dir` _cache-dir_
    
Path to the cache directory.
Defaults to `$XDG_CACHE_HOME/uv` or `$HOME/.cache/uv` on macOS and Linux, and `%LOCALAPPDATA%\uv\cache` on Windows.
To view the location of the cache directory, run `uv cache dir`.
May also be set with the `UV_CACHE_DIR` environment variable.
`--color` _color-choice_
    
Control the use of color in output.
By default, uv will automatically detect support for colors when writing to a terminal.
Possible values:
  * `auto`: Enables colored output only when the output is going to a terminal or TTY with support
  * `always`: Enables colored output regardless of the detected environment
  * `never`: Disables colored output


`--config-file` _config-file_
    
The path to a `uv.toml` file to use for configuration.
While uv configuration can be included in a `pyproject.toml` file, it is not allowed in this context.
May also be set with the `UV_CONFIG_FILE` environment variable.
`--directory` _directory_
    
Change to the given directory prior to running the command.
Relative paths are resolved with the given directory as the base.
See `--project` to only change the project root directory.
`--help`, `-h`
    
Display the concise help for this command
`--native-tls`
    
Whether to load TLS certificates from the platform’s native certificate store.
By default, uv loads certificates from the bundled `webpki-roots` crate. The `webpki-roots` are a reliable set of trust roots from Mozilla, and including them in uv improves portability and performance (especially on macOS).
However, in some cases, you may want to use the platform’s native certificate store, especially if you’re relying on a corporate trust root (e.g., for a mandatory proxy) that’s included in your system’s certificate store.
May also be set with the `UV_NATIVE_TLS` environment variable.
`--no-cache`, `-n`
    
Avoid reading from or writing to the cache, instead using a temporary directory for the duration of the operation
May also be set with the `UV_NO_CACHE` environment variable.
`--no-config`
    
Avoid discovering configuration files (`pyproject.toml`, `uv.toml`).
Normally, configuration files are discovered in the current directory, parent directories, or user configuration directories.
May also be set with the `UV_NO_CONFIG` environment variable.
`--no-pager`
    
Disable pager when printing help
`--no-progress`
    
Hide all progress outputs.
For example, spinners or progress bars.
May also be set with the `UV_NO_PROGRESS` environment variable.
`--no-python-downloads`
    
Disable automatic downloads of Python.
`--offline`
    
Disable network access.
When disabled, uv will only use locally cached data and locally available files.
May also be set with the `UV_OFFLINE` environment variable.
`--project` _project_
    
Run the command within the given project directory.
All `pyproject.toml`, `uv.toml`, and `.python-version` files will be discovered by walking up the directory tree from the project root, as will the project’s virtual environment (`.venv`).
Other command-line arguments (such as relative paths) will be resolved relative to the current working directory.
See `--directory` to change the working directory entirely.
This setting has no effect when used in the `uv pip` interface.
`--python-preference` _python-preference_
    
Whether to prefer uv-managed or system Python installations.
By default, uv prefers using Python versions it manages. However, it will use system Python installations if a uv-managed Python is not installed. This option allows prioritizing or ignoring system Python installations.
May also be set with the `UV_PYTHON_PREFERENCE` environment variable.
Possible values:
  * `only-managed`: Only use managed Python installations; never use system Python installations
  * `managed`: Prefer managed Python installations over system Python installations
  * `system`: Prefer system Python installations over managed Python installations
  * `only-system`: Only use system Python installations; never use managed Python installations


`--quiet`, `-q`
    
Do not print any output
`--verbose`, `-v`
    
Use verbose output.
You can configure fine-grained logging using the `RUST_LOG` environment variable. (<https://docs.rs/tracing-subscriber/latest/tracing_subscriber/filter/struct.EnvFilter.html#directives>)
`--version`, `-V`
    
Display the uv version
Back to top 
![](https://cdn.usefathom.com/?h=https%3A%2F%2Fdocs.astral.sh&p=%2Fuv%2Freference%2Fcli%2F&r=&sid=ESKBRHGN&qs=%7B%7D&cid=22997052)
