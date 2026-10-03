# Test Cases: VideoDW Web Frontend

| Field | Value |
|---|---|
| Application | VideoDW Web Interface |
| Author | Denis Pelekh |
| Role | QA Engineer |
| Environment | Production, macOS, Safari |
| Frontend URL | `https://videodw.pp.ua` |

## Functional Test Cases

### TC-WEB-001 - Load the main page

- **Priority:** High
- **Type:** Smoke / Functional
- **Steps:**
  1. Open `https://videodw.pp.ua` in Safari.
- **Expected result:** The page loads with the URL input, format selector, Download button, theme switch, language switch, and platform showcase.
- **Actual result:** The page loaded with the URL input, format selector, Download button, theme and language switches, and platform showcase.
- **Status:** Pass

### TC-WEB-002 - Submit a valid YouTube URL as MP4

- **Priority:** High
- **Type:** Positive / Integration
- **Steps:**
  1. Enter a valid public YouTube URL.
  2. Select `MP4 (Video)`.
  3. Click `Download`.
- **Expected result:** The button becomes disabled during processing, progress is displayed, the file is downloaded, and a success message appears.
- **Actual result:** The video was downloaded to the user's computer.
- **Status:** Pass

### TC-WEB-003 - Submit a valid YouTube URL as MP3

- **Priority:** High
- **Type:** Positive / Integration
- **Steps:**
  1. Enter a valid public YouTube URL.
  2. Select `MP3 (Audio)`.
  3. Click `Download`.
- **Expected result:** The audio file is downloaded and a success message appears.
- **Actual result:** The MP3 file was downloaded and the success message appeared.
- **Status:** Pass

### TC-WEB-004 - Reject an invalid URL

- **Priority:** High
- **Type:** Negative
- **Steps:**
  1. Enter `htt://not-a-link`.
  2. Click `Download`.
- **Expected result:** The page displays a clear invalid-link error and does not start a download request.
- **Actual result:** The page displayed a message asking the user to enter a valid supported URL.
- **Status:** Pass
### TC-WEB-005 - Verify download progress

- **Priority:** High
- **Type:** Functional / UI
- **Steps:**
  1. Submit a valid small video URL.
  2. Observe the status and progress bar during processing.
- **Expected result:** The progress bar becomes visible, progress updates are displayed, and the progress reaches completion before the success state.
- **Actual result:** The progress bar was visible, the download status displayed percentage progress, and the progress reached completion.
- **Status:** Pass

### TC-WEB-006 - Verify download button state

- **Priority:** Medium
- **Type:** UI / Functional
- **Steps:**
  1. Submit a valid URL.
  2. Observe the Download button while the request is running.
- **Expected result:** The button is disabled during processing and becomes enabled after success or failure.
- **Actual result:** The button was disabled during the download and became available after success or failure.
- **Status:** Pass

### TC-WEB-007 - Switch the interface language

- **Priority:** Medium
- **Type:** UI / Functional
- **Steps:**
  1. Click the language switch.
  2. Switch between Ukrainian and English.
  3. Reload the page.
- **Expected result:** Headings, labels, placeholders, format options, and status text use the selected language. The selection persists after reload.
- **Actual result:** Headings, labels, placeholders, format options, and status text used the selected language. The selection persisted after reload.
- **Status:** Pass

### TC-WEB-008 - Switch the color theme

- **Priority:** Medium
- **Type:** UI / Functional
- **Steps:**
  1. Click the theme switch.
  2. Switch between light and dark themes.
  3. Reload the page.
- **Expected result:** The theme changes correctly and remains selected after reload.
- **Actual result:** The theme changes correctly and remains selected after reload.
- **Status:** Pass

### TC-WEB-009 - Switch desktop and mobile showcase

- **Priority:** Low
- **Type:** UI / Functional
- **Steps:**
  1. Click the device switch.
  2. Switch between desktop and mobile showcase videos.
- **Expected result:** The corresponding platform showcase videos are loaded and the selected device mode persists.
- **Actual result:** The corresponding platform showcase videos are loaded and the selected device mode persists.
- **Status:** Pass

### TC-WEB-010 - Submit using the Enter key

- **Priority:** Medium
- **Type:** Functional / Accessibility
- **Steps:**
  1. Enter a valid or invalid URL.
  2. Press `Enter` while the Download button is enabled.
- **Expected result:** The same download or validation flow starts as when clicking the Download button.
- **Actual result:** The same download or validation flow started as when clicking the Download button.
- **Status:** Pass

## DevTools Checks

### TC-WEB-011 - Inspect the download request in Network

- **Priority:** High
- **Type:** Technical / Integration
- **Steps:**
  1. Open Safari Web Inspector and select the `Network` tab.
  2. Submit a valid URL.
  3. Inspect the request to `/api/download` and the subsequent file request.
- **Expected result:** The request uses `POST`, contains JSON with `url` and `format`, returns the expected status, and the file request is visible.
- **Actual result:** Safari Web Inspector showed a `POST /api/download` request with `200 OK`. The response used `text/event-stream` and chunked transfer encoding. The request was initiated from `script.js:140`.
- **Status:** Pass

### TC-WEB-012 - Check the browser Console

- **Priority:** High
- **Type:** Technical / Negative
- **Steps:**
  1. Open Safari Web Inspector and select the `Console` tab.
  2. Submit a valid URL.
  3. Submit an invalid URL.
  4. Observe the Console during both flows.
- **Expected result:** No unexpected uncaught JavaScript errors are generated during normal success or validation-error flows.
- **Actual result:** No unexpected JavaScript errors were observed during the tested success and validation flows.
- **Status:** Pass
