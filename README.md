# WebRecon (or Your Chosen Name)

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

A Python-based reconnaissance tool to identify web technologies used by websites, including CMS, backend languages/servers, frontend libraries, and optionally Web Application Firewalls (WAF) and Shodan host information.

## Description

WebRecon scans one or more target URLs to fingerprint the underlying web technologies. It analyzes HTTP headers, cookies, and HTML source code to detect known signatures. For WAF detection, it can leverage the external tool `wafw00f`. Optionally, it can enrich findings with host information from Shodan using an API key stored in `config.json`. Results can be displayed in the terminal or saved to a structured Excel report.

## Features

* **CMS Detection:** Identifies Content Management Systems like WordPress, Joomla, Drupal, etc., and attempts version detection (from meta tags and common URL parameters).
* **Backend Technology Detection:** Detects server software (Nginx, Apache, IIS), backend languages (PHP, ASP.NET), and frameworks, attempting version detection from headers.
* **Frontend Technology Detection:** Identifies common JavaScript libraries/frameworks (jQuery, React, Angular, Vue.js, Bootstrap) and attempts version detection from filenames.
* **WAF Detection (Optional):** Uses the external `wafw00f` tool to identify Web Application Firewalls (requires `-w` flag).
* **Shodan Enrichment (Optional):** Queries the Shodan API for host details (IP, Org, ISP, Ports, Tags, Vulns) if the `-s` flag is used and a valid API key is present in `config.json`.
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
    git clone [https://github.com/your-username/your-repo-name.git](https://github.com/your-username/your-repo-name.git)
    cd your-repo-name
    ```
    *(Replace `your-username/your-repo-name` with your actual repository details)*

2.  **Install Python dependencies:**
    It's recommended to use a virtual environment.
    ```bash
    python -m venv venv
    source venv/bin/activate # On Windows use `venv\Scripts\activate`
    pip install -r requirements.txt
    ```
    *(Ensure `requirements.txt` includes `requests`, `beautifulsoup4`, `openpyxl`, `lxml`, and optionally `shodan`)*

3.  **Install wafw00f (Optional, for WAF detection):**
    Follow the installation guide on the [wafw00f GitHub page](https://github.com/EnableSecurity/wafw00f#installation). Ensure it's added to your system's PATH.

4.  **Create `config.json` (Optional, for Shodan):**
    If you want to use Shodan integration (`-s` flag), create a file named `config.json` in the same directory as the script with the following content:
    ```json
    {
      "shodan_api_key": "YOUR_SHODAN_API_KEY_HERE"
    }
    ```
    Replace `"YOUR_SHODAN_API_KEY_HERE"` with your actual Shodan API key (Membership tier recommended for sufficient credits).

## Usage

```bash
python WebRecon.py [-h] (-d DOMAIN | -l LIST) [-o OUTPUT] [-t THREADS] [-w] [-s] [-v]
Arguments:-h, --help: Show the help message and exit.-d DOMAIN, --domain DOMAIN: Single target domain or URL (e.g., example.com or https://example.com).-l LIST, --list LIST: File containing a list of target domains or URLs (one per line).-o OUTPUT, --output OUTPUT: Output filename for the combined Excel report (e.g., report.xlsx). If extension is omitted, .xlsx will be appended. If not provided, results print to terminal only.-t THREADS, --threads THREADS: Number of concurrent threads for list scanning (default: 4).-w, --waf: Enable WAF detection using the external 'wafw00f' tool (requires wafw00f installed).-s, --shodan: Enable Shodan host enrichment (requires config.json with API key and 'shodan' library).-v, --verbose: Increase output verbosity (show errors, thread activity, etc.).Examples:Scan a single domain and print to terminal:python WebRecon.py -d example.com
Scan a single domain with WAF and Shodan detection (requires setup):python WebRecon.py -d example.com -w -s -v
Scan a list of domains from a file using 10 threads:# targets.txt contains:
# example.com
# [https://github.com](https://github.com)
# anothersite.org

python WebRecon.py -l targets.txt -t 10
Scan a list, perform WAF detection, Shodan lookup, and save to Excel:python WebRecon.py -l targets.txt -w -s -o scan_results.xlsx
Scan a list and save to Excel named 'report' (auto-appends .xlsx):python WebRecon.py -l targets.txt -o report
OutputTerminal Output: When no -o option is specified, results for each target are printed to the console in a summarized format after the scan for that target completes. Includes CMS, Backend, Frontend, WAF (if checked), and Shodan Info (if checked and successful).Excel Output: If -o is used, an Excel file (.xlsx) is generated containing:Scan Summary Sheet: Lists all targets scanned, their final URL, status code, and a summary count of status codes at the top.Technologies Sheet: Details all detected technologies (WAF, CMS, Backend, Frontend, Shodan) with versions/details (where available) for each target. Cells for "Target URL" and "WAF" are merged vertically for readability, as is the "Category" column within each target's block. Domain results are separated by a border. The "Shodan" category only appears if the check was enabled and successful for that target.Example Terminal Output (with WAF and Shodan)--- Results for: example.com ---
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
  Vulnerabilities: Not Found
------------------------------------
