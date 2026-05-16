# API Contract — MVP RSS Reader

## Base

- Base path: `/api`

## Endpoints

### GET /api/subscriptions

- Description: Returns the list of saved subscriptions.
- Response: 200 OK
- Body: JSON array of `Subscription` objects.

### POST /api/subscriptions

- Description: Adds a new subscription.
- Request Body: `{ "url": "https://example.com/feed" }`
- Responses:
  - 201 Created — returns created `Subscription` object
  - 400 Bad Request — validation failed (empty or malformed URL)
  - 409 Conflict — duplicate subscription

## Subscription Schema

```json
{
  "id": "string (GUID)",
  "url": "string",
  "title": "string | null",
  "createdAt": "string (ISO 8601)"
}
```

## Notes

- CORS: Backend must allow frontend origin during development.
- Authentication: Not required for MVP.
