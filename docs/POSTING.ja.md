# 投稿方法（記事の追加・画像/メディア・公開フロー）

このサイトは **HugoBlox + Hugo** で動いており、記事は **Markdown（ページバンドル）** として管理します。

**記事と `content/` の実体は「コンテンツ用リポジトリ」**に置きます（サイト本体リポジトリにはコミットしません）。パスはそのリポジトリ内の相対パスとして読み替えてください。セットアップは `docs/SETUP.ja.md` の「2リポジトリ運用」を参照。

## 記事を追加する（最小手順）

1. **コンテンツ**リポジトリで `content/blog/<slug>/` を作る  
2. 同じく **コンテンツ**リポジトリで `content/blog/<slug>/index.md` を作る  
3. **コンテンツ**リポジトリに push したあと、サイト側のデプロイで `fetch-content` が最新を取り込みビルドされます

例:

- `content/blog/r2-images/index.md`

## `index.md` の雛形（Front Matter）

```yaml
---
title: "記事タイトル"
date: 2026-03-26
draft: true
tags: ["hugo", "メモ"]
categories: ["開発"]
summary: "一覧に出す短い要約（1〜2文）"
authors: ["me"]
---
```

- `draft: true` の間は公開されません（公開する時に `false`）。
- `tags` と `categories` は配列です。

## 画像の扱い（2通り）

### 1) 記事フォルダに同梱（ページバンドル）

記事内で相対パス参照できます。

- 例: `content/blog/my-post/diagram.png`
- 参照例: `![diagram](diagram.png)`

カバー画像を使いたい場合は、記事フォルダに `featured.jpg` / `featured.png` を置くのが簡単です（テーマ側が自動検出する前提）。

### 2) Cloudflare R2 配信（`assets-src/` → 同期 → `r2img`）

画像を R2 で配信したい場合は、**コンテンツ**リポジトリの `assets-src/` 配下に置き、**そのリポジトリ**の GitHub Actions で同期します（セットアップは `docs/SETUP.ja.md` 参照）。

貼り方（ショートコード）:

```
{{< r2img src="images/example.png" alt="example" >}}
```

- `params.assetsBaseURL` が設定されていて `src` が相対パスの場合、`assetsBaseURL + src` で展開されます。
- `src` が `https://...` の場合は、その URL がそのまま使われます。

## 音声/動画などのメディア

MP3 や動画ファイルは「記事フォルダに同梱」または「メディアライブラリに配置」して、ショートコードで埋め込めます。

例（音声）:

```
{{< audio src="sample.mp3" >}}
```

## ローカルで確認する

```bash
pnpm install
# 例: 隣に clone したコンテンツrepoを指す（Windows PowerShell）
# $env:CONTENT_LOCAL_PATH="..\blog.admin11467.net-content"
# または CONTENT_REPO_URL + 必要なら CONTENT_REPO_TOKEN
pnpm dev
```

ブラウザで表示された URL を開いて確認します。

## 公開する

1. `draft: false` にする
2. **コンテンツ**リポジトリへ push（記事・画像の変更）
3. サイト本体が連携している Cloudflare Pages が再ビルドされ、反映されます（ビルド時に `content/` を fetch）

