# Installing and using pipx for command-line Python tools

[pipx](https://github.com/pipxproject/pipx) is a utility for installing and running Python applications in isolated environments. This means it allows you to [install command-line tools like `tox`](#installing-command-line-tools-with-pipx) in their own virtualenv that pipx manages for you using simple, `pip`-like commands. 

- [Installing pipx](#installing-pipx)
- [Configuring pipx](#configuring-pipx)
- [Installing command-line tools with pipx](#installing-command-line-tools-with-pipx)
- [Further reading](#further-reading)

## Installing pipx

To [install pipx on macOS](https://github.com/pipxproject/pipx#install-pipx), use Homebrew:

```shell
brew update
brew install pipx
```

If you've already installed it, it couldn't hurt to update it:

```shell
brew update
brew upgrade pipx
```

## Configuring pipx

pipx comes with a little helper to add the things that pipx installs to your path, so you can run them:

```shell
pipx ensurepath
```

The only catch with that command is that it adds the pipx binary path _after_ your existing path;
as a result, the Docker Desktop version of Docker Compose will take precedence.
So, we need to modify what it added.
Open your `~/.bashrc` or `~/.zshrc` file and look toward the bottom for the following line:

```bash
export PATH="$PATH:/Users/<username>/.local/bin"
```

We're going to reverse the order of that so `~/.local/bin` comes before the existing `$PATH`.
Update it to look like so (remembering to replace `<username>` if you copy and paste from here):

```bash
export PATH="/Users/<username>/.local/bin:$PATH"
```

You might also want to take this opportunity to move it to wherever you have other path modifications.

pipx also has a helper that provides [shell completion instructions](https://pipxproject.github.io/pipx/installation/#shell-completion):

```shell
pipx completions
```

Now open a new shell, or `source` whatever file(s) you just modified in your current shell.


## Installing command-line tools with pipx

You can now use `pipx` to [install packages that provide command-line tools](https://pipxproject.github.io/pipx/getting-started/) just like `pip`. To install a package from PyPI:

```shell
pipx install PACKAGE
```

For example, to install `tox`:

```shell
pipx install tox
```

You can also inject another Python package into pipx's isolated environment for a package. For example, to install the `tox` plugin `tox-pip-extensions`:

```shell
pipx inject tox tox-pip-extensions
```

## Further reading

- [pipx documentation](https://pipxproject.github.io/pipx/)
- [pipx examples](https://pipxproject.github.io/pipx/examples/), including options for particular versions of Python, and more
- [How pipx works](https://pipxproject.github.io/pipx/how-pipx-works/)
