# Installing and using Python 2 and 3

- [Migrating from Homebrew-installed Python](#migrating-from-homebrew-installed-python)
   - [Remove Homebrew-installed virtualenvwrapper](#remove-homebrew-installed-virtualenvwrapper)
   - [Optional: remove Homebrew-installed Python and variables](#optional-remove-homebrew-installed-python-and-variables)
- [Installing pyenv](#installing-pyenv)
- [Configuring pyenv](#configuring-pyenv)
- [Installing Python](#installing-python)
- [Setting the global Python versions](#setting-the-global-python-versions)
- [Using local Python versions](#using-local-python-versions)
- [Upgrading Python versions](#upgrading-python-versions)
- [Further reading](#further-reading)

## Migrating from Homebrew-installed Python

**Note**: Any virtualenvs that you have prior to this process will have to be recreated to use Python from pyenv.

### Remove Homebrew-installed virtualenvwrapper

Before installing and configuring pyenv, uninstall virtualenvwrapper if it  is installed via Homebrew:

```shell
brew uninstall virtualenvwrapper
```

Then remove the sourcing of `virtualenvwrapper.sh` and related variables from your shell's dotfiles (`.profile`, `.bashrc` for bash users, `.zshrc` for zsh users):

```shell
export VIRTUALENVWRAPPER_PYTHON=$HOME/homebrew/bin/python
export VIRTUALENVWRAPPER_VIRTUALENV=$HOME/homebrew/bin/virtualenv
source $HOME/homebrew/bin/virtualenvwrapper.sh
```

### Optional: remove Homebrew-installed Python and variables

Before installing and configuring pyenv, it is a good idea to uninstall all old versions of Python and virtualenvwrapper that might be installed via Homebrew, and remove any varaibles from your shell's dotfiles (`.profile`, `.bashrc` for bash users, `.zshrc` for zsh users):

```shell
brew uninstall python
brew cleanup python
brew uninstall virtualenvwrapper
```
You should also remove any variables that reference Homebrew's Python from your shell's dotfiles (`.profile`, `.bashrc` for bash users, `.zshrc` for zsh users), for example, removing:

```shell
export PYTHONPATH=$HOME/homebrew/bin/python
```

If you do not wish to uninstall Homebrew's Python, or if you have Homebrew-installed packages that depend on Python-installed Homebrew, simply ensure that Homebrew's Python either does not exist in your shell's `PATH` variable, or that it is added to `PATH` *before* the pyenv configuration below.

## Installing pyenv

To [install pyenv on MacOS](https://github.com/pyenv/pyenv#homebrew-on-macos), use Homebrew:

```shell
brew update
brew install pyenv
brew install pyenv-virtualenvwrapper
```

**Note:** pyenv-virtualenvwrapper is not strictly required, depending on how you wish to manage your virtualenvs. See the [notes in the pyenv-virtualewrapper README](https://github.com/pyenv/pyenv-virtualenvwrapper#pyenv-virtualenvwrapper). Some of our documentation assumes `virtualenvwrapper` exists, so this guide installs it.

## Configuring pyenv

Per the [pyenv documentation](https://github.com/pyenv/pyenv#basic-github-checkout), the follow environment variables need to be added to your shell dotfiles (`.profile`, `.bashrc` for bash users, `.zshrc` for zsh users).

```shell
# pyenv
export PYENV_ROOT="$HOME/.pyenv"

# pyenv-virtualenvwrapper
export WORKON_HOME=$HOME/.virtualenvs

# Ensure this placed toward the end of the file
eval "$(pyenv init -)"

# Load virtualenvwrapper
pyenv virtualenvwrapper_lazy
```

Restart your shell.

## Installing Python

We have projects that require both Python 2.7 and Python 3.6:

```shell
pyenv install 3.6.8
pyenv install 2.7.15
```

Before you can use either of these versions of Python, you have to set them as either global or local versions.

## Setting the global Python versions

By default with pyenv when you invoke `python`, you will be using the system-installed Python. To set the [global Python version to the versions installed above](https://github.com/pyenv/pyenv/blob/master/COMMANDS.md#pyenv-global):

```shell
pyenv global 3.6.8 2.7.15
```

This will make `python` version 3.6.8, and `python2` will be 2.7.15. This is our recommended configuration.


## Using local Python versions

pyenv also allows you to set a local Python version (the version of Python that is available in the current directory):

```shell
pyenv local 2.7.15
```

This will create a `.python-version` file in the current directory. When you're in that directory or below it, pyenv will use the version you've specified here as `python`. 

**Note:** Please ensure that `.python-version` is in the `.gitignore` file for a given repository and that it does not get committed.

**Note:** When you create a virtualenv with `mkvirtualenv` with a local Python version set, it will default to that local Python version, and not the global version. You can still specify `-p` argument with the specific Python you wish to use.

## Upgrading Python versions

To change the local Python version to the new version, for example, 3.7.2, follow the same approach you took to setting it the first time:

```shell
pyenv local 3.7.2
```

And the local `.python-version` file will contain `3.7.2`.

This does not work for virtualenvs, however. If you install a new version of Python, for example, 3.7.2, you must recreate the virtualenv with the new version of Python. The virtualenv will have be recreated (`mkvirtualenv`) with the new version of Python either set locally, globally, or specified with `--python`:

```shell
mkvirtualenv --python=python2.7 [virtualenv name]
```

# Further reading

- [pyenv README](https://github.com/pyenv/pyenv/blob/master/README.md)
- [pyenv-virtualenvwrapper README](https://github.com/pyenv/pyenv-virtualenvwrapper/blob/master/README.md)

Virtualenv and its alternatives:

- [Introduction to Virtualenvs in the Virtualenv documentation](https://virtualenv.pypa.io/en/stable/#introduction) 
- [virtualenvwrapper documentation](https://virtualenvwrapper.readthedocs.io/en/latest/)
- [Python 3's venv module documentation](https://docs.python.org/3/library/venv.html)
- [pyenv-virtualenv README](https://github.com/pyenv/pyenv-virtualenv/blob/master/README.md)