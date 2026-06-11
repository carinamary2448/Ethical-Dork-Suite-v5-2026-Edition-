# Ethical Dork Suite — v5 (2026 Edition)

> **⚠️ DISCLAIMER: For authorized security testing, penetration testing, and educational purposes ONLY.**  
> This tool is intended for ethical hackers, security researchers, and penetration testers who have **explicit written authorization** to test the target systems. Unauthorized use against systems you do not own or have permission to test is **illegal** and may violate computer fraud laws in your jurisdiction. The author assumes no responsibility for misuse.

---

## Overview

**Ethical Dork Suite (EDSS)** is a comprehensive, single-file Python/Tkinter GUI application designed for authorized Google Dorking-based vulnerability research and open-source intelligence (OSINT) gathering. Built for penetration testers and security professionals, EDSS enables systematic discovery of exposed files, misconfigured servers, and publicly accessible sensitive resources across **13 search engines** simultaneously — providing real-time results in a clean, dark-themed desktop interface.

At its core, EDSS automates the process of crafting and submitting search engine dorks (specialized queries) to identify potential attack surfaces on systems within a defined authorized scope. All results are stored in a local SQLite database with a full audit trail, making it suitable for professional reporting and evidence collection during authorized engagements.

---

## Key Features

### 🔍 Multi-Engine Real-Time Scanner
- Supports **13 search engines**: Bing, DuckDuckGo, Brave, Yandex, Ask, Yahoo, Ecosia, Startpage, Google, AOL, Yahoo Japan, Mojeek, and Qwant
- Dual-mode scraping: **BS4 (BeautifulSoup)** for fast static HTML engines and **Selenium** for JavaScript-rendered engines
- Engine tier system (`bs4` / `hybrid` / `selenium`) routes requests to the optimal scraping strategy per engine
- Per-engine result counters displayed as live pills below the progress bar
- Concurrent multi-dork scanning with configurable thread pool (thread cap via `settings.yaml`)

### 🛡️ Vulnerability Pattern Detection
- Automatic classification of results against known vulnerability patterns (exposed `.env` files, open directories, login panels, database dumps, config files, and more)
- Vulnerable URLs flagged and separated from general results in real time
- Filter, export, and review flagged findings independently

### 🌐 Proxy Manager
- Load proxies from file (HTTP/HTTPS/SOCKS4/SOCKS5 supported via PySocks)
- Two-phase proxy validation: liveness check → HTTPS quality gate
- Live pool summary dashboard: alive / dead / untested / protocol breakdown
- Automatic proxy rotation during scans with rotation counter
- Runtime success/failure tracking with demotion logic
- Export valid/filtered proxy lists

### 🤖 CAPTCHA Handling
- Automatic detection of reCAPTCHA, hCaptcha, and generic CAPTCHA triggers
- Four handling modes: **Manual pause-popup**, **2Captcha API**, **AntiCaptcha API**, **hCaptcha bypass**
- Per-mode configurable retry delays and maximum wait times
- Auto-visible Selenium controls when a CAPTCHA is detected

### 🧰 Dork Tools
- Keyword-based dork generation with built-in dork template library
- Random dork suggestion (powered by Faker)
- Upload keyword lists or URL lists for bulk dork extraction
- Combine and generate compound dorks
- Save/export dork library for reuse across engagements

### 🔌 Port Scanner
- Lightweight concurrent TCP port scanner built into the UI
- Configurable target, port range, thread count, and per-port timeout
- Export open port results as TXT or JSON

### 🌍 Domain-to-IP Resolver
- Bulk concurrent DNS resolution of domains or URLs
- Retry logic for failed resolutions
- Export resolved / failed results as TXT or JSON

### 📊 Stats & Audit Logging
- Live memory usage chart (via Matplotlib + psutil)
- Full buffered audit log (`data/audit_log.txt`) capturing all scan activity
- SQLite result store (`data/results.db`) with WAL mode for safe concurrent writes
- Export results by domain, by engine, or full dump in JSON or TXT format

### ⚙️ Persistent Selenium Profiles
- Per-browser profile directories (`data/selenium_profiles/{chrome,firefox,edge}/`)
- Persistent cookies and localStorage across sessions
- One-click clear with bytes-freed reporting

---

## Architecture

```
Ethical_Management_Dork_Scanner/
├── ethical_dork_suite.py       # Single-file application (~5,380 lines)
├── requirements.txt            # Python dependencies
├── run.sh                      # Linux/macOS launcher (auto-installs deps)
├── run.bat                     # Windows launcher (auto-installs deps)
├── config/
│   └── settings.yaml           # Runtime configuration (auto-created)
└── data/
    ├── results.db              # SQLite scan results (WAL mode)
    ├── audit_log.txt           # Buffered audit trail
    ├── exports/                # Exported scan data
    └── selenium_profiles/
        ├── chrome/
        ├── firefox/
        └── edge/
```

**Core components:**

| Component | Description |
|---|---|
| `ProxyManager` | Thread-safe proxy pool with two-phase validation and rotation |
| `ResultStore` | WAL-mode SQLite backend with streaming export for large result sets |
| `DorkScanner` | Main scan engine — BS4 fast path, Selenium path, retry logic |
| `EthicalDorkSuiteApp` | Tkinter GUI — 6 tabs: Scanner, Proxy Manager, Dork Tools, Stats/Logs, Port Scanner, Domain-to-IP |

---

## Use Cases (Authorized Engagements Only)

- **Penetration Testing** — Discover exposed configuration files, credentials, or admin panels on authorized targets
- **Attack Surface Mapping** — Enumerate publicly indexed assets belonging to a target organization within scope
- **OSINT Research** — Gather open-source intelligence on authorized targets during red team engagements
- **Security Auditing** — Identify misconfigurations or unintended public exposure before malicious actors do
- **CTF & Security Training** — Practice dorking techniques in controlled lab environments

---

## Tech Stack

- **Language**: Python 3.10+
- **GUI**: Tkinter (built-in, no extra GUI framework needed)
- **Scraping**: BeautifulSoup4, lxml, Requests, Selenium 4.x, WebDriver Manager
- **Proxy**: PySocks (SOCKS4/5)
- **Database**: SQLite3 (WAL mode)
- **Visualization**: Matplotlib
- **Misc**: psutil, Faker, PyYAML

---

*Built for the security community. Use responsibly, within scope, and within the law.*
