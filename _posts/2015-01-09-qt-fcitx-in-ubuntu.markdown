---
layout: post
date:   2015-01-09 18:16:25
title:  "linux で qt5 開発時に fcitx でも日本語入力"
categories: ubuntu fcitx qt
---

Ununtu 上で fcitx を使っていて Qt の開発を行う場合、オフィシャルのウェブページから持ってきたインストーラから入れた QtCreator やそれでビルドしたツールで日本語入力ができない問題があります。その解決方法が見つかったっぽいのでまとめたいと思います。

fcitx とはインプットメソッドで ibus と同じようなものです。ibus が最近になって外部から制御できなくなったりよくわからない仕様変更があったために（特にVimで使いづらい）、 fcitx を使い始めた方も多いと思います。でも作った Qt 上で入力できないんですよね。だから僕もしばらく我慢して ibus を使っていたんですが、やっぱり使いづらいし、 Qt で入力できるといっても、なぜか変換中の入力した文字が最後まで消せずに1文字残っていしまうんですｗ

というわけで試行錯誤した結果のまとめです。
記事中の内容は Qt5 、Ubuntu での話になります。
いろいろやったので足りないところや無駄なところがあるかもしれないですが最小限で書いているつもりです。

fcitx 用のライブラリを置く
--------------------------

サイトからダウンロードしてきた Qt には fcitx 用のライブラリが入っていないので追加してやらないとだめです。

~~~~
sudo apt-get install fcitx-libs-qt5
~~~~

で、 /usr/lib/x86_64-linux-gnu/qt5/plugins/platforminputcontexts に libfcitxplatforminputcontextplugin.so がダウンロードされます。

これを Qt をサウンロードしたフォルダの以下の2つに配置します。\\
Tools/QtCreator/bin/plugins/platforminputcontexts\\
[version]/gcc_64/plugins/platforminputcontexts

環境変数を Qt で fcitx をつかうようにする
-----------------------------------------

~~~~
sudo apt-get install qt4-qtconfig
qtconfig
~~~~

を実行して、 Interface の Default Input Method を fcitx に変更して、 File メニューから save して再起動します。

これで以下のように環境変数が設定されていれば多分日本語入力できるはずです。

~~~~
$ echo $GTK_IM_MODULE
fcitx
$ echo $QT_IM_MODULE
fcitx
$ echo $XMODIFIERS
@im=fcitx
~~~~
{: .bash .hljs}