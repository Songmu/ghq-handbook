---
title: Introduction
type: tech
published: true
---

# Introduction

This document describes how to use command line tool `ghq`. This document is written by Songmu (Masayuki Matsuki), the maintainer of `ghq`. The English translation is by paul-jewell (Paul Jewell).

The source code for this document is available in Markdown format from the repository https://github.com/Songmu/ghq-handbook.

You can also purchase an e-book version of this document at https://leanpub.com/ghq-handbook.

## What is `ghq`

`ghq`simplifies the acquisition and management of source code repositories. This is due to the following simple idea:

- `ghq get <target>` Easily get the source code for anything
  - Flexible targeting method
  - Transparently handle multiple Version Control Systems (VCS)
- Enforcing a unique directory structure
  - `go get` layout is based on {{GHQ_ROOT}}/{{HOST}}/{{PATH}}

It will be easy for Go programmers to understand that this `go get` is an application of the library acquisition rules from the GOPATH to things other than Go. By using `ghq`, you will not only be able to manage multiple projects easily, but you will also be able to easily clone the OSS you are interested in, and you will not have to clone the repository to an ad hoc directory and lose track of it.

When `ghq` is combined with interactive filter tools such as `peco` and `fzf`, you can instantly navigate to the desired repository, even if you have hundreds of repositories at hand. Furthermore, since you can casually clone OSS at hand, there is a nice side effect that reading the source code becomes easier. In fact, the author has over 1000 repositories.

In `ghq`, \"gh\" means GitHub (git or hg), and \"q\" means quick or "sudden".

## Operating environment

ghqis developed in the repository https://github.com/x-motemen/ghq on GitHub. It is written in Go and has been tested on multiple OSs. Officially provides 64-bit binary builds for Linux, macOS, and Windows.

When writing this document, we mainly checked using ghq v1.0.0, macOS 10.15.2, and zsh 5.7.1, but we have taken care to minimize differences depending on the OS and shell environment.

## `ghq` Installation

It can be installed with Homebrew.

% brew install ghq

For other environments, you can obtain the executable from GitHub Releases.

If you have a Go development environment, you can use `go get` to fetch the development repository (https://github.com/x-motemen/ghq) or `git clone` and build it yourself, but unless you are a developer of `ghq`, we recommend using a stable build.
