# Data Model for MVP RSS Reader

## Entities

### Subscription

- `Id` (GUID) — unique identifier for the subscription (server-generated).
- `Url` (string, required) — the feed URL entered by the user.
- `Title` (string, optional) — optional feed title when available.
- `CreatedAt` (datetime) — timestamp when subscription was added.
- `Source` (string, optional) — origin or note (not required for MVP).

## Validation Rules

- `Url` must be non-empty and conform to URI syntax. Reject empty strings.
- No duplicate `Url` values allowed; adding a duplicate returns a 409/Conflict.

## Notes

- Storage: in-memory repository for MVP. Persistence (EF Core + SQLite) planned for Extended-MVP.
