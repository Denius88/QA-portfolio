# Denys Pelekh — QA Portfolio

[![QA Portfolio](https://img.shields.io/badge/QA-Portfolio-blue.svg)](#portfolio-projects)
[![English](https://img.shields.io/badge/English-C1%20Advanced-brightgreen.svg)](#education--certifications)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-blue?style=flat&logo=linkedin)](https://linkedin.com)
[![GitHub](https://img.shields.io/badge/GitHub-Denius88-black?style=flat&logo=github)](https://github.com/Denius88)

**QA Engineer**  
📍 Lviv, Ukraine | ✉️ [denis003500@gmail.com](mailto:denis003500@gmail.com) |  

---

## 👨‍💻 Professional Summary

Detail-oriented and analytical QA Engineer with an educational background in Computer Science and hands-on experience delivering structured QA documentation and performing end-to-end functional testing. Experienced in smoke, functional, regression, negative, boundary, and exploratory testing across Telegram and web applications. Skilled in creating test plans, test cases, checklists, bug reports, and test summary reports; validating REST APIs; inspecting application logs; and verifying data integrity with SQL. Strong English language proficiency with readiness for technical documentation and international team communication.

---

## 🛠 Technical Skills Matrix

| Category | Competencies & Tools |
|---|---|
| **Testing Types** | Manual Functional Testing, Smoke Testing, Regression Testing, Exploratory Testing (Session-Based), Negative Testing, Boundary Testing, API Testing, Web UI Testing, Log Inspection |
| **Test Design Techniques** | Boundary Value Analysis (BVA), Equivalence Partitioning (EP), Decision Tables, State Transition |
| **QA Documentation** | Test Plans, Test Cases, Checklists, Bug Reports, Test Summary Reports, Jira and TestRail basics |
| **API & Web Debugging** | REST API, Postman (Collections & Test Assertions), Swagger / OpenAPI, Safari Web Inspector (Network & Console) |
| **Databases & Data Verification** | SQL (SELECT, JOIN, WHERE, aggregate functions), SQLite 3 |
| **Tools & Environment** | Git, GitHub, macOS / Linux CLI basics, Terminal log inspection, pytest |

---

## 📁 Portfolio Projects

| # | Project | Platform / Type | Scope & Deliverables | Key Skills Demonstrated |
|---|---|---|---|---|
| **01** | [Store Bot](01-Store-Bot/) | Telegram E-Commerce Application | • [Test Plan](01-Store-Bot/Test-Plan.md)<br>• [Smoke Checklist](01-Store-Bot/Checklist.md)<br>• [23 Test Cases](01-Store-Bot/Test-Cases.md)<br>• [Test Design Techniques](01-Store-Bot/Test-Design-Techniques.md)<br>• [Exploratory Testing Charters](01-Store-Bot/Exploratory-Testing.md)<br>• [Database Verification](01-Store-Bot/Database-Verification.md)<br>• [Test Summary Report](01-Store-Bot/Test-Summary-Report.md)<br>• [UI Evidence](01-Store-Bot/evidence/) | Manual Functional Testing, BVA, Equivalence Partitioning, State Transitions, Decision Tables, SQL Data Verification, pytest automation |
| **02** | [VideoDW](02-VideoDW/) | Media Processing Service (REST API & Web) | • [API Test Plan](02-VideoDW/API/Test-Plan.md)<br>• [10 API Test Cases](02-VideoDW/API/Test-Cases.md)<br>• [Postman Collection](02-VideoDW/API/Postman-Collection.json)<br>• [Bug Reports (2)](02-VideoDW/API/Bug-Reports/)<br>• [Web Test Plan & Cases](02-VideoDW/Web/)<br>• [Logs & Edge Cases](02-VideoDW/Logs-and-Edge-Cases.md)<br>• [API Summary Report](02-VideoDW/API/Test-Summary-Report.md) | REST API Testing, Postman `pm.test` assertions, Safari Web Inspector, Bug Reporting (Severity/Priority/Impact), Backend log tracing |
| **03** | [Smart Monitor Bot](03-Smart-Monitor-Bot/) | Price Monitoring Application (Telegram & Background Workers) | • [Test Plan](03-Smart-Monitor-Bot/Test-Plan.md)<br>• [Smoke Checklist](03-Smart-Monitor-Bot/Checklist.md)<br>• [16 Test Cases](03-Smart-Monitor-Bot/Test-Cases.md)<br>• [Test Design Techniques](03-Smart-Monitor-Bot/Test-Design-Techniques.md)<br>• [Database Verification](03-Smart-Monitor-Bot/Database-Verification.md)<br>• [Logs & Edge Cases](03-Smart-Monitor-Bot/Logs-and-Edge-Cases.md)<br>• [BUG-SMB-001](03-Smart-Monitor-Bot/Bug-Reports/BUG-SMB-001.md)<br>• [Test Summary Report](03-Smart-Monitor-Bot/Test-Summary-Report.md) | Conversational FSM navigation, Boundary Value Analysis, SQLite Data Isolation & History Consistency, Background Scheduler & Scraper Fault Tolerance (403 / 404 / DOM changes) |

---

### Project 1: [Store Bot — E-Commerce Telegram Application](01-Store-Bot/)
* **Application Context:** A Telegram e-commerce store with catalog navigation, cart management, checkout with input validation, order history, and an administrator panel.
* **Testing Highlights:**
  - Executed smoke, functional, negative, boundary, exploratory, and regression testing across customer and administrator flows.
  - Applied **Boundary Value Analysis** and **Equivalence Partitioning** to phone numbers, delivery addresses, and product prices.
  - Designed **State Transition** diagrams and a **Decision Table** for cart and checkout states.
  - Verified SQLite data persistence, user order privacy, order total calculations (`SUM(quantity * unit_price)`), and cart cleanup using SQL queries and automated `pytest` tests.
  - Collected full screenshot evidence covering the main menu, catalog, product details, cart, checkout, order creation, admin panel, and status updates.

### Project 2: [VideoDW — Media Processing Service](02-VideoDW/)
* **Application Context:** Media downloader and converter supporting MP4/MP3 extraction, progress event streaming, and file retrieval via REST API and Safari Web Interface.
* **Testing Highlights:**
  - Conducted REST API testing with Postman and Swagger/OpenAPI, covering payload validation, supported formats, streamed SSE events, file retrieval, status codes, and JSON errors.
  - Authored **10 API test cases**, an API checklist, an automated Postman collection with test assertions, and documented **two high-impact bug reports**:
    - [BUG-API-001](02-VideoDW/API/Bug-Reports/BUG-API-001.md): API accepts unsupported `avi` format and processes as MP4.
    - [BUG-API-002](02-VideoDW/API/Bug-Reports/BUG-API-002.md): Unhandled `500 Internal Server Error` on malformed download ID.
  - Tested the Safari web interface (URL validation, format selection, progress states, language/theme switches); monitored Network requests and JavaScript Console in Safari Web Inspector.
  - Documented edge cases in backend terminal logs: unavailable media handling, error events, temporary-file cleanup, and process stability.

### Project 3: [Smart Monitor Bot — Price Monitoring Application](03-Smart-Monitor-Bot/)
* **Application Context:** Telegram price tracking bot with asynchronous background workers that periodically scrape e-commerce storefronts, evaluate user thresholds, and trigger instant price drop notifications.
* **Testing Highlights:**
  - Authored a complete test suite: Test Plan, Smoke Checklist, 16 positive/negative/boundary test cases, and Test Design documentation.
  - Applied **Boundary Value Analysis** on price thresholds ($0.01, $0.00, negative values) and background polling intervals (lower limit 5 minutes, upper limit 24 hours).
  - Documented **[BUG-SMB-001](03-Smart-Monitor-Bot/Bug-Reports/BUG-SMB-001.md)**: Missing boundary validation allowing negative and zero target prices in SQLite.
  - Prepared SQL verification queries in SQLite to confirm **user data isolation** (multi-tenancy) and **chronological price history consistency** with deduplication.
  - Designed resilience test scenarios and log inspection patterns for external scraper failures: connection timeouts (`httpx.ConnectTimeout`), anti-bot challenges (`403 Forbidden` / Cloudflare), and store DOM layout changes (`SelectorNotFoundError`).

---

## 🎓 Education & Certifications

* **Lviv Polytechnic National University** (2025 – Present)  
  *Bachelor of Science in Computer Science* — Lviv, Ukraine
* **Zolochiv Vocational College** (2021 – 2025)  
  *Professional Junior Bachelor in Software Engineering* — Ukraine
* **EF SET English Certificate: C1 Advanced** (Score: 62/100, Sep 2026)  
  *Reading:* C2 (71) | *Listening:* C2 (79) | *Writing:* B2 (51) | *Speaking:* B1 (48)  
  *Languages:* English (Fluent reading/listening, working written), Ukrainian (Native)
