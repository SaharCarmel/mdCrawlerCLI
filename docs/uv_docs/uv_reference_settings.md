Skip to content 
# Settings
## Project metadata
### `build-constraint-dependencies`
Constraints to apply when solving build dependencies.
Build constraints are used to restrict the versions of build dependencies that are selected when building a package during resolution or installation.
Including a package as a constraint will _not_ trigger installation of the package during a build; instead, the package must be requested elsewhere in the project's build dependency graph.
Note
In `uv lock`, `uv sync`, and `uv run`, uv will only read `build-constraint-dependencies` from the `pyproject.toml` at the workspace root, and will ignore any declarations in other workspace members or `uv.toml` files.
**Default value** : `[]`
**Type** : `list[str]`
**Example usage** :
pyproject.toml```
[tool.uv]
# Ensure that the setuptools v60.0.0 is used whenever a package has a build dependency
# on setuptools.
build-constraint-dependencies=["setuptools==60.0.0"]

```

### `conflicts`
Declare collections of extras or dependency groups that are conflicting (i.e., mutually exclusive).
It's useful to declare conflicts when two or more extras have mutually incompatible dependencies. For example, extra `foo` might depend on `numpy==2.0.0` while extra `bar` depends on `numpy==2.1.0`. While these dependencies conflict, it may be the case that users are not expected to activate both `foo` and `bar` at the same time, making it possible to generate a universal resolution for the project despite the incompatibility.
By making such conflicts explicit, uv can generate a universal resolution for a project, taking into account that certain combinations of extras and groups are mutually exclusive. In exchange, installation will fail if a user attempts to activate both conflicting extras.
**Default value** : `[]`
**Type** : `list[list[dict]]`
**Example usage** :
pyproject.toml```
[tool.uv]
# Require that `package[extra1]` and `package[extra2]` are resolved
# in different forks so that they cannot conflict with one another.
conflicts=[
[
{extra="extra1"},
{extra="extra2"},
]
]

# Require that the dependency groups `group1` and `group2`
# are resolved in different forks so that they cannot conflict
# with one another.
conflicts=[
[
{group="group1"},
{group="group2"},
]
]

```

### `constraint-dependencies`
Constraints to apply when resolving the project's dependencies.
Constraints are used to restrict the versions of dependencies that are selected during resolution.
Including a package as a constraint will _not_ trigger installation of the package on its own; instead, the package must be requested elsewhere in the project's first-party or transitive dependencies.
Note
In `uv lock`, `uv sync`, and `uv run`, uv will only read `constraint-dependencies` from the `pyproject.toml` at the workspace root, and will ignore any declarations in other workspace members or `uv.toml` files.
**Default value** : `[]`
**Type** : `list[str]`
**Example usage** :
pyproject.toml```
[tool.uv]
# Ensure that the grpcio version is always less than 1.65, if it's requested by a
# direct or transitive dependency.
constraint-dependencies=["grpcio<1.65"]

```

### `default-groups`
The list of `dependency-groups` to install by default.
**Default value** : `["dev"]`
**Type** : `list[str]`
**Example usage** :
pyproject.toml```
[tool.uv]
default-groups=["docs"]

```

### `dev-dependencies`
The project's development dependencies.
Development dependencies will be installed by default in `uv run` and `uv sync`, but will not appear in the project's published metadata.
Use of this field is not recommend anymore. Instead, use the `dependency-groups.dev` field which is a standardized way to declare development dependencies. The contents of `tool.uv.dev-dependencies` and `dependency-groups.dev` are combined to determine the final requirements of the `dev` dependency group.
**Default value** : `[]`
**Type** : `list[str]`
**Example usage** :
pyproject.toml```
[tool.uv]
dev-dependencies=["ruff==0.5.0"]

```

### `environments`
A list of supported environments against which to resolve dependencies.
By default, uv will resolve for all possible environments during a `uv lock` operation. However, you can restrict the set of supported environments to improve performance and avoid unsatisfiable branches in the solution space.
These environments will also be respected when `uv pip compile` is invoked with the `--universal` flag.
**Default value** : `[]`
**Type** : `str | list[str]`
**Example usage** :
pyproject.toml```
[tool.uv]
# Resolve for macOS, but not for Linux or Windows.
environments=["sys_platform == 'darwin'"]

```

### `index`
The indexes to use when resolving dependencies.
Accepts either a repository compliant with PEP 503 (the simple repository API), or a local directory laid out in the same format.
Indexes are considered in the order in which they're defined, such that the first-defined index has the highest priority. Further, the indexes provided by this setting are given higher priority than any indexes specified via `index_url` or `extra_index_url`. uv will only consider the first index that contains a given package, unless an alternative index strategy is specified.
If an index is marked as `explicit = true`, it will be used exclusively for the dependencies that select it explicitly via `[tool.uv.sources]`, as in:
```
[[tool.uv.index]]
name="pytorch"
url="https://download.pytorch.org/whl/cu121"
explicit=true

[tool.uv.sources]
torch={index="pytorch"}

```

If an index is marked as `default = true`, it will be moved to the end of the prioritized list, such that it is given the lowest priority when resolving packages. Additionally, marking an index as default will disable the PyPI default index.
**Default value** : `[]`
**Type** : `dict`
**Example usage** :
pyproject.toml```
[[tool.uv.index]]
name="pytorch"
url="https://download.pytorch.org/whl/cu121"

```

### `managed`
Whether the project is managed by uv. If `false`, uv will ignore the project when `uv run` is invoked.
**Default value** : `true`
**Type** : `bool`
**Example usage** :
pyproject.toml```
[tool.uv]
managed=false

```

### `override-dependencies`
Overrides to apply when resolving the project's dependencies.
Overrides are used to force selection of a specific version of a package, regardless of the version requested by any other package, and regardless of whether choosing that version would typically constitute an invalid resolution.
While constraints are _additive_ , in that they're combined with the requirements of the constituent packages, overrides are _absolute_ , in that they completely replace the requirements of any constituent packages.
Including a package as an override will _not_ trigger installation of the package on its own; instead, the package must be requested elsewhere in the project's first-party or transitive dependencies.
Note
In `uv lock`, `uv sync`, and `uv run`, uv will only read `override-dependencies` from the `pyproject.toml` at the workspace root, and will ignore any declarations in other workspace members or `uv.toml` files.
**Default value** : `[]`
**Type** : `list[str]`
**Example usage** :
pyproject.toml```
[tool.uv]
# Always install Werkzeug 2.3.0, regardless of whether transitive dependencies request
# a different version.
override-dependencies=["werkzeug==2.3.0"]

```

### `package`
Whether the project should be considered a Python package, or a non-package ("virtual") project.
Packages are built and installed into the virtual environment in editable mode and thus require a build backend, while virtual projects are _not_ built or installed; instead, only their dependencies are included in the virtual environment.
Creating a package requires that a `build-system` is present in the `pyproject.toml`, and that the project adheres to a structure that adheres to the build backend's expectations (e.g., a `src` layout).
**Default value** : `true`
**Type** : `bool`
**Example usage** :
pyproject.toml```
[tool.uv]
package=false

```

