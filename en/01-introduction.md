---
title: Introduction
type: tech
published: true
---

# Introduction

This document explains how to use the command-line tool `ghq`. It was written by Songmu (Masayuki Matsuki), the maintainer of `ghq`.

The source of this document is available in Markdown format from the repository <https://github.com/Songmu/ghq-handbook>.

An e-book edition of this document can be purchased from <https://leanpub.com/ghq-handbook>. The content is also available for purchase at <https://zenn.dev/songmu/books/ghq-handbook>.

> **Translation note:** This is an unofficial English translation of the Japanese original. The original work is Copyright 2020 Masayuki Matsuki and licensed under [CC BY-NC 3.0](http://creativecommons.org/licenses/by-nc/3.0/); this translation is distributed under the same license.

## What is `ghq`?

`ghq` makes it easy to fetch and manage source code repositories. It is built on the following simple ideas:

- Fetch anything easily with `ghq get <target>`
    - Flexible ways of specifying the target
    - Transparent handling of multiple Version Control Systems (VCS)
- Enforce a unique directory structure
    - A `{{GHQ_ROOT}}/{{HOST}}/{{PATH}}` layout modeled after `go get`

Veteran Go programmers may find it easiest to think of it as applying the library-fetching rules of `go get` from the GOPATH era to code other than Go. With `ghq`, managing multiple projects becomes easier, you can casually clone any OSS that catches your interest, and you will no longer lose track of repositories cloned into ad-hoc directories.

Combine `ghq` with an interactive filter tool such as `peco` or `fzf`, and you can jump to the repository you want instantly, even with hundreds of repositories on hand. And because you can casually clone OSS locally, there is a pleasant side effect: reading source code becomes much easier. In fact, the author has more than 1,000 repositories locally.

The "gh" in `ghq` stands for GitHub, or git and hg, and the "q" stands for quick, or "急" (kyū, "hurry"). Probably. More recently, the original author revealed that it might be "Go Hot Kyushu".

## Supported environments

`ghq` is developed in the GitHub repository <https://github.com/x-motemen/ghq>. It is written in Go and has been confirmed to work on multiple operating systems. Official 64-bit binary builds are provided for Linux, macOS, and Windows.

This document was written mainly against ghq v1.0.0, macOS 10.15.2, and zsh 5.7.1, but care has been taken to minimize differences caused by OS and shell environment.

## Installing `ghq`

You can install it with Homebrew.

```console
% brew install ghq
```

In other environments, you can get the executable from [GitHub Releases](https://github.com/x-motemen/ghq/releases).

If you have a Go development environment, you can also `go install` the development repository ( https://github.com/x-motemen/ghq ), or `git clone` it and build it yourself. Unless you are a `ghq` developer, however, we recommend using a stable, tagged build.
