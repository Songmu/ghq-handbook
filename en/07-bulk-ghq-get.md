# Recipe collection for batch retrieval of repositories (`STDIN | ghq get`)

`ghq get` can also receive a target list from the standard input, which is useful for batch retrieval. It is recommended to use the `--parallel` (`-P`) option to obtain data in parallel at high speed. Here are a few ways this can be leveraged.

## Bulk update

To update the locally-managed repositories to the latest state all at once:

```console
% ghq list | ghq get --update --parallel
```

## Migrate repositories to another machine

If you want to move your repositories from one environment to another, you can save the output of `ghq list` and feed it to `ghq get` on another machine to duplicate the setup:

```console
% ghq list > repolist.txt
% cat repolist.txt | ghq get --parallel
```

## Collect submodules separately

If you want to fetch each Git submodule in a repository individually, you can do so with the following one-liner:

```console
% cat .gitmodules | grep '\turl = ' | cut -d' ' -f 3 | ghq get --parallel
```

## Get the dependencies of a Go project all at once

We are moving to the Go Modules era, and source repositories are no longer placed under `$GOPATH/src`. Even if your project supports Go Modules, you can get all dependent modules at once with the following one-liner:

```console
% go list -f '{{join .Deps "\n"}}' | xargs go list -f '{{if not .Standard}}{{.ImportPath}}{{end}}' | ghq get --parallel
```

Interestingly enough, `ghq` was originally created based on Go's package acquisition rules, but now `ghq` is needed to acquire Go libraries in the same fashion.

## Collect all starred repositories on GitHub

Using [github-list-starred](https://github.com/motemen/github-list-starred):

```console
% go get github.com/motemen/github-list-starred
% github-list-starred Songmu | ghq get --parallel
```

By the way, `github-list-starred` allows you to specify any user and list repositories starred by others, so it is widely useful with `ghq get`.

## Collect GitHub repositories added to Pocket

Using [go-pocket](https://github.com/motemen/go-pocket):

```console
% github.com/motemen/go-pocket/cmd/pocket
% pocket list --domain=github.com | ghq get --parallel
```

Using this feature, if you have something that outputs a list of repositories, you can easily fetch them all in bulk. There are countless ways to leverage this functionality; use your imagination!
