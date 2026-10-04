# Test Summary Report: VideoDW REST API

| Field | Value |
|---|---|
| **Author** | Denys Pelekh |
| **Role** | QA Engineer |
| **Test date** | 03.10.2026 |
| **Build** | Local working tree |
| **Environment** | Local macOS, FastAPI, Uvicorn, Postman |
| **Report status** | Final |

## 1. Executive Summary

Manual API testing was performed for the VideoDW media processing service. The testing covered request validation, supported media formats, successful MP4 and MP3 downloads, file retrieval, and error handling for invalid download IDs and missing files.

**Overall result:** Passed with known defects.

**Release recommendation:** Not ready for release until the high-priority error-handling defect is reviewed. The unsupported-format validation defect should also be fixed before production use.

## 2. Test Scope

### Tested

- `POST /api/download` request validation.
- Valid YouTube MP4 download.
- Valid YouTube MP3 download.
- File retrieval using `download_id`.
- Invalid URL and unsupported platform validation.
- Unsupported output format handling.
- Invalid and missing download ID handling.
- Empty URL and missing format validation.

### Not Tested

- Load and performance testing.
- Security penetration testing.
- Full Telegram bot flow.
- Full web frontend behavior.
- Private or restricted media.
- Screenshots and visual evidence.

## 3. Test Execution Summary

| Metric | Result |
|---|---:|
| Test cases | 10 |
| HTTP requests in Postman collection | 11 |
| Passed | 7 |
| Failed | 3 |
| Blocked | 0 |
| Bugs reported | 2 |
| Screenshots | Not collected |

## 4. Failed Tests and Defects

| Test case | Result | Related bug |
|---|---|---|
| TC-API-004 | Failed: unsupported `avi` format was processed as MP4 | BUG-API-001 |
| TC-API-007 | Failed: invalid ID returned `500 Internal Server Error` | BUG-API-002 |
| TC-API-008 | Failed: missing file response contained an internal variable error | BUG-API-002 |

## 5. Passed Tests

- Missing `url` validation.
- Malformed URL validation.
- Unsupported platform URL validation.
- Valid YouTube MP4 download and file retrieval.
- Valid YouTube MP3 download and file retrieval.
- Empty URL validation.
- Missing `format` validation.

## 6. Risks and Limitations

- Testing was performed in a local environment only.
- The service depends on external media platforms and `yt-dlp`.
- No load or performance testing was performed.
- No screenshots were collected; results are documented from Postman responses and terminal verification.
- The API currently exposes incorrect behavior for unsupported formats and download ID errors.

## 7. Evidence and Artifacts

- [Test Plan](Test-Plan.md)
- [Test Cases](Test-Cases.md)
- [Checklist](Checklist.md)
- [Postman Collection](Postman-Collection.json)
- [BUG-API-001](Bug-Reports/BUG-API-001.md)
- [BUG-API-002](Bug-Reports/BUG-API-002.md)

## 8. Conclusion

The main MP4 and MP3 download flows work successfully. Request validation for missing and malformed input also works. However, the API has three failed test results related to unsupported format handling and download ID error handling. These defects should be addressed before the service is considered ready for production release.

## 9. Next Steps

- Fix and retest `BUG-API-001`.
- Fix and retest `BUG-API-002`.
- Run the API regression checklist after fixes.
- Test the VideoDW web frontend separately.
