# XViewer for Pixiv - 紹介サイト

Chrome 拡張機能 **[XViewer for Pixiv](https://github.com/NEONS-DESIGN/XViewer-for-Pixiv)** の紹介サイトとプライバシーポリシー。
GitHub Pages で公開する静的サイト。ビルド工程は無い。

## 構成

| パス | 役割 |
| --- | --- |
| `index.html` | 日本語のランディングページ |
| `privacy.html` | 日本語のプライバシーポリシー |
| `en/index.html` | 英語のランディングページ |
| `en/privacy.html` | 英語のプライバシーポリシー |
| `assets/style.css` | 全ページ共通の CSS |
| `assets/site.js` | 配色の切り替えだけを行う JS |
| `assets/img/` | アイコンとスクリーンショット |
| `.nojekyll` | GitHub Pages の Jekyll 処理を無効にする |

配色トークンは拡張本体の `UI_DESIGN_KIT.md` §2 に合わせてある。値を変えるときは変数だけを直す。

## 公開の手順

1. GitHub のリポジトリ設定 → Pages
2. Source を **Deploy from a branch**、ブランチを `main` / `(root)` にする
3. 公開 URL は `https://neons-design.github.io/XViewer-for-Pixiv-Web/`

## 差し替えが必要な箇所

| 印 | 内容 | 場所 |
| --- | --- | --- |
| `__CONTACT_EMAIL__` | 問い合わせ用のメールアドレス | `privacy.html`, `en/privacy.html` |
| `__CONTACT_FORM_URL__` | Google フォームの URL | `privacy.html`, `en/privacy.html` |
| ストアのボタン | `aria-disabled="true"` を外し `href` にストア URL を入れる。直下の `.cta-note` の一文も消す | `index.html`, `en/index.html` |

残っている印は次で探せる。

```bash
grep -rn "__CONTACT_" .
```

## ローカルでの確認

```bash
python -m http.server 8000
```

`http://localhost:8000/` を開く。ファイルを直接開く (`file://`) と相対パスは動くが、
ルート相対のリンクだけ挙動が変わるので、確認はサーバー経由で行うこと。

## スクリーンショットについて

`assets/img/shot-*.png` は動作説明のためのデモ用データ。作品画像は作者自身のもので、
コメントやカウンタは説明のために差し込んだダミー値。その旨は各ページのフッターに明記している。

## ライセンス

サイトの文章とスタイルは拡張機能本体と同じライセンスに従う。
アイコンの図形は Material Symbols (Apache-2.0) と Font Awesome Free (CC BY 4.0)。
