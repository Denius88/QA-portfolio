# BUG-API-002 - Download ID errors are handled incorrectly

| Field | Value |
|---|---|
| **Bug ID** | BUG-API-002 |
| **Title** | API returns an unhandled error for invalid or missing download IDs |
| **Related test cases** | TC-API-007, TC-API-008 |
| **Severity** | High |
| **Priority** | High |
| **Status** | Open |
| **Environment** | Local, macOS, FastAPI, Uvicorn |
| **Endpoint** | `GET /api/download/{download_id}` |
| **Reproducibility** | Reproducible during test execution |

## Preconditions

- The VideoDW backend is running at `http://localhost:8000`.
- Postman is configured to send requests to the local backend.

## Steps to Reproduce

### Scenario A: Invalid download ID

1. Send a `GET` request to:

```text
http://localhost:8000/api/download/not-a-uuid
```

### Scenario B: Missing download file

1. Send a `GET` request to:

```text
http://localhost:8000/api/download/00000000-0000-0000-0000-000000000000
```

2. Check the response status and response body for each request.

## Expected Result

For Scenario A, the API should reject the malformed ID with:

```json
{
  "detail": "Invalid download ID"
}
```

Expected status:

```text
400 Bad Request
```

For Scenario B, the API should return:

```json
{
  "detail": "File not found"
}
```

Expected status:

```text
404 Not Found
```

## Actual Result

For Scenario A, the API returned:

```text
500 Internal Server Error
```

The response contained the message:

```text
Cannot access local variable 'download_dir' where it is not associated with a value
```

For Scenario B, the API returned:

```text
404 Not Found
```

However, the response contained the same `download_dir` error instead of the expected `File not found` message.

## Impact

Clients receive an unclear or internal implementation error when requesting an invalid or missing download. Invalid IDs can also cause a `500 Internal Server Error` instead of a controlled client error.

## Evidence

- Related test cases: `TC-API-007`, `TC-API-008`
- **Evidence:** None. Both scenarios were verified in Postman.

## Suggested Fix

Validate `download_id` before using the download directory. Return the intended HTTP errors and messages for malformed IDs and missing files instead of exposing an internal variable error.

## Notes

The issue was found during manual API testing in Postman. The test results were recorded as `Failed` in `Test-Cases.md`.
