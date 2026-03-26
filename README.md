# blog.admin11467.net-content

このリポジトリは **サイト本体**（Hugo の設定・レイアウト等）とは分離した **コンテンツ**用です。

- `content/` … 記事・ホーム・Publications など（Markdown）
- `assets-src/` … Cloudflare R2 に同期する画像等（GitHub Actions）

## 初回セットアップ（GitHub 上に新規リポジトリを作る場合）

1. GitHub で空のリポジトリを作成する（例: `blog.admin11467.net-content`）。
2. このフォルダで:

```bash
git init
git add .
git commit -m "chore: initial content repository"
git remote add origin https://github.com/<you>/<repo>.git
git branch -M main
git push -u origin main
```

## R2 同期

`.github/workflows/sync-r2-images.yml` が `main` への push で `assets-src/` をバケットに同期します。

GitHub の Secrets（リポジトリ設定）:

- `R2_ACCESS_KEY_ID`
- `R2_SECRET_ACCESS_KEY`
- `R2_BUCKET`
- `R2_ENDPOINT`

## サイト本体との関係

サイト側リポジトリのビルド（Cloudflare Pages / ローカル）で、`CONTENT_REPO_URL` をこのリポジトリの clone URL に設定すると、ビルド前に `content/` が取り込まれます。`assets-src` はビルドには不要です（公開 URL は R2）。
