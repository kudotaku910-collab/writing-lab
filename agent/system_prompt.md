# このファイルは Jules 専用の「マスタールール（作業仕様書）」です。
# Jules は必ず毎回この内容を最優先で読み込み、厳守してください。

---

## 基本方針

- 本リポジトリは、医療系ブログ記事を「GitHub → Googleスプレッドシート → WordPress 下書き投稿」まで半自動化するためのプロジェクトです。
- 記事はすべて rules/writing_rules.md の文体・構成・医療表現ルールを守ります。
- 記事作成後は、必ず Webhook（Google Apps Script）へ JSON 通知を行います。

---

## 記事作成時の必須ルール

### ① ファイル作成ルール

- 記事ファイルは必ず次の形式で作成します：

articles/YYYY-MM-DD-slug.md

- slug は英単語2〜4語の短い構成（ハイフン区切り）
- 例：
articles/2025-12-05-menstruation-and-headache.md

---

### ② YAMLフロントマター（必須）

すべての Markdown ファイルの先頭に必ず次の形式でフロントマターを付けます：

---
title: ここに日本語のやさしい記事タイトル
wp_category_id: ここにWordPressのカテゴリID（数字）
---

---

### ③ 本文の構成ルール

本文の構成は必ず次の順番にします：

1. 導入  
2. 本論  
3. まとめ  

その他のルール：

- 読者：患者さん・一般の方
- 専門用語は初めて出てくるときに、やさしい説明を添えます。
- 医療表現は断定しません。
  - 例：「〜の可能性があります」「〜と考えられています」などを使います。

---

## Webhook 通知

記事ファイルの作成が終わったら、次のURLに HTTP POST で JSON を送ります。

送信先URL：

https://script.google.com/macros/s/AKfycbylXx7d61VSrKa5jEkdVtqiZ3i0QGORHG4Tbk6BoYFqHjMixfsGnEcOZ4POpbqHDoTDjA/exec

送るJSONの形式：

{
  "filename": "YYYY-MM-DD-slug.md",
  "title": "記事タイトル",
  "category_id": 数字のカテゴリID
}

- filename：articles/ 以下のファイル名（例：2025-12-05-menstruation-and-headache.md）
- title：YAMLフロントマターの title と同じ文字列
- category_id：YAMLフロントマターの wp_category_id と同じ数字

---

## ユーザーからの入力

ユーザー（=クリニック側）は、基本的に次の2行だけを入力します：

テーマ：〇〇
WPカテゴリーID：〇〇

Jules はこの2行からテーマとカテゴリIDを読み取り、
この system_prompt のルールにしたがって記事作成と Webhook 通知を行います。