### `required-environments`
A list of required platforms, for packages that lack source distributions.
When a package does not have a source distribution, it's availability will be limited to the platforms supported by its built distributions (wheels). For example, if a package only publishes wheels for Linux, then it won't be installable on macOS or Windows.
By default, uv requires each package to include at least one wheel that is compatible with the designated Python version. The `required-environments` setting can be used to ensure that the resulting resolution contains wheels for specific platforms, or fails if no such wheels are available.
While the `environments` setting _limits_ the set of environments that uv will consider when resolving dependencies, `required-environments` _expands_ the set of platforms that uv _must_ support when resolving dependencies.
For example, `environments = ["sys_platform == 'darwin'"]` would limit uv to solving for macOS (and ignoring Linux and Windows). On the other hand, `required-environments = ["sys_platform == 'darwin'"]` would _require_ that any package without a source distribution include a wheel for macOS in order to be installable.
**Default value** : `[]`
**Type** : `str | list[str]`
**Example usage** :
pyproject.toml```
[tool.uv]
# Require that the package is available for macOS ARM and x86 (Intel).
required-environments=[
"sys_platform == 'darwin' and platform_machine == 'arm64'",
"sys_platform == 'darwin' and platform_machine == 'x86_64'",
]

```

### `sources`
The sources to use when resolving dependencies.
`tool.uv.sources` enriches the dependency metadata with additional sources, incorporated during development. A dependency source can be a Git repository, a URL, a local path, or an alternative registry.
See Dependencies for more.
**Default value** : `{}`
**Type** : `dict`
**Example usage** :
pyproject.toml```
[tool.uv.sources]
httpx={git="https://github.com/encode/httpx",tag="0.27.0"}
pytest={url="https://files.pythonhosted.org/packages/6b/77/7440a06a8ead44c7757a64362dd22df5760f9b12dc5f11b6188cd2fc27a0/pytest-8.3.3-py3-none-any.whl"}
pydantic={path="/path/to/pydantic",editable=true}

```

### `workspace`
#### `exclude`
Packages to exclude as workspace members. If a package matches both `members` and `exclude`, it will be excluded.
Supports both globs and explicit paths.
For more information on the glob syntax, refer to the `glob` documentation.
**Default value** : `[]`
**Type** : `list[str]`
**Example usage** :
pyproject.toml```
[tool.uv.workspace]
exclude=["member1","path/to/member2","libs/*"]

```

#### `members`
Packages to include as workspace members.
Supports both globs and explicit paths.
For more information on the glob syntax, refer to the `glob` documentation.
**Default value** : `[]`
**Type** : `list[str]`
**Example usage** :
pyproject.toml```
[tool.uv.workspace]
members=["member1","path/to/member2","libs/*"]

```

## Configuration
### `allow-insecure-host`
Allow insecure connections to host.
Expects to receive either a hostname (e.g., `localhost`), a host-port pair (e.g., `localhost:8080`), or a URL (e.g., `https://localhost`).
WARNING: Hosts included in this list will not be verified against the system's certificate store. Only use `--allow-insecure-host` in a secure network with verified sources, as it bypasses SSL verification and could expose you to MITM attacks.
**Default value** : `[]`
**Type** : `list[str]`
**Example usage** :
pyproject.tomluv.toml
```
[tool.uv]
allow-insecure-host=["localhost:8080"]

```

```
allow-insecure-host=["localhost:8080"]

```

### `cache-dir`
Path to the cache directory.
Defaults to `$HOME/Library/Caches/uv` on macOS, `$XDG_CACHE_HOME/uv` or `$HOME/.cache/uv` on Linux, and `%LOCALAPPDATA%\uv\cache` on Windows.
**Default value** : `None`
**Type** : `str`
**Example usage** :
pyproject.tomluv.toml
```
[tool.uv]
cache-dir="./.uv_cache"

```

```
cache-dir="./.uv_cache"

```

### `cache-keys`
The keys to consider when caching builds for the project.
Cache keys enable you to specify the files or directories that should trigger a rebuild when modified. By default, uv will rebuild a project whenever the `pyproject.toml`, `setup.py`, or `setup.cfg` files in the project directory are modified, i.e.:
```
cache-keys=[{file="pyproject.toml"},{file="setup.py"},{file="setup.cfg"}]

```

As an example: if a project uses dynamic metadata to read its dependencies from a `requirements.txt` file, you can specify `cache-keys = [{ file = "requirements.txt" }, { file = "pyproject.toml" }]` to ensure that the project is rebuilt whenever the `requirements.txt` file is modified (in addition to watching the `pyproject.toml`).
Globs are supported, following the syntax of the `glob` crate. For example, to invalidate the cache whenever a `.toml` file in the project directory or any of its subdirectories is modified, you can specify `cache-keys = [{ file = "**/*.toml" }]`. Note that the use of globs can be expensive, as uv may need to walk the filesystem to determine whether any files have changed.
Cache keys can also include version control information. For example, if a project uses `setuptools_scm` to read its version from a Git commit, you can specify `cache-keys = [{ git = { commit = true }, { file = "pyproject.toml" }]` to include the current Git commit hash in the cache key (in addition to the `pyproject.toml`). Git tags are also supported via `cache-keys = [{ git = { commit = true, tags = true } }]`.
Cache keys can also include environment variables. For example, if a project relies on `MACOSX_DEPLOYMENT_TARGET` or other environment variables to determine its behavior, you can specify `cache-keys = [{ env = "MACOSX_DEPLOYMENT_TARGET" }]` to invalidate the cache whenever the environment variable changes.
Cache keys only affect the project defined by the `pyproject.toml` in which they're specified (as opposed to, e.g., affecting all members in a workspace), and all paths and globs are interpreted as relative to the project directory.
**Default value** : `[{ file = "pyproject.toml" }, { file = "setup.py" }, { file = "setup.cfg" }]`
**Type** : `list[dict]`
**Example usage** :
pyproject.tomluv.toml
```
[tool.uv]
cache-keys=[{file="pyproject.toml"},{file="requirements.txt"},{git={commit=true}]

```

```
cache-keys=[{file="pyproject.toml"},{file="requirements.txt"},{git={commit=true}]

```

### `check-url`
Check an index URL for existing files to skip duplicate uploads.
This option allows retrying publishing that failed after only some, but not all files have been uploaded, and handles error due to parallel uploads of the same file.
Before uploading, the index is checked. If the exact same file already exists in the index, the file will not be uploaded. If an error occurred during the upload, the index is checked again, to handle cases where the identical file was uploaded twice in parallel.
The exact behavior will vary based on the index. When uploading to PyPI, uploading the same file succeeds even without `--check-url`, while most other indexes error.
The index must provide one of the supported hashes (SHA-256, SHA-384, or SHA-512).
**Default value** : `None`
**Type** : `str`
**Example usage** :
pyproject.tomluv.toml
```
[tool.uv]
check-url="https://test.pypi.org/simple"

```

```
check-url="https://test.pypi.org/simple"

```

### `compile-bytecode`
Compile Python files to bytecode after installation.
By default, uv does not compile Python (`.py`) files to bytecode (`__pycache__/*.pyc`); instead, compilation is performed lazily the first time a module is imported. For use-cases in which start time is critical, such as CLI applications and Docker containers, this option can be enabled to trade longer installation times for faster start times.
When enabled, uv will process the entire site-packages directory (including packages that are not being modified by the current operation) for consistency. Like pip, it will also ignore errors.
**Default value** : `false`
**Type** : `bool`
**Example usage** :
pyproject.tomluv.toml
```
[tool.uv]
compile-bytecode=true

```

```
compile-bytecode=true

```

### `concurrent-builds`
The maximum number of source distributions that uv will build concurrently at any given time.
Defaults to the number of available CPU cores.
**Default value** : `None`
**Type** : `int`
**Example usage** :
pyproject.tomluv.toml
```
[tool.uv]
concurrent-builds=4

```

