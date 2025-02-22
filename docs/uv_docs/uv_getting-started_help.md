Skip to content 
# Getting help
## Help menus
The `--help` flag can be used to view the help menu for a command, e.g., for `uv`:
```
$ uv--help

```

To view the help menu for a specific command, e.g., for `uv init`:
```
$ uvinit--help

```

When using the `--help` flag, uv displays a condensed help menu. To view a longer help menu for a command, use `uv help`:
```
$ uvhelp

```

To view the long help menu for a specific command, e.g., for `uv init`:
```
$ uvhelpinit

```

When using the long help menu, uv will attempt to use `less` or `more` to "page" the output so it is not all displayed at once. To exit the pager, press `q`.
## Viewing the version
When seeking help, it's important to determine the version of uv that you're using — sometimes the problem is already solved in a newer version.
To check the installed version:
```
$ uvversion

```

The following are also valid:
```
$ uv--version# Same output as `uv version`
$ uv-V# Will not include the build commit and date
$ uvpip--version# Can be used with a subcommand

```

## Troubleshooting issues
The reference documentation contains a troubleshooting guide for common issues.
## Open an issue on GitHub
The issue tracker on GitHub is a good place to report bugs and request features. Make sure to search for similar issues first, as it is common for someone else to encounter the same problem.
## Chat on Discord
Astral has a Discord server, which is a great place to ask questions, learn more about uv, and engage with other community members.
Back to top 
![](https://cdn.usefathom.com/?h=https%3A%2F%2Fdocs.astral.sh&p=%2Fuv%2Fgetting-started%2Fhelp%2F&r=&sid=ESKBRHGN&qs=%7B%7D&cid=96861300)
