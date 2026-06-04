# Known Issues

Found during Playwright smoke test on 2026-06-04.

## 1. Missing `PurchaseOrderModal` component
- **Type**: Vue warning
- **Location**: `client/src/views/Dashboard.vue`
- **Detail**: Component is referenced in the Dashboard template but never imported/registered. Renders silently as an unknown element.

## 2. `/api/tasks` endpoint missing
- **Type**: Runtime error (404)
- **Location**: `client/src/App.vue:66`
- **Detail**: Frontend calls `GET http://localhost:8001/api/tasks` on mount, but the backend has no such endpoint. Results in an AxiosError logged to console on every page load.

## 3. Missing favicon
- **Type**: 404 (cosmetic)
- **Location**: `http://localhost:3002/favicon.ico`
- **Detail**: No favicon file in the project. Browser request fails silently.
