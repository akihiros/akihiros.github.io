---
title: HOME
layout: home
nav_order: 1
---

# HOME

## サイト目的

akihiros.github.ioはサイト作成者が学んだ内容等を書き留めるための備忘録です。

免責事項に関しては[プライベートポリシー](./docs/policy.html)をご参照ください。

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