```
concurrent-builds=4

```

### `concurrent-downloads`
The maximum number of in-flight concurrent downloads that uv will perform at any given time.
**Default value** : `50`
**Type** : `int`
**Example usage** :
pyproject.tomluv.toml
```
[tool.uv]
concurrent-downloads=4

```

```
concurrent-downloads=4

```

### `concurrent-installs`
The number of threads used when installing and unzipping packages.
Defaults to the number of available CPU cores.
**Default value** : `None`
**Type** : `int`
**Example usage** :
pyproject.tomluv.toml
```
[tool.uv]
concurrent-installs=4

```

```
concurrent-installs=4

```

### `config-settings`
Settings to pass to the PEP 517 build backend, specified as `KEY=VALUE` pairs.
**Default value** : `{}`
**Type** : `dict`
**Example usage** :
pyproject.tomluv.toml
```
[tool.uv]
config-settings={editable_mode="compat"}

```

```
config-settings={editable_mode="compat"}

```

### `dependency-metadata`
Pre-defined static metadata for dependencies of the project (direct or transitive). When provided, enables the resolver to use the specified metadata instead of querying the registry or building the relevant package from source.
Metadata should be provided in adherence with the Metadata 2.3 standard, though only the following fields are respected:
  * `name`: The name of the package.
  * (Optional) `version`: The version of the package. If omitted, the metadata will be applied to all versions of the package.
  * (Optional) `requires-dist`: The dependencies of the package (e.g., `werkzeug>=0.14`).
  * (Optional) `requires-python`: The Python version required by the package (e.g., `>=3.10`).
  * (Optional) `provides-extras`: The extras provided by the package.


**Default value** : `[]`
**Type** : `list[dict]`
**Example usage** :
pyproject.tomluv.toml
```
[tool.uv]
dependency-metadata=[
{name="flask",version="1.0.0",requires-dist=["werkzeug"],requires-python=">=3.6"},
]

```

```
dependency-metadata=[
{name="flask",version="1.0.0",requires-dist=["werkzeug"],requires-python=">=3.6"},
]

```

### `exclude-newer`
Limit candidate packages to those that were uploaded prior to the given date.
Accepts both RFC 3339 timestamps (e.g., `2006-12-02T02:07:43Z`) and local dates in the same format (e.g., `2006-12-02`) in your system's configured time zone.
**Default value** : `None`
**Type** : `str`
**Example usage** :
pyproject.tomluv.toml
```
[tool.uv]
exclude-newer="2006-12-02"

```

```
exclude-newer="2006-12-02"

```

### `extra-index-url`
Extra URLs of package indexes to use, in addition to `--index-url`.
Accepts either a repository compliant with PEP 503 (the simple repository API), or a local directory laid out in the same format.
All indexes provided via this flag take priority over the index specified by `index_url` or `index` with `default = true`. When multiple indexes are provided, earlier values take priority.
To control uv's resolution strategy when multiple indexes are present, see `index_strategy`.
(Deprecated: use `index` instead.)
**Default value** : `[]`
**Type** : `list[str]`
**Example usage** :
pyproject.tomluv.toml
```
[tool.uv]
extra-index-url=["https://download.pytorch.org/whl/cpu"]

```

```
extra-index-url=["https://download.pytorch.org/whl/cpu"]

```

### `find-links`
Locations to search for candidate distributions, in addition to those found in the registry indexes.
If a path, the target must be a directory that contains packages as wheel files (`.whl`) or source distributions (e.g., `.tar.gz` or `.zip`) at the top level.
If a URL, the page must contain a flat list of links to package files adhering to the formats described above.
**Default value** : `[]`
**Type** : `list[str]`
**Example usage** :
pyproject.tomluv.toml
```
[tool.uv]
find-links=["https://download.pytorch.org/whl/torch_stable.html"]

```

```
find-links=["https://download.pytorch.org/whl/torch_stable.html"]

```

### `fork-strategy`
The strategy to use when selecting multiple versions of a given package across Python versions and platforms.
By default, uv will optimize for selecting the latest version of each package for each supported Python version (`requires-python`), while minimizing the number of selected versions across platforms.
Under `fewest`, uv will minimize the number of selected versions for each package, preferring older versions that are compatible with a wider range of supported Python versions or platforms.
**Default value** : `"requires-python"`
**Possible values** :
  * `"fewest"`: Optimize for selecting the fewest number of versions for each package. Older versions may be preferred if they are compatible with a wider range of supported Python versions or platforms
  * `"requires-python"`: Optimize for selecting latest supported version of each package, for each supported Python version


**Example usage** :
pyproject.tomluv.toml
```
[tool.uv]
fork-strategy="fewest"

```

```
fork-strategy="fewest"

```

### `index`
The package indexes to use when resolving dependencies.
Accepts either a repository compliant with PEP 503 (the simple repository API), or a local directory laid out in the same format.
Indexes are considered in the order in which they're defined, such that the first-defined index has the highest priority. Further, the indexes provided by this setting are given higher priority than any indexes specified via `index_url` or `extra_index_url`. uv will only consider the first index that contains a given package, unless an alternative index strategy is specified.
If an index is marked as `explicit = true`, it will be used exclusively for those dependencies that select it explicitly via `[tool.uv.sources]`, as in:
```
[[tool.uv.index]]
name="pytorch"
url="https://download.pytorch.org/whl/cu121"
explicit=true

[tool.uv.sources]
torch={index="pytorch"}

```

If an index is marked as `default = true`, it will be moved to the end of the prioritized list, such that it is given the lowest priority when resolving packages. Additionally, marking an index as default will disable the PyPI default index.
**Default value** : `"[]"`
**Type** : `dict`
**Example usage** :
pyproject.tomluv.toml
```
[[tool.uv.index]]
name="pytorch"
url="https://download.pytorch.org/whl/cu121"

```

```
[[tool.uv.index]]
name="pytorch"
url="https://download.pytorch.org/whl/cu121"

```

### `index-strategy`
The strategy to use when resolving against multiple index URLs.
By default, uv will stop at the first index on which a given package is available, and limit resolutions to those present on that first index (`first-index`). This prevents "dependency confusion" attacks, whereby an attacker can upload a malicious package under the same name to an alternate index.
**Default value** : `"first-index"`
**Possible values** :
  * `"first-index"`: Only use results from the first index that returns a match for a given package name
  * `"unsafe-first-match"`: Search for every package name across all indexes, exhausting the versions from the first index before moving on to the next
  * `"unsafe-best-match"`: Search for every package name across all indexes, preferring the "best" version found. If a package version is in multiple indexes, only look at the entry for the first index


**Example usage** :
pyproject.tomluv.toml
```
[tool.uv]
index-strategy="unsafe-best-match"

```

```
index-strategy="unsafe-best-match"

```

