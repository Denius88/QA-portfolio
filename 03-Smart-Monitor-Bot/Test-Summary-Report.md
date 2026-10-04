# Test Summary Report: Smart Monitor Bot

| Field | Value |
|---|---|
| **Author** | Denys Pelekh |
| **Role** | QA Engineer |
| **Test period** | 04.10.2026 |
| **Build / version** | v1.0-beta |
| **Environment** | Local, macOS, Telegram Desktop, SQLite 3 (`monitor.db`) |
| **Report status** | Final |

## 1. Executive Summary

Manual functional testing, Boundary Value Analysis, SQLite data verification, background scheduler monitoring, and scraper resilience checks were successfully executed for the Smart Monitor Bot.

**Overall result:** Passed  
**Release recommendation:** Ready for release within the tested scope.

---

## 2. Scope of Testing

### Tested Areas

- Bot commands and conversational flow (`/start`, `/add`, `/list`, `/history`, `/delete`, `/cancel`).
- Input validation for supported e-commerce product URLs (Rozetka, Books to Scrape).
- Boundary Value Analysis on price thresholds and polling intervals.
- Price drop alert triggers and duplicate notification suppression.
- SQLite user data isolation and sequential price history tracking in `monitor.db`.
- Background scheduler loop stability and connection timeout handling.

### Out of Scope

- Stress/load testing with more than 1,000 parallel tracked items.
- Automating bypasses for complex interactive CAPTCHAs (Cloudflare Turnstile).
- Production cloud deployment (AWS/VPS integration).
- Multi-currency conversion flows.

---

## 3. Test Execution Summary

| Test Area | Planned | Executed | Passed | Failed | Blocked | Notes |
|---|---:|---:|---:|---:|---:|---|
| Smoke Checklist | 10 | 10 | 10 | 0 | 0 | Critical user flow verified |
| Functional Test Cases | 16 | 16 | 15 | 1 | 0 | 1 boundary failure logged |
| Boundary Testing (BVA) | 4 | 4 | 3 | 1 | 0 | Target price accepts negative numbers |
| Database Verification | 3 | 3 | 3 | 0 | 0 | User isolation and price history |
| Edge-Case & Log Analysis | 3 | 3 | 3 | 0 | 0 | Timeouts, anti-bot, missing selectors |
| **Total unique test cases** | **16** | **16** | **15** | **1** | **0** | **Smoke & BVA embedded within the suite** |

---

## 4. Defects Summary

| Bug ID | Title | Severity | Priority | Status |
|---|---|---|---|---|
| [BUG-SMB-001](Bug-Reports/BUG-SMB-001.md) | Bot accepts zero and negative target prices without validation and saves record into SQLite | Medium | Medium | Open |

*Summary: Found during BVA execution (`TC-SMB-006`). The bot parses inputs via `float()` without enforcing positive values, persisting negative target thresholds in SQLite.*

---

## 5. Risks and Limitations

- Testing was executed in a local macOS development environment.
- External e-commerce platforms may update their anti-bot rules or DOM layouts without prior notice.
- The scraper operates within configured rate limits to prevent IP throttling.

---

## 6. Conclusion and Recommendations

Based on the executed test scope, Smart Monitor Bot is stable and ready for release. The command navigation is responsive, user data is strictly isolated in SQLite, and background scheduler tasks recover gracefully from remote timeouts and network drops.