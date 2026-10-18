---
title: Basic Usage
type: tech
published: true
---

# Basic Usage

This chapter covers the basic usage of `ghq`. The contents of this chapter alone should be enough for everyday use.

The main features of `ghq` are just two: "fetching repositories" and "listing fetched repositories".

## Fetching a repository with `ghq get`

You can fetch the source code of `ghq` itself with `ghq get` as follows:

```console
% ghq get x-motemen/ghq
```

## Listing local repositories with `ghq list`

`ghq list` gives you a list of your local repositories in `{{HOST}}/{{PATH}}` form.

```console
% ghq list
github.com/x-motemen/ghq
```

Add the `--full-path` option to get a list of full paths.

```console
% ghq list --full-path
/Users/Songmu/ghq/github.com/x-motemen/ghq
```

In the example above, the source code is placed under `/Users/Songmu/ghq`. You can display this location with the `ghq root` command.

```console
% ghq root
/Users/Songmu/ghq
```

By default, this location is `$HOME/ghq`. It can, of course, be changed in the configuration.

## `ghq get` everything

From the explanation so far, the convenience of `ghq` may not have clicked yet. But once you see how convenient it is, you will never `git clone` again and will fetch every repository with `ghq get`.

`ghq` is not limited to Git and GitHub. It is designed to be especially convenient with Git and GitHub, but it can fetch source code from any repository host besides GitHub, and it supports the following VCSs:

- Git (git)
- Mercurial (hg)
- Subversion (svn)
- git-svn (git-svn)
- Bazaar (bzr)
- Fossil (fossil)
- Darcs (darcs)

Public or private, work or hobby, Git or otherwise: we recommend fetching and managing all of it with ghq.

* * *

The following chapters explain `ghq` in detail. Rather than covering each command exhaustively, they are organized around the main use cases. When command-line options are discussed, option names are given in their long form throughout, but some also have a short form. For those, see the command-line help (`ghq -h`).
