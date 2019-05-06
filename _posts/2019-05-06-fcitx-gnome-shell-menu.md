---
layout: post
title:  "Fcitx の GNOME Shell のメニューが一瞬で閉じてしまう"
categories: fcitx ubuntu
---

結構前から気になっていたが、Ubuntu が GNOME Shell になってから、Fcitx のインジケータの中の開くメニューが一瞬で閉じてしまい、Mozc などのメニューが開けなくて困ってました。

![Fcitx menu old]({{ "/images/posts/fcitx-gnome-menu-old.png" | prepend: site.baseurl }})

今回ソースレベルで調べるぞと言う勢いで調べ始めましたが、最新のインジケータ（kimpanel-for-gnome-shell というらしい）を入れるだけでなおってしまったので記録しておきます。
ただ UI が少し変わっているようです。
おそらく Debian のパッケージが更新されてないだけだと思います。

kimpanel-for-gnome-shell を最新に更新する
-----------------------------------------

kimpanel-for-gnome-shell のリポジトリ: <https://github.com/wengxt/kimpanel-for-gnome-shell>

リポジトリをクローン

``` sh
git clone https://github.com/wengxt/kimpanel-for-gnome-shell.git
cd kimpanel-for-gnome-shell
```

Ubuntu 18.04 の場合は GNOME Shell のバージョンが少し古いので、バージョンを合わせる。

``` sh
git checkout 9465479f50045cccc707f9c8e0b9dc9cfe03e3a7
```

インストール

``` sh
sudo apt install cmake
./install.sh
```

`Alt + F2` から `r` を実行すると GNOME Shell が再起動し、gnome-tweaks などから extension を有効にする。

すでに Fcitx が立ち上げっていれば、再起動すれば今回入れた Extension で起動してくれます。

こんな UI になって、そもそも開くものじゃなくなります。
個人的には開くほうが見やすかったですが、開けないよりはマシですねw

![Fcitx menu new]({{ "/images/posts/fcitx-gnome-menu-new.png" | prepend: site.baseurl }})
