# GEMINI.md

## 目的
- Gemini CLI 向けのコンテキストと作業方針を定義する。

## 出力スタイル
- 言語: 日本語
- トーン: 簡潔で事実ベース
- 形式: Markdown

## 共通ルール
- 会話は日本語で行う。
- PR とコミットは Conventional Commits に従う。
- PR タイトルとコミット本文の言語: PR タイトルは Conventional Commits 形式（英語推奨）。PR 本文は日本語。コミットは Conventional Commits 形式（description は日本語）。
- 日本語と英数字の間には半角スペースを入れる。

## プロジェクト概要
Provides IntelliJ IDEA run configurations to easily set up and test Bukkit/Spigot/PaperMC server plugins.

### 技術スタック
- **言語**: Java, XML (IntelliJ config)
- **フレームワーク**: Bukkit, Spigot, PaperMC
- **パッケージマネージャー**: Maven or Gradle (plugin dependent)
- **主要な依存関係**:

## コーディング規約
- フォーマット: 既存設定（ESLint / Prettier / formatter）に従う。
- 命名規則: 既存のコード規約に従う。
- コメント言語: 日本語
- エラーメッセージ: 英語

### 開発コマンド
```bash
# install
Clone repository

# dev
Copy .run/ configurations to project

# build
Maven/Gradle (plugin dependent)

# test
Run via IntelliJ IDE

```

## 注意事項
- 認証情報やトークンはコミットしない。
- ログに機密情報を出力しない。
- 既存のプロジェクトルールがある場合はそれを優先する。

## リポジトリ固有
- **type**: Development Tool / IDE Configuration
**platforms:**
  - IntelliJ IDEA IDE
- **status**: TODO: i18n support, one-liner installer
- **purpose**: Simplifies plugin testing workflow by providing pre-configured run environments
**compatible_servers:**
  - Bukkit
  - Spigot
  - PaperMC