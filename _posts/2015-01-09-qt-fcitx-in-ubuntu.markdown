---
layout: post
title:  "linux で qt5 開発時に fcitx でも日本語入力"
categories: ubuntu fcitx qt
---

Ununtu 上で fcitx を使っていて Qt の開発を行う場合、オフィシャルのウェブページから持ってきたインストーラから入れた QtCreator やそれでビルドしたツールで日本語入力ができない問題があります。その解決方法が見つかったっぽいのでまとめたいと思います。

fcitx とはインプットメソッドで ibus と同じようなものです。ibus が最近になって外部から制御できなくなったりよくわからない仕様変更があったために（特にVimで使いづらい）、 fcitx を使い始めた方も多いと思います。でも作った Qt 上で入力できないんですよね。だから僕もしばらく我慢して ibus を使っていたんですが、やっぱり使いづらいし、 Qt で入力できるといっても、なぜか変換中の入力した文字が最後まで消せずに1文字残っていしまうんですｗ

というわけで試行錯誤した結果のまとめです。
記事中の内容は Qt5 、Ubuntu での話になります。
いろいろやったので足りないところや無駄なところがあるかもしれないですが最小限で書いているつもりです。

1.fcitx 用のライブラリを置く (3/13/2015 変更・追記)
--------------------------

サイトからダウンロードしてきた Qt には fcitx 用のライブラリが入っていないので追加してやらないとだめです。

Ubuntu のリポジトリで配布されている Qt と同じバージョンを使用している時のみ apt-get で持ってくる方法が使えます。違うバージョンで入れるとセグフォを起こすようになるので注意してください。

### + apt-get でライブラリを持ってくる方法

~~~~
sudo apt-get install fcitx-libs-qt5
~~~~
{: .nohighlight}

で、 /usr/lib/x86_64-linux-gnu/qt5/plugins/platforminputcontexts に libfcitxplatforminputcontextplugin.so がダウンロードされます。

これを Qt をサウンロードしたフォルダの以下の2つに配置します。\\
Tools/QtCreator/bin/plugins/platforminputcontexts\\
[version]/gcc_64/plugins/platforminputcontexts

### + 自前でライブラリをビルドする方法

以下のページが参考になります。apt-get でライブラリを入手するときの問題も指摘されていました。

[fcitx環境のQt5.4で日本語入力できるようにする](http://blog.pyyoshi.com/2015/03/04/fcitxhuan-jing-noqt5-4deri-ben-yu-ru-li-dekiruyounisuru/)

### コンパイルしたライブラリをおいておきます

環境が同じならどうぞ

[Ubuntu 14.04 Qt 5.4]({{ site.baseurl }}/contents/libfcitxplatforminputcontextplugin_qt5-4.so)

##### ソース


Release 0.1.3 を使用 [fcitx-qt5](https://github.com/fcitx/fcitx-qt5)

[Qt Project](http://www.qt.io/)

2.環境変数を Qt で fcitx をつかうようにする
-----------------------------------------

~~~~
sudo apt-get install qt4-qtconfig
qtconfig
~~~~
{: .nohighlight}

を実行して、 Interface の Default Input Method を fcitx に変更して、 File メニューから save して再起動します。

これで以下のように環境変数が設定されていれば多分日本語入力できるはずです。

~~~~ bash
$ echo $GTK_IM_MODULE
fcitx
$ echo $QT_IM_MODULE
fcitx
$ echo $XMODIFIERS
@im=fcitx
~~~~
