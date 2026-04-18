# AGENTS.md

このファイルは Codex CLI がリポジトリ内のコードを扱う際のガイダンスを提供します。

## 開発環境

プロジェクトは Dev Containers を使用しています。すべてのビルド、テスト、リント処理はコンテナ内で実行されます。

## 利用可能なコマンド

### Node.js

- `pnpm install` — 依存パッケージの導入
- `pnpm run build` — ビルド処理
- `pnpm run test` — テスト実行
- `pnpm run lint` — コード検査

### Python

- `pip install -r requirements.txt` — 依存パッケージの導入
- `pytest` — テスト実行
- `ruff check .` — コード検査

### Rust

- `cargo build` — ビルド
- `cargo test` — テスト実行
- `cargo clippy` — リント

## 開発規約

- TypeScript プロジェクトでは strict mode を有効にすること
- Conventional Commits 形式でコミットする（feat: / fix: / refactor: / test: / docs: / chore:）
- Python では型ヒントを必須とし、ruff でフォーマットすること
- Rust では `cargo clippy` の警告をゼロにすること
- テストカバレッジ 80% 以上を維持すること

## コーディング標準

- イミュータブルなデータ操作を優先する
- 関数は 50 行以内、ファイルは 800 行以内に収める
- エラーは適切にハンドリングし、ユーザーフレンドリーなメッセージを返す
- ハードコードされた秘密情報（API キー等）を含めない
- console.log をプロダクションコードに残さない

## PR 規約

- PR タイトルは 70 文字以内
- Summary セクションに変更内容を箇条書きで記載
- Test Plan セクションにテスト方法を記載
