# Improved Phishing URL Scanner

A lightweight, heuristic-based phishing URL scanner written in Python. Use it as a CLI tool or import it as a module in other projects to get a quick, JSON-serializable assessment of whether a URL looks suspicious or likely to be a phishing attempt.

---

## Features

- Robust URL parsing with normalization
- Accurate IP detection using `ipaddress` (not just regex)
- Domain/subdomain analysis using `tldextract`
- Heuristics for suspicious keywords, excessive special characters, long URLs, and subdomain depth
- Similarity check against a list of well-known legitimate domains (to detect typosquatting)
- Tunable thresholds and scoring (0â€“10) with human-readable labels
- JSON output for easy integration with other tools
- CLI + batch (`--stdin`) support

---

## Quickstart

### Requirements

- Python 3.8+
- `tldextract` (install with pip)

```bash
pip install tldextract
```

### Run as CLI

Scan one or more URLs from the command line:

```bash
python improved_phish_scanner.py https://example.com http://192.0.2.1
```

Scan URLs from stdin (one per line):

```bash
cat urls.txt | python improved_phish_scanner.py --stdin
```

Each scanned URL prints a JSON object containing details and the final `phishing_score` and `label`.

### Use as a module

```python
from improved_phish_scanner import analyze_url

result = analyze_url('http://account-google.com/login')
print(result['label'], result['phishing_score'])
```

---

## What the scanner checks

- **IP in host** â€” IPv4/IPv6 hosts are suspicious when used in place of domain names.
- **Suspicious keywords** â€” Looks for words like `login`, `secure`, `verify`, `bank`, etc., inside domain, subdomain, path and query.
- **Excessive special characters** â€” Flags long runs of `-`, `%`, `=` and multiple `@` in path/query.
- **HTTPS** â€” Lack of HTTPS increases suspicion (but isn't a definitive signal).
- **URL length** â€” Very long URLs are more suspicious.
- **Subdomain depth** â€” Many nested subdomains can indicate obfuscation.
- **Similarity to legitimate domains** â€” Detects typosquatting by measuring string similarity to a configured whitelist.

The heuristics produce a score (0â€“10) and a label:

- `âš ï¸ Likely PHISHING` (high confidence)
- `â— Suspicious` (medium risk)
- `âœ… Likely Safe` (low risk)

---

## Configuration / Tuning

Open the script and adjust the following constants as needed for your environment:

- `LEGITIMATE_DOMAINS` â€” list of brands/domains to compare against
- `SUSPICIOUS_KEYWORDS` â€” terms to consider suspicious
- `SIMILARITY_PENALTY_THRESHOLD` â€” similarity ratio that triggers a penalty (default `0.85`)
- `PHISHING_SCORE_HIGH` / `PHISHING_SCORE_MED` â€” score thresholds for labels

You can also change scoring rules inside `analyze_url()` to weight features differently.

---

## Integration ideas

- Integrate with a mail gateway or webhook to pre-check links in inbound messages
- Batch-scan historical logs and produce reports (CSV/JSON)
- Plug into VirusTotal / WHOIS for higher-confidence checks (requires API keys)
- Combine with a lightweight ML model trained on labeled phishing/benign URLs for improved precision

---

## Testing

Add unit tests (recommended tools: `pytest`) to validate heuristics and edge cases. Example tests to add:

- URLs with IPv4/IPv6 hosts
- URLs with and without schemes
- Typosquatting examples
- Very long path/query strings

---

## Known caveats

- `tldextract` may fetch the public suffix list on first run. In air-gapped or offline environments, pre-cache or vendor the list.
- Heuristic-based scanners produce false positives/negatives. For production use, consider enriching with external threat feeds or human-review workflows.

---

## Contributing

1. Fork the repo
2. Create a feature branch (`git checkout -b feature/my-change`)
3. Add tests and documentation
4. Open a pull request with a description of your changes

---

## License & Attribution

This project is provided as-is. Add a license file (e.g., MIT) to your repo if you want to grant explicit permissions.

---

## Contact

If you want help tuning thresholds, adding unit tests, or converting output to CSV/web UI, open an issue or contact the author via the repo.

---

*Generated README for the improved phishing URL scanner â€” copy this into `README.md` in your GitHub repository.*