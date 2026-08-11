# Luftelli公式ページ - Copilot Instructions

## プロジェクト概要

ゲーム制作サークルLuftelliの公式静的ウェブサイト（GitHub Pages）。Bootstrap 5.3ベースのレスポンシブデザインで、画面幅を活かしたワイドレイアウトを採用。

## アーキテクチャ

- **静的サイト**: フレームワーク/ビルドツール不使用
  - `index.html`: トップページ（レイアウトCSSはページ内の `<style>` に記述）
  - `terms.html` / `privacy.html` / `content-guidelines.html` / `ai-policy.html` / `contact.html`: 方針・規約系ページ
  - `site-policy.css`: 全ページ共通のCSS（ナビ、フッター、シェル幅、規約ページのレイアウト）
- **ホスティング**: GitHub Pages（カスタムドメイン: luftelli.com via `CNAME`）
- **スタイリング**: Bootstrap 5.3.2 + Font Awesome 6.4.2 + Noto Sans JP（CDN経由）
- **アナリティクス**: Google Analytics（G-8YXW37KQM9）
- **言語**: 日本語（`lang="ja"`）

## 重要な規約

### デザイン規約（全ページ共通）

**`site-policy.css` が全ページ共通のスタイルシート**（規約ページ専用ではない）。
`index.html` のページ内 `<style>` はトップページ固有のスタイルだけを持つ。
**色・角丸・影・余白を個別ページに直書きしないこと。** 必ず `:root` の変数を使う。

| 変数 | 用途 |
|---|---|
| `--shell-max` / `--shell-gutter` | `.container` の最大幅（1500px）と左右余白 |
| `--header-offset` | 固定ヘッダー分のアンカーオフセット（90px） |
| `--card-radius` / `--card-border` / `--card-shadow` / `--card-shadow-hover` | カード・パネルの共通見た目 |
| `--panel-radius` | 本文コンテナ（`.main-container` / `.policy-main`）の角丸 |
| `--text-dark` / `--text-body` / `--text-light` | 文字色の3段階 |

共通CSSが持つもの（個別ページで再定義しない）:

- 背景（空のグラデーション、`body::before` の雲アニメ、`body::after` のグリッド）
- ナビゲーション、フッター、スクロールバー
- 本文コンテナ `.main-container, .policy-main`（両者は同一定義。上端のシマー線も共通）

### ヘッダー

**全ページで完全に同一のマークアップを使うこと。** 現在ページの印
（`active` / `aria-current="page"`）だけがページごとの差分。

- `position: sticky; top: 0` で上端に追従する（HTML側のクラスは `sticky-top`）
- `body` は `overflow-x: clip`。**`hidden` にすると body がスクロールコンテナ扱いになり
  ヘッダーの sticky が効かなくなる**ので戻さないこと
- **`<a class="nav-link">` の中に前後の改行・インデントを入れないこと。**
  `dropdown-toggle` は `::after` で▼が付くため、閉じタグ前の空白が文字と▼の間に残り、
  リンク幅が約3.6pxずれてナビ全体の位置が狂う

その他の規約:

- **本文の行長**: 幅を広げても読みやすさを保つため、長文は1080px程度で頭打ちにする
  （規約ページは `.policy-layout > .policy-list` などで制御）。
- **アンカー位置**: 固定ヘッダーに隠れないよう `scroll-margin-top` に `--header-offset` を使う。
- **`site-policy.css` の更新時**: 各HTMLの `?v=YYYYMMDD-N` を揃えて更新する（キャッシュ対策）。

意図的に揃えていない箇所:

- ヒーロー: トップは `.hero-section`（左右2カラム＋キービジュアル）、下層は `.policy-hero`（中央寄せ）
- 行間: 長文を読ませる規約ページのみ `body.policy-page { line-height: 1.9 }`

### HTMLの編集

1. **日本語コンテンツ**: すべてのテキストは日本語で記述（メタデータ含む）
2. **外部リンク**: `target="blank"` を使用（例: X、YouTube、ブログ、Steam）
3. **画像**: `images/` ディレクトリに配置
4. **CDNのバージョン変更時**: 整合性ハッシュ（`integrity` 属性）も更新する

### トップページ（index.html）の構成

- **ヒーロー**: `.hero-grid` の左右2カラム（左=コピー＋CTA、右=キービジュアル `images/ctm_header.jpg`）。
  lg未満では1カラムに折り返す。
