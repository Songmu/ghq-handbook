---
title: Fetching Repositories (`ghq get`)
type: tech
published: true
---

# Fetching Repositories (`ghq get`)

`ghq get` takes targets as arguments or via standard input and fetches those repositories.

```console
% ghq get <target> [<target>...]
% cat repolist.txt | ghq get --parallel
```

`ghq` interprets targets sensibly, so you can specify them flexibly.

The basic form of a target is `https://{{Host}}/{{Path}}`. `https://` can always be omitted, and when the host is `github.com`, that can be omitted too. In other words, the following three are equivalent:

```console
% ghq get https://github.com/x-motemen/ghq
% ghq get github.com/x-motemen/ghq
% ghq get x-motemen/ghq
```

Hosts other than GitHub can of course be specified as well.

```console
% ghq get launchpad.net/terminator
```

You can also specify a target as a normal repository URL. URLs with schemes other than http are accepted too.

```console
% ghq get git@github.com:x-motemen/ghq.git
% ghq get svn+ssh://svn.example.com/yourproject
```

With the `ghq get --update` (`-u`) option, a repository is updated to the latest state even if it has already been fetched.

## Using SSH, or fetching private repositories

By default, `ghq` tries to fetch repositories over https. However, you may want to fetch private repositories over SSH, or, even for OSS, push code over SSH. Without going into the details of Git configuration, there are the following options for this:

- Set `url.<base>.insteadOf` to force SSH
- Use an SSH URL as the `ghq get` target
- Use the `ghq get -p` option to switch to fetching over SSH
- Don't use SSH (configure git credentials etc. properly and pull and push over https)

Choose whichever you prefer, but setting up your Git configuration and fetching that way is more consistent, and preferable to fetching over SSH with the `-p` option. The author fetches source code over https and pushes over SSH by setting `url.<base>.pushInsteadOf`.

## Switching the fetch directory per project (`ghq.<base>.root`)

We touched on the `ghq root` setting earlier, but you may want to use different fetch directories and git settings for work code and hobby code. This can be done with the `ghq.<base>.root` setting.

The following example fetches into `~/hobby` by default, and fetches repositories under your company's GitHub organization or your company's VCS host into `~/work`:

```gitconfig
[ghq]
root = ~/hobby

[ghq "https://github.com/mycompany"]
root = ~/work

[ghq "https://myvcs.example.com"]
root = ~/work
```

This is about Git configuration, but combining this with the `includeIf` setting lets you apply a specific gitconfig only to repositories under a specific directory. The following example applies a work gitconfig (`~/work/.gitconfig`) to repositories under `~/work`:

```gitconfig
[includeIf "gitdir:~/work/"]
path = ~/work/.gitconfig
```

As an aside, if the `$GHQ_ROOT` environment variable is set, repositories are fetched under that directory regardless of any other `ghq.root` settings. This can be useful when you want to switch the fetch directory even more dynamically.

## Owner detection with `ghq get {{Project}}`

GitHub repository paths take the form `{{Owner}}/{{Project}}`, but if you pass `ghq get` only a repository name without a slash, as in `ghq get {{Project}}`, `ghq` tries to fill in the owner. For example, in my (Songmu's) case, `ghq get horenso` fetches the `github.com/Songmu/horenso` repository.

The owner is filled in in the following order:

1. `ghq.user`, if set
2. The GitHub username inferred from the local environment
3. The login name (the `$USER` environment variable; on Windows, the `$USERNAME` environment variable)

Step 2 uses the `github.com/Songmu/gitconfig.GitHubUser` function, so refer to it if you want the details.

Incidentally, you may want the owner to be filled in with the repository name instead of yourself. For example, ruby to github.com/ruby/ruby, vim to github.com/vim/vim, peco to github.com/peco/peco, and so on.

If you prefer this behavior, you can switch to it by setting `ghq.completeUser` to false.

```console
% git config --global ghq.completeUser false
```

## Specifying the VCS explicitly (`--vcs`)

`ghq get` tries its best to determine the appropriate VCS for the given target, but you can also specify it explicitly with the `--vcs` option. Here is an example with fossil:

```console
% ghq get --vcs fossil https://www.sqlite.org/src
```

Also, if the VCS used under a particular repository host is fixed, you can configure it with `ghq.<base>.vcs`. The following configuration fetches repositories under `https://svn.example.com` with Subversion:

```gitconfig
[ghq "https://svn.example.com"]
vcs = svn
```

## Looking at the source right after fetching (`--look, -l`)

It is natural to want to look at a repository's contents as soon as you fetch it. The `ghq get --look` option is for that. Inspired by `cpanm --look`, it moves you into the fetched directory after fetching the repository.

The move into the directory happens even if the repository has already been fetched, so it is also handy for a quick look at a local repository. And with `ghq get --look --update <target>`, you can look at the source while updating it to the latest state.

When you are done looking, leave with the `exit` command.

Also, please keep `--look` for temporary glances at source code only, and do not use it to move to your working directory.

The `--look` option starts a separate shell process as a child process to make it look as if you changed directories. As a result, you get unintuitive behavior such as shell settings not being carried over properly, or having to `exit` to go back. Restarting the shell also costs the shell's startup time.

So to move to a working directory, simply `cd` like this:

```console
% cd $(ghq list --full-path --exact x-motemen/ghq)
```

This is explained in the next chapter on `ghq list`, but the standard pattern for moving between directories is to combine it with a filter tool like peco or fzf, so we recommend setting those up.

## Specifying the target as a relative path

A little-known way to specify a `ghq get` target is with a relative path.

```console
% ghq get ../projB
```

This is useful, for example, when you are working in `github.com/<your-org>/projA` and want to fetch another repository under the same organization (projB in this example).

## Other `ghq get` options

`ghq get` has several options not covered so far. Finally, here they are all together:

- `--parallel, -P`
    - When there are multiple targets, fetch them in parallel. When this option is given, failures to fetch some repositories only print the error and do not abort the command. How to make use of this option is covered separately in a later chapter.
- `--shallow`
    - Fetch the repository quickly by fetching only the latest history. Supported only for Git, git-svn, and Darcs.
- `--branch, -b`
    - Fetch a specified branch. Supported only for Git, git-svn, Mercurial, and Subversion. With Git in particular, the `git clone --single-branch` option is used, so fetching can be expected to be fast.
- `--no-recursive`
    - With Git, submodules are fetched automatically and recursively by default; this option suppresses that.
- `--silent, -s`
    - Reduce `ghq`'s console output. Specifically, it suppresses the display of external command output. Defaults to false, but is forced to true when the `--parallel` option is given, so that outputs don't get mixed together.
