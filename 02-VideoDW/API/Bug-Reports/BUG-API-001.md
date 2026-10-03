# BUG-API-001 - Unsupported format is processed as MP4

| Field | Value |
|---|---|
| **Bug ID** | BUG-API-001 |
| **Title** | API accepts unsupported `avi` format and processes the video as MP4 |
| **Related test case** | TC-API-004 |
| **Severity** | Medium |
| **Priority** | High |
| **Status** | Open |
| **Environment** | Local, macOS, FastAPI, Uvicorn |
| **Endpoint** | `POST /api/download` |
| **Reproducibility** | Reproducible during test execution |

## Preconditions

- The VideoDW backend is running at `http://localhost:8000`.
- A valid public YouTube URL is available.
- Postman is configured to send JSON requests.

## Steps to Reproduce

1. Send a `POST` request to `/api/download`.
2. Set the `Content-Type` header to `application/json`.
3. Use the following request body:

```json
{
  "url": "https://www.youtube.com/watch?v=4iHWSAYXPLc&t=57s",
  "format": "avi"
}
```

4. Wait for the download process to finish.
5. Check the response status, filename, and downloaded file format.

## Expected Result

The API rejects the unsupported `avi` format and returns a validation error:

```text
422 Unprocessable Entity
```

The response should clearly state that only supported formats such as `mp4` and `mp3` are allowed.

## Actual Result

The API accepts the unsupported `avi` format and returns:

```text
200 OK
```

The service processes the video and returns it as an MP4 file instead of rejecting the request.

## Impact

Clients cannot rely on the API to validate the requested output format. This may lead to unexpected file types and inconsistent behavior between the selected format and the downloaded file.

## Evidence

- Related test case: `TC-API-004`
- **Evidence:** None. The request and response were verified in Postman.

## Suggested Fix

Validate the `format` field before starting the download. Accept only supported values:

```text
mp4
mp3
```

Return `422 Unprocessable Entity` for any other value.

## Notes

The issue was found during manual API testing in Postman. The test result was recorded as `Failed` in `Test-Cases.md`.