### `index-url`
The URL of the Python package index (by default: https://pypi.org/simple).
Accepts either a repository compliant with PEP 503 (the simple repository API), or a local directory laid out in the same format.
The index provided by this setting is given lower priority than any indexes specified via `extra_index_url` or `index`.
(Deprecated: use `index` instead.)
**Default value** : `"https://pypi.org/simple"`
**Type** : `str`
**Example usage** :
pyproject.tomluv.toml
```
[tool.uv]
index-url="https://test.pypi.org/simple"

```

```
index-url="https://test.pypi.org/simple"

```

### `keyring-provider`
Attempt to use `keyring` for authentication for index URLs.
At present, only `--keyring-provider subprocess` is supported, which configures uv to use the `keyring` CLI to handle authentication.
**Default value** : `"disabled"`
**Type** : `str`
**Example usage** :
pyproject.tomluv.toml
```
[tool.uv]
keyring-provider="subprocess"

```

```
keyring-provider="subprocess"

```

### `link-mode`
The method to use when installing packages from the global cache.
Defaults to `clone` (also known as Copy-on-Write) on macOS, and `hardlink` on Linux and Windows.
**Default value** : `"clone" (macOS) or "hardlink" (Linux, Windows)`
**Possible values** :
  * `"clone"`: Clone (i.e., copy-on-write) packages from the wheel into the `site-packages` directory
  * `"copy"`: Copy packages from the wheel into the `site-packages` directory
  * `"hardlink"`: Hard link packages from the wheel into the `site-packages` directory
  * `"symlink"`: Symbolically link packages from the wheel into the `site-packages` directory


**Example usage** :
pyproject.tomluv.toml
```
[tool.uv]
link-mode="copy"

```

```
link-mode="copy"

```

### `native-tls`
Whether to load TLS certificates from the platform's native certificate store.
By default, uv loads certificates from the bundled `webpki-roots` crate. The `webpki-roots` are a reliable set of trust roots from Mozilla, and including them in uv improves portability and performance (especially on macOS).
However, in some cases, you may want to use the platform's native certificate store, especially if you're relying on a corporate trust root (e.g., for a mandatory proxy) that's included in your system's certificate store.
**Default value** : `false`
**Type** : `bool`
**Example usage** :
pyproject.tomluv.toml
```
[tool.uv]
native-tls=true

```

```
native-tls=true

```

### `no-binary`
Don't install pre-built wheels.
The given packages will be built and installed from source. The resolver will still use pre-built wheels to extract package metadata, if available.
**Default value** : `false`
**Type** : `bool`
**Example usage** :
pyproject.tomluv.toml
```
[tool.uv]
no-binary=true

```

```
no-binary=true

```

### `no-binary-package`
Don't install pre-built wheels for a specific package.
**Default value** : `[]`
**Type** : `list[str]`
**Example usage** :
pyproject.tomluv.toml
```
[tool.uv]
no-binary-package=["ruff"]

```

```
no-binary-package=["ruff"]

```

### `no-build`
Don't build source distributions.
When enabled, resolving will not run arbitrary Python code. The cached wheels of already-built source distributions will be reused, but operations that require building distributions will exit with an error.
**Default value** : `false`
**Type** : `bool`
**Example usage** :
pyproject.tomluv.toml
```
[tool.uv]
no-build=true

```

```
no-build=true

```

### `no-build-isolation`
Disable isolation when building source distributions.
Assumes that build dependencies specified by PEP 518 are already installed.
**Default value** : `false`
**Type** : `bool`
**Example usage** :
pyproject.tomluv.toml
```
[tool.uv]
no-build-isolation=true

```

```
no-build-isolation=true

```

### `no-build-isolation-package`
Disable isolation when building source distributions for a specific package.
Assumes that the packages' build dependencies specified by PEP 518 are already installed.
**Default value** : `[]`
**Type** : `list[str]`
**Example usage** :
pyproject.tomluv.toml
```
[tool.uv]
no-build-isolation-package=["package1","package2"]

```

```
no-build-isolation-package=["package1","package2"]

```

### `no-build-package`
Don't build source distributions for a specific package.
**Default value** : `[]`
**Type** : `list[str]`
**Example usage** :
pyproject.tomluv.toml
```
[tool.uv]
no-build-package=["ruff"]

```

```
no-build-package=["ruff"]

```

### `no-cache`
Avoid reading from or writing to the cache, instead using a temporary directory for the duration of the operation.
**Default value** : `false`
**Type** : `bool`
**Example usage** :
pyproject.tomluv.toml
```
[tool.uv]
no-cache=true

```

```
no-cache=true

```

### `no-index`
Ignore all registry indexes (e.g., PyPI), instead relying on direct URL dependencies and those provided via `--find-links`.
**Default value** : `false`
**Type** : `bool`
**Example usage** :
pyproject.tomluv.toml
```
[tool.uv]
no-index=true

```

```
no-index=true

```

### `no-sources`
Ignore the `tool.uv.sources` table when resolving dependencies. Used to lock against the standards-compliant, publishable package metadata, as opposed to using any local or Git sources.
**Default value** : `false`
**Type** : `bool`
**Example usage** :
pyproject.tomluv.toml
```
[tool.uv]
no-sources=true

```

```
no-sources=true

```

### `offline`
Disable network access, relying only on locally cached data and locally available files.
**Default value** : `false`
**Type** : `bool`
**Example usage** :
pyproject.tomluv.toml
```
[tool.uv]
offline=true

```

```
offline=true

```

### `prerelease`
The strategy to use when considering pre-release versions.
By default, uv will accept pre-releases for packages that _only_ publish pre-releases, along with first-party requirements that contain an explicit pre-release marker in the declared specifiers (`if-necessary-or-explicit`).
**Default value** : `"if-necessary-or-explicit"`
**Possible values** :
  * `"disallow"`: Disallow all pre-release versions
  * `"allow"`: Allow all pre-release versions
  * `"if-necessary"`: Allow pre-release versions if all versions of a package are pre-release
  * `"explicit"`: Allow pre-release versions for first-party packages with explicit pre-release markers in their version requirements
  * `"if-necessary-or-explicit"`: Allow pre-release versions if all versions of a package are pre-release, or if the package has an explicit pre-release marker in its version requirements


**Example usage** :
pyproject.tomluv.toml
```
[tool.uv]
prerelease="allow"

```

```
prerelease="allow"

```

### `preview`
Whether to enable experimental, preview features.
**Default value** : `false`
**Type** : `bool`
**Example usage** :
pyproject.tomluv.toml
```
[tool.uv]
preview=true

```

```
preview=true

```

### `publish-url`
The URL for publishing packages to the Python package index (by default: https://upload.pypi.org/legacy/).
**Default value** : `"https://upload.pypi.org/legacy/"`
**Type** : `str`
**Example usage** :
pyproject.tomluv.toml
```
[tool.uv]
publish-url="https://test.pypi.org/legacy/"

```

```
publish-url="https://test.pypi.org/legacy/"

```

### `pypy-install-mirror`
Mirror URL to use for downloading managed PyPy installations.
By default, managed PyPy installations are downloaded from downloads.python.org. This variable can be set to a mirror URL to use a different source for PyPy installations. The provided URL will replace `https://downloads.python.org/pypy` in, e.g., `https://downloads.python.org/pypy/pypy3.8-v7.3.7-osx64.tar.bz2`.
Distributions can be read from a local directory by using the `file://` URL scheme.
**Default value** : `None`
**Type** : `str`
**Example usage** :
pyproject.tomluv.toml
```
[tool.uv]
pypy-install-mirror="https://downloads.python.org/pypy"

```

```
pypy-install-mirror="https://downloads.python.org/pypy"

```

### `python-downloads`
Whether to allow Python downloads.
**Default value** : `"automatic"`
**Possible values** :
  * `"automatic"`: Automatically download managed Python installations when needed
  * `"manual"`: Do not automatically download managed Python installations; require explicit installation
  * `"never"`: Do not ever allow Python downloads


**Example usage** :
pyproject.tomluv.toml
```
[tool.uv]
python-downloads="manual"

```

```
python-downloads="manual"

```

### `python-install-mirror`
Mirror URL for downloading managed Python installations.
By default, managed Python installations are downloaded from `python-build-standalone`. This variable can be set to a mirror URL to use a different source for Python installations. The provided URL will replace `https://github.com/astral-sh/python-build-standalone/releases/download` in, e.g., `https://github.com/astral-sh/python-build-standalone/releases/download/20240713/cpython-3.12.4%2B20240713-aarch64-apple-darwin-install_only.tar.gz`.
Distributions can be read from a local directory by using the `file://` URL scheme.
**Default value** : `None`
**Type** : `str`
**Example usage** :
pyproject.tomluv.toml
```
[tool.uv]
python-install-mirror="https://github.com/astral-sh/python-build-standalone/releases/download"

```

```
python-install-mirror="https://github.com/astral-sh/python-build-standalone/releases/download"

```

### `python-preference`
Whether to prefer using Python installations that are already present on the system, or those that are downloaded and installed by uv.
**Default value** : `"managed"`
**Possible values** :
  * `"only-managed"`: Only use managed Python installations; never use system Python installations
  * `"managed"`: Prefer managed Python installations over system Python installations
  * `"system"`: Prefer system Python installations over managed Python installations
  * `"only-system"`: Only use system Python installations; never use managed Python installations


**Example usage** :
pyproject.tomluv.toml
```
[tool.uv]
python-preference="managed"

```

```
python-preference="managed"

```

### `reinstall`
Reinstall all packages, regardless of whether they're already installed. Implies `refresh`.
**Default value** : `false`
**Type** : `bool`
**Example usage** :
pyproject.tomluv.toml
```
[tool.uv]
reinstall=true

```

```
reinstall=true

```

### `reinstall-package`
Reinstall a specific package, regardless of whether it's already installed. Implies `refresh-package`.
**Default value** : `[]`
**Type** : `list[str]`
**Example usage** :
pyproject.tomluv.toml
```
[tool.uv]
reinstall-package=["ruff"]

```

```
reinstall-package=["ruff"]

```

### `required-version`
Enforce a requirement on the version of uv.
If the version of uv does not meet the requirement at runtime, uv will exit with an error.
Accepts a PEP 440 specifier, like `==0.5.0` or `>=0.5.0`.
**Default value** : `null`
**Type** : `str`
**Example usage** :
pyproject.tomluv.toml
```
[tool.uv]
required-version=">=0.5.0"

```

```
required-version=">=0.5.0"

```

### `resolution`
The strategy to use when selecting between the different compatible versions for a given package requirement.
By default, uv will use the latest compatible version of each package (`highest`).
**Default value** : `"highest"`
**Possible values** :
  * `"highest"`: Resolve the highest compatible version of each package
  * `"lowest"`: Resolve the lowest compatible version of each package
  * `"lowest-direct"`: Resolve the lowest compatible version of any direct dependencies, and the highest compatible version of any transitive dependencies


**Example usage** :
pyproject.tomluv.toml
```
[tool.uv]
resolution="lowest-direct"

```

```
resolution="lowest-direct"

```

### `trusted-publishing`
Configure trusted publishing via GitHub Actions.
By default, uv checks for trusted publishing when running in GitHub Actions, but ignores it if it isn't configured or the workflow doesn't have enough permissions (e.g., a pull request from a fork).
**Default value** : `automatic`
**Type** : `str`
**Example usage** :
pyproject.tomluv.toml
```
[tool.uv]
trusted-publishing="always"

```

```
trusted-publishing="always"

```

### `upgrade`
Allow package upgrades, ignoring pinned versions in any existing output file.
**Default value** : `false`
**Type** : `bool`
**Example usage** :
pyproject.tomluv.toml
```
[tool.uv]
upgrade=true

```

```
upgrade=true

```

### `upgrade-package`
Allow upgrades for a specific package, ignoring pinned versions in any existing output file.
Accepts both standalone package names (`ruff`) and version specifiers (`ruff<0.5.0`).
**Default value** : `[]`
**Type** : `list[str]`
**Example usage** :
pyproject.tomluv.toml
```
[tool.uv]
upgrade-package=["ruff"]

```

```
upgrade-package=["ruff"]

```

### `pip`
Settings that are specific to the `uv pip` command-line interface.
These values will be ignored when running commands outside the `uv pip` namespace (e.g., `uv lock`, `uvx`).
#### `all-extras`
Include all optional dependencies.
Only applies to `pyproject.toml`, `setup.py`, and `setup.cfg` sources.
**Default value** : `false`
**Type** : `bool`
**Example usage** :
pyproject.tomluv.toml
```
[tool.uv.pip]
all-extras=true

```

```
[pip]
all-extras=true

```

#### `allow-empty-requirements`
Allow `uv pip sync` with empty requirements, which will clear the environment of all packages.
**Default value** : `false`
**Type** : `bool`
**Example usage** :
pyproject.tomluv.toml
```
[tool.uv.pip]
allow-empty-requirements=true

```

```
[pip]
allow-empty-requirements=true

```

#### `annotation-style`
The style of the annotation comments included in the output file, used to indicate the source of each package.
**Default value** : `"split"`
**Possible values** :
  * `"line"`: Render the annotations on a single, comma-separated line
  * `"split"`: Render each annotation on its own line


**Example usage** :
pyproject.tomluv.toml
```
[tool.uv.pip]
annotation-style="line"

```

```
[pip]
annotation-style="line"

```

#### `break-system-packages`
Allow uv to modify an `EXTERNALLY-MANAGED` Python installation.
WARNING: `--break-system-packages` is intended for use in continuous integration (CI) environments, when installing into Python installations that are managed by an external package manager, like `apt`. It should be used with caution, as such Python installations explicitly recommend against modifications by other package managers (like uv or pip).
**Default value** : `false`
**Type** : `bool`
**Example usage** :
pyproject.tomluv.toml
```
[tool.uv.pip]
break-system-packages=true

```

```
[pip]
break-system-packages=true

```

#### `compile-bytecode`
Compile Python files to bytecode after installation.
By default, uv does not compile Python (`.py`) files to bytecode (`__pycache__/*.pyc`); instead, compilation is performed lazily the first time a module is imported. For use-cases in which start time is critical, such as CLI applications and Docker containers, this option can be enabled to trade longer installation times for faster start times.
When enabled, uv will process the entire site-packages directory (including packages that are not being modified by the current operation) for consistency. Like pip, it will also ignore errors.
**Default value** : `false`
**Type** : `bool`
**Example usage** :
pyproject.tomluv.toml
```
[tool.uv.pip]
compile-bytecode=true

```

```
[pip]
compile-bytecode=true

```

#### `config-settings`
Settings to pass to the PEP 517 build backend, specified as `KEY=VALUE` pairs.
**Default value** : `{}`
**Type** : `dict`
**Example usage** :
pyproject.tomluv.toml
```
[tool.uv.pip]
config-settings={editable_mode="compat"}

```

```
[pip]
config-settings={editable_mode="compat"}

```

#### `custom-compile-command`
The header comment to include at the top of the output file generated by `uv pip compile`.
Used to reflect custom build scripts and commands that wrap `uv pip compile`.
**Default value** : `None`
**Type** : `str`
**Example usage** :
pyproject.tomluv.toml
```
[tool.uv.pip]
custom-compile-command="./custom-uv-compile.sh"

```

```
[pip]
custom-compile-command="./custom-uv-compile.sh"

```

#### `dependency-metadata`
Pre-defined static metadata for dependencies of the project (direct or transitive). When provided, enables the resolver to use the specified metadata instead of querying the registry or building the relevant package from source.
Metadata should be provided in adherence with the Metadata 2.3 standard, though only the following fields are respected:
  * `name`: The name of the package.
  * (Optional) `version`: The version of the package. If omitted, the metadata will be applied to all versions of the package.
  * (Optional) `requires-dist`: The dependencies of the package (e.g., `werkzeug>=0.14`).
  * (Optional) `requires-python`: The Python version required by the package (e.g., `>=3.10`).
  * (Optional) `provides-extras`: The extras provided by the package.


**Default value** : `[]`
**Type** : `list[dict]`
**Example usage** :
pyproject.tomluv.toml
```
[tool.uv.pip]
dependency-metadata=[
{name="flask",version="1.0.0",requires-dist=["werkzeug"],requires-python=">=3.6"},
]

```

```
[pip]
dependency-metadata=[
{name="flask",version="1.0.0",requires-dist=["werkzeug"],requires-python=">=3.6"},
]

```

#### `emit-build-options`
Include `--no-binary` and `--only-binary` entries in the output file generated by `uv pip compile`.
**Default value** : `false`
**Type** : `bool`
**Example usage** :
pyproject.tomluv.toml
```
[tool.uv.pip]
emit-build-options=true

```

```
[pip]
emit-build-options=true

```

#### `emit-find-links`
Include `--find-links` entries in the output file generated by `uv pip compile`.
**Default value** : `false`
**Type** : `bool`
**Example usage** :
pyproject.tomluv.toml
```
[tool.uv.pip]
emit-find-links=true

```

```
[pip]
emit-find-links=true

```

#### `emit-index-annotation`
Include comment annotations indicating the index used to resolve each package (e.g., `# from https://pypi.org/simple`).
**Default value** : `false`
**Type** : `bool`
**Example usage** :
pyproject.tomluv.toml
```
[tool.uv.pip]
emit-index-annotation=true

```

```
[pip]
emit-index-annotation=true

```

#### `emit-index-url`
Include `--index-url` and `--extra-index-url` entries in the output file generated by `uv pip compile`.
**Default value** : `false`
**Type** : `bool`
**Example usage** :
pyproject.tomluv.toml
```
[tool.uv.pip]
emit-index-url=true

```

```
[pip]
emit-index-url=true

```

#### `emit-marker-expression`
Whether to emit a marker string indicating the conditions under which the set of pinned dependencies is valid.
The pinned dependencies may be valid even when the marker expression is false, but when the expression is true, the requirements are known to be correct.
**Default value** : `false`
**Type** : `bool`
**Example usage** :
pyproject.tomluv.toml
```
[tool.uv.pip]
emit-marker-expression=true

```

```
[pip]
emit-marker-expression=true

```

#### `exclude-newer`
Limit candidate packages to those that were uploaded prior to a given point in time.
Accepts a superset of RFC 3339 (e.g., `2006-12-02T02:07:43Z`). A full timestamp is required to ensure that the resolver will behave consistently across timezones.
**Default value** : `None`
**Type** : `str`
**Example usage** :
pyproject.tomluv.toml
```
[tool.uv.pip]
exclude-newer="2006-12-02T02:07:43Z"

```

```
[pip]
exclude-newer="2006-12-02T02:07:43Z"

```

#### `extra`
Include optional dependencies from the specified extra; may be provided more than once.
Only applies to `pyproject.toml`, `setup.py`, and `setup.cfg` sources.
**Default value** : `[]`
**Type** : `list[str]`
**Example usage** :
pyproject.tomluv.toml
```
[tool.uv.pip]
extra=["dev","docs"]

```

```
[pip]
extra=["dev","docs"]

```

#### `extra-index-url`
Extra URLs of package indexes to use, in addition to `--index-url`.
Accepts either a repository compliant with PEP 503 (the simple repository API), or a local directory laid out in the same format.
All indexes provided via this flag take priority over the index specified by `index_url`. When multiple indexes are provided, earlier values take priority.
To control uv's resolution strategy when multiple indexes are present, see `index_strategy`.
**Default value** : `[]`
**Type** : `list[str]`
**Example usage** :
pyproject.tomluv.toml
```
[tool.uv.pip]
extra-index-url=["https://download.pytorch.org/whl/cpu"]

```

```
[pip]
extra-index-url=["https://download.pytorch.org/whl/cpu"]

```

#### `find-links`
Locations to search for candidate distributions, in addition to those found in the registry indexes.
If a path, the target must be a directory that contains packages as wheel files (`.whl`) or source distributions (e.g., `.tar.gz` or `.zip`) at the top level.
If a URL, the page must contain a flat list of links to package files adhering to the formats described above.
**Default value** : `[]`
**Type** : `list[str]`
**Example usage** :
pyproject.tomluv.toml
```
[tool.uv.pip]
find-links=["https://download.pytorch.org/whl/torch_stable.html"]

```

```
[pip]
find-links=["https://download.pytorch.org/whl/torch_stable.html"]

```

#### `fork-strategy`
The strategy to use when selecting multiple versions of a given package across Python versions and platforms.
By default, uv will optimize for selecting the latest version of each package for each supported Python version (`requires-python`), while minimizing the number of selected versions across platforms.
Under `fewest`, uv will minimize the number of selected versions for each package, preferring older versions that are compatible with a wider range of supported Python versions or platforms.
**Default value** : `"requires-python"`
**Possible values** :
  * `"fewest"`: Optimize for selecting the fewest number of versions for each package. Older versions may be preferred if they are compatible with a wider range of supported Python versions or platforms
  * `"requires-python"`: Optimize for selecting latest supported version of each package, for each supported Python version


**Example usage** :
pyproject.tomluv.toml
```
[tool.uv.pip]
fork-strategy="fewest"

```

```
[pip]
fork-strategy="fewest"

```

#### `generate-hashes`
Include distribution hashes in the output file.
**Default value** : `false`
**Type** : `bool`
**Example usage** :
pyproject.tomluv.toml
```
[tool.uv.pip]
generate-hashes=true

```

```
[pip]
generate-hashes=true

```

#### `index-strategy`
The strategy to use when resolving against multiple index URLs.
By default, uv will stop at the first index on which a given package is available, and limit resolutions to those present on that first index (`first-index`). This prevents "dependency confusion" attacks, whereby an attacker can upload a malicious package under the same name to an alternate index.
**Default value** : `"first-index"`
**Possible values** :
  * `"first-index"`: Only use results from the first index that returns a match for a given package name
  * `"unsafe-first-match"`: Search for every package name across all indexes, exhausting the versions from the first index before moving on to the next
  * `"unsafe-best-match"`: Search for every package name across all indexes, preferring the "best" version found. If a package version is in multiple indexes, only look at the entry for the first index


**Example usage** :
pyproject.tomluv.toml
```
[tool.uv.pip]
index-strategy="unsafe-best-match"

```

```
[pip]
index-strategy="unsafe-best-match"

```

#### `index-url`
The URL of the Python package index (by default: https://pypi.org/simple).
Accepts either a repository compliant with PEP 503 (the simple repository API), or a local directory laid out in the same format.
The index provided by this setting is given lower priority than any indexes specified via `extra_index_url`.
**Default value** : `"https://pypi.org/simple"`
**Type** : `str`
**Example usage** :
pyproject.tomluv.toml
```
[tool.uv.pip]
index-url="https://test.pypi.org/simple"

```

```
[pip]
index-url="https://test.pypi.org/simple"

```

#### `keyring-provider`
Attempt to use `keyring` for authentication for index URLs.
At present, only `--keyring-provider subprocess` is supported, which configures uv to use the `keyring` CLI to handle authentication.
**Default value** : `disabled`
**Type** : `str`
**Example usage** :
pyproject.tomluv.toml
```
[tool.uv.pip]
keyring-provider="subprocess"

```

```
[pip]
keyring-provider="subprocess"

```

#### `link-mode`
The method to use when installing packages from the global cache.
Defaults to `clone` (also known as Copy-on-Write) on macOS, and `hardlink` on Linux and Windows.
**Default value** : `"clone" (macOS) or "hardlink" (Linux, Windows)`
**Possible values** :
  * `"clone"`: Clone (i.e., copy-on-write) packages from the wheel into the `site-packages` directory
  * `"copy"`: Copy packages from the wheel into the `site-packages` directory
  * `"hardlink"`: Hard link packages from the wheel into the `site-packages` directory
  * `"symlink"`: Symbolically link packages from the wheel into the `site-packages` directory


**Example usage** :
pyproject.tomluv.toml
```
[tool.uv.pip]
link-mode="copy"

```

```
[pip]
link-mode="copy"

```

#### `no-annotate`
Exclude comment annotations indicating the source of each package from the output file generated by `uv pip compile`.
**Default value** : `false`
**Type** : `bool`
**Example usage** :
pyproject.tomluv.toml
```
[tool.uv.pip]
no-annotate=true

```

```
[pip]
no-annotate=true

```

#### `no-binary`
Don't install pre-built wheels.
The given packages will be built and installed from source. The resolver will still use pre-built wheels to extract package metadata, if available.
Multiple packages may be provided. Disable binaries for all packages with `:all:`. Clear previously specified packages with `:none:`.
**Default value** : `[]`
**Type** : `list[str]`
**Example usage** :
pyproject.tomluv.toml
```
[tool.uv.pip]
no-binary=["ruff"]

```

```
[pip]
no-binary=["ruff"]

```

#### `no-build`
Don't build source distributions.
When enabled, resolving will not run arbitrary Python code. The cached wheels of already-built source distributions will be reused, but operations that require building distributions will exit with an error.
Alias for `--only-binary :all:`.
**Default value** : `false`
**Type** : `bool`
**Example usage** :
pyproject.tomluv.toml
```
[tool.uv.pip]
no-build=true

```

```
[pip]
no-build=true

```

#### `no-build-isolation`
Disable isolation when building source distributions.
Assumes that build dependencies specified by PEP 518 are already installed.
**Default value** : `false`
**Type** : `bool`
**Example usage** :
pyproject.tomluv.toml
```
[tool.uv.pip]
no-build-isolation=true

```

```
[pip]
no-build-isolation=true

```

#### `no-build-isolation-package`
Disable isolation when building source distributions for a specific package.
Assumes that the packages' build dependencies specified by PEP 518 are already installed.
**Default value** : `[]`
**Type** : `list[str]`
**Example usage** :
pyproject.tomluv.toml
```
[tool.uv.pip]
no-build-isolation-package=["package1","package2"]

```

```
[pip]
no-build-isolation-package=["package1","package2"]

```

#### `no-deps`
Ignore package dependencies, instead only add those packages explicitly listed on the command line to the resulting requirements file.
**Default value** : `false`
**Type** : `bool`
**Example usage** :
pyproject.tomluv.toml
```
[tool.uv.pip]
no-deps=true

```

```
[pip]
no-deps=true

```

#### `no-emit-package`
Specify a package to omit from the output resolution. Its dependencies will still be included in the resolution. Equivalent to pip-compile's `--unsafe-package` option.
**Default value** : `[]`
**Type** : `list[str]`
**Example usage** :
pyproject.tomluv.toml
```
[tool.uv.pip]
no-emit-package=["ruff"]

```

```
[pip]
no-emit-package=["ruff"]

```

#### `no-extra`
Exclude the specified optional dependencies if `all-extras` is supplied.
**Default value** : `[]`
**Type** : `list[str]`
**Example usage** :
pyproject.tomluv.toml
```
[tool.uv.pip]
all-extras=true
no-extra=["dev","docs"]

```

```
[pip]
all-extras=true
no-extra=["dev","docs"]

```

#### `no-header`
Exclude the comment header at the top of output file generated by `uv pip compile`.
**Default value** : `false`
**Type** : `bool`
**Example usage** :
pyproject.tomluv.toml
```
[tool.uv.pip]
no-header=true

```

```
[pip]
no-header=true

```

#### `no-index`
Ignore all registry indexes (e.g., PyPI), instead relying on direct URL dependencies and those provided via `--find-links`.
**Default value** : `false`
**Type** : `bool`
**Example usage** :
pyproject.tomluv.toml
```
[tool.uv.pip]
no-index=true

```

```
[pip]
no-index=true

```

#### `no-sources`
Ignore the `tool.uv.sources` table when resolving dependencies. Used to lock against the standards-compliant, publishable package metadata, as opposed to using any local or Git sources.
**Default value** : `false`
**Type** : `bool`
**Example usage** :
pyproject.tomluv.toml
```
[tool.uv.pip]
no-sources=true

```

```
[pip]
no-sources=true

```

#### `no-strip-extras`
Include extras in the output file.
By default, uv strips extras, as any packages pulled in by the extras are already included as dependencies in the output file directly. Further, output files generated with `--no-strip-extras` cannot be used as constraints files in `install` and `sync` invocations.
**Default value** : `false`
**Type** : `bool`
**Example usage** :
pyproject.tomluv.toml
```
[tool.uv.pip]
no-strip-extras=true

```

```
[pip]
no-strip-extras=true

```

#### `no-strip-markers`
Include environment markers in the output file generated by `uv pip compile`.
By default, uv strips environment markers, as the resolution generated by `compile` is only guaranteed to be correct for the target environment.
**Default value** : `false`
**Type** : `bool`
**Example usage** :
pyproject.tomluv.toml
```
[tool.uv.pip]
no-strip-markers=true

```

```
[pip]
no-strip-markers=true

```

#### `only-binary`
Only use pre-built wheels; don't build source distributions.
When enabled, resolving will not run code from the given packages. The cached wheels of already-built source distributions will be reused, but operations that require building distributions will exit with an error.
Multiple packages may be provided. Disable binaries for all packages with `:all:`. Clear previously specified packages with `:none:`.
**Default value** : `[]`
**Type** : `list[str]`
**Example usage** :
pyproject.tomluv.toml
```
[tool.uv.pip]
only-binary=["ruff"]

```

```
[pip]
only-binary=["ruff"]

```

#### `output-file`
Write the requirements generated by `uv pip compile` to the given `requirements.txt` file.
If the file already exists, the existing versions will be preferred when resolving dependencies, unless `--upgrade` is also specified.
**Default value** : `None`
**Type** : `str`
**Example usage** :
pyproject.tomluv.toml
```
[tool.uv.pip]
output-file="requirements.txt"

```

```
[pip]
output-file="requirements.txt"

```

#### `prefix`
Install packages into `lib`, `bin`, and other top-level folders under the specified directory, as if a virtual environment were present at that location.
In general, prefer the use of `--python` to install into an alternate environment, as scripts and other artifacts installed via `--prefix` will reference the installing interpreter, rather than any interpreter added to the `--prefix` directory, rendering them non-portable.
**Default value** : `None`
**Type** : `str`
**Example usage** :
pyproject.tomluv.toml
```
[tool.uv.pip]
prefix="./prefix"

```

```
[pip]
prefix="./prefix"

```

#### `prerelease`
The strategy to use when considering pre-release versions.
By default, uv will accept pre-releases for packages that _only_ publish pre-releases, along with first-party requirements that contain an explicit pre-release marker in the declared specifiers (`if-necessary-or-explicit`).
**Default value** : `"if-necessary-or-explicit"`
**Possible values** :
  * `"disallow"`: Disallow all pre-release versions
  * `"allow"`: Allow all pre-release versions
  * `"if-necessary"`: Allow pre-release versions if all versions of a package are pre-release
  * `"explicit"`: Allow pre-release versions for first-party packages with explicit pre-release markers in their version requirements
  * `"if-necessary-or-explicit"`: Allow pre-release versions if all versions of a package are pre-release, or if the package has an explicit pre-release marker in its version requirements


**Example usage** :
pyproject.tomluv.toml
```
[tool.uv.pip]
prerelease="allow"

```

```
[pip]
prerelease="allow"

```

#### `python`
The Python interpreter into which packages should be installed.
By default, uv installs into the virtual environment in the current working directory or any parent directory. The `--python` option allows you to specify a different interpreter, which is intended for use in continuous integration (CI) environments or other automated workflows.
Supported formats: - `3.10` looks for an installed Python 3.10 in the registry on Windows (see `py --list-paths`), or `python3.10` on Linux and macOS. - `python3.10` or `python.exe` looks for a binary with the given name in `PATH`. - `/home/ferris/.local/bin/python3.10` uses the exact Python at the given path.
**Default value** : `None`
**Type** : `str`
**Example usage** :
pyproject.tomluv.toml
```
[tool.uv.pip]
python="3.10"

```

```
[pip]
python="3.10"

```

#### `python-platform`
The platform for which requirements should be resolved.
Represented as a "target triple", a string that describes the target platform in terms of its CPU, vendor, and operating system name, like `x86_64-unknown-linux-gnu` or `aarch64-apple-darwin`.
**Default value** : `None`
**Type** : `str`
**Example usage** :
pyproject.tomluv.toml
```
[tool.uv.pip]
python-platform="x86_64-unknown-linux-gnu"

```

```
[pip]
python-platform="x86_64-unknown-linux-gnu"

```

#### `python-version`
The minimum Python version that should be supported by the resolved requirements (e.g., `3.8` or `3.8.17`).
If a patch version is omitted, the minimum patch version is assumed. For example, `3.8` is mapped to `3.8.0`.
**Default value** : `None`
**Type** : `str`
**Example usage** :
pyproject.tomluv.toml
```
[tool.uv.pip]
python-version="3.8"

```

```
[pip]
python-version="3.8"

```

#### `reinstall`
Reinstall all packages, regardless of whether they're already installed. Implies `refresh`.
**Default value** : `false`
**Type** : `bool`
**Example usage** :
pyproject.tomluv.toml
```
[tool.uv.pip]
reinstall=true

```

```
[pip]
reinstall=true

```

#### `reinstall-package`
Reinstall a specific package, regardless of whether it's already installed. Implies `refresh-package`.
**Default value** : `[]`
**Type** : `list[str]`
**Example usage** :
pyproject.tomluv.toml
```
[tool.uv.pip]
reinstall-package=["ruff"]

```

```
[pip]
reinstall-package=["ruff"]

```

#### `require-hashes`
Require a matching hash for each requirement.
Hash-checking mode is all or nothing. If enabled, _all_ requirements must be provided with a corresponding hash or set of hashes. Additionally, if enabled, _all_ requirements must either be pinned to exact versions (e.g., `==1.0.0`), or be specified via direct URL.
Hash-checking mode introduces a number of additional constraints:
  * Git dependencies are not supported.
  * Editable installs are not supported.
  * Local dependencies are not supported, unless they point to a specific wheel (`.whl`) or source archive (`.zip`, `.tar.gz`), as opposed to a directory.


**Default value** : `false`
**Type** : `bool`
**Example usage** :
pyproject.tomluv.toml
```
[tool.uv.pip]
require-hashes=true

```

```
[pip]
require-hashes=true

```

#### `resolution`
The strategy to use when selecting between the different compatible versions for a given package requirement.
By default, uv will use the latest compatible version of each package (`highest`).
**Default value** : `"highest"`
**Possible values** :
  * `"highest"`: Resolve the highest compatible version of each package
  * `"lowest"`: Resolve the lowest compatible version of each package
  * `"lowest-direct"`: Resolve the lowest compatible version of any direct dependencies, and the highest compatible version of any transitive dependencies


**Example usage** :
pyproject.tomluv.toml
```
[tool.uv.pip]
resolution="lowest-direct"

```

```
[pip]
resolution="lowest-direct"

```

#### `strict`
Validate the Python environment, to detect packages with missing dependencies and other issues.
**Default value** : `false`
**Type** : `bool`
**Example usage** :
pyproject.tomluv.toml
```
[tool.uv.pip]
strict=true

```

```
[pip]
strict=true

```

#### `system`
Install packages into the system Python environment.
By default, uv installs into the virtual environment in the current working directory or any parent directory. The `--system` option instructs uv to instead use the first Python found in the system `PATH`.
WARNING: `--system` is intended for use in continuous integration (CI) environments and should be used with caution, as it can modify the system Python installation.
**Default value** : `false`
**Type** : `bool`
**Example usage** :
pyproject.tomluv.toml
```
[tool.uv.pip]
system=true

```

```
[pip]
system=true

```

#### `target`
Install packages into the specified directory, rather than into the virtual or system Python environment. The packages will be installed at the top-level of the directory.
**Default value** : `None`
**Type** : `str`
**Example usage** :
pyproject.tomluv.toml
```
[tool.uv.pip]
target="./target"

```

```
[pip]
target="./target"

```

#### `universal`
Perform a universal resolution, attempting to generate a single `requirements.txt` output file that is compatible with all operating systems, architectures, and Python implementations.
In universal mode, the current Python version (or user-provided `--python-version`) will be treated as a lower bound. For example, `--universal --python-version 3.7` would produce a universal resolution for Python 3.7 and later.
**Default value** : `false`
**Type** : `bool`
**Example usage** :
pyproject.tomluv.toml
```
[tool.uv.pip]
universal=true

```

```
[pip]
universal=true

```

#### `upgrade`
Allow package upgrades, ignoring pinned versions in any existing output file.
**Default value** : `false`
**Type** : `bool`
**Example usage** :
pyproject.tomluv.toml
```
[tool.uv.pip]
upgrade=true

```

```
[pip]
upgrade=true

```

#### `upgrade-package`
Allow upgrades for a specific package, ignoring pinned versions in any existing output file.
Accepts both standalone package names (`ruff`) and version specifiers (`ruff<0.5.0`).
**Default value** : `[]`
**Type** : `list[str]`
**Example usage** :
pyproject.tomluv.toml
```
[tool.uv.pip]
upgrade-package=["ruff"]

```

```
[pip]
upgrade-package=["ruff"]

```

#### `verify-hashes`
Validate any hashes provided in the requirements file.
Unlike `--require-hashes`, `--verify-hashes` does not require that all requirements have hashes; instead, it will limit itself to verifying the hashes of those requirements that do include them.
**Default value** : `true`
**Type** : `bool`
**Example usage** :
pyproject.tomluv.toml
```
[tool.uv.pip]
verify-hashes=true

```

```
[pip]
verify-hashes=true

```

Back to top 
