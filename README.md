# sample-actions-paths-filter

このリポジトリは [dorny/paths-filter](https://github.com/dorny/paths-filter) の動作を調査・確認するためのサンプルプロジェクトです。

## 概要

GitHub Actions の `dorny/paths-filter` アクションを使用して、変更されたファイルのパスに基づいて条件付きでジョブを実行する方法を示します。

## ディレクトリ構成

```
.
├── frontend/     # フロントエンドのコード
├── backend/      # バックエンドのコード
├── docs/         # ドキュメント
└── .github/
    └── workflows/
        └── paths-filter-test.yml  # paths-filter を使用するワークフロー
```

## paths-filter の動作

`.github/workflows/paths-filter-test.yml` ワークフローは、以下のパターンに基づいてファイルの変更を検出します:

- **frontend**: `frontend/` ディレクトリ内のファイル
- **backend**: `backend/` ディレクトリ内のファイル
- **docs**: `docs/` ディレクトリ内のファイルまたはルートの `.md` ファイル
- **config**: `.github/` ディレクトリ内のファイルまたは `.yml`, `.yaml` ファイル

各カテゴリのファイルが変更された場合、対応するジョブが実行されます。

## テスト方法

1. ブランチを作成
2. 任意のディレクトリのファイルを変更
3. プルリクエストを作成
4. GitHub Actions のワークフローが実行され、変更されたパスに応じて適切なジョブが実行されることを確認

例:
- `frontend/app.js` を変更 → `frontend-job` が実行される
- `backend/server.js` を変更 → `backend-job` が実行される
- `docs/api.md` を変更 → `docs-job` が実行される
- 複数のディレクトリを変更 → 複数のジョブが実行される