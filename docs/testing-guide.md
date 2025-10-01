# paths-filter テストガイド

このガイドでは、dorny/paths-filter の動作を確認するための手順を説明します。

## ワークフローの概要

`.github/workflows/paths-filter-test.yml` は以下の仕組みで動作します:

1. **detect-changes ジョブ**: 変更されたファイルのパスを検出
2. **条件付きジョブ**: 変更されたパスに応じて、適切なジョブを実行

## テストシナリオ

### シナリオ 1: フロントエンドのみ変更

```bash
# ブランチを作成
git checkout -b test-frontend

# フロントエンドファイルを変更
echo "// 変更" >> frontend/app.js

# コミット & プッシュ
git add frontend/app.js
git commit -m "Test: frontend change"
git push origin test-frontend
```

**期待される結果**: `frontend-job` のみが実行される

### シナリオ 2: バックエンドのみ変更

```bash
# ブランチを作成
git checkout -b test-backend

# バックエンドファイルを変更
echo "// 変更" >> backend/server.js

# コミット & プッシュ
git add backend/server.js
git commit -m "Test: backend change"
git push origin test-backend
```

**期待される結果**: `backend-job` のみが実行される

### シナリオ 3: ドキュメントのみ変更

```bash
# ブランチを作成
git checkout -b test-docs

# ドキュメントファイルを変更
echo "## 新しいセクション" >> docs/api.md

# コミット & プッシュ
git add docs/api.md
git commit -m "Test: docs change"
git push origin test-docs
```

**期待される結果**: `docs-job` のみが実行される

### シナリオ 4: README.md の変更

```bash
# ブランチを作成
git checkout -b test-readme

# READMEを変更
echo "## 追加情報" >> README.md

# コミット & プッシュ
git add README.md
git commit -m "Test: README change"
git push origin test-readme
```

**期待される結果**: `docs-job` が実行される（`*.md` パターンにマッチ）

### シナリオ 5: ワークフローファイルの変更

```bash
# ブランチを作成
git checkout -b test-workflow

# ワークフローファイルを変更
echo "# コメント" >> .github/workflows/paths-filter-test.yml

# コミット & プッシュ
git add .github/workflows/paths-filter-test.yml
git commit -m "Test: workflow change"
git push origin test-workflow
```

**期待される結果**: `config-job` と `docs-job` が実行される（`.yml` ファイルは両方のパターンにマッチ）

### シナリオ 6: 複数のディレクトリを変更

```bash
# ブランチを作成
git checkout -b test-multiple

# 複数のファイルを変更
echo "// 変更" >> frontend/app.js
echo "// 変更" >> backend/server.js

# コミット & プッシュ
git add frontend/app.js backend/server.js
git commit -m "Test: multiple changes"
git push origin test-multiple
```

**期待される結果**: `frontend-job` と `backend-job` の両方が実行される

## フィルターパターンの説明

| フィルター | パターン | 例 |
|----------|---------|-----|
| frontend | `frontend/**` | frontend/app.js, frontend/src/index.ts |
| backend | `backend/**` | backend/server.js, backend/api/routes.go |
| docs | `docs/**`, `*.md` | docs/api.md, README.md, CHANGELOG.md |
| config | `.github/**`, `*.yml`, `*.yaml` | .github/workflows/test.yml, config.yaml |

## ワークフロー実行結果の確認方法

1. GitHub リポジトリページを開く
2. "Actions" タブをクリック
3. 最新のワークフロー実行を選択
4. "detect-changes" ジョブのログで、どのパスが変更されたか確認
5. 条件付きジョブが適切に実行されているか確認

## トラブルシューティング

### ワークフローが実行されない

- main ブランチへのプッシュまたはプルリクエストを作成していることを確認
- ワークフローファイルが正しいパスに配置されていることを確認

### 期待したジョブが実行されない

- フィルターパターンが正しいことを確認
- detect-changes ジョブのログで、実際に検出されたパスを確認
- ファイルのパスがフィルターパターンにマッチしているか確認
