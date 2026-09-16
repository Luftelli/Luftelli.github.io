# Luftelli公式サイト — エージェント作業指針

このファイルはリポジトリ全体に適用する。ユーザーの明示的な指示を優先し、以下を通常の作業基準とする。

## 作業の進め方

- 依頼された変更を、調査・編集・必要な検証まで完了する。提案だけで止めない。
- 最初に `git status --short` と対象ファイルを確認し、既存の未コミット変更を保持する。
- 通常の実装判断は既存コードと本指針から決める。結果を大きく変える不明点だけを質問し、回答に依存しない作業は進める。既に許可された作業について再確認しない。
- 変更は依頼の範囲に絞る。無関係な整形、依存更新、フレームワークやビルド工程の追加は行わない。
- 構成説明と実装が異なる場合は関連箇所を確認する。説明に合わせるためだけにサイトを改変しない。
- 検証は変更の影響に合わせる。必要な確認が通った後、理由なく検証を拡大・反復しない。
- 完了時は日本語で、変更内容・検証結果・未確認事項を簡潔に報告する。未実施の確認を実施済みと書かない。

## プロジェクトと編集先

ゲーム制作サークルLuftelliの日本語公式サイト。ビルド不要の静的HTML/CSS/JavaScriptで、GitHub Pagesに公開する。

| ファイル | 役割 |
|---|---|
| `index.html` | トップページ。ページ内の `<style>` はトップ固有のレイアウトのみ |
| `terms.html` / `privacy.html` / `content-guidelines.html` / `ai-policy.html` | 規約・方針ページ |
| `contact.html` | お問い合わせページ |
| `site-policy.css` | **全ページ共通**の変数、背景、ナビ、フッター、本文コンテナ、規約ページのレイアウト |
| `images/` | 画像素材 |
| `CNAME` | カスタムドメイン `luftelli.com` |
| `.github/pull_request_template.md` | PRの書式。対応Issue番号が必須 |
| `LICENSE` | CC BY-NC-ND 4.0（非商用・改変不可・クレジット表示必須）の全文 |

CDN依存はBootstrap 5.3.2（CSSとJS Bundle）、Font Awesome 6.4.2、Noto Sans JP（300/400/500/700）。jQueryや追加のCarouselライブラリは不要。URLと `integrity` は各HTMLを参照する。Google AnalyticsのIDは `G-8YXW37KQM9`。

## 共通デザインの制約

- 共通スタイルは `site-policy.css` に集約し、個別ページで再定義しない。
- 色・角丸・影・余白は同ファイルの `:root` の変数を再利用する。必要なトークンがなければ共通側に定義する。個別ページに色値や `rgba(...)` を追加しない。
- 青は `--hero-sky*` の色相から派生させる。ロイヤルブルーやBootstrap既定の `#0d6efd` を混ぜず、Bootstrapのリンク色の上書きも維持する。
- 通常の文字は背景とのコントラスト比4.5:1以上を保つ。

| 変数 | 用途・基準 |
|---|---|
| `--shell-max` / `--shell-gutter` | 共通コンテナの最大幅1320pxと左右余白 |
| `--header-offset` | アンカーの `scroll-margin-top`。現在90px |
| `--card-radius` / `--card-border` / `--card-shadow` / `--card-shadow-hover` | カードの共通見た目。角丸は6px |
| `--panel-radius` / `--button-radius` | 本文コンテナは現在0、ボタンはカードと同じ6px |
| `--hero-sky-light` / `--hero-sky` / `--hero-sky-deep` | 空のグラデーションの基準色 |
| `--sky-blue` / `--interactive` / `--interactive-hover` | 境界線・リンク・ボタンの青 |
| `--sky-tint` / `--sky-tint-strong` | 薄い面・罫線・ホバー背景の透明な空色 |
| `--surface` / `--cloud-white` | 白い面・淡い補足面 |
| `--text-dark` / `--text-body` / `--text-light` | 紺系の文字色3段階 |
| `--text-on-dark` / `--text-on-dark-muted` / `--text-on-sky` | 濃い背景・空色背景上の文字 |
| `--hero-bg` / `--band-bg` | ページ背景と本文の淡い帯 |
| `--space-medium` / `--section-gap` / `--hero-padding` / `--policy-*` など | 共通の余白。具体的な定義は `:root` を参照 |

- 背景は `--hero-bg` と `body::before` の静かな雲。ナビとフッターも共通CSSで管理する。
- `.main-container, .policy-main` は共通定義と `flex: 1` を維持し、本文が短くてもフッターを最下部に置く。
- 長文の本文幅は1080px程度を上限にする。シェル幅と本文幅を混同しない。
- スクロールバーはOS既定のままにし、`::-webkit-scrollbar` で装飾しない。
- ピル型バッジ・チップ、グラデーション文字、ガラス風ぼかし、光る線、日本語小見出しの字間拡張を追加しない。
- 公開情報などは `／` 区切りのテキストにする。装飾目的のFont Awesomeアイコンは使わない。ブランドアイコン（X・YouTube・Steam）と外部リンク印は可。

## 全HTMLにまたがる変更

- ユーザー向けテキストとメタデータは日本語、文書言語は `lang="ja"` とする。固有名詞は元の表記を保つ。
- 外部リンクは既存規約の `target="blank"` に合わせる。画像は `images/` に配置する。
- CDNのバージョンを変更したら、対応する `integrity` も更新する。
- `site-policy.css` を変更したら、それを参照する全HTMLの `?v=YYYYMMDD-N` を同じ新しい値に更新する。

### ヘッダー

