---
title: Basic usage
type: tech
published: true
---

# Basic usage

This chapter explains the basic usage of `ghq`. The content of this chapter alone will get you to a level where you won't have any trouble using it on a daily basis.

The `ghq` main functions are only `obtaining repositories` and `listing the obtained repositories`.

## Get the repository `ghq get`

You can use `ghq get` to get the source code of `ghq` using:

```console
% ghq get x-motemen/ghq
```

## List local repositories `ghq list`

You can get a list of your repositories in {{HOST}}/{{PATH}} format with `ghq list`.

```console
% ghq list
github.com/x-motemen/ghq
```

You can get a list of full paths by adding the `--full-path` option.

```console
% ghq list --full-path
/Users/Songmu/ghq/github.com/x-motemen/ghq
```

In the above example, the source code is placed below `/Users/Songmu/ghq`. This location can be displayed using the command `ghq root`.

```console
% ghq root
/Users/Songmu/ghq
```

This location is set to `$HOME/ghq` by default. Of course, this can be changed in the settings.

## `ghq get` everything

From the explanation so far, the convenience of `ghq` may not be clear to you. But once you realize how useful it is, you won't be doing `git clone` anymore, and will be using `ghq get` to get your repositories.

The target of `ghq` is not limited to Git and GitHub. Although it is made more convenient for Git and GitHub, it can retrieve source code from any repository host other than GitHub, and supports the following VCSs:

- Git (git)
- Mercurial (hg)
- Subversion (svn)
- git-svn (git-svn)
- Bazaar (bzr)
- Fossil
- Darcs (darcs)
  We recommend that you use ghq to acquire and manage all public and private, work and hobby codes, Git and other code.

We will provide a detailed explanation of `ghq` in the following chapters. Rather than exhaustively explaining each command, each major use case will be explained. When discussing command line options, the option names are always long options, but some options have short options. See the command line help (`ghq -h`) for more details.
