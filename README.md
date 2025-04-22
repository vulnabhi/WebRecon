# WebRecon v1.1

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

A Python-based reconnaissance tool to identify web technologies used by websites, including CMS, backend languages/servers, frontend libraries, and optionally Web Application Firewalls (WAF) and Shodan host information.

## Description

WebRecon scans one or more target URLs to fingerprint the underlying web technologies. It analyzes HTTP headers, cookies, and HTML source code to detect known signatures. For WAF detection, it can leverage the external tool `wafw00f`. Optionally, it can enrich findings with host information from Shodan using an API key stored. Results can be displayed in the terminal or saved to a structured Excel report.

## Features

* **CMS Detection:** Identifies Content Management Systems like WordPress, Joomla, Drupal, etc., and attempts version detection (from meta tags and common URL parameters).
* **Backend Technology Detection:** Detects server software (Nginx, Apache, IIS), backend languages (PHP, ASP.NET), and frameworks, attempting version detection from headers.
* **Frontend Technology Detection:** Identifies common JavaScript libraries/frameworks (jQuery, React, Angular, Vue.js, Bootstrap) and attempts version detection from filenames.
* **WAF Detection (Optional):** Uses the external `wafw00f` tool to identify Web Application Firewalls (requires `-w` flag).
* **Shodan Host Information (Optional):** Queries the Shodan API for host details (IP, Org, ISP, Ports, Tags, Vulns) if the `-s` flag is used and a valid API key is present in `config.json`.
* **Input Flexibility:** Accepts a single domain/URL (`-d`) or a file containing a list of targets (`-l`).
* **Output Options:** Displays results directly in the terminal or saves a detailed report to an Excel (`.xlsx`) file (`-o`).
* **Threading:** Supports concurrent scanning using multiple threads (`-t`) for faster processing of lists.
* **Excel Formatting:** Generates a well-formatted Excel report with summaries, merged cells for readability, and borders.

## Requirements

* **Python:** 3.7+ recommended.
* **Python Libraries:** `requests`, `beautifulsoup4`, `openpyxl`, `lxml`.
* **Shodan Library (Optional):** `shodan` library (`pip install shodan`) is required if using the `-s` flag.
* **External Tool (Optional):** `wafw00f` must be installed and accessible in your system's PATH if using the `-w` flag. See [wafw00f installation instructions](https://github.com/EnableSecurity/wafw00f#installation).

## Installation

1.  **Clone the repository (or download the script):**
    ```bash
    git clone https://github.com/offsecabhi/WebRecon.git
    cd WebRecon
    chmod +x WebRecon.py
    ```

2.  **Install Python dependencies:**
    ```bash
    pip install -r requirements.txt
    ```

3.  **Install wafw00f (Optional, for WAF detection):**
    Follow the installation guide on the [wafw00f GitHub page](https://github.com/EnableSecurity/wafw00f#installation). Ensure it's added to your system's PATH.

4.  **Create `config.json` (Optional, for Shodan):**
    If you want to use Shodan integration (`-s` flag), also make sure to put your shodan api key under `config.json` in the same directory as the script with the following content:
    ```json
    {
      "shodan_api_key": "YOUR_SHODAN_API_KEY_HERE"
    }
    ```
    Replace `"YOUR_SHODAN_API_KEY_HERE"` with your actual Shodan API key (Membership tier recommended for sufficient credits).

## Usage

```bash
python WebRecon.py -l targets.txt -t 10
Results for: example.com ---
Final URL   : [https://example.com/](https://example.com/) (Status: 200)
CMS         : WordPress (6.1.1)
Backend     : Nginx
Frontend    : jQuery (3.6.0)
WAF         : Cloudflare

[+] Shodan Info:
  IP Address     : 93.184.216.34
  Organization   : Edgecast Inc.
  ISP            : Edgecast Inc.
  Hostnames      : example.com, [www.example.com](https://www.example.com)
  Open Ports     : 80, 443
  Tags           : cloud
  Vulnerabilities: CVE-2021-44228
------------------------------------
```
**🙌 Credits**

Developed by [@offsecabhi](https://github.com/offsecabhi)
Feel free to contribute, suggest features, or report bugs via Issues or PRs.
