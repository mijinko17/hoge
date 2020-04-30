---
title: "Macを買った時に設定したこと"
date: 2020-02-29T05:10:50+09:00
draft: false
---

# dotfiles

-   brew のインスコ->brew install git->git clone->brew bundle の流れ。ちなみに git 自体は元から入っていた。

-   brew のインストールにめちゃ時間掛かってびっくりしてしまった。xcode も一緒に入れるらしくそこで時間吸われたっぽい。

-   brew bundle も永遠に終わらず、バイトに行く前に回し始めておいたのだが帰ってきたらエラー終了していた。llvm にやたら時間が掛かっていたっぽい。

# neovim

概ね問題なかったが、pip で neovim の何かを入れるのを忘れていて deoplete に怒られてしまった。

# mac のシステム環境設定

-   dock を右寄せ、自動的に非表示。使わないアプリを dock から消して iterm2 を追加。最近使用したアプリを dock に追加しないよう設定。

-   finder での並び順を常に種類順に。

-   タッチによるクリックを有効化。

-   capslock に control を割り当て。

-   command+option+J で iterm2 を起動するようにした。Automator を使った。

# ターミナル

-   iterm2 に hybrid-material のカラースキームを適用。

-   zsh に移行。具体的には git の補完とか branch をプロンプトに表示する設定をした。これからやる人は vcs_info とかで調べると幸せになれそう。

-   ターミナルをリロードするスクリプトをエイリアスに割り当てた。.zshrc を書き換えまくるときの QOL が爆上がりする。

# これからやりたいこと

-   zsh のプロンプトに git の情報を載せるところをもう少しちゃんとやる(まだ branch 表示とコミットしてないファイルの検知しかできない)

-   lsp 周りをすっきりさせる。まあこれは buildin lsp を待とうかな。
