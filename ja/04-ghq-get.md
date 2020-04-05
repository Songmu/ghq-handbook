# リポジトリを取得する(`ghq get`)

`ghq get`は引数か標準入力経由でtargetを受け取り、そのリポジトリを取得します。

```console
% ghq get <target> [<target>...]
% cat repolist.txt | ghq get --parallel
```

`ghq`はいい感じにこのtargetを解釈するので、柔軟な指定が可能です。

targetは`https://{{Host}}/{{Path}}`が基本形です。`https://`は常に省略可能で、ホストが`github.com`の場合、それも省略できます。つまり、以下の3つは同じ意味になります。

```console
% ghq get https://github.com/x-motemen/ghq
% ghq get github.com/x-motemen/ghq
% ghq get x-motemen/ghq
```

もちろんGitHub以外のホストも指定可能です。

```console
% ghq get launchpad.net/terminator
```

また、通常のリポジトリURLでのtarget指定も可能です。http scheme以外のURLも受け付けます。

```console
% ghq get git@github.com:x-motemen/ghq.git
% ghq get svn+ssh://svn.example.com/yourproject
```

`ghq get --update`(`-u`) オプションを利用すれば、リポジトリが取得済みの場合に最新の状態への更新をおこないます。

## SSHを用いたい場合や、プライベートリポジトリを取得したい場合

`ghq`は標準でhttpsでのリポジトリ取得を試みます。しかし、プライベートリポジトリをSSHで取得したい場合や、OSSであってもコードのpushはSSHでおこないたいということもあるでしょう。Gitの細かい設定には立ち入りませんが、この解決のためには以下のような選択肢があります。

- `url.<base>.insteadOf` を設定してSSHを強制する
- `ghq get`のtargetにSSH URLを利用する
- `ghq get -p`オプションを使ってSSHでの取得に切り替える
- SSHを使わない(git credential等を適切に設定してhttpsでpullとpushをおこなう)

好みの方法を選択すれば良いのですが、`-p`オプションを使ってSSH取得をするより、Gitの設定を整えて、取得したほうが統一感が出て望ましいでしょう。筆者はソースコードの取得はhttpsでおこない、pushは`url.<base>.pushInsteadOf`を設定してSSHでpushするようにしています。

## プロジェクトごとに取得ディレクトリを切り替える(`ghq.<base>.root`)

`ghq root`の設定には先程触れましたが、仕事用と趣味用のコードで取得ディレクトリやgitの設定を切り替えたいこともあるでしょう。これは`ghq.<base>.root`設定で実現できます。

以下は、標準では`~/hobby`に、自社のGitHubオーガニゼーションや自社のVCSホスト配下のリポジトリは`~/work`に取得する設定例です。

```gitconfig
[ghq]
root = ~/hobby

[ghq "https://github.com/mycompany"]
root = ~/work

[ghq "https://myvcs.example.com"]
root = ~/work
```

Gitの設定の話になりますが、このとき、`includeIf`設定を併用すると、特定のgitconfigを特定のディレクトリ配下のリポジトリのみに適用することができます。`~/work`以下のリポジトリに対して仕事用のgitconfig(`~/work/.gitconfig`)を適用させる例が以下です。

```gitconfig
[includeIf "gitdir:~/work"]
path = ~/work/.gitconfig
```

余談ですが、`$GHQ_ROOT`環境変数が設定されている場合、他の`ghq.root`設定に関わらず、そのディレクトリ配下にリポジトリ取得をおこないます。これは、更に動的に取得ディレクトリを切り替えたい場合に役に立つこともあるでしょう。

## `ghq get {{Project}}`でのowner検出

GitHubのリポジトリパスは`{{Owner}}/{{Project}}`の形式を取りますが、`ghq get {{Project}}`のように、スラッシュを含まないリポジトリ名のみを`ghq get`に与えた場合、`ghq`はownerの補完を試みます。例えば、私(Songmu)の場合、 `ghq get horenso` とすると、`github.com/Songmu/horenso`リポジトリを取得します。

この補完は以下の順番でおこなわれます。

1. `ghq.user` が設定されている場合それを使う
2. GitHubユーザー名をローカル環境から推定する
3. ログイン名(`$USER`環境変数。Windowsの場合`$USERNAME`環境変数)

