# VideoDW Logs and Edge Cases

| Field | Value |
|---|---|
| **Application** | VideoDW Media Processing Service |
| **Tester** | Denis Pelekh |
| **Environment** | Local FastAPI backend, macOS |
| **Test date** | 03.10.2026 |

## Edge Case: Unavailable YouTube Video

### Request

```json
{
  "url": "https://www.youtube.com/watch?v=AAAAAAAAAAA",
  "format": "mp4"
}
```

### API Result

The API returned a controlled streaming error event:

```json
{
  "error": "ERROR: [youtube] AAAAAAAAAAA: This video is unavailable",
  "progress": 0,
  "status": "Download error"
}
```

### Terminal Log

```text
ERROR:WebSite.backend.main:Download error: ERROR: [youtube] AAAAAAAAAAA: This video is unavailable
```

### Expected Result

- The user receives a clear download error.
- The backend process remains running.
- The failed download does not produce a successful file response.

### Actual Result

- A clear error event was returned.
- The backend remained available after the failed request.
- The error was logged by the backend.

### Status

**Pass**

## Temporary File Cleanup

The backend creates a separate temporary directory for each download ID and schedules cleanup after the retention delay. Successful downloads were observed being cleaned up by the backend log:

```text
Cleaned up expired download folder: .../downloads/<download_id>
```

**Status:** Pass - verified in terminal logs.

## Timeout and Network Drop Checks

| Scenario | Status | Notes |
|---|---|---|
| Network interruption during download | Not Run | Not reproduced in the local session. |
| Conversion timeout | Not Run | No controlled timeout test was executed. |

These scenarios should be tested separately before claiming full timeout and network-drop coverage.
