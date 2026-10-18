---
title: Recipes for Fetching Repositories in Bulk (`STDIN | ghq get`)
type: tech
published: true
---

# Recipes for Fetching Repositories in Bulk (`STDIN | ghq get`)

`ghq get` can also receive a list of targets from standard input, which is very handy for fetching in bulk. Here, the `--parallel` (`-P`) option is useful because it fetches in parallel, which is fast. Here are some tips that make use of this.

## Bulk update

Update all the repositories you manage locally to their latest state in one go.

```console
% ghq list | ghq get --update --parallel
```

## Migrating repositories to another machine

To move your repositories from one environment to another, save the output of `ghq list` and feed it to `ghq get` on the other environment.

```console
% ghq list > repolist.txt
% cat repolist.txt | ghq get --parallel
```

## Fetching submodules separately in bulk

If you want to fetch each Git submodule individually, you can do it with the following one-liner:

```console
% cat .gitmodules | grep '\turl = ' | cut -d' ' -f 3 | ghq get --parallel
```

## Fetching all dependency modules of a Go project

With the shift to the Go Modules era, source repositories are no longer placed under GOPATH/src. Even for projects that use Go Modules, the following one-liner fetches all dependency modules:

```console
% go list -f '{{join .Deps "\n"}}' | xargs go list -f '{{if not .Standard}}{{.ImportPath}}{{end}}' | ghq get --parallel
```

It's an amusing twist that `ghq`, a tool originally modeled on Go's package-fetching rules, is now needed to fetch Go libraries.

## Fetching all repositories you have starred on GitHub

Use [github-list-starred](https://github.com/motemen/github-list-starred).

```console
% go get github.com/motemen/github-list-starred
% github-list-starred Songmu | ghq get --parallel
```

By the way, `github-list-starred` accepts any user, so it can also list repositories starred by people other than yourself. Feeding that to `ghq get` might be fun too.

## Fetching all GitHub repositories you have added to Pocket

Use [go-pocket](https://github.com/motemen/go-pocket).

```console
% github.com/motemen/go-pocket/cmd/pocket
% pocket list --domain=github.com | ghq get --parallel
```

As you can see, if you have anything that outputs a list of repositories, you can easily fetch them all at once. There must be many more ideas out there, so please come up with your own.
