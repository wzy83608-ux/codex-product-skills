# API contract

## Health

`GET /health`

Success:

```json
{"ok":true,"missing":[]}
```

Incomplete configuration returns HTTP `503` with missing variable names only.

## Append document

`POST /documents/append`

Headers:

```text
Authorization: Bearer <HULY_WRITE_KEY>
Content-Type: application/json
```

Body:

```json
{
  "teamspace": "user-filled name or ID",
  "document": "user-filled document ID",
  "oldText": "user-confirmed current text",
  "appendText": "user-confirmed test text"
}
```

Success:

```json
{"ok":true,"result":{"id":"...","updated":true}}
```

Required behavior:

- `401` for a missing or invalid local write key.
- `400` for invalid input or an exact-match/concurrent-change failure.
- `413` for an oversized request.
- `415` for a non-JSON request.
- `503` when Huly configuration or connectivity is unavailable.
- Never include passwords, tokens, cookies, or unredacted account identifiers in responses.