- **Luftelliについて**: `.about-grid`（本文カード＋サークル名の由来カード）
- **現在開発中のタイトル**: `.showcase`（左=スクリーンショットのスライド、右=情報とCTA）。
  スライドはBootstrapのCarousel（`#shotCarousel`）で、追加のライブラリは使っていない。
  画像は `aspect-ratio: 16/9` 固定なので、素材が16:9ならトリミングされない。
  **`object-fit: cover` で画像をカードの高さに追従させないこと**（上下が切れる）。

  カードの高さは「画像の高さ」と「本文の高さ」の大きい方になる。本文の方が高いと
  画像の上下に帯が出るため、**常に画像 ≧ 本文** になるよう次の2点で調整している。
  本文の要素を増やす・余白を広げる場合は、各幅で帯が出ないか実測して確認すること。

  1. 左右2カラムにするのは1400px以上（それ未満は縦積みで画像が全幅になり帯は出ない）
  2. 2カラム時の列比は `1.7fr : 1fr` で画像側を大きく取る

  素材の縦横比が16:9でない場合は `aspect-ratio` を素材に合わせること。
- **最新情報／活動実績**: `.section-duo` で2つの `<section>` を横並びにする
- **メンバー**: `.member-grid`（`auto-fit` の自動折り返し。カード追加時のCSS変更は不要）

### 規約・方針ページの構成

`<main class="policy-main">` > `<div class="container policy-layout">` の下に、
`<aside class="policy-side">`（目次）と `.policy-list` / `.policy-stack`（本文）を並べる。

- 目次は `.policy-nav` > `.policy-toc` で、番号は CSS counter で自動採番される
- 各セクションの `<article class="card" id="...">` と目次の `href` を対応させる
- 1200px未満では目次が本文の上に回り込む（sticky解除）
- セクションが1つだけのページ（contact.html）は目次を置かず、本文幅だけ揃える

### 開発ワークフロー

1. **デプロイ**: `master-deploy` ブランチへのマージで自動デプロイ（GitHub Pages）
2. **PR作成**: `.github/pull_request_template.md` を使用（Issue番号必須）
3. **ローカルプレビュー**: `index.html` を直接ブラウザで開く（ビルド不要）

### ライセンス

- **CC BY-NC-ND 4.0**: 非商用、改変不可、クレジット表示必須
- `LICENSE` ファイル参照（403行のCreative Commons全文）

## CDN依存関係

```html
<!-- Bootstrap 5.3.2（CSS + JS Bundle。jQuery/Popperは不要） -->
<link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.2/dist/css/bootstrap.min.css" rel="stylesheet">
<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.2/dist/js/bootstrap.bundle.min.js"></script>

<!-- Font Awesome 6.4.2 -->
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.2/css/all.min.css">

<!-- Google Fonts: Noto Sans JP -->
<link href="https://fonts.googleapis.com/css2?family=Noto+Sans+JP:wght@300;400;500;700&display=swap" rel="stylesheet">
```

バージョン変更時は整合性ハッシュ（`integrity` 属性）の更新が必要。

## 一般的なタスク

### 開発中タイトルの情報を更新

`index.html` の `#current-project` セクション（`.showcase`）を編集する。
スクリーンショットを増減する場合は、`#shotCarousel` の `.carousel-inner` に
`<div class="carousel-item"><img ...></div>` を追加し、`.carousel-indicators` の
ボタン（`data-bs-slide-to`）も同じ数だけ増やす。`active` は先頭の1組だけに付ける。

なお `prefers-reduced-motion: reduce` の環境では、ページ末尾のスクリプトが
`data-bs-ride` を外して自動送りを止める（Bootstrapの初期化前に実行する必要がある）。

### メンバーを追加/更新

`index.html` の `.member-grid` 内に追加する（列数は自動調整されるためCSSの変更は不要）:

```html
<div class="member-card">
    <div class="member-header">
        名前 <a href="XのURL" target="blank" title="X"><i class="fab fa-x-twitter"></i></a>
    </div>
    <div class="member-text">説明<br>追加情報。</div>
</div>
```

活動休止中のメンバーは `member-card member-card-inactive` とし、
`<div class="member-status">現在は活動休止中</div>` を添える。

### 規約ページにセクションを追加

1. `.policy-list` に `<article class="card" id="新しいID">` を追加する
2. 同じページの `.policy-toc` に `<li><a href="#新しいID">見出し</a></li>` を追加する
   （番号はCSSが自動採番するため手で書かない）

## タスク実施時の順守事項

- ライブラリのドキュメントはcontext7ツールを使用して参照すること
- 変更後はブラウザでの表示確認を必ず行うこと
- パフォーマンスが問題ないかchrome-devtoolsツールで確認すること