2の推定には、`github.com/Songmu/gitconfig.GitHubUser`関数を用いているので詳細を知りたい場合はそちらを参照してください。

ところで、補完するowner名に自身を使うのではなく、そのリポジトリ名を使いたいこともあるでしょう。例えば、rubyをgithub.com/ruby/rubyに、vimをgithub.com/vim/vimに、pecoをgithub.com/peco/pecoに、といった具合です。

この挙動の方が好みの場合、`ghq.completeUser`設定をfalseに設定することで、挙動を切り替えられます。

```console
% git config --global ghq.completeUser false
```

## VCSを明示する(`--vcs`)

`ghq get`は指定されたtargetに対して、極力適切なVCSを判別しようとしますが、`--vcs`オプションで明示的に指定することもできます。以下はfossilの例です。

```console
% ghq get --vcs fossil https://www.sqlite.org/src
```

また、特定のリポジトリホスト下のVCSを決め打ちできる場合、それを `ghq.<base>.vcs` で設定できます。以下は、`https://svn.example.com` 下のリポジトリをSubversionで取得する設定です。

```gitconfig
[ghq "https://svn.example.com"]
vcs = svn
```

## 取得と同時にソースを確認する(`--look, -l`)

リポジトリ取得と同時に、そのリポジトリの内容をすぐに見たいと思うのは自然でしょう。そのための`ghq get --look`オプションがあります。これは`cpanm --look`にインスパイアされた機能ですが、リポジトリ取得後に取得ディレクトリに移動します。

リポジトリが取得済の場合でも、ディレクトリへの移動はおこなわれるため、手元のリポジトリの一時的な確認のためにも便利です。また、`ghq get --look --update <target>`とすれば、最新の状態に更新しながらソースの確認がおこなえます。

確認が終わったら、`exit`コマンドで抜けてください。

また、この`--look`の利用はあくまでソースの一時的なチラ見のために留め、作業ディレクトリの移動用には利用しないようにしてください。

`--look`オプションは子プロセスで別のシェルプロセスを起動して、ディレクトリを移動したように見せかけています。このため、シェルの設定がうまく引き継がれなかったり、戻るときにexitする必要があるといった直感的でない挙動が発生します。また、シェルを起動し直すため、シェルの起動時間がかかってしまうという問題もあります。

ですので、作業ディレクトリの移動には以下のように、素直にcdをするのがおすすめです。

```console
% cd $(ghq list --full-path --exact x-motemen/ghq)
```

このあたりは、次の`ghq list`の章で説明しますが、ディレクトリの移動はpecoやfzfのようなフィルタツールと連携して移動するのが王道パターンなので、それらの設定を整えることをおすすめします。

## targetの相対パス指定

あまり知られざる`ghq get`のtarget指定方法として、相対パス指定があります。

```console
% ghq get ../projB
```

これは例えば、 `github.com/<your-org>/projA` で作業しているときに、同じオーガニゼーション配下の別リポジトリ(この例の場合はprojB)を取得したい場合に利用します。

## `ghq get`のその他のオプション

`ghq get`にはここまで取り上げていない幾つかのオプションがあります。最後にそれらをまとめて紹介します。

- `--parallel, -P`
    - 複数のtargetがある場合、並列で取得をおこないます。このオプションが指定された場合、一部のリポジトリの取得に失敗してもそのエラーを表示するのみで、コマンドの中断はされません。このオプションの活用方法は後の章で個別に取り上げます。
- `--shallow`
    - 最新の履歴のみ取得することで高速にリポジトリを取得します。Git, git-svn, Darcsのみ対応しています
- `--branch, -b`
    - ブランチを指定して取得します。Git, git-svn, Mercurial, Subversionのみ対応しています。特にGitの場合`git clone --single-branch`オプションを利用するため、高速な取得が望めます
- `--no-recursive`
    - Gitの場合、標準ではsubmoduleの取得も自動で再帰的におこないますが、それを抑制するオプションです
- `--silent, -s`
    - `ghq`のコンソール出力を削減します。具体的には外部コマンド出力の表示を抑制します。デフォルトfalseですが、`--parallel`オプション指定時には出力が混ざらないように強制的にtrueになります
