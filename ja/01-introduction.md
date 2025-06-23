---
title: イントロダクション
type: tech
published: true
---

# イントロダクション

本文書は、コマンドラインツール`ghq`の使い方を説明するものです。`ghq`のメンテナであるSongmu(Masayuki Matsuki)によって書かれています。

本文書のソースコードはMarkdownフォーマットでリポジトリ<https://github.com/Songmu/ghq-handbook>から入手できます。

また、本文書の電子書籍を<https://leanpub.com/ghq-handbook>から購入できます。<https://zenn.dev/songmu/books/ghq-handbook>でもコンテンツを購入可能です。

## `ghq`とはなにか

`ghq`はソースコードリポジトリの取得と管理を簡単にするものです。これは以下のシンプルなアイデアによるものです。

- `ghq get <target>` で何でも簡単にソースコード取得
    - 柔軟なターゲット指定方式
    - 複数のVersion Control System(VCS)を透過的に扱う
- ユニークなディレクトリ構造の強制
    - `go get`を参考にした`{{GHQ_ROOT}}/{{HOST}}/{{PATH}}`レイアウト

これは、GOPATH時代の`go get`でのライブラリ取得ルールをGo以外にも適用したものと言えばGoプログラマには分かりやすいでしょう。`ghq`を使えば、複数プロジェクトの管理が楽になるだけではなく、気になったOSSを気軽にcloneできますし、リポジトリをアドホックなディレクトリにcloneしてしまい見失ってしまうこともなくなります。

`ghq`を`peco`や`fzf`等のインタラクティブフィルタツールと組み合わせれば、手元に数百を超えるリポジトリがあっても、瞬時に目的のリポジトリに移動できます。さらに、OSSをカジュアルに手元にcloneできるようになるため、ソースコードリーディングが捗るという嬉しい副作用もあります。実際作者の手元には1000を超えるリポジトリがあります。

`ghq`の"gh"はGitHubもしくはgitとhg、qはquickや「急」を意味しています。おそらく。

## 動作環境

`ghq`はGitHub上のリポジトリ<https://github.com/x-motemen/ghq>で開発されています。Goで書かれており、複数OSでの動作を確認しています。公式で、Linux, macOS, Windowsの64bitバイナリビルドを提供しています。

本文書の執筆にあたっては、主に、ghq v1.0.0, macOS 10.15.2, zsh 5.7.1で確認をしていますが、OSやシェル環境による差異は少なくなるように留意しています。

## `ghq`の導入

Homebrewでインストール可能です。

```console
% brew install ghq
```

その他の環境では、[GitHub Releases](https://github.com/x-motemen/ghq/releases)から実行ファイルを取得できます。

Goの開発環境がある場合、開発リポジトリ( https://github.com/x-motemen/ghq )を`go install`したり、`git clone`して自前ビルドすることも可能ですが、`ghq`の開発者でも無い限り、タグが打たれた安定ビルドの利用をおすすめします。
