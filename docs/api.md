# API ドキュメント

## エンドポイント一覧

### GET /api/hello
サンプルのエンドポイントです。

**レスポンス:**
```json
{
  "message": "Hello, World!"
}
```

### POST /api/data
データを送信するエンドポイントです。

**リクエスト:**
```json
{
  "name": "Sample",
  "value": 123
}
```

**レスポンス:**
```json
{
  "status": "success",
  "id": "abc123"
}
```
