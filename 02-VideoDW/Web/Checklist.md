# VideoDW Web Smoke Checklist

| ID | Action | Expected result | Status | Comment |
|---|---|---|---|---|
| CK-WEB-01 | Open the frontend | Main page and all controls are displayed | Pass | Page loaded correctly in Safari. |
| CK-WEB-02 | Enter valid URL and select MP4 | Download flow starts | Pass | MP4 file was downloaded successfully. |
| CK-WEB-03 | Enter valid URL and select MP3 | Audio download flow starts | Pass | MP3 file was downloaded successfully. |
| CK-WEB-04 | Enter invalid URL | Clear validation error is displayed | Pass | Invalid URL message was displayed. |
| CK-WEB-05 | Observe progress | Progress and status updates are displayed | Pass | Progress bar and percentage status were displayed. |
| CK-WEB-06 | Observe Download button | Button is disabled during processing and enabled afterward | Pass | Button state changed correctly. |
| CK-WEB-07 | Switch Ukrainian/English | Interface text changes and preference persists | Pass | Language switch and persistence worked. |
| CK-WEB-08 | Switch light/dark theme | Theme changes and preference persists | Pass | Theme switch and persistence worked. |
| CK-WEB-09 | Switch desktop/mobile showcase | Correct showcase videos are loaded | Pass | Correct showcase videos were loaded. |
| CK-WEB-10 | Inspect Network and Console | Requests are correct and no unexpected JS errors appear | Pass | Safari Network showed `POST /api/download` with `200 OK`; no unexpected console errors were observed. |

## Execution Summary

- **Planned checks:** 10
- **Executed:** 10
- **Passed:** 10
- **Failed:** 0
- **Blocked:** 0
