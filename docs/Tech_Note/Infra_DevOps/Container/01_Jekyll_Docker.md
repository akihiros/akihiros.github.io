---
layout: default
title:  Jekyll Docker
parent: コンテナ
---

# Jekyll Docker

Official？のJekyll用dockerイメージも配布されている（[リンク](https://hub.docker.com/r/jekyll/jekyll)）が、更新が２年前で止まっておりメンテナンスされていないように見える。jekyllが使えるようにするだけならそれ程大変ではないはずなので、個人用にDockerfileを作成することにした。

- [本題](#本題)
- [簡易解説](#簡易解説)
- [使い方](#使い方)
- [備考](#備考)

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

## 備考

- jekyllのテーマによってはエラーが出る可能性があるが、その場合はGemfileの修正が必要になる
- これはalpineの仕様によるものが多く、例えばjust-the-docsでは`gem "jekyll-sass-converter", "~> 2.0"`を追加しエラーを回避した
