Skip to content 
# Installing uv
## Installation methods
Install uv with our standalone installers or your package manager of choice.
### Standalone installer
uv provides a standalone installer to download and install uv:
macOS and LinuxWindows
Use `curl` to download the script and execute it with `sh`:
```
$ curl-LsSfhttps://astral.sh/uv/install.sh|sh

```

If your system doesn't have `curl`, you can use `wget`:
```
$ wget-qO-https://astral.sh/uv/install.sh|sh

```

Request a specific version by including it in the URL:
```
$ curl-LsSfhttps://astral.sh/uv/0.6.2/install.sh|sh

```

Use `irm` to download the script and execute it with `iex`:
```
$ powershell-ExecutionPolicyByPass-c"irm https://astral.sh/uv/install.ps1 | iex"

```

Changing the execution policy allows running a script from the internet.
Request a specific version by including it in the URL:
```
$ powershell-ExecutionPolicyByPass-c"irm https://astral.sh/uv/0.6.2/install.ps1 | iex"

```

Tip
The installation script may be inspected before use:
macOS and LinuxWindows
```
$ curl-LsSfhttps://astral.sh/uv/install.sh|less

```

```
$ powershell-c"irm https://astral.sh/uv/install.ps1 | more"

```

Alternatively, the installer or binaries can be downloaded directly from GitHub.
See the documentation on installer configuration for details on customizing your uv installation.
### PyPI
For convenience, uv is published to PyPI.
If installing from PyPI, we recommend installing uv into an isolated environment, e.g., with `pipx`:
```
$ pipxinstalluv

```

However, `pip` can also be used:
```
$ pipinstalluv

```

Note
uv ships with prebuilt distributions (wheels) for many platforms; if a wheel is not available for a given platform, uv will be built from source, which requires a Rust toolchain. See the contributing setup guide for details on building uv from source.
### Cargo
uv is available via Cargo, but must be built from Git rather than crates.io due to its dependency on unpublished crates.
```
$ cargoinstall--githttps://github.com/astral-sh/uvuv

```

### Homebrew
uv is available in the core Homebrew packages.
```
$ brewinstalluv

```

### WinGet
uv is available via WinGet.
```
$ wingetinstall--id=astral-sh.uv-e

```

### Scoop
uv is available via Scoop.
```
$ scoopinstallmain/uv

```

### Docker
uv provides a Docker image at `ghcr.io/astral-sh/uv`.
See our guide on using uv in Docker for more details.
### GitHub Releases
uv release artifacts can be downloaded directly from GitHub Releases.
Each release page includes binaries for all supported platforms as well as instructions for using the standalone installer via `github.com` instead of `astral.sh`.
## Upgrading uv
When uv is installed via the standalone installer, it can update itself on-demand:
```
$ uvselfupdate

```

Tip
Updating uv will re-run the installer and can modify your shell profiles. To disable this behavior, set `INSTALLER_NO_MODIFY_PATH=1`.
When another installation method is used, self-updates are disabled. Use the package manager's upgrade method instead. For example, with `pip`:
```
$ pipinstall--upgradeuv

```

## Shell autocompletion
Tip
You can run `echo $SHELL` to help you determine your shell.
To enable shell autocompletion for uv commands, run one of the following:
BashZshfishElvishPowerShell / pwsh
```
echo'eval "$(uv generate-shell-completion bash)"'>>~/.bashrc

```

```
echo'eval "$(uv generate-shell-completion zsh)"'>>~/.zshrc

```

```
echo'uv generate-shell-completion fish | source'>>~/.config/fish/config.fish

```

```
echo'eval (uv generate-shell-completion elvish | slurp)'>>~/.elvish/rc.elv

```

```
if (!(Test-Path -Path $PROFILE)) {
 New-Item -ItemType File -Path $PROFILE -Force
}
Add-Content -Path $PROFILE -Value '(& uv generate-shell-completion powershell) | Out-String | Invoke-Expression'

```

To enable shell autocompletion for uvx, run one of the following:
BashZshfishElvishPowerShell / pwsh
```
echo'eval "$(uvx --generate-shell-completion bash)"'>>~/.bashrc

```

```
echo'eval "$(uvx --generate-shell-completion zsh)"'>>~/.zshrc

```

```
echo'uvx --generate-shell-completion fish | source'>>~/.config/fish/config.fish

```

```
echo'eval (uvx --generate-shell-completion elvish | slurp)'>>~/.elvish/rc.elv

```

```
if (!(Test-Path -Path $PROFILE)) {
 New-Item -ItemType File -Path $PROFILE -Force
}
Add-Content -Path $PROFILE -Value '(& uvx --generate-shell-completion powershell) | Out-String | Invoke-Expression'

```

Then restart the shell or source the shell config file.
## Uninstallation
If you need to remove uv from your system, follow these steps:
  1. Clean up stored data (optional):
```
$ uvcacheclean
$ rm-r"$(uvpythondir)"
$ rm-r"$(uvtooldir)"

```

Tip
Before removing the binaries, you may want to remove any data that uv has stored.
  2. Remove the uv and uvx binaries:
macOS and LinuxWindows
```
$ rm~/.local/bin/uv~/.local/bin/uvx

```

```
$ rm $HOME\.local\bin\uv.exe
$ rm $HOME\.local\bin\uvx.exe

```

Note
Prior to 0.5.0, uv was installed into `~/.cargo/bin`. The binaries can be removed from there to uninstall. Upgrading from an older version will not automatically remove the binaries from `~/.cargo/bin`.


## Next steps
See the first steps or jump straight to the guides to start using uv.
Back to top 
![](https://cdn.usefathom.com/?h=https%3A%2F%2Fdocs.astral.sh&p=%2Fuv%2Fgetting-started%2Finstallation%2F&r=&sid=ESKBRHGN&qs=%7B%7D&cid=47967344)
