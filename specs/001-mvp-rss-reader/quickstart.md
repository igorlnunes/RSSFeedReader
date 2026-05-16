# Quickstart — MVP RSS Reader

## Prerequisites

- .NET 8 SDK installed
- Git (optional)

## Run Backend (API)

```bash
# From repository root
cd backend/RSSFeedReader.Api
dotnet run
```

Default backend URL: `http://localhost:5151`

## Run Frontend (Blazor WebAssembly)

```bash
cd frontend/RSSFeedReader.UI
# ensure appsettings.json ApiBaseUrl points to backend URL
dotnet run
```

Default frontend URL: `http://localhost:5213`

## MVP Manual Test

1. Open the frontend URL in a browser.
2. Paste a feed URL into the "Add subscription" input and submit.
3. Confirm the subscription appears in the list.

## Notes

- MVP uses in-memory storage; stopping the app will clear subscriptions.
- If ports differ, update `appsettings.json` in the frontend and backend launch settings accordingly.
