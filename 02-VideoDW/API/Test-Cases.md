
# Test Cases: VideoDW REST API

| Field | Value |
|---|---|
| Application | VideoDW |
| Author | Denis Pelekh |
| Role | QA Engineer |
| Environment | Local, macOS |
| Test data | Empty body, malformed URL, unsupported URL, invalid format, and public YouTube video |

### TC-API-001 - Reject request without URL

- **Priority:** High
- **Type:** Negative
- **Method:** POST
- **Endpoint:** `/api/download`
- **Request body:**
  ```json
  {}
  ```
- **Expected status:** `422 Unprocessable Entity`
- **Expected result:** The API returns a validation error explaining that the required `url` field is missing.
- **Actual status:** `422 Unprocessable Entity`
- **Actual result:** The response returned a validation error with `type: "missing"`, location `body.url`, and message `Field required`.
- **Status:** Pass

### TC-API-002 - Reject invalid URL

- **Priority:** High
- **Type:** Negative
- **Method:** POST
- **Endpoint:** `/api/download`
- **Request body:**
  ```json
  {
    "url": "htt://not-a-link",
    "format": "mp4"
  }
  ```
- **Expected status:** `422 Unprocessable Entity`
- **Expected result:** The API returns a validation error explaining that the provided `url` value is invalid.
- **Actual status:** `422 Unprocessable Entity`
- **Actual result:** The response returned a validation error with `type: "value-error"`, location `body.url`, and message `Value error, Invalid URL`.
- **Status:** Pass

### TC-API-003 - Reject unsupported platform URL

- **Priority:** High
- **Type:** Negative
- **Method:** POST
- **Endpoint:** `/api/download`
- **Request body:**
  ```json
  {
    "url": "https://en.wikipedia.org/wiki/Video",
    "format": "mp4"
  }
  ```
- **Expected status:** `422 Unprocessable Entity`
- **Expected result:** The API returns a validation error explaining that the provided `url` value is invalid.
- **Actual status:** `422 Unprocessable Entity`
- **Actual result:** The response returned a validation error with `type: "value-error"`, location `body.url`, and message `Value error, Invalid URL`.
- **Status:** Pass

### TC-API-004 - Invalid format with valid URL

- **Priority:** Medium
- **Type:** Negative
- **Method:** POST
- **Endpoint:** `/api/download`
- **Request body:**
  ```json
  {
    "url": "https://www.youtube.com/watch?v=4iHWSAYXPLc&t=57s",
    "format": "avi"
  }
  ```
- **Expected status:** `422 Unprocessable Entity`
- **Expected result:** The API returns a validation error explaining that the provided `avi` format is invalid.
- **Actual status:** `200 OK`
- **Actual result:** The response returned a video in MP4 format.
- **Status:** Failed

### TC-API-005 - Download valid YouTube video as MP4

- **Priority:** High
- **Type:** Positive / Integration
- **Method:** POST, then GET
- **Endpoint:** `/api/download`, then `/api/download/{download_id}`
- **Request body:**
  ```json
  {
    "url": "https://www.youtube.com/watch?v=4iHWSAYXPLc&t=57s",
    "format": "mp4"
  }
  ```
- **Expected status:** `200 OK` for both requests.
- **Expected result:** The POST response contains `success: true`, a `download_id`, a filename, and progress `100`. The GET request returns the generated file.
- **Actual status:** `200 OK` for both requests.
- **Actual result:** The POST response returned `success: true`, progress `100`, a `download_id`, and an MP4 filename. The GET response returned `200 OK` with a downloadable file of `154,479,775` bytes.
- **Status:** Pass

### TC-API-006 - Download valid YouTube video as MP3

- **Priority:** High
- **Type:** Positive / Integration
- **Method:** POST, then GET
- **Endpoint:** `/api/download`, then `/api/download/{download_id}`
- **Request body:**
  ```json
  {
    "url": "https://www.youtube.com/watch?v=4iHWSAYXPLc&t=57s",
    "format": "mp3"
  }
  ```
- **Expected status:** `200 OK` for both requests.
- **Expected result:** The POST response contains `success: true`, a `download_id`, a filename, and progress `100`. The GET request returns the generated file.
- **Actual status:** `200 OK` for both requests.
- **Actual result:** The POST response returned `success: true`, progress `100`, a `download_id`, and an MP3 filename. The GET response returned `200 OK` with a downloadable MP3 file of `9,465,872` bytes.
- **Status:** Pass

### TC-API-007 - Reject invalid download ID

- **Priority:** High
- **Type:** Negative
- **Method:** GET
- **Endpoint:** `/api/download/not-a-uuid`
- **Expected status:** `400 Bad Request`
- **Expected result:** The API rejects the invalid download ID and returns a clear error message.
- **Actual status:** `500 Internal Server Error`
- **Actual result:** The API returned the error message `Cannot access local variable 'download_dir' where it is not associated with a value`.
- **Status:** Failed

### TC-API-008 - Return error when download file is missing

- **Priority:** High
- **Type:** Negative
- **Method:** GET
- **Endpoint:** `/api/download/00000000-0000-0000-0000-000000000000`
- **Expected status:** `404 Not Found`
- **Expected result:** The API returns a file-not-found error for a valid UUID that does not belong to an existing download.
- **Actual status:** `404 Not Found`
- **Actual result:** The API returned the error message `Cannot access local variable 'download_dir' where it is not associated with a value` instead of the expected `File not found` message.
- **Status:** Failed

### TC-API-009 - Reject empty URL

- **Priority:** High
- **Type:** Negative / Boundary
- **Method:** POST
- **Endpoint:** `/api/download`
- **Request body:**
  ```json
  {
    "url": "",
    "format": "mp4"
  }
  ```
- **Expected status:** `422 Unprocessable Entity`
- **Expected result:** The API rejects the empty URL and returns a validation error.
- **Actual status:** `422 Unprocessable Entity`
- **Actual result:** The API returned a validation error with the message `Value error, Invalid URL`.
- **Status:** Pass

### TC-API-010 - Reject request without format

- **Priority:** High
- **Type:** Negative / Boundary
- **Method:** POST
- **Endpoint:** `/api/download`
- **Request body:**
  ```json
  {
    "url": "https://www.youtube.com/watch?v=4iHWSAYXPLc&t=57s"
  }
  ```
- **Expected status:** `422 Unprocessable Entity`
- **Expected result:** The API returns a validation error explaining that the `format` field is required.
- **Actual status:** `422 Unprocessable Entity`
- **Actual result:** The API returned a validation error with `type: "missing"`, location `body.format`, and message `Field required`.
- **Status:** Pass
