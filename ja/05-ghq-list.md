# ローカルリポジトリの一覧・パス取得をおこなう(`ghq list`)

`ghq list`コマンドはローカルリポジトリの一覧を表示します。`ghq list --full-path`で一覧をフルパスで表示します。

この結果をpecoやfzf等のインタラクティブフィルタツールと組み合わせれば、目的のローカルリポジトリに瞬時に移動できます。以下はpecoを使った一番簡単な例です。

```console
% cd "$(ghq list --full-path | peco)"
```

以下はもう少し進んだ例で、実際に筆者が利用しているzshの設定です。

```zsh
peco-src () {
    local repo=$(ghq list | peco --query "$LBUFFER")
    if [ -n "$repo" ]; then
        repo=$(ghq list --full-path --exact $repo)
        BUFFER="cd ${repo}"
        zle accept-line
    fi
    zle clear-screen
}
zle -N peco-src
bindkey '^]' peco-src
```

`Ctrl-]` を押せばリポジトリ一覧が表示され、選択したリポジトリに移動します。

一旦、素の`ghq list`でフルパスではない一覧の中からリポジトリを選択し、その結果を、`ghq list --full-path --exact $repo` のように `ghq list`の引数に渡して再度`ghq list`を呼び出してフルパスを得ています。

フィルタツールを使わなくても、以下のようにシェルに`ghq-cd`関数を定義しておけば、ディレクトリ移動が簡単になります。`ghq-cd ghq`などとして利用できます。

```shell
ghq-cd () {
    if [ -n "$1" ]; then
        dir="$(ghq list --full-path --exact "$1")"
        if [ -z "$dir" ]; then
            echo "no directroies found for '$1'"
            return 1
        fi
        cd "$dir"
        return
    fi
    echo 'usage: ghq-cd $repo'
    return 1
}
```

## `ghq list`に検索用引数を渡す

`ghq list`は素の状態ではローカルリポジトリの全件を一覧表示しますが、検索用のクエリ引数を渡すこともできます。

```console
% ghq list <query>
```

このとき`ghq`はクエリ文字列に部分一致したリポジトリを一覧します。また、クエリ文字列が、`<hostname>/` のような文字列で始まっている場合、そのホスト配下のリポジトリに対して検索をします。

- `motemen`を含むリポジトリを一覧する
    - `ghq list motemen`
- `github.com`配下のリポジトリを一覧する
    - `ghq list github.com/`

## クエリを厳密にマッチさせる(`--exact`)

`--exact`オプションは挙動の説明が難しいのですが、これを指定した場合、クエリに後方一致し、サブパスも完全一致したリポジトリが一覧されます。

例えば、`ghq list go`とした場合、プロジェクトやオーナーに"go"が含まれるリポジトリが大量にリストアップされてしまいますが、`ghq list --exact go`とすれば、プロジェクトが"go"に完全一致するリポジトリがリストアップされます。以下は筆者の手元の例です。

```console
% ghq list --exact go
cloud.google.com/go
9fans.net/go
github.com/golang/go
github.com/ugorji/go
```

ownerを含めて、更に絞り込めます。

```console
% ghq list --exact golang/go
github.com/golang/go
```

ここで`--exact`を指定しないと、部分一致で以下のようになってしまいます。

```console
% ghq list golang/go
github.com/golang/go
github.com/golang/gofrontend
```

さらにホストまで含めて`ghq list --full-path --exact github.com/x-motemen/ghq` などとすれば、フルパスが一意に定まり、出力されるため、他のツールとの連携で重宝します。先のzshの設定内で使われていたテクニックです。

## VCSで絞り込みをおこなう(`--vcs`)

`--vcs`オプションを使って、VCSでの絞り込みができます。以下は、Gitリポジトリのみに絞り込み、一斉に`git gc`をかける例です。

```console
% ghq list --full-path --vcs=git | xargs -I@ git -C @ gc --aggressive --prune=now
```

## 最短のユニークなプロジェクト一覧を表示する(`--unique`)

ローカルでユニークさが保証される最短の名前の一覧が出力されます。プロジェクト名だけで一意になる場合は、プロジェクト名のみが、複数のownerの同名のプロジェクトがローカルにある場合は、ownerも出力されます。例えば以下のような具合です。

```console
% ghq list --unique
cloud.google.com/go
golang/go
tcnksm/go-gitconfig
motemen/go-gitconfig
ghq
perl
```

ちなみに、この機能がどういったところで役に立つのか、作者のmotemenも覚えていないそうです。
