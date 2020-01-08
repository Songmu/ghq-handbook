# リポジトリ一括取得のレシピ集(`STDIN | ghq get`)

`ghq get`は標準入力からもtargetリストを受け取れますが、これは一括取得に重宝します。このとき、`--parallel`オプションを利用すると並列で高速に取得をおこなうため便利です。これを利用したtipsを幾つか紹介します。

## 一括アップデート

手元の管理リポジトリを一括で最新の状態に更新します。

```console
% ghq list | ghq get --update --parallel
```

## 別マシンへのリポジトリ移行

ある環境のリポジトリを別環境に移したい場合、`ghq list`の出力を保存しておいて、それを別環境で`ghq get`に食わせてやれば、リポジトリの移行ができます。

```console
% ghq list > repolist.txt
% cat repolist.txt | ghq get --parallel
```

## submodulesを別で一括取得する

Git submoduleをそれぞれ個別に取得したい場合、以下のワンライナーで実現できます。

```console
% cat .gitmodules | grep '\turl = ' | cut -d' ' -f 3 | ghq get --parallel
```

## Goのプロジェクトの依存モジュールを一括取得する

Go Modules時代に移行しつつあり、GOPATH/src以下にソースリポジトリが配置されなくなってきています。Go Modulesに対応したプロジェクトであっても、以下のワンライナーで、依存モジュールを一括取得できます。

```console
% go list -f '{{join .Deps "\n"}}' | xargs go list -f '{{if not .Standard}}{{.ImportPath}}{{end}}' | ghq get --parallel
```

`ghq`は元々Goのパッケージ取得のルールを参考にして作られたツールなのに、Goのライブラリを取得するために逆に`ghq`が必要になったというのも面白い話です。

## GitHub上でスターをつけたリポジトリを一括取得する

[github-list-starred](https://github.com/motemen/github-list-starred)を使います。

```console
% go get github.com/motemen/github-list-starred
% github-list-starred Songmu | ghq get --parallel
```

ちなみに、`github-list-starred`は任意のユーザーを指定して、自分以外がスターを付けたリポジトリも一覧できるため、それを`ghq get`に食わても面白いかもしれません。

## Pocketに追加したGitHubリポジトリを一括取得する

[go-pocket](https://github.com/motemen/go-pocket)を使います。

```console
% github.com/motemen/go-pocket/cmd/pocket
% pocket list --domain=github.com | ghq get --parallel
```

このように、リポジトリ一覧を出力する何かがあれば、それを簡単に一括取得できます。他にもアイデアはたくさんあると思うので、皆さんも是非考えてみてください。
