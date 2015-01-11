---
layout: post
title:  "Ubuntu でデフォルトブラウザを変更する"
categories: ubuntu
---

オフィシャルに書いてある方法だけだと System Settings > Details > Default Aplications が変更されません。そこで、オフィシャルに書いてあることも含めて完全に Ubuntu でのデフォルトブラウザを変える方法をメモしときます。

ちなみに僕がなぜデフォルトブラウザを変更するのかというと、普段は firefox を使っているのですが、最新の Flash を使うために、 Flash を使いたいサイトは自動で Chromium で開くようなシェルスクリプトを書いています。それをデフォルトブラウザにするためです。


update-alternatives
-------------------

オフィシャルなど書いてあるやつですが、 config しても登録されていない実行ファイルは以下のようにします。

~~~~ bash
sudo update-alternatives --install /usr/bin/x-www-browser x-www-browser ~/bin/smartBrowser.sh 200
sudo update-alternatives --install /usr/bin/gnome-www-browser gnome-www-browser ~/bin/smartBrowser.sh 200
~~~~

すでに登録されている実行ファイルを選択するだけの場合は

sudo update-alternatives --config x-www-browser

で設定できます。


desktop ファイルを作る
----------------------

実行したいファイルは ~/bin/smartBrowser.sh です。

~/.local/share/applications/ に以下にデスクトップファイルを作成します。
内容は適当に

smartBrowser.desktop

~~~~ ini
[Desktop Entry]
Type=Application
Exec=smartBrowser.sh %u
Name=smartBrowser
GenericName=open URL in the best browser
#Icon=
Terminal=false
Categories=Network;Browser;
~~~~

***ここでこのファイルに（当然実行ファイルにも）実行権限をつけることを忘れないでください。***

さらに同ディレクトリにある mimeapps.list に以下のように追加します。

mimeapps.list

~~~~ ini
[Default Applications]
.
.
x-scheme-handler/http=smartBrowser.desktop

[Added Associations]
.
.
x-scheme-handler/http=smartBrowser.desktop;
~~~~

シェルから以下を実行

~~~~ bash
xdg-settings set default-web-browser iron.desktop
~~~~

最後に再起動（たぶんログアウトでいい）します。

![setting_window]({{ "/images/posts/default-browser.png" | prepend: site.baseurl }})

