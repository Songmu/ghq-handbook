---
title: Configuring the Repository Directory (`ghq root`)
type: tech
published: true
---

# Configuring the Repository Directory (`ghq root`)

By default, `ghq` places repositories under `$HOME/ghq`. This can be changed.

`ghq` uses gitconfig for its configuration. `ghq.root` is the setting for the directory where repositories are fetched. If you are a Go programmer, repositories under `$GOPATH/src` are already laid out the way `ghq` expects, so try configuring it like this:

```console
% git config --global ghq.root '~/go/src'
```

Your `.gitconfig` should now contain the following:

```gitconfig
[ghq]
root = ~/go/src
```

With this, `ghq get` fetches repositories into `~/go/src`, and `ghq list` lists them.

However, you may not want Go code mixed with code in other languages. For that case, `ghq.root` can be set multiple times.

```gitconfig
[ghq]
root = ~/go/src
root = ~/myprojects
```

When multiple values are set, the directory written last becomes the primary one, and `ghq get` fetches repositories into that directory. With this configuration, `ghq get` fetches repositories into `~/myprojects`, and `ghq list` lists repositories under both `~/myprojects` and `~/go/src`.

You can check this configuration with the `ghq root` command.

```console
% ghq root
/Users/Songmu/myprojects
```

`ghq root` outputs only the single highest-priority entry; when multiple are configured, `ghq root --all` outputs all of them.

```console
% ghq root --all
/Users/Songmu/myprojects
/Users/Songmu/go/src
```

`ghq.<base>.root` settings are also listed by `ghq root --all`. `ghq.<base>.root` is covered in the next chapter on `ghq get`.
