# Notion Clock + Builder (Pure HTML/CSS/JS)

Notionに `/embed` で埋め込めるデジタル時計と、見た目をGUI編集して埋め込みURLを作るビルダーです。依存ライブラリは使っていません。

## ファイル構成

- `clock.html` : 埋め込み対象の時計ページ
- `builder.html` : GUIビルダー（URL生成 / プレビュー / コピー）
- `presets.json` : `profile=xxx` 用のプリセット定義
- `README.md` : 使い方

## ローカル動作確認

> `file://` 直開きではなく、HTTPサーバー経由で確認してください。

```bash
python -m http.server 8000
```

ブラウザで以下を開きます。

- Builder: `http://localhost:8000/builder.html`
- Clock: `http://localhost:8000/clock.html`

## GitHub Pages で公開（推奨）

1. このリポジトリを GitHub に push
2. GitHub の **Settings → Pages** を開く
3. **Build and deployment** を `Deploy from a branch` に設定
4. Branch は `main`、Folder は `/ (root)` を選択して保存
5. 公開URL（例: `https://<user>.github.io/<repo>/`）が発行される

ビルダーの「ベースURL」に上記公開URLを入力すると、Notion用にそのURLを基準に `clock.html` が生成されます。

## Notion への埋め込み手順

1. `builder.html` を開いて見た目を調整
2. 「URLコピー」で埋め込みURLをコピー
3. Notionページで `/embed` を入力
4. コピーしたURLを貼り付けて埋め込み

## 重要: Notion は `file://` を埋め込めません

Notionはクラウドサービス上で表示を行うため、あなたのPCローカルファイル (`file://`) や `localhost` にはアクセスできません。必ず外部から到達可能な `https://` 公開URLを使用してください。

## Clock クエリパラメータ

- `layout`: `card | bar | minimal`
- `align`: `left | center | right`
- `tz`: 例 `Asia/Tokyo`（未指定ならローカルTZ）
- `sec`: `1/0` 秒表示
- `date`: `1/0` 日付表示
- `tzShow`: `1/0` TZ表示
- `h12`: `1/0` 12時間表記
- `scale`: `0.7〜1.5`
- `bg`: 背景色（`transparent` 推奨）
- `cardBg`: カード背景色（rgba可）
- `fg`, `sub`, `accent`: 文字色
- `radius`, `pad`: 数値（px）
- `shadow`: `1/0`
- `profile`: `presets.json` のキー名

### profile の優先順位

`profile=xxx` を指定すると `presets.json` を読み込み、その後にクエリ文字列の値で上書きします（クエリ最優先）。

## 手動テスト観点

- `layout` を `card/bar/minimal` で切替し崩れない
- `align` 左/中央/右で配置が反映される
- `tz` を `Asia/Tokyo`, `UTC`, 空欄で切替できる
- `sec` ON/OFFで秒表示が切り替わる
- `date` ON/OFFで日付表示が切り替わる
- `tzShow` ON/OFFでTZラベルが切り替わる
- `h12` ON/OFFで12/24時間表記が切り替わる
- `scale/radius/pad` に不正値を入れてもデフォルトにフォールバックする
- `profile` 指定URLでプリセットが適用される
- `file://` または `localhost` 系URLで警告が表示される
- 「URLコピー」がClipboard API失敗時にpromptフォールバックする
