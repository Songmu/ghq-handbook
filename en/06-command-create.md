---
title: Create a local repository ( ghq create)
type: tech
published: true
---

# Create a local repository (`ghq create`)

You can also create a local repository according to ghq rules. Create a directory and initialize the repository:

```console
% ghq create <target>
```

The target specification rules are almost the same as `ghq get`, but when using `ghq create {{Project}}` the owner will be added, regardless of the `ghq.completeUser` setting.

# VCS specification

ghq create initializes the repository with the appropriate VCS based on the settings in ghq.<base>.vcs, but it can also be specified explicitly using the --vcs option.

```console
% ghq create --vcs hg yourhg.example.com/project
```

The functions of `ghq` have been explained - next applied usage will be discussed.
