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
| `assets/img/` | アイコン、スクリーンショット、無限スクロールの動画 |
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

## スクリーンショットと動画について

`assets/img/shot-*` は動作説明のためのデモ用データ。作品画像は作者自身のもので、
コメントやカウンタは説明のために差し込んだダミー値。その旨は各ページのフッターに明記している。

`assets/img/infinite-scroll.mp4` は無限スクロールの実動作を画面録画したもの。
pixiv 事務局の公式アカウント (`/users/11`) を実際にスクロールしており、作品にもカウンタにも手を加えていない。
ブラウザのアドレスバーまで写しているので、URL の `?p=` が画面に追従する様子もそのまま見える。

動画は `<video autoplay loop muted playsinline>` で音無し・自動ループ再生。
5 秒を超えて自動で動くものには止める手段が要る (WCAG 2.2.2) ので、右下に停止ボタンを常設している。
OS の「動きを減らす」設定まで効かせて最初から止めたい場合は、`assets/site.js` の
`PAUSE_WHEN_CALM` を `true` にする (既定は `false`)。

**録画をそのまま置かないこと。** 画面録画は HEVC (H.265) で出ることが多く、Chrome と Firefox では再生できない。
H.264 に変換し、ポスター画像 `infinite-scroll.jpg` も一緒に作り直す。
`scale` の幅は表示幅 (約 560px) の 2 倍あれば足りる。

```bash
ffmpeg -i recording.mp4 -vf "scale=1280:-2" -an   -c:v libx264 -crf 32 -preset slow -pix_fmt yuv420p -movflags +faststart   assets/img/infinite-scroll.mp4
ffmpeg -i assets/img/infinite-scroll.mp4 -frames:v 1 -q:v 3 assets/img/infinite-scroll.jpg
```

変換後は `codec_name` が `h264` になっていることを確かめる。

```bash
ffprobe -v error -show_entries stream=codec_name,width,height -of default=noprint_wrappers=1   assets/img/infinite-scroll.mp4
```

## ライセンス

サイトの文章とスタイルは拡張機能本体と同じライセンスに従う。
アイコンの図形は Material Symbols (Apache-2.0) と Font Awesome Free (CC BY 4.0)。
