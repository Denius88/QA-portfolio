# Test Plan: VideoDW Web Frontend

| Field | Value |
|---|---|
| **Application** | VideoDW Web Interface |
| **Author** | Denis Pelekh |
| **Role** | QA Engineer |
| **Status** | Completed |
| **Environment** | Production, macOS, Safari |
| **Frontend URL** | `https://videodw.pp.ua` |
| **Backend URL** | `https://videodw.pp.ua/api` |
| **Test date** | 03.10.2026 |

## 1. Objectives

The purpose of this test plan is to verify the main VideoDW browser workflow, client-side validation, download states, language and theme controls, responsive presentation, and frontend communication with the FastAPI backend.

## 2. Scope

### In Scope

- Initial page loading.
- YouTube, Instagram, and TikTok URL validation.
- MP4 and MP3 format selection.
- Download button behavior and disabled state.
- Progress bar and streamed status updates.
- Successful file download.
- Error message handling.
- Ukrainian and English language switches.
- Light and dark theme switches.
- Desktop and mobile showcase switch.
- Enter key submission.
- Chrome DevTools Network and Console checks.

### Out of Scope

- Full API contract testing, covered in `../API/`.
- Telegram bot testing.
- Load and performance testing.
- Browser compatibility beyond Chrome.
- Security penetration testing.

## 3. Test Approach

Testing will be performed manually in Chrome using the local frontend and backend. Network requests will be inspected in Chrome DevTools, and the Console will be checked for uncaught JavaScript errors.

## 4. Entry Criteria

- Frontend is available at `http://localhost:5500`.
- Backend is available at `http://localhost:8000`.
- A small public test video is available.
- Chrome DevTools can be opened.

## 5. Exit Criteria

- All planned web test cases are executed.
- The main MP4 and MP3 download flows work.
- Invalid URLs show a clear error.
- No unexpected uncaught JavaScript errors remain in the tested flow.
- Network requests and response statuses are documented.
- Failed tests have linked bug reports.

## 6. Deliverables

- `Test-Plan.md`.
- `Test-Cases.md`.
- `Checklist.md`.
- DevTools Network and Console notes.
- Web Test Summary Report.
