# エディタ/IDEとの連携Tips

## telescope-project.nvim (Neovim)

[telescope-project.nvim](https://github.com/nvim-telescope/telescope-project.nvim) で ghq のレポジトリに移動できます。

```lua
require('telescope').setup({
  extensions = {
    project = {
      base_dirs = {{'~/ghq', max_depth = 3}}
    }
  }
})
```

Telescope.nvim の専用拡張 [telescope-ghq.nvim](https://github.com/nvim-telescope/telescope-ghq.nvim) もありますが、telescope-project.nvim の方が少なくとも執筆時点 (2022/02) では高機能です。

## Projectile (Emacs)

[Projectile](https://docs.projectile.mx/projectile/index.html) の [Automated Project Discovery](https://docs.projectile.mx/projectile/usage.html#automated-project-discovery) で、ghq のレポジトリを Projectile プロジェクトとして一括で追加できます。

```emacs-lisp
(setq projectile-project-search-path '(("~/ghq" . 3)))
```

[emacs-ghq](https://github.com/rcoedo/emacs-ghq) を使えば、ghq のルートディレクトリを設定から取得できます。

```emacs-lisp
(with-eval-after-load 'projectile
  (require 'ghq)
  (setq projectile-project-search-path `((,ghq--root . 3))))
```

ghq のレポジトリを Projectile のプロジェクトとして一括追加するには `M-x projectile-discover-projects-in-search-path` を実行します。

## Magit (Emacs)

[Magit](https://magit.vc) の [Repository List](https://magit.vc/manual/magit/Repository-List.html) で ghq のレポジトリを一覧表示できます。

```emacs-lisp
(setq magit-repository-directories '(("~/ghq" . 3))
```

[emacs-ghq](https://github.com/rcoedo/emacs-ghq) を使えば、ghq のルートディレクトリを設定から取得したり、Clone 機能を `ghq get` で置き換えれます。

```emacs-lisp
(with-eval-after-load 'magit
  (require 'ghq)
  (setq magit-repository-directories `((,ghq--root . 3))))

(defun ghq-clone (repo)
  (interactive "sClone from url or name: ")
  (require 'ghq)
  (ghq--get-repository repo))
```

ghq のレポジトリを Magit で一覧表示するには `M-x magit-list-repositories` を実行します。

## tmux-ghq (tmux)

執筆者 ([@sei40kr](https://github.com/sei40kr)) の拙作ですが、[tmux-ghq](https://github.com/sei40kr/tmux-ghq) で、ghq のレポジトリをインタラクティブに選択し、そのレポジトリのルートをデフォルトの作業ディレクトリとする tmux セッションを作成できます。

動作には `fzf` もしくは `fzf-tmux` が必要です。詳しくは [Dependencies](https://github.com/sei40kr/tmux-ghq#dependencies) の項をご参照ください。またインストールには tmux のプラグインマネージャが必要です。

```
set -g @plugin 'sei40kr/tmux-ghq'
```

機能は [tmuxinator](https://github.com/tmuxinator/tmuxinator) や [tmuxp](https://github.com/tmux-python/tmuxp) と比べると遥かに劣りますが、設定ゼロですぐに使えるという利点があります。是非ご検討ください。
