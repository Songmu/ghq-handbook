# 基本的な使い方

この章では`ghq`の基本的な使い方を説明します。この章の内容だけで、普段遣いには困らないレベルになるでしょう。

`ghq`のメイン機能は「リポジトリを取得する」ことと「取得したリポジトリを一覧する」ことだけです。

## リポジトリを取得する`ghq get`

`ghq get`を使って`ghq`自体のソースコードを以下のように取得できます。

```console
% ghq get x-motemen/ghq
```

## ローカルリポジトリを一覧する`ghq list`

`ghq list`で手元のリポジトリ一覧を`{{HOST}}/{{PATH}}`形式で得られます。

```console
% ghq list
github.com/x-motemen/ghq
```

`--full-path`オプションを付与すれば、フルパスの一覧を得られます。

```console
% ghq list --full-path
/Users/Songmu/ghq/github.com/x-motemen/ghq
```

上記の例では、`/Users/Songmu/ghq` 配下にソースコードが配置されています。この配置先は`ghq root`コマンドで表示できます。

```console
% ghq root
/Users/Songmu/ghq
```

この配置先はデフォルトでは`$HOME/ghq`になっています。もちろん設定で変更可能です。

## すべてを`ghq get`する

ここまでの説明では、`ghq`の便利さがピンときていないかもしれません。しかしその便利さが分かれば、あなたはもう`git clone`することはなくなり、すべて`ghq get`でリポジトリを取得するようになるでしょう。

`ghq`の対象はGit及びGitHubに限りません。GitやGitHubに対してより便利に作られていますが、GitHub以外の任意のリポジトリホストからソースコードを取得できますし、以下のVCSをサポートしています。

- Git (git)
- Mercurial (hg)
- Subversion (svn)
- git-svn (git-svn)
- Bazaar (bzr)
- Fossil (fossil)
- Darcs (darcs)

パブリックもプライベートも、仕事用も趣味コードも、Gitもそれ以外も、すべてghqで取得・管理することをおすすめします。

* * *

以降の章で`ghq`の詳細な説明をしていきます。コマンドごとに網羅的に説明するというよりかは、主なユースケース毎に説明します。コマンドラインオプションを説明に取り上げる場合、オプション名はlong optionで統一しますが、short optionが用意されているものもあります。それについてはコマンドラインヘルプ(`ghq -h`)を参照してください。
