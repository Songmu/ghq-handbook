# Creating a local repository (`ghq create`)

You can also create a local repository conforming to the `ghq` directory hierarchy. To create a directory and initialize a repository:

```console
% ghq create <target>
```

The target specification format is almost the same as with `ghq get`, but owner inference will be performed when `ghq create {{Project}}` is run regardless of the `ghq.completeUser` setting.

## Specify VCS

`ghq create` will determine and initialize the repository with the appropriate VCS based on the `ghq.<base>.vcs` settings, but you can also specify it explicitly with the `--vcs` option.

```console
% ghq create --vcs hg yourhg.example.com/project
```

* * *

Thus concludes the command functionality of `ghq`. In the following chapter, we will take a look at applied usage and recipes.
