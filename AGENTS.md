# Luftelli公式ページ - Copilot Instructions

## プロジェクト概要

ゲーム制作サークルLuftelliの公式静的ウェブサイト（GitHub Pages）。シンプルな単一HTMLページ構成で、Bootstrap 4.3.1ベースのレスポンシブデザイン。

## アーキテクチャ

- **静的サイト**: `index.html` 1ファイルのみ（フレームワーク/ビルドツール不使用）
- **ホスティング**: GitHub Pages（カスタムドメイン: luftelli.com via `CNAME`）
- **スタイリング**: Bootstrap 4.3.1 + Font Awesome 5.6.1（CDN経由）
- **アナリティクス**: Google Analytics（G-8YXW37KQM9）
- **言語**: 日本語（`lang="ja"`）

## 重要な規約

### HTMLの編集

1. **Bootstrap構造を維持**: `container` > `list-group` > `card` のネスト構造を維持する
2. **日本語コンテンツ**: すべてのテキストは日本語で記述（メタデータ含む）
3. **外部リンク**: `target="blank"` を使用（例: Twitter、YouTube、ブログ）
4. **画像**: `images/` ディレクトリに配置、`img-thumbnail` クラスで表示

### コンテンツセクション

- **ナビゲーション**: ダークテーマのnavbar、SNSリンク（ブログ、Twitter、YouTube）
- **制作中のゲーム**: `光影の塔`（専用サイト: koueinotou.luftelli.com）
- **サークル情報**: メンバー紹介カード（Cdec、Lui、Van、Zip）

### 開発ワークフロー

1. **デプロイ**: `master-deploy` ブランチへのマージで自動デプロイ（GitHub Pages）
2. **PR作成**: `.github/pull_request_template.md` を使用（Issue番号必須）
3. **ローカルプレビュー**: `index.html` を直接ブラウザで開く（ビルド不要）

### ライセンス

- **CC BY-NC-ND 4.0**: 非商用、改変不可、クレジット表示必須
- `LICENSE` ファイル参照（403行のCreative Commons全文）

## CDN依存関係

```html
<!-- Bootstrap 4.3.1 -->
<link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css">

<!-- Font Awesome 5.6.1 -->
<link href="https://use.fontawesome.com/releases/v5.6.1/css/all.css" rel="stylesheet">

<!-- jQuery 3.3.1 + Popper.js + Bootstrap JS -->
<script src="https://code.jquery.com/jquery-3.3.1.slim.min.js"></script>
```

バージョン変更時は整合性ハッシュ（`integrity` 属性）の更新が必要。

## 一般的なタスク

### 新しいゲームを追加

`index.html` の「制作中のゲーム」セクション（70-86行目付近）に新しい `card` を追加:

```html
<div class="card">
    <div class="card-header">
        <h3>ゲームタイトル</h3>
    </div>
    <div class="card-body">
        <a href="公式サイトURL">
            <ul class="list-group list-group-flush">
                <p>ゲームの説明</p>
                <li class="list-group-item">
                    <img src="images/game_image.jpg" alt="ゲーム名" class="img-thumbnail">
                </li>
                <p>詳細は公式サイトへ！</p>
            </ul>
        </a>
    </div>
</div>
```

### メンバーを追加/更新

`index.html` の「メンバー」カードデッキ（95-129行目付近）に追加:

```html
<div class="card">
    <div class="card-header">
        名前 <a href="TwitterURL" target="blank" title="Twitter"><i class="fab fa-twitter"></i></a>
    </div>
    <div class="card-body">
        <div class="card-text">説明<br>追加情報。</div>
    </div>
</div>
```

## タスク実施時の順守事項

- ライブラリのドキュメントはcontext7ツールを使用して参照すること
- 変更後はブラウザでの表示確認を必ず行うこと
- パフォーマンスが問題ないかchrome-devtoolsツールで確認すること
