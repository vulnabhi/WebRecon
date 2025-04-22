# WebRecon

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

A Python-based reconnaissance tool to identify web technologies used by websites, including CMS, backend languages/servers, frontend libraries, and optionally Web Application Firewalls (WAF).

## Description

BackRecon scans one or more target URLs to fingerprint the underlying web technologies. It analyzes HTTP headers, cookies, and HTML source code to detect known signatures. For WAF detection, it leverages the external tool `wafw00f`. Results can be displayed in the terminal or saved to a structured Excel report.

## Features

* **CMS Detection:** Identifies Content Management Systems like WordPress, Joomla, Drupal, etc., and attempts version detection.
* **Backend Technology Detection:** Detects server software (Nginx, Apache, IIS), backend languages (PHP, ASP.NET), and frameworks, attempting version detection from headers.
* **Frontend Technology Detection:** Identifies common JavaScript libraries/frameworks (jQuery, React, Angular, Vue.js, Bootstrap) and attempts version detection from filenames.
* **WAF Detection (Optional):** Uses the external `wafw00f` tool to identify Web Application Firewalls (requires `-w` flag).
* **Input Flexibility:** Accepts a single domain/URL (`-d`) or a file containing a list of targets (`-l`).
* **Output Options:** Displays results directly in the terminal or saves a detailed report to an Excel (`.xlsx`) file (`-o`).
* **Threading:** Supports concurrent scanning using multiple threads (`-t`) for faster processing of lists.
* **Excel Formatting:** Generates a well-formatted Excel report with summaries, merged cells for readability, and borders.

## Requirements

* **Python:** 3.7+ recommended.
* **Python Libraries:** `requests`, `beautifulsoup4`, `openpyxl`, `lxml`
* **External Tool (for WAF detection):** `wafw00f` must be installed and accessible in your system's PATH if you intend to use the `-w` flag. See [wafw00f installation instructions](https://github.com/EnableSecurity/wafw00f#installation).

## Installation

1.  **Clone the repository (or download the script):**
    ```bash
    git clone https://github.com/vulnabhi/WebRecon.git
    cd WebRecon
    chmod +x WebRecon.py
    pip install -r requirements.txt
    ```
  

3.  **Install wafw00f (Optional, for WAF detection):**
    Follow the installation guide on the [wafw00f GitHub page](https://github.com/EnableSecurity/wafw00f#installation). Ensure it's added to your system's PATH.

## Usage

```bash
python WebRecon.py [-h] (-d DOMAIN | -l LIST) [-o OUTPUT] [-t THREADS] [-w] [-v]
```

**🙌 Credits**

Developed by [@offsecabhi](https://github.com/offsecabhi)
Feel free to contribute, suggest features, or report bugs via Issues or PRs.

