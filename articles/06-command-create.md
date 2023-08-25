---
title: ローカルリポジトリを作成する(`ghq create`)
type: tech
published: true
---

# ローカルリポジトリを作成する(`ghq create`)

ghqのルールに則って、ローカルリポジトリの作成もできます。ディレクトリの作成とリポジトリの初期化までおこないます。

```console
% ghq create <target>
```

targetの指定ルールは`ghq get`とほぼ同様ですが、`ghq create {{Project}}`とした場合のowner補完では、`ghq.completeUser`設定に関わらず、自分自身が補完されます。

## VCSの指定

`ghq create`は`ghq.<base>.vcs`の設定などから判別して、適切なVCSでリポジトリの初期化をおこないますが、`--vcs`オプションで明示的な指定も可能です。

```console
% ghq create --vcs hg yourhg.example.com/project
```

* * *

これで、`ghq`の機能を一通り説明しました。以降は応用的な使い方を取り上げます。
