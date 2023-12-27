---
title: Get the repository (ghq get)
type: tech
published: true
---

# Get the repository (`ghq get`)

`ghq get` takes a target as an argument or via standard input and retrieves its repository.

```console
% ghq get < target > [ < target > ...]
% cat repolist.txt | ghq get --parallel
```

`ghq` interprets this target in a nice way, so you can specify it flexibly.

The basic form of the target is `https://{{Host}}/{{Path}}`. `https://` is always optional, and if the host is `github.com`, it can also be omitted. In other words, the following three have the same meaning.

```console
% ghq get https://github.com/x-motemen/ghq
% ghq get github.com/x-motemen/ghq
% ghq get x-motemen/ghq
```

Of course, hosts other than GitHub can also be specified.

```console
% ghq get launchpad.net/terminator
```

It is also possible to specify a target using a regular repository URL. URLs other than http schemes are also accepted.

```console
% ghq get git@github.com:x-motemen/ghq.git
% ghq get svn+ssh://svn.example.com/yourproject
```

If you use the `ghq get --update` ( -u) option, it will update to the latest state even if the repository has already been acquired.

## If you want to use SSH or obtain a private repository

`ghq` attempts to retrieve the repository using https by default. However, there may be cases where you want to obtain a private repository using SSH, or you may want to push code using SSH even if it is OSS. The detailed settings of Git won't be covered here, but you have the following options to solve this problem.

- Force **SSH** use by configuring `url.<base>.insteadOf`
- `ghq get` - Use SSH URL as target
- `ghq get -p` - Switch to SSH acquisition using option
- Do not use **SSH** (set **git credentials** etc. appropriately and perform **pull** and **push** using **https**)

You can choose your preferred method, but rather than using the -p option to fetch with SSH, it is better to configure Git settings and fetch with **HTTPS**, as it gives a sense of uniformity. The author uses **https** to obtain the source code, and `url.<base>.pushInsteadOf` settings to push via **SSH**.

## Switch the acquisition directory for each project (`ghq.<base>.root`)

`ghq root` settings were mentioned earlier, but you may want to switch the fetch directory and git settings between work and hobby code. This can be achieved with the `ghq.<base>.root` settings.

The following is an example of a configuration that defaults to `~/hobby`, but repositories under your company's GitHub organization or your company's VCS host are placed in `~/work`.

```gitconfig
[ ghq ]
root  =  ~/hobby

[ ghq  "https://github.com/mycompany" ]
root  =  ~/work

[ ghq  "https://myvcs.example.com" ]
root  =  ~/work
```

Speaking of Git settings, if you use the `includeIf` setting, you can apply a specific **gitconfig** only to repositories under a specific directory. The following is an example of applying work **gitconfig** (`~/work/.gitconfig`) to the repository under `~/work`.

```gitconfig
[ includeIf "gitdir: ~/work/"]
path  =  ~/work/.gitconfig
```

As a side note, if the `$GHQ**ROOT` environment variable is set, the repository will be acquired under that directory regardless of other `ghq.root` settings. This may also be useful if you want to fetch directories more dynamically.

## `ghq get {{Project}}` owner detection

GitHub's repository path takes the form {{Owner}}/{{Project}}, but if you give `ghq get` the repository name without slashes, like `ghq get {{Project}}`, it will attempt to complete the owner. For example, in my case **(Songmu)**, `ghq get horenso retrieves the repository `github.com/Songmu/horenso`.

This completion is done in the following order:

- If `ghq.user` is set, use it
- Infer GitHub username from local environment
- Login name ( $USERenvironment variable.Environment variable for Windows $USERNAME)

A function is used to estimate 2, `github.com/Songmu/gitconfig.GitHubUser``, so please refer to it if you want to know more.

By the way, you may want to use the repository name instead of using yourself as the completed owner name. For example, **ruby** to **github.com/ruby/ruby**, **vim** to **github.com/vim/vim**, **peco** to **github.com/peco/peco**, etc.

If you prefer this behavior, you can toggle it by setting the setting `ghq.completeUser` to false.

```console
% git config --global ghq.completeUser false
```

## Specify VCS ( --vcs)

`ghq get` will try to determine the most appropriate VCS for the specified target, but you optionally use `--vcs` to specify it explicitly. Here is an example from fossil:

```console
% ghq get --vcs fossil https://www.sqlite.org/src
```

Also, if you can specify the VCS for a particular repository host by configuring it with `ghq.<base>.vcs`. Here are the settings to retrieve the repository `https://svn.example.com` with Subversion:

```gitconfig
[ ghq "https://svn.example.com"]
vcs  =  svn
```

## Check the source at the same time as acquisition (`--look, -l`)

It's natural to want to see the contents of a repository as soon as you retrieve it. There is a `ghq get --look` option for that. This is a feature inspired by `cpanm --look`, but moves to the fetch directory after repository fetch.

Even if the repository has already been acquired, you will be moved to the directory, so it is useful for temporarily checking the repository at hand. Also, if you run `ghq get --look --update <target>`, you can check the source while updating to the latest version.

Once you have finished checking, exit using the `exit` command.

Also, please use `--look` only for a temporary review of the source, and do not use it to move the working directory.

The `--look` option launches another shell process as a child process, making it appear as though the directory has been moved. This results in unintuitive behavior such as shell settings not being carried over properly and the need to exit when returning. There is also the problem that it takes time to start the shell because it restarts the shell.

Therefore, we recommend that you simply use a CD to move the working directory as shown below.

```console
% cd $( ghq list --full-path --exact x-motemen/ghq )
```

This will be explained in the next `ghq --list` chapter, but the standard way to move directories is to work with filter tools like peco and fzf, so we recommend configuring those settings.

## Target relative path specification

A relatively unknown method of specifying a target for `ghq get` is relative path specification.

```console
% ghq get ../projB
```

This can be used, for example, if you are working on `github.com/<your-org>/projA` and want to retrieve another repository under the same organization (`projB` in this example).

## More options for `ghq get`

`ghq get` has several options not covered so far:

- --parallel, -P
  - If there are multiple targets, they will be acquired in parallel. If this option is specified, even if some repositories fail to be retrieved, the error will only be displayed and the command will not be interrupted. We will discuss how to take advantage of this option separately in a later chapter.
- --shallow
  - Retrieve repositories quickly by retrieving only the latest history. Only Git, git-svn, Darcs are supported
- --branch, -b
  - Get a specified branch. Only Git, git-svn, Mercurial, and Subversion are supported. Especially in the case of Git git clone --single-branch, you can expect high-speed acquisition because you use options.
- --no-recursive
  - In the case of Git, submodule acquisition is automatically recursive by default, but this is an option to suppress that.
- --silent, -s
  - Reduce `ghq` console output. Specifically, it suppresses the display of external command output. Default is false, but if the `--parallel` option is specified it will be forced to true to avoid mixing outputs.
