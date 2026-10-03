# VideoDW REST API Checklist

| ID | Check | Expected result | Status | Comment |
|---|---|---|---|---|
| CK-API-01 | Request without `url` | API returns `422 Unprocessable Entity` | Pass | Required `url` validation works. |
| CK-API-02 | Malformed URL | API returns `422 Unprocessable Entity` | Pass | Invalid URL was rejected. |
| CK-API-03 | Unsupported platform URL | API returns `422 Unprocessable Entity` | Pass | Wikipedia URL was rejected. |
| CK-API-04 | Unsupported `avi` format | API rejects the format with `422` | Fail | API returned `200 OK` and processed the file as MP4. |
| CK-API-05 | Valid YouTube MP4 download | API returns `success: true` and a downloadable file | Pass | MP4 download and file retrieval succeeded. |
| CK-API-06 | Valid YouTube MP3 download | API returns `success: true` and a downloadable file | Pass | MP3 download and file retrieval succeeded. |
| CK-API-07 | Invalid download ID | API returns `400 Bad Request` | Fail | API returned `500 Internal Server Error`. |
| CK-API-08 | Missing download file | API returns `404` with `File not found` | Fail | Status was `404`, but the response contained an internal variable error. |
| CK-API-09 | Empty URL | API returns `422 Unprocessable Entity` | Pass | Empty URL was rejected. |
| CK-API-10 | Missing `format` | API returns `422 Unprocessable Entity` | Pass | Missing `format` was rejected. |

## Execution Summary

- **Test cases:** 10
- **Passed:** 7
- **Failed:** 3
- **Blocked:** 0
- **Screenshots:** Not collected
