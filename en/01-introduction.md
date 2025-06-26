# Introduction

This document describes how to use `ghq` command. This is written by Songmu(Masayuki Matsuki), a maintainer of `ghq`.

Source code of this book is available on <https://github.com/Songmu/ghq-handbook> in Markdown format.

You can purchase Ebook edition on <https://leanpub.com/ghq-handbook>.

## What is `ghq`

`ghq` makes cloning and managing source code repositories easy by these simple ideas;

- `ghq get <target>` to get the source code
    - can specify targets in flexible way
    - can deal with various Version Control System(VCS)s transparently
- ghq forces unique directory hierarchy
    - `{{GHQ_ROOT}}/{{HOST}}/{{PATH}}` style, inspired by `go get`

Go programmers will find this is an application of library-getting rule of `go get` in the times of GOPATH. Not only does ghq let you manage many projects easily, it can let you casually clone OSS repos you come across and prevent you from lose sight of repos cloned to ad-hoc directories. 

By combination with interactive filtering tools like `peco` or `fzf`, you can move to any desired directory blazing fast even if you have hundreds of repos cloned. Furthermore, `ghq` enables you to clone OSS repos casually, letting you do more code-readings. In fact, the author has more than 1k repos cloned locally.

"gh" of `ghq` stands for 'GitHub' or 'git & hg' and "q" stands for 'quick' or 「急」 (hurry, sudden, steep or quick, pronounced "queue"). PROBABLY.

## Environment

`ghq` is developed on GitHub <https://github.com/motemen/ghq>. It is written in Go and works on multi platform. It officially provides 64 bit binary for Linux, macOS and Windows.

This content is confirmed with ghq v1.0.0, macOS 10.15.2 and zsh 5.7.1. Differences come from OSes and shells are removed as much as possible.

## Install `ghq`

`ghq` is available on Homebrew

```console
% brew install ghq
```

You can download executables for other environments from [GitHub Releases](https://github.com/motemen/ghq/releases).
We recommend to use tagged stable build version unless you want to develop `ghq`. If you're a Go developer, you can `go get` or `git clone` the developing repository ( https:github.com/motemen/ghq ) and build yourself.
