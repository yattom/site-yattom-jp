# site-yattom-jp

yattom.jp の営業用サイト。Cloudflare Workers (Static Assets) でホスティング。

## 編集ワークフロー

1. ページ構成・情報アーキテクチャは Miro ボード「職務経歴・提案資料 整理」で検討する
   https://miro.com/app/board/uXjVHsfkPi8=/
2. 本文テキストは `content/*.md` を編集する。**ここが編集の起点**
3. `content/` の内容を HTML(`index.html`, `training/index.html`)に反映する
   - 当面は自動化せず、Claude Code が `.md` を読んで HTML を更新する
   - 手作業で繰り返すうちに必要な変換ルールが見えてきたら、ビルドスクリプト化を検討する
4. commit / push すると Cloudflare が自動デプロイする

`.md` と HTML の内容は常に一致させる(HTML を直接直したら `.md` にも戻す)。

## ファイル構成

- `content/index.md` — トップページの原稿
- `content/training.md` — 研修・ワークショップ情報ページの原稿
- `index.html` / `training/index.html` — 公開される HTML(`content/` から生成)
- `style.css` — Pico.css で足りない微調整のみ
- `wrangler.jsonc` — Cloudflare Workers の設定
