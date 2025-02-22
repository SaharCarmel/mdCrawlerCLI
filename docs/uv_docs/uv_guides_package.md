Skip to content 
# Building and publishing a package
uv supports building Python packages into source and binary distributions via `uv build` and uploading them to a registry with `uv publish`.
## Preparing your project for packaging
Before attempting to publish your project, you'll want to make sure it's ready to be packaged for distribution.
If your project does not include a `[build-system]` definition in the `pyproject.toml`, uv will not build it by default. This means that your project may not be ready for distribution. Read more about the effect of declaring a build system in the project concept documentation.
Note
If you have internal packages that you do not want to be published, you can mark them as private:
```
[project]
classifiers=["Private :: Do Not Upload"]

```

This setting makes PyPI reject your uploaded package from publishing. It does not affect security or privacy settings on alternative registries.
We also recommend only generating per-project tokens: Without a PyPI token matching the project, it can't be accidentally published.
## Building your package
Build your package with `uv build`:
```
$ uvbuild

```

By default, `uv build` will build the project in the current directory, and place the built artifacts in a `dist/` subdirectory.
Alternatively, `uv build <SRC>` will build the package in the specified directory, while `uv build --package <PACKAGE>` will build the specified package within the current workspace.
Info
By default, `uv build` respects `tool.uv.sources` when resolving build dependencies from the `build-system.requires` section of the `pyproject.toml`. When publishing a package, we recommend running `uv build --no-sources` to ensure that the package builds correctly when `tool.uv.sources` is disabled, as is the case when using other build tools, like `pypa/build`.
## Publishing your package
Publish your package with `uv publish`:
```
$ uvpublish

```

Set a PyPI token with `--token` or `UV_PUBLISH_TOKEN`, or set a username with `--username` or `UV_PUBLISH_USERNAME` and password with `--password` or `UV_PUBLISH_PASSWORD`. For publishing to PyPI from GitHub Actions, you don't need to set any credentials. Instead, add a trusted publisher to the PyPI project.
Note
PyPI does not support publishing with username and password anymore, instead you need to generate a token. Using a token is equivalent to setting `--username __token__` and using the token as password.
If you're using a custom index through `[[tool.uv.index]]`, add `publish-url` and use `uv publish --index <name>`. For example:
```
[[tool.uv.index]]
name="testpypi"
url="https://test.pypi.org/simple/"
publish-url="https://test.pypi.org/legacy/"

```

Note
When using `uv publish --index <name>`, the `pyproject.toml` must be present, i.e. you need to have a checkout step in a publish CI job.
Even though `uv publish` retries failed uploads, it can happen that publishing fails in the middle, with some files uploaded and some files still missing. With PyPI, you can retry the exact same command, existing identical files will be ignored. With other registries, use `--check-url <index url>` with the index URL (not the publish URL) the packages belong to. When using `--index`, the index URL is used as check URL. uv will skip uploading files that are identical to files in the registry, and it will also handle raced parallel uploads. Note that existing files need to match exactly with those previously uploaded to the registry, this avoids accidentally publishing source distribution and wheels with different contents for the same version.
## Installing your package
Test that the package can be installed and imported with `uv run`:
```
$ uvrun--with<PACKAGE>--no-project--python-c"import <PACKAGE>"

```

The `--no-project` flag is used to avoid installing the package from your local project directory.
Tip
If you have recently installed the package, you may need to include the `--refresh-package <PACKAGE>` option to avoid using a cached version of the package.
## Next steps
To learn more about publishing packages, check out the PyPA guides on building and publishing.
Or, read on for guides on integrating uv with other software.
Back to top 
![](https://cdn.usefathom.com/?h=https%3A%2F%2Fdocs.astral.sh&p=%2Fuv%2Fguides%2Fpackage%2F&r=&sid=ESKBRHGN&qs=%7B%7D&cid=12245377)
