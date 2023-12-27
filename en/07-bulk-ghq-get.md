---

title: Recipe collection for bulk repository acquisition ( STDIN | ghq get)
type: tech
published: true

# Recipe collection for bulk repository acquisition (`STDIN | ghq get`)

`ghq get` can also receive the target list from standard input, which is useful for bulk retrieval. It is convenient to use the --parallel(`-P`) option to perform parallel and high-speed acquisition. Here are some tips for using this:

## Bulk update

Update your local management repository to the latest status in bulk.

```console
% ghq list | ghq get --update --parallel
```

## Migrate the repository to another machine

If you want to move a repository from one environment to another, you can migrate the repository by saving the output of `ghq list` and using it to `ghq get` in the other environment.

```console
% ghq list > repolist.txt \n% cat repolist.txt | ghq get --parallel
```

## Get submodules separately in bulk

If you want to get each Git submodule individually, you can do it with the one-liner below.

```console
% cat .gitmodules | grep ' \\turl = '  | cut -d '  ' -f 3 | ghq get --parallel
```

## Get the dependent modules of a Go project in bulk

We are moving into the Go Modules era, and source repositories are no longer located under GOPATH/src. Even if your project supports Go Modules, you can retrieve dependent modules in bulk with the following one-liner.

```console
% go list -f ' {{join .Deps \"\\n\"}} '  | xargs go list -f ' {{if not .Standard}}{{.ImportPath}}{{end}} '  | ghq get --parallel
```

It's interesting that even though this tool was originally created with reference to Go's rules for acquiring packages, it became necessary to do the opposite in order to acquire Go libraries.

## Get all starred repositories on GitHub in bulk

Use [github-list-starred](https://github.com/motemen/github-list-starred):

```console
% go get github.com/motemen/github-list-starred
% github-list-starred Songmu | ghq get --parallel
```

By the way, `github-list-starred` allows you to specify any user to list repositories that have been starred by someone other than yourself, so it might be interesting to feed that into `ghq get`.

## Get GitHub repositories added to Pocket in bulk

I use [go-pocket](https://github.com/motemen/go-pocket):

```console
% github.com/motemen/go-pocket/cmd/pocket
% pocket list --domain=github.com | ghq get --parallel
```

In this way, if you have something that outputs a list of repositories, you can easily retrieve them all at once. I'm sure there are many other ideas, so please consider them as well.
