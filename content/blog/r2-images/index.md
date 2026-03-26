---
title: "R2配信の画像を記事に貼る"
date: 2026-03-25
draft: false
tags: ["hugo", "cloudflare", "r2"]
categories: ["運用"]
summary: "assets-src/ に置いた画像をR2へ同期し、記事内で参照する最小手順。"
---

## 手順

1. 画像を `assets-src/images/` に置く（例: `assets-src/images/example.png`）
2. `main` にpushすると GitHub Actions がR2へ同期します
3. `params.assetsBaseURL` を設定した上で、記事に貼ります

## 貼り方（ショートコード）

```
{{< r2img src="images/example.png" alt="example" >}}
```

`src` が `https://...` の場合は、そのURLがそのまま使われます。

