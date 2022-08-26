# Introduction

This document describes the usage of the command line tool, `ghq`. It is written and maintained by Songmu (Masayuki Matsuki): <https://github.com/Songmu>.

The source code of this document is available as Markdown on GitHub: <https://github.com/Songmu/ghq-handbook>.

It can also be purchased as an e-book on LeanPub: <https://leanpub.com/ghq-handbook>.

## What is `ghq`?

`ghq` makes cloning and managing source code repositories easy using these simple ideas:

- `ghq get <target>` retrieves a repository
    - Targets can be specified in a flexible way
    - Various different Version Control Systems (or VCSs) can handled transparently
- `ghq` enforces a unique directory hierarchy
    - It follows a style of `{{GHQ_ROOT}}/{{HOST}}/{{PATH}}`, inspired by that of `go get`

Go programmers will recognize this as an application of the library-fetching rule of `go get` based on [`GOPATH`](https://golangr.com/what-is-gopath). Not only does it make it easier to manage multiple projects, it allows you to more easily clone the OSS repositories you come across and prevents you from losing track of repositories cloned to ad-hoc directories.

Through the use of interactive filtering tools like `peco` or `fzf`, you can move to any desired directory at a blazing pace even if you have hundreds of repositories cloned. Furthermore, since `ghq` enables you to more easily clone OSS repositories, it augments code reading. As a matter of fact, the author has more than a thousand repositories cloned locally!


The *gh* of `ghq` stands for "GitHub" or "git & hg", whereas the *q* stands for "quick" or 「急」 (hurry, sudden, steep or quick, pronounced "queue"). Something like that...

## Environment

`ghq` is developed on GitHub <https://github.com/motemen/ghq>. It is written in [Go](https://go.dev) and runs on all major platforms. It officially provides 64-bit binary for Linux, MacOS and Windows.

The content of this handbook has been verified using `ghq` v1.0.0, macOS 10.15.2 and zsh 5.7.1. Differences between OSes and shells are accounted for as much as possible.

## Install `ghq`

### Package Manager Overview
[![Packaging status](https://repology.org/badge/vertical-allrepos/ghq.svg)](https://repology.org/project/ghq/versions)

### Homebrew

```console
% brew install ghq
```

### Scoop (Windows)

```console
% scoop install ghq
```

### AUR

```console
% yay -S ghq
```

### NixOS

```console
% nix-env -iA nixpkgs.ghq
```

Alternatively, you can download an executable for your environment from the [GitHub Releases](https://github.com/x-motemen/ghq/releases) page. This can be trivially accomplished with [eget](https://github.com/zyedidia/eget):

```console
% eget x-motemen/ghq
```

We recommend using a tagged, stable build version unless you want to contribute to the development of `ghq`.

If you are a contributor or want to build from source, you can `go get` or `git clone` the development repository.
