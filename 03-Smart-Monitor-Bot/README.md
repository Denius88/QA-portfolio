# Smart Monitor Bot - QA Portfolio Project

Smart Monitor Bot is a Telegram application designed for automated price monitoring across e-commerce storefronts. The application parses product URLs, tracks price shifts via background scheduled tasks, evaluates user-defined thresholds, and delivers real-time Telegram alerts.

This QA project focuses on functional end-to-end verification, input validation with Boundary Value Analysis, SQLite data persistence and isolation, background scheduler fault tolerance, and scraper error handling.

## Scope of Testing

### In Scope

- User registration, command navigation (`/start`, `/add`, `/list`, `/history`, `/delete`, `/cancel`).
- Input validation for supported e-commerce URLs (Rozetka, Books to Scrape) and malformed inputs.
- Boundary Value Analysis on price thresholds and polling intervals.
- Notification triggers on price drops and duplicate alert suppression.
- Relational integrity and complete user data isolation in SQLite.
- Background worker stability: handling connection timeouts, 404 missing pages, 403 anti-bot walls, and DOM layout changes.
- Terminal log monitoring for uncaught runtime exceptions.

### Out of Scope

- High-concurrency load testing (> 1,000 parallel workers).
- Bypassing enterprise CAPTCHA challenges (Cloudflare Turnstile).
- Payment gateway integration.

## Test Execution Summary

| Area | Result | Notes |
|---|---:|---|
| Smoke Checklist | 10/10 Passed | Verified critical customer flow |
| Functional Test Cases | 16 total (15 Passed, 1 Failed) | TC-SMB-006 failed on negative price boundary |
| Database Verification | 3/3 Passed | SQL verification of SQLite isolation and price snapshots |
| Edge-Case & Log Analysis | 3/3 Passed | Connection timeouts, 403 anti-bot walls, and missing selectors |
| Defects Logged | 1 Open | [BUG-SMB-001](Bug-Reports/BUG-SMB-001.md) (missing negative price validation) |

## QA Artifacts

- [Test Plan](Test-Plan.md) - Scope, test strategy, entry/exit criteria, and deliverables.
- [Smoke Checklist](Checklist.md) - Rapid verification checklist for the core user path.
- [Test Cases](Test-Cases.md) - 16 detailed positive, negative, boundary, and scraper test cases.
- [Test Design Techniques](Test-Design-Techniques.md) - Application of BVA, Equivalence Partitioning, State Transitions, and Decision Tables.
- [Database Verification](Database-Verification.md) - SQL queries confirming user isolation and price snapshot consistency.
- [Logs and Edge Cases](Logs-and-Edge-Cases.md) - Terminal log analysis for connection timeouts, 403 anti-bot challenges, and missing DOM selectors.
- [BUG-SMB-001](Bug-Reports/BUG-SMB-001.md) - Defect report: missing validation on negative/zero target price.
- [Test Summary Report](Test-Summary-Report.md) - Executive summary, execution metrics, and release recommendations.

## Limitations

- Testing was performed in a local macOS development environment.
- The scraper relies on external marketplace HTML markup, which may change independently.
- Polling intervals are subject to rate limiting (minimum 1 minute) to avoid remote IP blacklisting.