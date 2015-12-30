---
layout: post
title:  "Linux環境でLINEをしたい(xwindowにキーストロークを送信する)"
categories: ubuntu linux
---

いやー、ずっとWineでやろうと奮闘したりいろいろしていましたが、最初の頃なんかはほとんどまともに文字が打てなかったり…
最近はまぁまぁまともに（通話が来た時は落ちてたけど）動くようになったので喜んだりしてたりとかw

それもChrome拡張のLINEができたのでLinux環境でもちゃんと動くようになりました。が、このLINE拡張、基本的に外出先なんかで使うように出来てるらしく、自動でログインしてくれないんですね。
そこでログインの自動化を（無理やり）実現した話です。

結局は、メールは記憶されるので、LINE起動直後にパスワードのキーストロークとエンターを投げてやればいいのです。
以下手順です。

まずはxwindowに対して操作するためのツールを入れます。

~~~ sh
sudo apt-get install xdotool
~~~

あとはLINEを起動してパスワードを入力するスクリプトを書きます。

~~~ sh
# このアプリIDは共通だと思います
lineAppId=menkifleemblimdogmoihpfopnplikde

# Chromiumの場合。適時書き換えてください。
/usr/bin/chromium-browser --profile-directory=Default --app-id=$lineAppId &

# 完全に起動するまで。マシンスペックに合わせてください
sleep 3

# LINEのwindowを選択してキーを送出します。
# スペース区切りでLINEのパスワードを指定してください。
xdotool search --classname "^crx_$lineAppId$" windowfocus windowactivate key P a s s S e p a r a t e d B y S p a c e Return
~~~

あとは実行権限をつけて実行すればLINEが立ち上がってパスワードが入力されてログインされます。
