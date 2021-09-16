# Scrubbing Git History
When preparing a private repo for public release,
it is often necessary to remove instances of internal hostnames
from the repo's history.
It may also be necessary to remove instances of internal hostnames that
accidentally slip into public code when developing in the open.

This is a guide to rewriting Git history to remove such instances.

---

There are two places in a Git repository where such hostnames could appear:
the actual source code history, and commit messages.

There are plenty of
[good resources](https://help.github.com/en/github/authenticating-to-github/removing-sensitive-data-from-a-repository) for
[rewriting Git history](http://git-scm.com/docs/git-filter-branch),
but amazingly, none of them really directly
address what we will most commonly need to do:

- Replace instances of an internal hostname with a fake hostname
  (so that the surrounding context still makes sense)
- Edit commit messages that are potentially very old

The examples below will demonstrate the case where we need to change references
to an internal hostname "internal.host.gov" to simply "INTERNAL".

Before we get started, **some words of warning:**

Pushing these changes to a remote repository will require forced pushes.
This means that the history will diverge from
any of your collaborators' GitHub forks and local clones.
See below for
[instructions on disseminating the updates](#disseminating-the-scrubbed-data-to-other-forks-and-clones)
to collaborators.


## Replacing strings in the source history

This one is fairly simple.

First, make sure you need to do this.
Run `git log -Sinternal.host.gov --all` and `git log -Ginternal.host.gov --all`.
This will search diffs to show you
any commits where our internal hostname was added or removed.

If there are results, you've got some scrubbin' to do.
Before you do, though, it would be wise to
resolve any uncommitted changes and push them.
You may also then want to create a fresh clone to perform the scrubbing with,
just in case anything weird happens.

Here's the command you can run to replace that internal hostname with a placeholder:

```bash
git filter-branch --force --tree-filter "find . -type f -exec grep -I -l -q . {} \; -print0 | xargs -0 sed -i '' 's/internal\.host\.gov/INTERNAL/g'" --tag-name-filter cat -- --all
```

That command will replace all instances of `internal.host.gov` with `INTERNAL`.
It will affect all branches; to only affect the current branch,
remove `-- --all` from the end of the command.

Once it completes, you need to tell Git to purge the old objects from its cache:

```bash
git for-each-ref --format="delete %(refname)" refs/original | git update-ref --stdin
git reflog expire --expire=now --all
git gc --prune=now
```

Once you've taken out the trash,
if you re-run `git log -Sinternal.host.gov --all`
and `git log -Ginternal.host.gov --all`,
you should no longer see any results.
Force push to your remotes, and all is well.
See below for
[instructions on disseminating these changes](#disseminating-the-scrubbed-data-to-other-forks-and-clones)
to collaborators.


## Editing old commit messages

The most likely scenario for an internal hostname
appearing in a commit message is **merge commits**.
For example, if one were to do a standard pull from an upstream repo
while working on a feature branch, they would be asked to
enter a commit message for that merge.
This will default to something like
"Merge branch 'master' of internal.host.gov:user/repository into feature",
so if you usually leave it at that default,
your repos' histories may be littered with these kinds of merge commit messages.
(Unless you rebase everything, which this developer would generally recommend.)

That said, this process will work for editing any old commit messages,
whether or not they are merge commits.

First, make sure you need to do this.
Run `git log --grep='internal.host.gov' --all`.
If there are results, you've got some scrubbin' to do.
Before you do, though, it would be wise to
resolve any uncommitted changes and push them.
You may also then want to create a fresh clone to perform the scrubbing with,
just in case anything weird happens.

We'll use the same `git filter-branch` command,
but this time with the `--msg-filter` option.
Here's the command to run this time:

```bash
git filter-branch -f --msg-filter 'sed "s/internal.host.gov/INTERNAL/"'
```

Once it completes, you need to tell Git to purge the old objects from its cache:

```bash
git for-each-ref --format="delete %(refname)" refs/original | git update-ref --stdin
git reflog expire --expire=now --all
git gc --prune=now
```

Once you've taken out the trash, force push to your remotes, and all is well.

See below for
[instructions on disseminating these changes](#disseminating-the-scrubbed-data-to-other-forks-and-clones)
to collaborators.


## Disseminating the scrubbed data to other forks and clones

Tell your collaborators to **rebase, not merge,** any branches they created
off of your old (tainted) repository history.
One merge commit could reintroduce some or all of the tainted history
that you just went to the trouble of purging.

If your collaborators have no branches with work in progress,
it may be simpler for them to just delete their local clone and re-clone it.


## tl;dr (command quick reference)

### Source code history

```bash
# Find instances of 'internal.host.gov' in any diffs on any branch
git log -Sinternal.host.gov --all
git log -Ginternal.host.gov --all
# These two searches are slightly different, so be sure to run them both

# Replace all instances of 'internal.host.gov' with 'INTERNAL'
git filter-branch --force --tree-filter "find . -type f -exec grep -I -l -q . {} \; -print0 | xargs -0 sed -i '' 's/internal\.host\.gov/INTERNAL/g'" --tag-name-filter cat -- --all

# Take out the trash
git for-each-ref --format="delete %(refname)" refs/original | git update-ref --stdin
git reflog expire --expire=now --all
git gc --prune=now

# Re-run searches to ensure the string is no longer found
git log -Sinternal.host.gov --all
git log -Ginternal.host.gov --all

# Force push the updated code to your remote(s)
git push <remote> <branch> --force-with-lease

# Remember to tell your collaborators to clone a fresh copy!
```

### Commit messages

```bash
# Find instances of 'internal.host.gov' in any commit message on any branch
git log --grep='internal.host.gov' --all

# Replace all instances of 'internal.host.gov' with 'INTERNAL'
git filter-branch -f --msg-filter 'sed "s/internal.host.gov/INTERNAL/"'

# Take out the trash
git for-each-ref --format="delete %(refname)" refs/original | git update-ref --stdin
git reflog expire --expire=now --all
git gc --prune=now

# Force push the updated code to your remote(s)
git push <remote> <branch> --force-with-lease

# Remember to tell your collaborators to clone a fresh copy!
```
