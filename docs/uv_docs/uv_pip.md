Skip to content 
# The pip interface
uv provides a drop-in replacement for common `pip`, `pip-tools`, and `virtualenv` commands. These commands work directly with the virtual environment, in contrast to uv's primary interfaces where the virtual environment is managed automatically. The `uv pip` interface exposes the speed and functionality of uv to power users and projects that are not ready to transition away from `pip` and `pip-tools`.
The following sections discuss the basics of using `uv pip`:
  * Creating and using environments
  * Installing and managing packages
  * Inspecting environments and packages
  * Declaring package dependencies
  * Locking and syncing environments


Please note these commands do not _exactly_ implement the interfaces and behavior of the tools they are based on. The further you stray from common workflows, the more likely you are to encounter differences. Consult the pip-compatibility guide for details.
Important
uv does not rely on or invoke pip. The pip interface is named as such to highlight its dedicated purpose of providing low-level commands that match pip's interface and to separate it from the rest of uv's commands which operate at a higher level of abstraction.
Back to top 
![](https://cdn.usefathom.com/?h=https%3A%2F%2Fdocs.astral.sh&p=%2Fuv%2Fpip%2F&r=&sid=ESKBRHGN&qs=%7B%7D&cid=58645705)
