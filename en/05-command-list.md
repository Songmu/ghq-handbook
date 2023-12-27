---
title: Get a list and path of local repositories (ghq list)
type: tech
published: true
---

# Get the list and path of local repositories (`ghq list`)

The command `ghq list` displays a list of local repositories. `ghq list --full-path` displays the list with the full path.

If you combine this result with an interactive filter tool such as peco or fzf, you can instantly navigate to the desired local repository. Below is the simplest example using peco.

```console
% cd  "$( ghq list --full-path | peco )"
```

The following is a slightly more advanced example of the zsh settings that I actually use.

```zsh
peco-src () {
    local repo= $(ghq list | peco --query "$LBUFFER")
    if [ -n "$repo" ] ;  then
        repo= $(ghq list --full-path --exact $repo)
        BUFFER= "cd ${repo}"
        zle accept-line\n
    fi
    zle clear-screen
}
zle -N peco-src
bindkey '^]' peco-src
```

Press `Ctrl-]` to display a list of repositories and move to the selected repository.

Once you select a repository from a list that is not a full path with plain `ghq list`, pass the result to the argument of `ghq list` like `ghq list --full-path --exact $repo`. Then call `ghq list` again to get the full path.

Even without using a filter tool, you can easily move directories by defining a `ghq-cd` function in the shell as shown below. It can be used as `ghq-cd ghq`.

```shell
ghq-cd () {
    if [ -n  "$1" ] ;  then
        dir= "$(ghq list --full-path --exact "$1")"
        if [ -z  "$dir" ] ;  then
            echo  " no directories found for '$1'"
            return 1
        fi
        cd "$dir"
        return
    fi
    echo  'usage: ghq-cd $repo'
    return 1
}
```

## Passing search arguments to `ghq list`

By default, `ghq list` lists all items in the local repository, but you can also pass query arguments for searching.

```console
% ghq list <query>
```

In this example, `ghq` will list repositories that partially match the query string. Also, if the query string starts with a string such as `<hostname>/`, it will search the repositories under that host.

- List repositories containing `motomen`
  - ghq list motemen
- List the repositories under `github.com`
  - ghq list github.com/

# Match queries exactly (`--exact`)

The behavior of the `--exact` option is difficult to explain, but if you specify this option, repositories that match the query from the end and whose subpaths also match exactly are listed.

For example, if you run `ghq list go`, a large number of repositories whose project or owner includes "go" will be listed, but if you run `ghq list --exact go` you will see a list of repositories whose project exactly matches "go". Here is an example:

```console
% ghq list --exact go
cloud.google.com/go
9fans.net/go
github.com/golang/g
github.com/ugorji/go
```

You can further narrow down your search by including the owner.

```console
% ghq list --exact golang/go
github.com/golang/go
```

If you do not specify here --exact, a partial match will result as shown below.

```console
% ghq list golang/go
github.com/golang/go
github.com/golang/gofrontend
```

Furthermore, if you include the host `ghq list --full-path --exact github.com/x-motemen/ghq`, the full path will be uniquely determined and output, which is useful when collaborating with other tools. This is the technique used in the zsh configuration above.

## Filter using VCS (`--vcs`)

You can use filter by **VCS** using the `--vcs` option. The following is an example of narrowing down to only Git repositories and applying `git gc` to them all at once.

```console
% ghq list --full-path --vcs=git | xargs -I@ git -C @ gc --aggressive --prune=now
```

## Display the shortest unique project list (`--unique`)

A list of the shortest locally guaranteed unique names will be output. If only the project name is unique, only the project name will be output; if there are local projects with the same name with multiple owners, the owner will also be output. For example:

```console
% ghq list --unique
cloud.google.com/go
golang/go
tcnksm/go-gitconfig
motemen/go-gitconfig
ghq
perl
```

By the way, the author, motemen, doesn't seem to remember where this feature is useful.
