---
title: Creating Local Repositories (`ghq create`)
type: tech
published: true
---

# Creating Local Repositories (`ghq create`)

You can also create local repositories following ghq's rules. It creates the directory and initializes the repository.

```console
% ghq create <target>
```

The rules for specifying the target are almost the same as for `ghq get`, but when owner completion happens with `ghq create {{Project}}`, the owner is always filled in with yourself, regardless of the `ghq.completeUser` setting.

## Specifying the VCS

`ghq create` initializes the repository with the appropriate VCS, determined from settings such as `ghq.<base>.vcs`, but you can also specify it explicitly with the `--vcs` option.

```console
% ghq create --vcs hg yourhg.example.com/project
```

* * *

That covers all of `ghq`'s features. The following chapters cover more advanced usage.
