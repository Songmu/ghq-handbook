# Basic Usage

This chapter explains the basic of using `ghq`. The contents of this chapter encompass a level of proficiency most will already have when they start using `ghq`.

The main functionality of `ghq` revolves around "getting repositories" and "listing retrieved repositories".

## Get a repository - `ghq get`

To learn by example, you can use `ghq get` to retrieve the source repository for `ghq` as follows:

```console
% ghq get x-motemen/ghq
```

## List local repostiories - `ghq list`

You can use `ghq list` to get a list of repositories formatted as `{{HOST}}/{{PATH}}`.

```console
% ghq list
github.com/x-motemen/ghq
```

The `--full-path` option can be used to list the repositories by their absolute path.

```console
% ghq list --full-path
/Users/Songmu/ghq/github.com/x-motemen/ghq
```

In the example above, notice that the repository is stored in `/Users/Songmu/ghq`; you can view where `ghq` is cloning repositories with `ghq root`:

```console
% ghq root
/Users/Songmu/ghq
```

As is apparent, `$HOME/ghq` is the default storage directory. This can be changed in the settings.

## Do everything with `ghq get`

The examples above do not do the justice of highlighting the convenience offered by `ghq`. However, once you start using it regularly, you'll wonder how you were ever content with just using `git clone`.

While Git and GitHub are the primary targets for `ghq`, it can retrieve from any repository hosting service and supports the following Source Code Management (SCM) tools:

- [Git](https://git-scm.com/) (`git`)
- [Mercurial](https://www.mercurial-scm.org/) (`hg`)
- [Subversion](https://subversion.apache.org/) (`svn`)
- [git-svn](https://git-scm.com/docs/git-svn) (`git-svn`)
- [Bazaar](http://bazaar.canonical.com/) (`bzr`)
- [Fossil](https://fossil-scm.org/) (`fossil`)
- [Darcs](https://darcs.net/) (`darcs`)

We recommend that you get and manage all of your code using `ghq`, whether it's public, private, for work, a hobby, or anything else.

* * *

In the chapters following, we will go into more detail about each command. Rather than exhaustively describing each of them, we will focus on their primary use cases. When discussing command-line options, the option names will be in long form, but some also provide short options. see the command-line help (`ghq -h`) for more information.
