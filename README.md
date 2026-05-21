# skills

共有 GitHub Actions ワークフローと Claude Code プロンプトのリポジトリ。

## セットアップ

利用する各リポジトリの Settings → Secrets → Actions に `CLAUDE_CODE_OAUTH_TOKEN` を追加してください。

トークンは Claude Code CLI で取得できます：

```bash
claude auth token
```

## 使い方

### Claude Code PR レビュー

draft PR 作成時に Claude Code が自動レビューを行います。

**呼び出し側リポジトリに追加するワークフロー:**

```yaml
# .github/workflows/draft-pr-review.yml
name: Draft PR Review

on:
  pull_request:
    types: [opened, converted_to_draft]

jobs:
  claude-review:
    if: github.event.pull_request.draft == true
    uses: KaiKusakari/skills/.github/workflows/claude-pr-review.yml@main
    secrets: inherit
```

### 利用可能なプロンプト

| `review_type` | 用途 |
|--------------|------|
| `onion-arch-review` (デフォルト) | Go オニオンアーキテクチャのレビュー |

**カスタムプロンプトを指定する場合:**

```yaml
jobs:
  claude-review:
    if: github.event.pull_request.draft == true
    uses: KaiKusakari/skills/.github/workflows/claude-pr-review.yml@main
    with:
      review_type: onion-arch-review
    secrets: inherit
```

## プロンプトの追加

`.github/prompts/` に Markdown ファイルを追加し、`review_type` でファイル名（拡張子なし）を指定します。
