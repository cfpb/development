# Installing and using Python 2 and 3

## Remove Homebrew Python, virtualenvwrapper, and variables

Before installing and configuring pyenv, uninstall all old versions of Python that might be installed via Homebrew and virtualenvwrapper, and remove any varaibles from your shell's dotfiles (`.bashrc` for bash users, `.zshrc` for zsh users):

```shell
brew uninstall python
brew cleanup python
```

Example of variables you may find that need to be removed:

```shell
export WORKON_HOME=$HOME/.virtualenvs
export VIRTUALENVWRAPPER_PYTHON=$HOME/homebrew/bin/python
export VIRTUALENVWRAPPER_VIRTUALENV=$HOME/homebrew/bin/virtualenv
source $HOME/homebrew/bin/virtualenvwrapper.sh
export PYTHONPATH=$HOME/homebrew/bin/python
```

Note: Any virtualenvs that you have prior to this process will have to be recreated. 

## Install pyenv

To [install pyenv on MacOS](https://github.com/pyenv/pyenv#homebrew-on-macos), use Homebrew:

```shell
brew update
brew install pyenv
brew install pyenv-virtualenv
brew install pyenv-virtualenvwrapper
```

Note: pyenv-virtualenvwrapper is not strictly required, depending on how you wish to manage your virtualenvs. See the [notes in the pyenv-virtualewrapper README](https://github.com/pyenv/pyenv-virtualenvwrapper#pyenv-virtualenvwrapper). Some of our documentation assumes `virtualenvwrapper` exists, so this guide installs it.

## Configure pyenv

Per the [pyenv documentation](https://github.com/pyenv/pyenv#basic-github-checkout), the follow environment variables need to be added to your shell dotfiles (`.bashrc` for bash users, `.zshrc` for zsh users).

```shell
# pyenv
export PYENV_ROOT="$HOME/.pyenv"
export PATH="$PYENV_ROOT/bin:$PATH"

# pyenv-virtualenvwrapper
export PYENV_VIRTUALENVWRAPPER_PREFER_PYVENV="true"
export WORKON_HOME=$HOME/.virtualenvs

# Ensure this placed toward the end of the file
eval "$(pyenv init -)"

# Load virtualenvwrapper
pyenv virtualenvwrapper_lazy
```

Restart your shell.

## Install Python

We have projects that require both Python 2.7 and Python 3.6:

```shell
pyenv install 3.6.8
pyenv install 2.7.15
```

With these two versions of Python installed, you 

## Set the global Python versions

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