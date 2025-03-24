---
title: HOME
layout: home
nav_order: 0
---

# HOME

## サイト目的

akihiros.github.ioはサイト作成者が学んだ内容等を書き留めるための備忘録です。

免責事項に関しては[プライベートポリシー](./docs/policy.html)をご参照ください。

## 使い方

採用しているテーマ`just-the-docs`は検索機能が便利です。カテゴリ分けは雑にしているので、必要な検索ワードを入力してご利用ください。

## 構成

URL | 概要 
:---: | :---:
[技術メモ](./docs/1_tech_note) | 技術的な気づきやメモ
[技術書](./docs/2_tech_books) | 読んだ技術書のレビュー
[英語学習](./docs/3_english) | 英語に関する学習メモ
[小説](./docs/4_novel) | 読んだ小説のレビュー

## 更新履歴

<ul>
{% for update in site.data.updates.updates %}
  <li>
    {{ update.date }} - {{ update.message }}
  </li>
{% endfor %}
</ul>

## 外部リンク

- [GitHub](https://github.com/akihiros)
- [Qiita](https://qiita.com/akihiros1207)

## フィードバック

記事の内容に関するフィードバックや質問は[GitHub Issues](https://github.com/akihiros/akihiros.github.io/issues)をご利用ください。
