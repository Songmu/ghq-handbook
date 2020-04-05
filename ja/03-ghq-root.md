# リポジトリ取得ディレクトリの設定(`ghq root`)

`ghq`はデフォルトで`$HOME/ghq`配下にリポジトリを配置します。これは変更可能です。

`ghq`は設定にgitconfigを用います。`ghq.root`がリポジトリ取得ディレクトリのための項目です。Goプログラマーであれば、`$GOPATH/src`以下に`ghq`が期待するレイアウトでリポジトリが配置されているので、以下のように設定してみましょう。

```console
% git config --global ghq.root '~/go/src'
```

このとき`.gitconfig`には以下のような設定が書き加えられているはずです。

```gitconfig
[ghq]
root = ~/go/src
```

こうすれば、`ghq get`で`~/go/src`にリポジトリが取得され、`ghq list`でそれを一覧できます。

ただ、Goのコードとそれ以外の言語のコードの混在が困る場合もあるでしょう。そんなときのために、`ghq.root`は複数設定できるようになっています。

```gitconfig
[ghq]
root = ~/go/src
root = ~/myprojects
```

複数設定されている場合、一番後ろに書かれたディレクトリがプライマリとなり、`go get`はそのディレクトリにリポジトリの取得をおこないます。この設定の場合、`ghq get`で`~/myprojects`の方にリポジトリが取得され、`ghq list`では`~/myprojects`及び`~/go/src`両方の配下のリポジトリが一覧されます。

この設定は`ghq root`コマンドで確認できます。

```console
% ghq root
/Users/Songmu/myprojects
```

`ghq root`は最優先の1件のみを出力しますが、複数設定されている場合は`ghq root --all`で全件出力されます。

```console
% ghq root --all
/Users/Songmu/myprojects
/Users/Songmu/go/src
```

また、`ghq.<base>.root`の設定も、`ghq root --all`で一覧されます。`ghq.<base>.root`については、次の`ghq get`の章で取り上げます。
