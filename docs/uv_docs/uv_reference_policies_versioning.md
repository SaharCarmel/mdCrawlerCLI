Skip to content 
# Versioning
uv uses a custom versioning scheme in which the minor version number is bumped for breaking changes, and the patch version number is bumped for bug fixes, enhancements, and other non-breaking changes.
uv does not yet have a stable API; once uv's API is stable (v1.0.0), the versioning scheme will adhere to Semantic Versioning.
uv's changelog can be viewed on GitHub.
## Cache versioning
Cache versions are considered internal to uv, and so may be changed in a minor or patch release. See Cache versioning for more.
## Lockfile versioning
The `uv.lock` schema version is considered part of the public API, and so will only be incremented in a minor release as a breaking change. See Lockfile versioning for more.
Back to top 
![](https://cdn.usefathom.com/?h=https%3A%2F%2Fdocs.astral.sh&p=%2Fuv%2Freference%2Fpolicies%2Fversioning%2F&r=&sid=ESKBRHGN&qs=%7B%7D&cid=73525877)
