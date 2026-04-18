# devcontainer-codex

> OpenAI Codex CLI + Node.js 22 + Python 3.12 + Rust を含む Dev Container テンプレート

## 概要

開発環境をまるごと Docker コンテナに入れる仕組みです。ローカルマシンを汚さず、チーム全体で統一された Codex CLI 環境を実現します。

## 含まれるツール

| ツール | バージョン | 用途 |
|--------|-----------|------|
| Node.js | 22 | JavaScript/TypeScript ランタイム |
| Python | 3.12 | Python ランタイム |
| Rust | latest | Rust ツールチェーン |
| pnpm | latest | Node.js パッケージ管理 |
| GitHub CLI | latest | GitHub 操作 |
| Codex CLI | latest | AI コーディングエージェント |

## 前提条件

- [Docker Desktop](https://www.docker.com/products/docker-desktop/)
- [VS Code](https://code.visualstudio.com/) + [Dev Containers 拡張機能](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers)
- OpenAI API キーまたは ChatGPT サブスクリプション

## セットアップ

### 1. テンプレートから新規リポジトリを作成

GitHub の「Use this template」ボタンからリポジトリを作成するか、既存プロジェクトに以下のファイルをコピーします：

```
.devcontainer/devcontainer.json
.codex/config.toml
.codex/rules/default.rules
AGENTS.md
```

### 2. Codex CLI の認証設定

コンテナ起動前に、ホストマシンで Codex CLI の認証を済ませてください：

```bash
# npm でインストール
npm install -g @openai/codex

# ChatGPT アカウントでログイン（推奨）
codex auth login

# または API キーを設定
export OPENAI_API_KEY="sk-..."
```

認証情報は `~/.codex/` に保存され、コンテナ内に自動マウントされます。

### 3. Dev Container を起動

VS Code でプロジェクトを開き、コマンドパレットから：

```
Dev Containers: Reopen in Container
```

### 4. プロジェクトを信頼済みとしてマーク

プロジェクトの `.codex/config.toml` を読み込ませるため、ユーザーレベルの設定で信頼を追加します：

```bash
# ~/.codex/config.toml に追加
[projects."/workspaces/your-project"]
trust_level = "trusted"
```

## ファイル構成

```
.devcontainer/
  devcontainer.json     # Dev Container 設定
.codex/
  config.toml           # Codex CLI プロジェクト設定
  rules/
    default.rules       # 実行ポリシールール（Starlark）
AGENTS.md               # Codex CLI へのプロジェクト指示
.gitignore              # Git 除外設定
```

## Codex CLI の使い方

```bash
# インタラクティブモード
codex

# ワンショット実行
codex "このコードベースの構造を説明して"

# プロファイル指定
codex --profile review "このPRをレビューして"
codex --profile fast "簡単なバグ修正をして"

# Full-auto モード（サンドボックス内で自動実行）
codex --full-auto "テストを追加して"
```

## 設定のカスタマイズ

### 承認モード

`.codex/config.toml` の `approval_policy` を変更：

| 値 | 動作 |
|----|------|
| `on-request` | モデルが必要時にユーザーに確認（推奨） |
| `untrusted` | 読み取り専用以外すべて確認が必要 |
| `never` | 確認なし（`danger-full-access` と組み合わせ） |

### サンドボックスモード

`.codex/config.toml` の `sandbox_mode` を変更：

| モード | ファイルシステム | ネットワーク |
|--------|----------------|-------------|
| `read-only` | 読み取り専用 | ブロック |
| `workspace-write` | ワークスペース内書き込み可 | ブロック |
| `danger-full-access` | 全アクセス | 全アクセス |

### 実行ポリシールール

`.codex/rules/default.rules` に Starlark 形式でルールを追加：

```python
prefix_rule(
    pattern = ["npm", "run", "deploy"],
    decision = "prompt",
    justification = "デプロイは確認が必要",
)
```

### 個人設定の上書き

プロジェクトルートに `AGENTS.override.md` を作成すると、`AGENTS.md` より優先されます。`.gitignore` に含まれているため、個人用の指示を安全に追加できます。

## トラブルシューティング

| 問題 | 対処法 |
|------|--------|
| コンテナが起動しない | Docker Desktop が起動しているか確認 |
| Codex CLI が認証エラー | ホスト側で `codex auth login` を再実行 |
| プロジェクト設定が読み込まれない | `trust_level = "trusted"` を確認 |
| キャッシュ問題 | VS Code で「Dev Containers: Rebuild Without Cache」を実行 |

## ライセンス

MIT
