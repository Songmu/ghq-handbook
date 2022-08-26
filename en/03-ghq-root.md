# Setting the target repository directory (`ghq root`)

`ghq` clones repositories under `$HOME/ghq` by default. This is configurable.

`ghq` will use the `ghq.root` field in your git config to determine where to clone repositories.

If you are a Go programmer, repositories are typically located under `$GOPATH/src` with the layout expected by `ghq`, so let's set it as follows:

```console
% git config --global ghq.root '~/go/src'
```

After running the above command, your `~/.gitconfig` should look like this:

```gitconfig
[ghq]
root = ~/go/src
```

With this in place, `ghq get` will clone repositories to `~/go/src`, and `ghq list` will list them as expected.

However, mixing Go code with code from other languages ​​can be problematic. For such cases, `ghq.root` can be set multiple times.

```gitconfig
[ghq]
root = ~/go/src
root = ~/myprojects
```

If `ghq.root` is set multiple times, the last value will be used the primary directory. With the above settings, `ghq get` will clone repositories to `~/myprojects`, and `ghq list` will list repositories under both `~/myprojects` and `~/go/src`.

You can check which directory is being used with `ghq root` command:

```console
% ghq root
/Users/Songmu/myprojects
```

`ghq root` will only output the directory with the highest priority, but if multiple are set, `ghq root --all` will output all directories.

```console
% ghq root --all
/Users/Songmu/myprojects
/Users/Songmu/go/src
```

The `ghq.<base>.root` setting is also listed with `ghq root --all`. `ghq.<base>.root` will be covered in the next chapter, `ghq get`.
