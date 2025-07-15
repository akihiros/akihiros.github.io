---
layout: post
title:  Github Pages(jekyll)用のDocker環境を作った
categories: [ブログ]
published: true
---

前回、余った端末にElementary OS 8を導入してみた話を書いた。その経緯を忘れないようこのブログに書いたわけだが、当然Github Pagesに投稿するにはPJをCloneしてローカルにJekyllを実行できる環境を揃えなければならない（`_post`配下にmdファイルを追加するだけなので厳密にはローカルでローカルで実行できなくても然程影響はないが）。そこで本機にも実行環境を作りたいところだが、できればそのまま環境構築するのは避けたいところである。

他のRubyアプリへの影響や、機器を入れ替えたときの利便性を考えてDockerfileにすることにした。どうやらOfficial？のJekyll用dockerイメージも配布されている（[リンク](https://hub.docker.com/r/jekyll/jekyll)）が、更新が２年前で止まっておりメンテナンスされていないように見える。jekyllが使えるようにするだけならそれ程大変ではないはずなので、個人用にDockerfileを作成することにした。

- [本題](#本題)
- [簡易解説](#簡易解説)
- [使い方](#使い方)
- [所感](#所感)

## 本題

結論から言えば、作ったファイルは[ここに置いた](https://github.com/akihiros/jekyll-docker)。必要に応じて自由に使ってほしい。

以下、簡易解説と使い方。

## 簡易解説

全ての記述は上記リンク先を参照いただきたい。ここでは所々を個別に解説する。

- ベースイメージには公式のRuby(alpine)を採用し、最低限必要なライブラリのみ追加することにした

```sh
FROM ruby:3.2-alpine
```

- Dockerコンテナ内でgithub.ioのPJをCloneせず、ローカルにCloneしたPJ配下をマウントさせる方式にした
- そのためコンテナ内のユーザとローカルユーザのIDを揃えることで権限エラーを避ける仕様にしている

```sh
# セキュリティ対応
# 非rootユーザを作成(権限をjekyll projectに合わせる)
ARG HOST_UID=1000
ARG HOST_GID=1000

RUN addgroup -g $HOST_GID jekyll && \
    adduser -D -u $HOST_UID -G jekyll jekyll
```

- `entrypoint.sh`も用意した
- 中身的には必要なRubyライブラリのインストールとjekyllを立ち上げているだけ

## 使い方

- [このPJ](https://github.com/akihiros/jekyll-docker)の配下で次のコマンドを実行する

```sh
# ビルド
$ sudo docker build . -t jekyll

# ビルド結果の例
$ sudo docker images
REPOSITORY   TAG       IMAGE ID       CREATED          SIZE
jekyll       latest    242cd1664d71   1 minutes ago   467MB
```

- ローカルで起動したいjekyllのPJフォルダ（xxx.github.io）に移動して実行

```sh
# run
# Ctrl+Cを押すまで起動し続けます
$ sudo docker run --rm -it -p 4000:4000 -v $(pwd):/jekyll jekyll
```

## 所感

`docker run`ごとにライブラリをインストールする仕様のため、起動に多少時間がかかるが、ローカルを汚さずjekyllの実行環境を作ることができた。仮想化技術はあまり深く触れてこなかった悲しい人間なので、今年はK8s含め仮想化と仲良しになれるくらい頑張っていきたい。
