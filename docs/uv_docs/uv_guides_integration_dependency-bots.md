Skip to content 
# Dependency bots
It is considered best practice to regularly update dependencies, to avoid being exposed to vulnerabilities, limit incompatibilities between dependencies, and avoid complex upgrades when upgrading from a too old version. A variety of tools can help staying up-to-date by creating automated pull requests. Several of them support uv, or have work underway to support it.
## Renovate
uv is supported by Renovate.
Note
Updating `uv pip compile` outputs such as `requirements.txt` is not yet supported. Progress can be tracked at renovatebot/renovate#30909.
### `uv.lock` output
Renovate uses the presence of a `uv.lock` file to determine that uv is used for managing dependencies, and will suggest upgrades to project dependencies, optional dependencies and development dependencies. Renovate will update both the `pyproject.toml` and `uv.lock` files.
The lockfile can also be refreshed on a regular basis (for instance to update transitive dependencies) by enabling the `lockFileMaintenance` option:
renovate.json5```
{
$schema:"https://docs.renovatebot.com/renovate-schema.json",
lockFileMaintenance:{
enabled:true,
},
}

```

### Inline script metadata
Renovate supports updating dependencies defined using script inline metadata.
Since it cannot automatically detect which Python files use script inline metadata, their locations need to be explicitly defined using `fileMatch`, like so:
renovate.json5```
{
$schema:"https://docs.renovatebot.com/renovate-schema.json",
pep723:{
fileMatch:[
"scripts/generate_docs\\.py",
"scripts/run_server\\.py",
],
},
}

```

## Dependabot
Support for uv is not yet available. Progress can be tracked at dependabot/dependabot-core#10478.
Back to top 
![](https://cdn.usefathom.com/?h=https%3A%2F%2Fdocs.astral.sh&p=%2Fuv%2Fguides%2Fintegration%2Fdependency-bots%2F&r=&sid=ESKBRHGN&qs=%7B%7D&cid=43767125)
