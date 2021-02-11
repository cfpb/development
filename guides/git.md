# Git Best Practices

CFPB uses the Git version control system for tracking changes to our code and documentation.
GitHub.com is used to share and manage our Git repositories.

## Signing commits using GPG

![GitHub verified commit](https://raw.githubusercontent.com/cfpb/development/main/img/github-verified.png)

GPG keys are a way to sign and verify work from trusted collaborators.
You can generate a GPG key and add the public key to your account by following
[GitHub's guide](https://help.github.com/articles/signing-commits-with-gpg/).

### Why is commit signing important?

Anyone can make a commit to a codebase and attribute it to you:
`git commit --author='Jane Doe <jane@doe.com>' -m 'Really evil malicious change'`.
It will show up in the project's [log](https://github.com/cfpb/design-system/commits/main)
with your name and smiling face (assuming your GitHub email address is `jane@doe.com`).

To protect yourself and prove to your peers that you are the author of a given commit,
you can sign the commit using your GPG key.
Assuming your development machine is not compromised and you generated your key with a strong passphrase,
your colleagues can rest assured any commits with a "verified" badge next to them were written by you.
For more info read Mike Gerwitz's "[A Git Horror Story: Repository Integrity With Signed Commits](https://mikegerwitz.com/papers/git-horror-story)".

## Writing good commit messages

CFPB's [dotfiles](https://github.com/cfpb/dotfiles) include a git commit message template that encourages some
[best practices](https://chris.beams.io/posts/git-commit/).

### How to use the message template

Download the git commit message template to your home directory:

```sh
curl https://raw.githubusercontent.com/cfpb/dotfiles/main/.gitmessage -o ~/.gitmessage
```

Tell git to use it:

```
git config --global commit.template ~/.gitmessage
```

Whenever you create a new commit, use `git commit` (opposed to `git commit -m "Message"`).
Git will open the message template in your [default editor](https://help.github.com/articles/associating-text-editors-with-git/).

### Example

The template looks like this:

```sh
# 1. If applied, this commit will...


# 2. Explain why this change is being made


# 3. Provide links to any relevant issues, articles or other resources


#-----------------------------------------------@----------------------#
#
# 1. Commit summary. Keep it under 50 characters. @-symbol above is a
#    marker for that. Don't end it with a period. Imperative mood.
#
# 2. Description of commit should explain WHY a change was made and
#    lines should be wrapped to 72 characters; marked by the #-symbol.
#
# 3. Optionally, reference a GH issue number or other links.
#
# For more tips: http://chris.beams.io/posts/git-commit
#
```

Here's an example of a complete message:

![Commit message screenshot](https://raw.githubusercontent.com/cfpb/development/main/img/commit-message.png)
