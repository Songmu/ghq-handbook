---
title: Listing Local Repositories and Getting Their Paths (`ghq list`)
type: tech
published: true
---

# Listing Local Repositories and Getting Their Paths (`ghq list`)

The `ghq list` command displays a list of local repositories. `ghq list --full-path` displays the list as full paths.

Combine this output with an interactive filter tool such as peco or fzf, and you can jump to the local repository you want instantly. Here is the simplest example using peco:

```console
% cd "$(ghq list --full-path | peco)"
```

Here is a slightly more advanced example: the zsh configuration the author actually uses.

```zsh
peco-src () {
    local repo=$(ghq list | peco --query "$LBUFFER")
    if [ -n "$repo" ]; then
        repo=$(ghq list --full-path --exact $repo)
        BUFFER="cd ${repo}"
        zle accept-line
    fi
    zle clear-screen
}
zle -N peco-src
bindkey '^]' peco-src
```

Press `Ctrl-]` to display the list of repositories and move to the one you select.

It first selects a repository from the non-full-path list produced by plain `ghq list`, then passes the result as an argument to `ghq list`, as in `ghq list --full-path --exact $repo`, calling `ghq list` again to get the full path.

Even without a filter tool, defining a `ghq-cd` function in your shell as below makes moving between directories easy. Use it like `ghq-cd ghq`.

```shell
ghq-cd () {
    if [ -n "$1" ]; then
        dir="$(ghq list --full-path --exact "$1")"
        if [ -z "$dir" ]; then
            echo "no directories found for '$1'"
            return 1
        fi
        cd "$dir"
        return
    fi
    echo 'usage: ghq-cd $repo'
    return 1
}
```

## Passing a search argument to `ghq list`

By itself, `ghq list` lists all local repositories, but you can also pass it a query argument for searching.

```console
% ghq list <query>
```

`ghq` then lists repositories that partially match the query string. If the query string starts with something like `<hostname>/`, the search is restricted to repositories under that host.

- List repositories containing `motemen`
    - `ghq list motemen`
- List repositories under `github.com`
    - `ghq list github.com/`

## Matching the query strictly (`--exact`)

The behavior of the `--exact` option is hard to explain, but when it is given, repositories are listed whose paths end with the query and whose subpaths match it exactly.

For example, `ghq list go` lists a large number of repositories whose project or owner contains "go", but `ghq list --exact go` lists repositories whose project is exactly "go". Here is an example from the author's machine:

```console
% ghq list --exact go
cloud.google.com/go
9fans.net/go
github.com/golang/go
github.com/ugorji/go
```

You can narrow it down further by including the owner.

```console
% ghq list --exact golang/go
github.com/golang/go
```

Without `--exact`, partial matching gives the following:

```console
% ghq list golang/go
github.com/golang/go
github.com/golang/gofrontend
```

And if you include the host as well, as in `ghq list --full-path --exact github.com/x-motemen/ghq`, the full path is uniquely determined and output, which is very handy for integrating with other tools. This is the technique used in the zsh configuration above.

## Filtering by VCS (`--vcs`)

You can filter by VCS with the `--vcs` option. The following example narrows the list down to Git repositories only and runs `git gc` on all of them:

```console
% ghq list --full-path --vcs=git | xargs -I@ git -C @ gc --aggressive --prune=now
```

## Listing the shortest unique project names (`--unique`)

This outputs a list of the shortest names that are guaranteed to be unique locally. If the project name alone is unique, only the project name is output; if there are projects with the same name from multiple owners locally, the owner is output too. For example:

```console
% ghq list --unique
cloud.google.com/go
golang/go
tcnksm/go-gitconfig
motemen/go-gitconfig
ghq
perl
```

By the way, apparently even motemen, the original author, doesn't remember what this feature is useful for.
