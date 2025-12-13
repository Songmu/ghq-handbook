# Fetching repositories (`ghq get`)

`ghq get` takes a target, either as an argument or via standard input, and fetches the given repository.

```console
% ghq get <target> [<target>...]
% cat repolist.txt | ghq get --parallel
```

`ghq` infers the target intelligently, so you can specify it flexibly.

The basic form of a target is `https://{{Host}}/{{Path}}`. `https://` is always optional, and if the host is `github.com` you can also omit it. In other words, the following three commands are equivalent:

```console
% ghq get https://github.com/x-motemen/ghq
% ghq get github.com/x-motemen/ghq
% ghq get x-motemen/ghq
```

Of course, hosts other than GitHub can also be specified:

```console
% ghq get launchpad.net/terminator
```

It is also possible to specify a target with a normal repository URL. Protocols other than https can also be specified:

```console
% ghq get git@github.com:x-motemen/ghq.git
% ghq get svn+ssh://svn.example.com/yourproject
```

If you use the `ghq get --update`(`-u`) option, it will update to the latest commit even if the repository has already previously been cloned.

## Using SSH and cloning a private repositories

`ghq` will try to get the repository via https by default. However, there may be times when you want to get private repositories with SSH, or you want to push code with SSH even if it is OSS. I won't go into the details of Git settings, but there are a few strategies available:

- Force SSH by setting `url.<base>.insteadOf`
- Use an `ssh://` URL as the argument to `ghq get`
- Switch to fetching over SSH using the `ghq get -p` option
- Do not use SSH (set git credentials appropriately and perform pull and push with https)

You can choose your preferred method, but it is preferable to get it by adjusting Git settings rather than using the `-p` option to get SSH.

For example, I use https to clone the source code, and then set `url.<base>.pushInsteadOf` to push via SSH.

## Setting the target directory per project (`ghq.<base>.root`)

I mentioned the `ghq root` setting earlier, but you may want to switch the target directory and git settings between work and hobby code. This can be achieved with the `ghq.<base>.root` setting.

The following is an example of setting to get to `~/hobby` by default, and to `~/work` for repositories under your company's GitHub organization and your company's VCS host.

```gitconfig
[ghq]
root = ~/hobby

[ghq "https://github.com/mycompany"]
root = ~/work

[ghq "https://myvcs.example.com"]
root = ~/work
```

When it comes to Git settings, if you use the `includeIf` setting together, you can apply a specific gitconfig only to repositories under a specific directory. The following is an example of applying a work gitconfig (`~/work/.gitconfig`) to a repository under `~/work`.

```gitconfig
[includeIf "gitdir:~/work/"]
path = ~/work/.gitconfig
```

As an aside, if the `$GHQ_ROOT` environment variable is set, regardless of other `ghq.root` settings, repositories will be retrieved under that directory. This can be useful if you want to switch the target directory dynamically.

## Inferring owner in `ghq get {{Project}}`

GitHub's repository paths follow the format `{{Owner}}/{{Project}}`, but  if `ghq get` is only given a target repository name, as in `ghq get {{Project}}`, `ghq` will attempt to infer the owner. For example, for me (Songmu), `ghq get horenso` will resolve to `github.com/Songmu/horenso`.

This completion is done in the following order:

1. Use `ghq.user`, if set
2. Attempt to determine GitHub username from the local environment
3. Fall back to the login username (`$USER` environment variable; `$USERNAME` environment variable on Windows)

For the resolution of #2, the `github.com/Songmu/gitconfig.GitHubUser` function is used, so please refer to it for further details.

By the way, you may want to use the repository name instead of using yourself in the completed owner name. For example: ruby ​​at github.com/ruby/ruby, vim at github.com/vim/vim, peco at github.com/peco/peco, and so on.

If you prefer this behavior, you can toggle it by setting `ghq.completeUser` to `false` in your git config.

```console
% git config --global ghq.completeUser false
```

## Specifying VCS (`--vcs`)

`ghq get` tries to infer the appropriate VCS for the specified target whenever possible, but you can also specify it explicitly with the `--vcs` option. Below is an example using Fossil:

```console
% ghq get --vcs fossil https://www.sqlite.org/src
```

Also, if you want to hardcode the VCS under a particular repository host, you can configure it with the `ghq.<base>.vcs` setting. The following setting would make `ghq` always retrieve repositories hosted on `https://svn.example.com` with Subversion.

```gitconfig
[ghq "https://svn.example.com"]
vcs = svn
```

## Navigating to repositories after fetching (`--look, -l`)

As soon as you get a repository, it's natural to want to see the contents of that repository immediately. There is a `ghq get --look` option for that. This is a feature inspired by `cpanm --look`, but after fetching the repository it navigates to the target directory.

Even if the repository has already been fetched, the directory will still be navigated to, so it is convenient for quickly checking the retrieved repository. Additionally, if you run `ghq get --look --update <target>`, you can check the source while updating to the latest repository state.

After checking, exit with the `exit` command.

Note, you should use `--look` only for a quick glimpse at the source, and do not use it as a replacement for navigating to the working directory.

The `--look` option launches another shell process in the child process to emulate regular navigation. This can lead to unintuitive behavior such as shell settings not being inherited well and the need to exit when returning. There is also the problem that it may take a long time since it launches a new shell.

As such, it is recommended to simply cd to move the working directory as follows:

```console
% cd $(ghq list --full-path --exact x-motemen/ghq)
```

This area will be explained in the next chapter, `ghq list`, but it is an eventual goal to improve the implementation for changing directories since it is commonplace to do so using a fuzzy filter such as `peco` or `fzf`.

## Relative path specification for target

Relative path specification is a little-known method of specifying target for `ghq get`:

```console
% ghq get ../projB
```

For example, if you are working in `github.com/<your-org>/projA` and want to get another repository under the same organization (projB in this example).

## Additional options for `ghq get`

`ghq get` has some options that we haven't covered yet. They are as follows:

- `--parallel, -P`
    - If there are multiple targets, they will be acquired in parallel. If this option is specified, even if some repositories fail to be retrieved, the error will simply be displayed and the command will not be aborted. The use of this option will be covered separately in later chapters.
- `--shallow`
    - Fast repository retrieval by retrieving only the latest history. Only Git, git-svn and Darcs are supported with this option.
- `--branch, -b`
    - Get by specifying a branch. Only Git, git-svn, Mercurial and Subversion are supported with this option. When fetching wiht Git, it uses the `git clone --single-branch` option, so clones will be highly performant.
- `--no-recursive`
    - In the case of Git, submodule acquisition is automatically performed recursively by default, but this option will suppress that behavior.
- `--silent, -s`
    - Reduce `ghq` console output. Specifically, it suppresses the display of external command output. The default is false, but if the `--parallel` option is specified, it will be forced to true so that the output is not mixed.
