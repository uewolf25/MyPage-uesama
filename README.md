# MyPage-uesama

自宅サーバのウェルカムページ。issue #1 のデザイン案をそのまま静的サイトにしたもの。

## 構成

| ファイル | 内容 |
| --- | --- |
| `index.html` | トップページ（About / Hardware / Stack / Services / Network / Log） |
| `blog.html` | 記事一覧（プレースホルダ） |
| `styles.css` | 全ページ共通のスタイル |

ビルド不要・依存パッケージなし（issue #2 の「軽量で動かせると嬉しい」に合わせて、素の HTML / CSS + 十数行の JS）。
外部から取りに行くのは Google Fonts（IBM Plex Mono / Zen Kaku Gothic New）だけで、読めない環境ではシステムフォントにフォールバックする。

## ローカルで見る

`index.html` をブラウザで直接開くだけで表示できる。HTTP で確認したい場合は:

```sh
python3 -m http.server 8000
# http://localhost:8000/
```

## 公開（Nginx の例）

静的ファイルを配信するだけ。

```nginx
server {
    server_name fridge-mate.jp;
    root /var/www/mypage;
    index index.html;
}
```

## ステータス表示について

トップの UPTIME / CONTAINERS / SERVICES は、同一オリジンの `/api/status` から取得する。
返す JSON の形式:

```json
{ "uptime": "42d 6h", "containers": 12, "services": 4 }
```

エンドポイントが無い・落ちている場合は `—` のまま表示されるので、用意しなくてもページは壊れない。

## 書き換えポイント

- `index.html` の SERVICES 各行の `href="#"` — 各サービスの URL に差し替え
- `index.html` の `<div class="photo-frame">` — 実機写真 `<img class="photo-img" src="..." alt="...">` に差し替え
- HARDWARE の POWER、STACK の OPS — 「記載予定」のまま残してある
- LOG / `blog.html` の日付とタイトル — 記事を書いたら差し替え
- 色・フォントは `styles.css` 冒頭の `:root` にまとめてある
