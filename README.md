# site-yattom-jp

yattom.jp の営業用サイト。GitHub Pages でホスティング。

公開設定は Settings → Pages で「Deploy from a branch: `main` / `/docs`」。
独自ドメイン yattom.jp のネームサーバーは Squarespace にあり、そちらで
GitHub Pages 向けの A レコードを設定している。

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
3. `content/` の内容を HTML(`docs/index.html`, `docs/training/index.html`)に反映する
   - 当面は自動化せず、Claude Code が `.md` を読んで HTML を更新する
   - **その際、必ず先に Miro の IA フレームを参照し、構成・並び順・デザイン方針に従うこと。**
     `.md` のテキストを流し込むだけで、構成を勝手に決めない
   - 手作業で繰り返すうちに必要な変換ルールが見えてきたら、ビルドスクリプト化を検討する
4. commit / push すると GitHub Pages が自動デプロイする

`.md` と HTML の内容は常に一致させる(HTML を直接直したら `.md` にも戻す)。

## 生成ルール(`.md` には書かない、HTML にするときに機械的に付けるもの)

- **サイト外へのリンクはすべて別タブで開く**。`target="_blank" rel="noopener"` を付ける。
  `games.yattom.jp` も別サイトなので外部として扱う。同一サイト内(`../`、`../#contact`、
  `training/`)と `mailto:` は付けない
- `{.button}` が付いたリンクは `role="button"` にする
- フロントマターの中の `<br>` はそのまま改行として出す
- 各ページに `<link rel="canonical">` と OGP(`og:url` / `og:title` / `og:description`)を、
  そのページ自身の URL・タイトル・description で入れる
- 各ページに schema.org の JSON-LD を入れる。トップは `Person`、研修ページは `Service`。
  内容が変わったら JSON-LD の `description` などもあわせて直す
- ページを増やしたら `docs/sitemap.xml` に URL を追記する
- 全ページの `</head>` の直前に GA4 のタグを入れる。測定 ID は **`G-ESYWF33LKE`**。
  ブログ(`yattom.hatenablog.com`)と `games.yattom.jp` と同じプロパティに集約しており、
  ドメイン間の測定は GA の管理画面側で設定済みなので、タグにはその記述は不要
- 全ページのフッターに `<a href="/privacy/">プライバシーポリシー</a>` を置く

## ファイル構成

- `content/index.md` — トップページの原稿
- `content/training.md` — 研修・ワークショップ情報ページの原稿
- `content/privacy.md` — プライバシーポリシーの原稿
- `docs/` — **ここだけが配信される**。`content/` や `README.md` は公開されない。
  GitHub Pages がブランチ公開で選べるのは直下か `/docs` だけなので、この名前にしている
  - `docs/index.html` / `docs/training/index.html` — 公開される HTML(`content/` から反映)
  - `docs/style.css` — Pico.css で足りない微調整のみ
  - `docs/CNAME` — 独自ドメイン `yattom.jp`。**消さないこと**(消すとカスタムドメインが外れる)
  - `docs/.nojekyll` — Jekyll のビルドを止めて、ファイルをそのまま配信させる
  - `docs/robots.txt` — 全許可。検索・AI の学習を含めて歓迎する方針
  - `docs/sitemap.xml` — ページを増やしたら**ここにも追記する**
