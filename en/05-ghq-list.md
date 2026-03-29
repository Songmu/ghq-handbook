# Get the list and paths of local repositories (`ghq list`)

The `ghq list` command shows a list of local repositories. `ghq list --full-path` displays the list with full paths.

Combining this result with an interactive filter tool such as `peco` or `fzf`, you can instantly move to the desired local repository. Here is the simplest example using `peco`:

```console
% cd "$(ghq list --full-path | peco)"
```

Below is a slightly more advanced example, which is the actual `zsh` configuration I use.

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

Press `Ctrl-]` to display the list of repositories and navigate to the selected repository.

Once you select a repository from `ghq list`, pass the result again as in `ghq list --full-path --exact $repo` and to get the full path.

Even if you don't use the filter tool, you can easily change directories by defining the `ghq-cd` function in the shell as follows. It can be used as `ghq-cd ghq` etc.

```shell
ghq-cd() {
    if [ -n "$1" ];
        dir="$(ghq list --full-path --exact "$1")"
        if [ -z "$dir" ]; then
            echo "no directries found for '$1'"
            return 1
        fi
        cd "$dir"
        return
    fi
    echo 'usage: ghq-cd $repo'
    return 1
}
```

## Pass search arguments to `ghq list`

By default, `ghq list` will list everything in your local repository, but you can pass query arguments to search.

```console
% ghq list <query>
```

`ghq` will then list repositories that partially match the query string. Also, if the query string starts with a string like `<hostname>/`, it will search the repositories under that host.

- List repositories containing `motemen`
    - `ghq list motemen`
- List repositories under `github.com`
    - `ghq list github.com/`

## Match the query exactly (`--exact`)

If you specify this option, repositories that match the suffix of the query and have an exact subpath match will be listed.

For example, `ghq list go` will list a large number of repositories that include "go" in the project or owner, but `ghq list --exact go` will only list the repositories that end with "go". Below is an example I have on hand:

```console
% ghq list --exact go
cloud.google.com/go
9fans.net/go
github.com/golang/go
github.com/ugorji/go
```

You can further filter by including owner:

```console
% ghq list --exact golang/go
github.com/golang/go
```

If `--exact` is not specified here, partial matching will behave as follows:

```console
% ghq list golang/go
github.com/golang/go
github.com/golang/gofrontend
```

Furthermore, if you include the host and do `ghq list --full-path --exact github.com/x-motemen/ghq` etc., the full path will be uniquely determined and output, so it is helpful when used in tandem with other tools. This technique is used in the previous `zsh` configuration.

## Filter by VCS (`--vcs`)

You can filter by VCS using the `--vcs` option. The following is an example of narrowing down to only Git repositories and running `git gc` all at once:

```console
% ghq list --full-path --vcs=git | xargs -I@ git -C @ gc --aggressive --prune=now
```

## Display shortest unique project list (`--unique`)

A list of the shortest names whose uniqueness is guaranteed locally is printed. If only the project name is unique, only the project name is output, and if multiple projects with the same name exist locally, the owner is also output. For example:

```console
% ghq list --unique
cloud.google.com/go
golang/go
tcnksm/go-gitconfig
motemen/go-gitconfig
ghq
perl
```

Unfortunately, the author, motemen, has forgotten why this feature is useful...
