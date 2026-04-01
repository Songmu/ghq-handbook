---
title: Setting the repository acquisition directory (ghq root)
type: tech
published: true
---

# Setting the repository acquisition directory (`ghq root`)

`ghq` places the repository under `$HOME/ghq` by default. This can be changed.

`ghq` uses `gitconfig` for configuration. `ghq.root` is the key for the repository acquisition directory. If you are a Go programmer, the repository is located under `$GOPATH/src`, and can be set up as follows:

```console
% git config --global ghq.root ' ~/go/src '
```

At this point, the following settings should have been added to `.gitconfig`:

```gitconfig
[ ghq ]
root  =  ~/go/src
```

This way`ghq get` will retrieve the repository in`~/go/src` and you can list it with `ghq list`.

However, there may be cases where it is difficult to mix Go code and code from other languages. For such cases, multiple `ghq.root` settings can be made.

```gitconfig
[ ghq ]
root  =  ~/go/src
root  =  ~/myprojects
```

If multiple directories are set, the last directory written will be the primary one, and `go get` will retrieve the repository in that directory. With this setting,`ghq get` will retrieve the repository under `~/myprojects`, and `ghq list` will list the repositories under both `~/myprojects` and `~/go/src`.

You can check this setting with the `ghq root` command:

```console
% ghq root
/Users/Songmu/myprojects
```

`ghq root` will output only the one with the highest priority, but if multiple items are set `ghq root --all` will output all.

```console
% ghq root --all
/Users/Songmu/myprojects
/Users/Songmu/go/src
```

The `ghq.<base>.root` settings for are also listed by `ghq root --all`. `ghq.<base>.root` will be covered in the next chapter.
