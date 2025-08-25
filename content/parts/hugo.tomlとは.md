
---
title: "hugo.tomlとは — Hugo設定ファイルの役割と安全運用"
date: 2025-08-25
tags: ["Hugo", "設定", "TOML", "構文摩擦", "温度:中"]
---

## 1. 導入 — この記事でわかること
Hugoの設定ファイル **hugo.toml** の役割と基本構造、  
そして今回の「`unsafe` 設定事故」から学んだ安全運用のポイントをまとめます。

> 記事内の長文を折りたたみたかったがdetailタグを直接記述するとビルド警告がでて、生タグが記載できず、その原因が `unsafe` だったので、備忘も兼ねてまとめる

---

## 2. hugo.toml の基本
- **役割**：Hugoサイト全体の設定を記述するファイル
- **配置場所**：`config/_default/hugo.toml` が推奨されているが小規模の場合はサイト直下で⭕️
- 複数環境を作って複数tomlでの管理も可能。
- **対応形式**：TOML / YAML / JSON（TOML推奨）
- **config.tomlとの違い**：Hugo 0.110.0以降は `hugo.toml` がデフォルト名 [A](https://juggernautjp.info/getting-started/configuration/?copilot_analytics_metadata=eyJldmVudEluZm9fY2xpY2tEZXN0aW5hdGlvbiI6Imh0dHBzOlwvXC9qdWdnZXJuYXV0anAuaW5mb1wvZ2V0dGluZy1zdGFydGVkXC9jb25maWd1cmF0aW9uXC8iLCJldmVudEluZm9fY29udmVyc2F0aW9uSWQiOiJMelVpQXVrYkhyQ3BweVpVWHo4SmUiLCJldmVudEluZm9fY2xpY2tTb3VyY2UiOiJjaXRhdGlvbkxpbmsiLCJldmVudEluZm9fbWVzc2FnZUlkIjoiZEVDZE5mZjRpcGNZbjdFU213cnlGIn0%3D&citationMarker=9F742443-6C92-4C44-BF58-8F5A7C53B6F1)

---

## 3. TOMLの書き方基礎
```toml
# コメントは # から改行まで
title = "サイト名"
baseURL = "https://example.com"
hasCJKLanguage = true

[params]
  description = "サイトの説明"
```

- 文字列・数値・真偽値・日付・配列が使える
- `[table]` でテーブル（連想配列）、`[[array]]` でテーブルの配列を定義

[公式サイト](https://toml.io/ja/v1.0.0)

---

## 主な設定項目例
```toml
baseURL = "https://example.com"
title = "Typo職人の文化資産庫"
theme = ["PaperMod"]
defaultContentLanguage = "ja"
hasCJKLanguage = true

[params]
  dateFormat = "2006-01-02"
  description = "文化資産化の実験場"

[markup.goldmark.renderer]
  unsafe = true
```
- `baseURL`：公開URL
- `theme`：使用テーマ
- `params`：テーマ固有のパラメータ
- `markup.goldmark.renderer.unsafe`：生HTML許可（事故ポイント）

[公式サイト](https://juggernautjp.info/getting-started/configuration/)
---

## 5. 環境別設定
```
hugo --environment production
hugo --environment staging
```

- `config/_default/hugo.toml`（共通設定）
- `config/production/hugo.toml`（本番用）
- `config/staging/hugo.toml`（ステージング用）
- 上書きされるのは差分だけなので、共通設定は `_default` にまとめる


---

## 6. よくある落とし穴

- `unsafe` が false でmd記載の生HTMLが消える（今回の事故）
	-  falseのまま運用ならショートコード作成要
- `mainSections` 未設定でトップページが空になる
- 環境別設定で `baseURL` を書き変え忘れる
- 便利な設定ファイルだが機密情報は下手に記載しないこと

---

## 7. まとめと再利用ポイント
- hugo.toml は「サイトの設計図」
- 環境別設定で安全運用＆効率化
- 再利用部品： Goldmark設定テンプレ
- 環境別GA設定例
- バックアップ手順リンク

🗒 メモ
- tomlでなくても自分が慣れているfront matterでも良い！jsonなど。
- 設定値一覧
- 適用テーマで設定値可能項目名は結構ちがう。


---
