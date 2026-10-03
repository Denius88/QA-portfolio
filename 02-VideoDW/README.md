# VideoDW - QA Portfolio Project

VideoDW is a media processing service with a web interface, REST API, and Telegram integration. This QA project focuses on the web interface and REST API.

I tested the project from a user and API perspective: valid and invalid download requests, MP4/MP3 processing, file retrieval, browser behavior, progress streaming, and error handling.

## Test Scope

### REST API

- Request validation for URL and format fields.
- YouTube URL processing.
- MP4 and MP3 downloads.
- Download file retrieval by `download_id`.
- Invalid IDs and missing files.
- Error responses and streamed JSON events.
- Postman and Swagger/OpenAPI validation.

### Web Frontend

- URL input and validation.
- MP4/MP3 format selection.
- Download button and progress state.
- Successful file downloads.
- Ukrainian/English language switch.
- Light/dark theme switch.
- Desktop/mobile showcase switch.
- Safari Web Inspector Network and Console checks.

## Test Results

| Area | Result |
|---|---:|
| API test cases | 10 total: 7 passed, 3 failed |
| API bug reports | 2 |
| Web test cases | 12 passed |
| Web smoke checklist | 10/10 passed |
| Unavailable media edge case | Passed |
| Timeout/network interruption | Not run |

The API testing found defects in unsupported format validation and download ID error handling. The web interface passed the executed Safari production scope.

## QA Artifacts

### API

- [API Test Plan](API/Test-Plan.md)
- [API Test Cases](API/Test-Cases.md)
- [API Checklist](API/Checklist.md)
- [Postman Collection](API/Postman-Collection.json)
- [API Test Summary](API/Test-Summary-Report.md)
- [BUG-API-001](API/Bug-Reports/BUG-API-001.md)
- [BUG-API-002](API/Bug-Reports/BUG-API-002.md)

### Web

- [Web Test Plan](Web/Test-Plan.md)
- [Web Test Cases](Web/Test-Cases.md)
- [Web Checklist](Web/Checklist.md)
- [Web Test Summary](Web/Test-Summary-Report.md)

### Logs and Edge Cases

- [Logs and Edge Cases](Logs-and-Edge-Cases.md)

## Limitations

- API timeout and network interruption scenarios were not executed.
- Web testing was performed in Safari only.
- Screenshots were not collected.
- Load and performance testing were outside the scope.
