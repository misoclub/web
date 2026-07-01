# misoclub web

misoclub の公式サイト。素の HTML / CSS で作られた静的サイトで、GitHub Pages で公開します。

## 構成

アプリは **1 アプリ = 1 ディレクトリ**（`apps/<アプリ>/`）にまとめ、中身は固定ファイル名（`index.html` / `privacy.html` / `icon.png` / `splash.jpg`）で統一しています。追加するときはフォルダごとコピーするだけです。

```
.
├── index.html              # トップページ（ヒーロー＋アプリ一覧 #apps）
├── apps/
│   ├── index.html          # アプリ一覧ページ（/apps/）
│   ├── _template/          # アプリ追加用テンプレート（フォルダごとコピー）
│   │   ├── index.html      #   DLページの雛形
│   │   └── privacy.html    #   プライバシーポリシーの雛形
│   ├── nihon-tabinikki/    # 1 アプリ = 1 ディレクトリ（/apps/nihon-tabinikki/）
│   │   ├── index.html      #   DLページ（シェアからの遷移先）
│   │   ├── privacy.html    #   プライバシーポリシー
│   │   ├── icon.png        #   アイコン（一覧カード用・正方形 256px）
│   │   └── splash.jpg      #   スプラッシュ（DLページ上部・縦長）
│   └── memoria/
│       ├── index.html
│       ├── privacy.html
│       ├── icon.png
│       └── splash.jpg
├── css/
│   └── style.css           # 共通スタイル（色は :root の変数で調整。スマホ最適化済み）
├── site.webmanifest        # PWA/ホーム画面用マニフェスト
├── assets/                 # サイト共通の画像（アプリ固有のものは apps/<アプリ>/ 配下）
│   ├── logo.png            # ロゴ（logo-source.png から切り抜き済み）
│   ├── logo-source.png     # ロゴ原本（再クロップ用）
│   ├── icon-source.svg     # アイコン（ハート）の元データ。再生成用
│   ├── favicon.ico         # favicon（16/32/48）
│   ├── favicon-16/32.png   # favicon（PNG）
│   ├── apple-touch-icon.png# iOS ホーム画面アイコン（180）
│   ├── icon-192/512.png    # Android/PWA アイコン
│   ├── og-image.png        # OGP画像（1200x630）
│   ├── app-store-badge.svg # App Store 公式バッジ（日本語）
│   └── google-play-badge.png # Google Play 公式バッジ（日本語）
└── .nojekyll               # GitHub Pages の Jekyll 処理を無効化
```

> URL は `/`（トップ）、`/apps/`（一覧）、`/apps/<アプリ>/`（各アプリのDLページ）、`/apps/<アプリ>/privacy.html`（ポリシー）になります。

## アイコン・OGP画像の再生成

ロゴやハートを変えたら、`assets/icon-source.svg`（ハート）と `assets/logo.png`（ワードマーク）から ImageMagick で再生成できます。

```sh
cd assets
magick -background none icon-source.svg -resize 512x512 icon-512.png
magick -background none icon-source.svg -resize 192x192 icon-192.png
magick -background none icon-source.svg -resize 180x180 apple-touch-icon.png
magick -background none icon-source.svg -define icon:auto-resize=16,32,48 favicon.ico
# OGP（ダーク背景にワードマーク中央）
magick logo.png -resize 760x -background "#171716" -gravity center -extent 1200x630 og-image.png
```

## アプリを追加するとき

1. **`apps/_template/` フォルダを `apps/<アプリ>/` にコピー**
2. その中の `index.html`（DLページ）と `privacy.html`（ポリシー）を編集（アプリ名・各ストアの URL（`href="#"`）・ポリシー本文）
3. **`icon.png`**（正方形・256px 以上）と **`splash.jpg`**（縦長）を同フォルダに置く
4. **`apps/index.html`**（`/apps/` 一覧）の `<article class="app-card">` を 1 つ複製し、アイコンのパス（`./<アプリ>/icon.png`）・アプリ名・説明・各ストアの URL を差し替え
5. **トップページ `index.html`** の `#apps` セクションにも `<div class="app-card app-card--link">` を 1 つ複製して追記（カードのリンク先＝`./apps/<アプリ>/index.html`、鍵アイコンのリンク先＝`./apps/<アプリ>/privacy.html`、アイコン・アプリ名・説明を差し替え）。**アプリはトップと `/apps/` の 2 か所に載る**ので、両方の更新を忘れないこと。

ストアバッジは App Store / Google Play とも公式バッジ画像を使い、両者を**同じ高さ・横並び**で表示します（一覧カード内、および各アプリの DLページ）。

## アプリのダウンロードページ（`apps/<アプリ>/index.html`）

アプリのシェア機能などから飛ばす遷移先。上部に各アプリのスプラッシュ画像を表示し、iOS / Android を選んでダウンロードしてもらう最小ページです。misoclub の他アプリ一覧（`/apps/`）以外のリンクは置いていません（misoclub ロゴも置きません）。

## ローカルで確認する

ビルド不要。ファイルをブラウザで開くだけでも見られますが、相対リンクを正しく確認するには簡易サーバーが便利です。

```sh
python3 -m http.server 8000
# → http://localhost:8000 を開く
```

## プライバシーポリシー

各アプリの `apps/<アプリ>/privacy.html`。アプリ側からこの URL を直接リンクします（検索結果には出さない `noindex` 付き）。`apps/_template/privacy.html` が雛形です。

## GitHub Pages で公開する

リポジトリの Settings → Pages で、Source を `main` ブランチのルート (`/`) に設定すると公開されます。

## TODO

- [x] お問い合わせ先メールアドレス（`misoclubsupport@gmail.com`）を設定
- [x] favicon・ホーム画面アイコン・OGP画像を追加
- [x] App Store / Google Play とも公式バッジを使用
- [ ] ストアURL（App Store / Google Play）を差し替え（一覧カード内バッジ・各アプリの DLページ。現在は `href="#"`）
- [ ] 公開URL（独自ドメイン等）が決まったら、各ページの `og:image` を**絶対URL**に変更（SNS/メッセージのプレビューは相対パスだと表示されないことがある）。あわせて `og:url` も設定推奨
- [x] トップページ（`index.html`）にヒーロー＋アプリ一覧（`#apps`）を用意
