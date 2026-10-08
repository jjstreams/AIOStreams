# AGENTS.md

JJstreams' fork of [Viren070/AIOStreams](https://github.com/Viren070/AIOStreams),
carrying JJstreams-specific changes on top of upstream. The fork is **public**,
so everything written into its history — commit authors, branch names, commit
messages — is public too.

`CLAUDE.md` is upstream's file and describes the project itself. This file is
fork-only and describes how to work *in the fork*.

## Commit identity

Every commit here is authored and committed by the shared `jjstreams` account.
A maintainer's personal account must never appear in this repository: the author
line publishes their name and address, and it subscribes them as a participant
on every pull request that carries the commit.

Git config is not part of the repository and a fresh clone inherits whatever
identity the machine has globally, so pin it per clone, before the first commit:

```sh
git config user.name jjstreams
git config user.email 339098993+jjstreams@users.noreply.github.com
```

Verify rather than assume — `git config user.email` — and do it again after
re-cloning or after `git submodule update`, which both drop local config.

Pushing goes through the same account (`gh auth switch --user jjstreams`). The
account that pushes is independent of the identity recorded inside the commit,
so both need checking: a push as `jjstreams` still publishes a personal author
line if the local config was wrong.

Before every push or PR operation, verify `gh api user --jq .login` returns
`jjstreams`. Stop if it names another account. For commands that must retain
their identity while other repositories use GitHub CLI, scope `GH_TOKEN` to
that command using `gh auth token --hostname github.com --user jjstreams`;
never print or save that token. Account switching is global to GitHub CLI,
not local to this checkout.

If the wrong identity is already in a commit that has not been pushed, rewrite
it: `git commit --amend --reset-author --no-edit`. Once it is pushed, the name
stays visible in the pull request and its timeline even after a force-push, and
only deleting and recreating the repository removes it completely.

## Branch names

Branch names are public as well, so they carry no personal initials or names.
Describe the change instead: `add-safe-playback-logging`, not
`JEN-add-safe-playback-logging`.

## Pull requests

The base is always this fork's own `main`. Inside a fork, `gh pr create`
defaults to the upstream parent, which would propose JJstreams-internal changes
to `Viren070/AIOStreams`. Pin the repository explicitly:

```sh
gh pr create --repo jjstreams/AIOStreams --base main --head <branch>
```
