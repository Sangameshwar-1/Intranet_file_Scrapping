# Intranet_file_Scrapping

Intranet_file_Scrapping is a small project intended to automate downloading and indexing files from an internal corporate intranet. It provides a configurable scraping/harvesting workflow that can be adapted to different intranet sites and file types. Use this repository only on systems and networks where you have authorization to access and scrape files.

> NOTE: The word "Scrapping" in the repository name matches the repository title. If you intended "Scraping", consider renaming the repo.

Table of contents
- About
- Features
- Requirements
- Installation
- Configuration
- Usage
- Examples
- Security & compliance
- Troubleshooting
- Contributing
- License
- Contact

About
-----
This project collects files from intranet pages, saves them to a local store, and can optionally extract metadata (filename, size, timestamps) for indexing. The repository is intentionally generic so you can adapt the scraping logic, selectors, and storage backends to your environment.

Features
--------
- Config-driven crawling of intranet pages
- File download and local storage
- Metadata extraction (basic)
- Pluggable selectors for different HTML structures
- Optional rate-limiting and retry behavior

Requirements
------------
- Python 3.8+
- pip
- Recommended libraries (typical): requests, beautifulsoup4, pyyaml, tqdm
  - Install with: pip install -r requirements.txt (if provided)

Installation
------------
1. Clone the repository:
   ```bash
   git clone https://github.com/Sangameshwar-1/Intranet_file_Scrapping.git
   cd Intranet_file_Scrapping
   ```

2. Create a virtual environment and install dependencies:
   ```bash
   python -m venv .venv
   source .venv/bin/activate     # On Windows: .venv\Scripts\activate
   pip install -r requirements.txt
   ```

Configuration
-------------
This project expects a configuration file (YAML/JSON) to describe:
- start_urls: list of intranet pages to crawl
- selectors: CSS/XPath selectors to locate file links
- download_path: local destination for files
- auth: credentials or mechanism if the intranet requires login
- rate_limit: requests per second or delay between requests
- allowed_extensions: list of extensions to download

Example config (config.yaml):
```yaml
start_urls:
  - "https://intranet.example.local/reports"
selectors:
  file_links: "a[href$='.pdf'], a[href$='.docx']"
download_path: "./downloads"
auth:
  type: "basic"            # or "none", "form", "oauth"
  username: "YOUR_USER"
  password: "YOUR_PASS"
rate_limit:
  delay_seconds: 1.0
allowed_extensions:
  - ".pdf"
  - ".docx"
  - ".xlsx"
```

Usage
-----
There is no single enforced CLI entrypoint in this template. Typical usage patterns:

- Run the main script (example):
  ```bash
  python main.py --config config.yaml
  ```

- Or import the core downloader in your own script:
  ```python
  from intranet_scrapper import Scraper
  s = Scraper(config="config.yaml")
  s.run()
  ```

Replace `main.py` and module names above with the actual script/module names present in the repository.

Examples
--------
- Crawl and download PDFs from a reports page:
  - Update `config.yaml` with the reports URL and `.pdf` in `allowed_extensions`.
  - Run the main script.

- Authenticate using intranet form:
  - Implement a form login handler that posts credentials and maintains a requests.Session cookie jar.

Security & compliance
---------------------
- Only run this tool against systems you have explicit permission to crawl and download from.
- Do NOT store credentials in plaintext. Prefer environment variables, OS keyrings, or vaults.
- Respect robots.txt and intranet usage policies even if not enforced.
- Be mindful of PII and sensitive data: do not re-distribute internal documents.

Troubleshooting
---------------
- 401/403 errors: confirm credentials and that you are allowed to access the resources.
- Missing files: update selectors to match the actual HTML structure of the intranet pages.
- Slow or blocked: reduce rate, add delays, or ask for whitelist from network admins.

Contributing
------------
Contributions are welcome. Typical ways to contribute:
- Open issues describing problems or feature requests
- Provide pull requests that add selectors, auth methods, or storage backends
- Add tests and CI integration

Please follow these simple rules:
- Include tests where appropriate
- Keep changes focused and documented
- Respect the security note above about sensitive data

License
-------
This repository is provided under the MIT License. See LICENSE for details.

Contact
-------
Author: Sangameshwar-1
GitHub: https://github.com/Sangameshwar-1

If you want, I can:
- create a concrete main.py and a requirements.txt based on your code,
- add example config files for different intranet types,
- or open a draft README directly as a PR into your repository.
