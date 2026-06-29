# misoclub web

misoclub の公式サイト。素の HTML / CSS で作られた静的サイトで、GitHub Pages で公開します。

## 構成

```
.
├── index.html              # ランディングページ
├── contact.html            # お問い合わせ（mailto 中心）
├── privacy/
│   └── sample-app.html     # 各アプリ用テンプレート（複製して使う）
├── css/
│   └── style.css           # 共通スタイル（色は :root の変数で調整）
├── assets/                 # 画像など
└── .nojekyll               # GitHub Pages の Jekyll 処理を無効化
```

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
- [ ] ランディングページのキャッチコピー・「私たちについて」を実際の内容に
- [ ] OGP 画像・favicon の追加
- [ ] （必要なら）お問い合わせフォームの実送信対応
