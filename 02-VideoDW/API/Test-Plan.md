# Test Plan: VideoDW REST API

| Field | Value |
|---|---|
| **Author** | Denys Pelekh |
| **Role** | QA Engineer |
| **Document version** | 1.0.0 |
| **Status** | Completed |
| **Application** | VideoDW Media Processing Service |
| **Target build** | Local working tree |
| **Test date** | 03.10.2026 |
| **Environment** | Local FastAPI backend |

## 1. Introduction and Objectives

VideoDW is a media processing service that accepts supported video URLs, downloads and converts media, streams progress updates, and returns the processed file to the client.

The purpose of this test plan is to define the scope and approach for testing the VideoDW REST API before documenting the web interface and Telegram bot flows.

### Testing Objectives

- Verify that the API accepts valid YouTube, Instagram, and TikTok URLs.
- Verify validation errors for missing, empty, malformed, and unsupported URLs.
- Verify MP4 and MP3 format processing.
- Verify HTTP status codes and response headers.
- Verify streamed progress and completion events from the download endpoint.
- Verify that a completed file can be retrieved using its `download_id`.
- Verify handling of invalid IDs, missing files, oversized media, and download failures.
- Verify that errors are returned in a controlled way and do not terminate the backend process.

## 2. API Under Test

| Method | Endpoint | Purpose |
|---|---|---|
| POST | `/api/download` | Start media processing and stream progress events. |
| GET | `/api/download/{download_id}` | Retrieve a completed media file. |

### Expected Request Body

```json
{
	"url": "https://example.com/video",
	"format": "mp4"
}
```

Supported format values used by the web client:

- `mp4` for video;
- `mp3` for audio.

The download response is a stream of JSON objects. A successful completion event contains a `download_id` and a `filename`, which are then used to retrieve the file.

## 3. Scope of Testing

### 3.1 In Scope

- Request body validation.
- Supported and unsupported URL validation.
- YouTube, Instagram, and TikTok URL handling.
- MP4 and MP3 format selection.
- HTTP status codes and response headers.
- Progress and completion events.
- File retrieval by `download_id`.
- Invalid UUID and missing file handling.
- Media size limit of 200 MB.
- Download errors and timeout behavior.
- Backend terminal logs during successful and failed requests.

### 3.2 Out of Scope

- Testing the internal implementation of `yt-dlp`.
- Testing the availability or reliability of YouTube, Instagram, or TikTok.
- Load testing with many concurrent downloads.
- Security penetration testing.
- Full Telegram bot testing.
- Full responsive web UI testing; this will be documented separately.

## 4. Test Strategy and Test Types

| Test Type | Focus Area | Technique / Tool |
|---|---|---|
| Functional testing | Download and file retrieval flows | Postman |
| Negative testing | Invalid requests and unsupported URLs | Postman |
| Boundary testing | Empty values, invalid IDs, and 200 MB limit | Postman and test data |
| API contract testing | Status codes, headers, and response structure | Postman tests |
| Integration testing | API, `yt-dlp`, FFmpeg, and filesystem interaction | Local environment |
| Exploratory testing | Unexpected API and streaming behavior | Postman, DevTools, terminal |
| Log inspection | Error handling and process stability | Backend terminal logs |

## 5. Test Environment and Test Data

### 5.1 Environment

- **Operating system:** macOS, local development machine
- **Backend:** FastAPI with Uvicorn
- **Backend URL:** `http://localhost:8000`
- **API documentation:** `http://localhost:8000/docs`
- **Frontend URL:** `http://localhost:5500`
- **Client:** Postman and Chrome DevTools
- **Media tools:** `yt-dlp` and FFmpeg
- **Database:** Not applicable for the REST API scope

### 5.2 Test Data

- One small public YouTube video.
- One public Instagram video or Reel.
- One public TikTok video.
- Invalid URL: `htt://not-a-link`. є
- Unsupported URL: `https://en.wikipedia.org/wiki/Video`.є
- Empty request body: `{}`. є
- Invalid format: `avi`. є
- Invalid download ID: `not-a-uuid`. 
- Valid UUID with no matching download directory.
- Exact public test URLs will be stored in the Postman collection used for execution.

Do not store private links, personal data, or credentials in the portfolio repository.

## 6. Entry Criteria

Testing can start when:

- The FastAPI backend starts without a critical error.
- The API responds at `http://localhost:8000/docs`.
- Postman is installed and the collection is prepared.
- FFmpeg is available in the environment.
- Public test media links are available.
- The download directory is writable.

## 7. Exit Criteria

Testing can be completed when:

- All planned positive and negative API cases are executed.
- Status codes and response structures are documented.
- At least one successful MP4 and MP3 flow is verified.
- Invalid IDs and missing files return controlled errors.
- Progress streaming and file retrieval are verified.
- No open Blocker or Critical defects remain within the tested scope.
- Postman results, screenshots, and terminal evidence are saved.

## 8. Risks and Mitigations

| Risk ID | Risk | Impact | Mitigation |
|---|---|---|---|
| R-01 | External platforms may change their response or block test requests. | High | Use small public test media and document external dependency limitations. |
| R-02 | A long download may make manual execution slow or unstable. | Medium | Use short files for the main functional flow and record timeout behavior separately. |
| R-03 | FFmpeg may be missing or unavailable in the local environment. | High | Verify FFmpeg before execution and record the terminal version. |
| R-04 | Temporary files may remain after a failed download. | Medium | Check the downloads directory after failed and completed requests. |
| R-05 | Streamed JSON events may be split across network chunks. | Medium | Inspect the complete response in Postman and the browser Network tab. |

## 9. Deliverables

- `Test-Plan.md`.
- `Test-Cases.md` or `API-Test-Report.md`.
- `Checklist.md`.
- `Postman-Collection.json`.
- Postman response screenshots.
- Chrome DevTools Network and Console evidence.
- Terminal log evidence.
- Test Summary Report.
