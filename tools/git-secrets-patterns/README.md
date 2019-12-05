# Patterns for git-secrets

This is a set of patterns for use with git-secrets, which we can use to keep sensitive data out of git.

## Dependencies

 - [git-secrets](https://github.com/awslabs/git-secrets), and anything it depends on.

## Quick setup

1. Install git-secrets (homebrew users may wish to `brew install git-secrets --HEAD`)

1. Check this repo out to your filesystem

1. Install the default patterns into git-secrets with `git secrets  --add-provider --global -- egrep -v '^$|^#' /path/to/development/tools/git-secret-patterns/default.txt`

## Usage

Scan a repo with [`git secrets --scan`](https://github.com/awslabs/git-secrets#options-for-scan) or use [`git secrets --scan-history`](https://github.com/awslabs/git-secrets#operation-modes) to scan the entire commit history.

You can set up commit hooks in a particular repo with:

```
git secrets --install
```

Or install the secrets hooks globally (for **newly created or cloned repos only**):

```
git secrets --install ~/.git-templates/git-secrets
git config --global init.templateDir ~/.git-templates/git-secrets
```

For existing repos, you will want to go back and `git secrets --install` as described above.

See [the git-secrets README](https://github.com/awslabs/git-secrets#synopsis) for details.
