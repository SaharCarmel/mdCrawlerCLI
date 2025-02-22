Skip to content 
# Inspecting environments
## Listing installed packages
To list all of the packages in the environment:
```
$ uvpiplist

```

To list the packages in a JSON format:
```
$ uvpiplist--formatjson

```

To list all of the packages in the environment in a `requirements.txt` format:
```
$ uvpipfreeze

```

## Inspecting a package
To show information about an installed package, e.g., `numpy`:
```
$ uvpipshownumpy

```

Multiple packages can be inspected at once.
## Verifying an environment
It is possible to install packages with conflicting requirements into an environment if installed in multiple steps.
To check for conflicts or missing dependencies in the environment:
```
$ uvpipcheck

```

Back to top 
![](https://cdn.usefathom.com/?h=https%3A%2F%2Fdocs.astral.sh&p=%2Fuv%2Fpip%2Finspection%2F&r=&sid=ESKBRHGN&qs=%7B%7D&cid=69380017)
