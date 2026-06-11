# Ethical Dork Suite — Installation & Setup Guide

> **⚠️ LEGAL DISCLAIMER**  
> This tool is provided **strictly for educational purposes, authorized penetration testing, and ethical security research**. You must have **explicit written permission** from the system owner before scanning any target. Unauthorized use is illegal. The author accepts no liability for misuse.

---

## Requirements

| Requirement | Version |
|---|---|
| Python | 3.10 or higher |
| Operating System | Windows 10/11, Ubuntu 20.04+, macOS 12+ |
| Browser (optional) | Chrome, Firefox, or Edge (for Selenium mode) |

---

## Quick Start

### Windows

1. Make sure Python 3.10+ is installed from [python.org](https://python.org)  
   ✅ Check **"Add Python to PATH"** during installation.

2. Download and extract the project zip.

3. Double-click **`run.bat`** inside the project folder.  
   It will automatically install all dependencies and launch the app.

---

### Linux / macOS

1. Install Python 3.10+ if not already present:

   ```bash
   # Ubuntu / Debian
   sudo apt update && sudo apt install python3 python3-pip

   # macOS (Homebrew)
   brew install python
   ```

2. Extract the project zip:

   ```bash
   unzip Ethical_Management_Dork_Scanner.zip
   cd Ethical_Management_Dork_Scanner
   ```

3. Run the launcher script:

   ```bash
   chmod +x run.sh
   ./run.sh
   ```

   The script detects your Python binary, installs dependencies, and launches the GUI.

---

## Manual Installation

If you prefer to install dependencies yourself:

```bash
cd Ethical_Management_Dork_Scanner
pip install -r requirements.txt
python ethical_dork_suite.py
```

---

## Dependencies

All dependencies are listed in `requirements.txt` and installed automatically by the launcher scripts.

**Core (required):**

| Package | Purpose |
|---|---|
| `requests>=2.31.0` | HTTP client for BS4-mode engine scraping |
| `beautifulsoup4>=4.12.0` | HTML parsing for search result extraction |
| `lxml` | Faster BS4 parser backend |
| `pysocks>=1.7.1` | SOCKS4/5 proxy support |

**Selenium (optional — needed for Brave, Google, Qwant and other JS engines):**

| Package | Purpose |
|---|---|
| `selenium>=4.18.0` | Browser automation (4.x Service API required) |
| `webdriver-manager>=4.0.0` | Auto-downloads ChromeDriver, GeckoDriver, EdgeDriver |

> Selenium drivers are cached in `~/.wdm` and refreshed automatically.

**Optional extras:**

| Package | Purpose |
|---|---|
| `psutil>=5.9.0` | Memory monitoring & guard |
| `matplotlib>=3.8.0` | Memory usage chart in Stats tab |
| `faker>=24.0.0` | Random keyword suggestions in Dork Tools |
| `pyyaml>=6.0.1` | Reads `config/settings.yaml` at startup |

---

## Project Structure

```
Ethical_Management_Dork_Scanner/
├── ethical_dork_suite.py       # Main application (~5,380 lines)
├── requirements.txt
├── run.bat                     # Windows launcher
├── run.sh                      # Linux/macOS launcher
├── config/
│   └── settings.yaml           # Auto-created on first run
└── data/
    ├── results.db              # SQLite scan database
    ├── audit_log.txt           # Audit trail
    ├── exports/                # Exported results land here
    └── selenium_profiles/
        ├── chrome/
        ├── firefox/
        └── edge/
```

The `data/` and `config/` directories are created automatically on first launch.

---

## Configuration (Optional)

Edit `config/settings.yaml` to customize runtime behavior. The file is auto-generated with defaults on first run.

```yaml
# Search engine delays (seconds between requests)
delay_min: 1
delay_max: 2

# Selenium-specific delays
selenium_delay_min: 2
selenium_delay_max: 4

# Default engines to enable
engines:
  - bing
  - duckduckgo

# Thread pool cap
thread_cap: 8

# CAPTCHA handling
captcha_detection: true
captcha_mode: manual          # manual | 2captcha | anticaptcha | hcaptcha
captcha_retry_delay: 10
captcha_max_wait: 60

# Proxy settings
proxy_require_https: true
proxy_min_https_checks: 1

# Port scanner timeout (seconds per port)
port_timeout: 1

# Memory guard (MB)
max_memory_mb: 500
```

---

## Using the Application

The GUI is organized into **6 tabs**:

### 1. Scanner
- Enter one or more dorks (one per line) in the dork input box
- Select target engines by checking the engine checkboxes
- Configure page count, thread count, and date filter
- Click **Start Scan** — results appear in real time
- Vulnerable URLs are flagged automatically and can be filtered/exported separately

### 2. Proxy Manager
- Load a proxy list from a `.txt` file (one proxy per line, supports `http://`, `https://`, `socks4://`, `socks5://`)
- Click **Validate** to run two-phase proxy checking
- The live pool summary shows alive / dead / untested counts and protocol breakdown
- Export valid proxies for use in external tools

### 3. Dork Tools
- Generate dorks from keywords, upload keyword lists, or use the random dork generator
- Browse the built-in dork template library
- Send selected dorks directly to the Scanner tab

### 4. Stats / Logs
- View the live audit log
- Plot a real-time memory usage chart
- Export the audit log or clear the results cache

### 5. Port Scanner
- Enter a target IP/range and port range
- Set thread count and per-port timeout
- Export open port results as TXT or JSON

### 6. Domain-to-IP
- Bulk-resolve domains or URLs to IP addresses
- Retry failed resolutions
- Export resolved and failed results

---

## Troubleshooting

**App won't launch / Tkinter not found:**
```bash
# Ubuntu/Debian
sudo apt install python3-tk

# Fedora
sudo dnf install python3-tkinter
```

**Selenium errors / browser not found:**  
Make sure Chrome, Firefox, or Edge is installed. `webdriver-manager` will download the matching driver automatically on first use.

**No results from Google/Brave/Qwant:**  
These engines require Selenium mode. Enable Selenium in the Scanner tab and ensure a browser is installed.

**Proxy validation is slow:**  
The default validation uses up to 30 concurrent workers. Large proxy lists (1000+) may take several minutes.

---

## Legal & Ethical Use

- Always obtain **written authorization** before scanning any system you do not own.
- This tool is intended for **penetration testers**, **CTF participants**, **security researchers**, and **students** studying OSINT and ethical hacking.
- The audit log (`data/audit_log.txt`) records all scan activity — retain it as part of your engagement documentation.
- Do not use this tool to collect data on individuals or systems outside your authorized scope.

---

## License

This project is released for **educational and authorized security research purposes only**. See `LICENSE` for full terms.