- 6ページのヘッダーは同一マークアップにする。ページごとの差分は `active` と `aria-current="page"` のみ。
- 現在のナビ順は「Luftelliについて → 光影の塔 → お問い合わせ → ブログ → X → YouTube」。変更時は全6ページに反映する。
- HTMLの `sticky-top` とCSSの `position: sticky; top: 0` を維持する。
- `body` の `overflow-x: clip` を `hidden` に変更しない。スクロールコンテナが変わるとヘッダーのstickyが効かなくなる。
- `<a class="nav-link">` の内容の前後に改行・インデントを入れない。特に `dropdown-toggle::after` の前の空白はリンク幅をずらす。

## トップページの構造と更新

### 全体

- ヒーローは `.hero-grid` の2カラム（左コピー、右 `images/ctm_header.jpg`）。992px未満で1カラムにする。
- キービジュアル下部のバーは「詳しく見る」の導線だけにする。公開時期などはショーケースに集約する。
- 本文順は「Luftelliについて → 現在開発中のタイトル → 活動実績 → メンバー」。ナビの項目構成とは区別する。
- 各セクションは `.main-container > section.section > .container` とし、帯を全幅に敷ける構造を保つ。
- 作品を強調する空色の面はヒーローのキービジュアルとショーケースに使う。本文の `.section-band` は開発中タイトルだけに付け、Aboutやメンバーには付けない。共通背景やUIの青はこの制限の対象外。
- `.section-intro` は見出しとカードの間に置く。フッターは `--cloud-white` の淡い面で閉じる。
- Aboutは `.about-grid`（本文と名前の由来）、活動実績は `.achievement-list`（年月と本文の罫線リスト）を使う。

### 開発中タイトル・スクリーンショット

- 更新先は `#current-project` 内の `.showcase`。左にBootstrap Carouselの `#shotCarousel`、右に説明とCTAを置く。
- 2カラムは1400px以上、列比は `1.7fr : 1fr`。それ未満では縦積みにする。
- 画像の `aspect-ratio: 16/9` を維持し、異なる比率の素材を使う場合は素材に合わせる。`object-fit: cover` でカード高に追従させて画像を切り取らない。
- 2カラムでは画像の高さが本文以上になるようにする。本文・余白を増やしたら、画像の上下に帯が出ないことを各幅で実測する。
- 画像の増減時は `.carousel-inner` の項目と `.carousel-indicators` のボタン数を揃え、`data-bs-slide-to` を0からの連番にする。`active` は先頭の画像とボタンの1組だけに付ける。
- `prefers-reduced-motion: reduce` で `data-bs-ride` を外す処理を維持する。BootstrapのCarousel自動初期化より前に実行する必要がある。

### メンバー

- `.member-grid` 内の既存 `.member-card` を参考に追加する。`auto-fit` で折り返すため、人数変更だけなら列数のCSS変更は不要。
- 名前とリンクは `.member-header`、紹介文は `.member-text` に置く。
- 活動休止中は `member-card member-card-inactive` とし、`.member-status` に「現在は活動休止中」を添える。バッジ化しない。

## 規約・方針ページの構造と更新

- `<main class="policy-main"> > <div class="container policy-layout">` 内に、目次の `<aside class="policy-side">` と本文の `.policy-list` または `.policy-stack` を置く。
- 目次は `.policy-nav > .policy-toc`。番号はCSS counterに任せ、手入力しない。
- セクション追加時は本文の `<article class="card" id="...">` と目次のリンクをセットで追加し、`id` と `href` を一致させる。
- 1200px未満では目次を本文の上に移し、stickyを解除する。
- `contact.html` のような1セクションのページには目次を置かず、本文幅だけ揃える。
- 下層ヒーローは左寄せの `.policy-hero`。`.policy-hero-head` にh1と制定日の `.policy-hero-meta`、その下に `.policy-hero-lead` を置く。トップのヒーロー構造には統一しない。
- 規約ページだけ `body.policy-page { line-height: 1.9 }` を適用する。

## 検証と完了条件

ライブラリ仕様の確認が必要な場合はContext7を使用する。利用できなければ公式ドキュメントを参照し、代替したことを報告する。
ローカル表示はHTMLを直接ブラウザで開く。必要な場合のみローカルの静的HTTPサーバーを使い、ビルド工程は追加しない。

| 変更の種類 | 必要な確認 |
|---|---|
| `AGENTS.md` など文書だけ | 差分、記述の整合性、参照先の存在を確認。サイトの表示・性能検証は不要 |
| HTML・CSS・JavaScript・画像 | 対象ページをブラウザで表示し、デスクトップとモバイルで表示・操作・コンソールエラーを確認 |
| 共通CSS・ナビ・フッター | 全6ページの共通部分、CSSのキャッシュ値、リンク先の整合性を確認 |
| レイアウト | 変更したブレークポイントの前後も確認。対象に応じて992px・1200px・1400px付近で折り返し、横スクロール、アンカー位置を確認 |
| Carousel・動き | 画像とボタンの対応、手動送り、通常時と `prefers-reduced-motion: reduce` 時の動作を確認 |
| サイトの表示・動作に関わる変更 | Chrome DevToolsで読み込み・レイアウトシフト・操作時の性能を確認。性能への影響がある変更では同条件の変更前後を比較 |

- ブラウザやChrome DevToolsツールが使えない場合は、可能な静的検証を進め、未実施の確認と理由を報告する。利用できないツールを使ったことにしない。
- 差分の最終確認に `git diff --check` を使う。変更内容に見合わないテスト基盤の新設は不要。
- PRを作成する場合はテンプレートを使用する。Issue番号が不明なら捏造せず、その情報だけを確認する。
- `master-deploy` へのマージでGitHub Pagesが自動デプロイされる。公開を伴う操作はユーザーから依頼された範囲で実施する。
