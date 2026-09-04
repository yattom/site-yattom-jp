# site-yattom-jp

yattom.jp の営業用サイト。Cloudflare Workers (Static Assets) でホスティング。

## 編集ワークフロー

1. **情報アーキテクチャ・ページ構成・デザイン方針は Miro で作る。ここがサイト構成の「正」**
   - フレーム「yattom.jp サイト構成(IA)」
     https://miro.com/app/board/uXjVHsfkPi8=/?moveToWidget=3458764682727675075
   - サイトマップ、各ページのセクションと並び順、デザイン・トーンの方針が置いてある
   - 構成を変えたいときは、先にここを直す
   - フレーム「yattom.jp スタイルタイル(プレゼンデザイン由来)」
     https://miro.com/app/board/uXjVHsfkPi8=/?moveToWidget=3458764682728710391
     色・フォント・モチーフの具体値。長年使っているプレゼンテンプレート
     `000テンプレート2023.pptx` から抽出したもの
2. 本文テキストは `content/*.md` を編集する。**ここが文章の編集の起点**
3. `content/` の内容を HTML(`public/index.html`, `public/training/index.html`)に反映する
   - 当面は自動化せず、Claude Code が `.md` を読んで HTML を更新する
   - **その際、必ず先に Miro の IA フレームを参照し、構成・並び順・デザイン方針に従うこと。**
     `.md` のテキストを流し込むだけで、構成を勝手に決めない
   - 手作業で繰り返すうちに必要な変換ルールが見えてきたら、ビルドスクリプト化を検討する
4. commit / push すると Cloudflare が自動デプロイする

`.md` と HTML の内容は常に一致させる(HTML を直接直したら `.md` にも戻す)。

## ファイル構成

- `content/index.md` — トップページの原稿
- `content/training.md` — 研修・ワークショップ情報ページの原稿
- `public/` — **ここだけが配信される**。`content/` や `README.md` は公開されない
  - `public/index.html` / `public/training/index.html` — 公開される HTML(`content/` から反映)
  - `public/style.css` — Pico.css で足りない微調整のみ
- `wrangler.jsonc` — Cloudflare Workers の設定(`assets.directory` が `./public`)
