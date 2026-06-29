# misoclub web

misoclub の公式サイト。素の HTML / CSS で作られた静的サイトで、GitHub Pages で公開します。

## 構成

```
.
├── index.html              # ランディングページ
├── apps.html               # アプリ一覧（リリース済みアプリ専用ページ）
├── contact.html            # お問い合わせ（mailto 中心）
├── dl/                     # アプリDL専用ページ（シェアからの遷移先）
│   ├── nihon-tabinikki.html
│   └── memoria.html
├── privacy/
│   └── sample-app.html     # 各アプリ用テンプレート（複製して使う）
├── css/
│   └── style.css           # 共通スタイル（色は :root の変数で調整。スマホ最適化済み）
├── site.webmanifest        # PWA/ホーム画面用マニフェスト
├── assets/
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

## アプリのダウンロードページ（`dl/`）

アプリのシェア機能などから飛ばす遷移先。iOS / Android を選んでダウンロードしてもらうための最小ページで、misoclub の他アプリ一覧（`apps.html`）以外のリンクは置いていません。

新しいアプリを追加するときは `dl/` のファイルを複製し、アプリ名と各ストアの URL（`href="#"` の箇所）を差し替えてください。

## ローカルで確認する

ビルド不要。ファイルをブラウザで開くだけでも見られますが、相対リンクを正しく確認するには簡易サーバーが便利です。

```sh
python3 -m http.server 8000
# → http://localhost:8000 を開く
```

## 新しいアプリのプライバシーポリシーを追加する

1. `privacy/sample-app.html` を `privacy/<アプリ名>.html` という名前で複製
2. アプリ名・更新日・各項目を編集
3. アプリ側からそのページの URL を直接リンク

## GitHub Pages で公開する

リポジトリの Settings → Pages で、Source を `main` ブランチのルート (`/`) に設定すると公開されます。

## TODO

- [x] お問い合わせ先メールアドレス（`misoclubsupport@gmail.com`）を設定
- [x] favicon・ホーム画面アイコン・OGP画像を追加
- [x] App Store / Google Play とも公式バッジを使用
- [ ] 各DLページのストアURL（App Store / Google Play）を差し替え（現在は `href="#"`）
- [ ] 公開URL（独自ドメイン等）が決まったら、各ページの `og:image` を**絶対URL**に変更（SNS/メッセージのプレビューは相対パスだと表示されないことがある）。あわせて `og:url` も設定推奨
- [ ] ランディングページのキャッチコピー・「私たちについて」を実際の内容に
- [ ] （必要なら）お問い合わせフォームの実送信対応
