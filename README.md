🛡️ SOC Alert Intel Hub
> **Enrich security alerts with OSINT. Generate incident reports. Produce shift-log-ready documentation — automatically.**
![Python](https://img.shields.io/badge/Python-3.9%2B-blue?logo=python)
![VirusTotal](https://img.shields.io/badge/API-VirusTotal-394EFF?logo=virustotal)
![AbuseIPDB](https://img.shields.io/badge/API-AbuseIPDB-red)
![MITRE ATT&CK](https://img.shields.io/badge/Framework-MITRE%20ATT%26CK-orange)
![License: MIT](https://img.shields.io/badge/License-MIT-green)
---
What it does
SOC Alert Intel Hub simulates the L1 analyst enrichment workflow — the first thing a SOC analyst does after receiving an alert from a SIEM like Microsoft Sentinel or Splunk: extract IOCs, check reputation sources, tag with MITRE ATT&CK techniques, and document findings.
Input	Output
JSON or CSV alert export from any SIEM	`report.html` — full incident report per alert
Raw alert descriptions with IPs, URLs, hashes	`shift_log.txt` — structured shift documentation
---
Features
IOC Extraction — auto-parses IPs, URLs, and file hashes from alert descriptions using regex
VirusTotal Enrichment — IP reputation, URL scan results, file hash malware scoring (free-tier compatible: 4 req/min rate limiting built in)
AbuseIPDB Enrichment — abuse confidence score, ISP, country, TOR node detection
MITRE ATT&CK Auto-Tagging — keyword-based technique mapping (T1078, T1110, T1021, T1071, T1566, and more)
HTML Incident Report — color-coded by severity, tabular IOC display, enrichment summary per alert
Shift Log Generator — plain-text, sorted by severity, ready for SIEM or ticketing system paste
---
Quickstart
```bash
# 1. Clone
git clone https://github.com/vinith-sec/soc-hub.git
cd soc-hub

# 2. Install dependencies
pip install -r requirements.txt

# 3. Set API keys (optional — tool runs in demo mode without them)
export VT_API_KEY="your_virustotal_free_api_key"
export ABUSEIPDB_KEY="your_abuseipdb_api_key"

# 4. Run against sample alerts
python soc_hub.py sample_alerts.json

# 5. Skip API calls for demo
python soc_hub.py sample_alerts.json --no-vt --no-abuseipdb
```
Output:
```
[SOC Alert Intel Hub] Loading alerts from sample_alerts.json
[✓] Loaded 5 alerts

[*] Starting OSINT enrichment pipeline...
[1/5] Enriching: ALT-2025-001 — Suspicious Outbound DNS Query to Known C2 Domain
[2/5] Enriching: ALT-2025-002 — Brute Force Attack — Entra ID Sign-In Failures
...

[✓] HTML report saved → report.html
[✓] Shift log saved  → shift_log.txt
[✓] Done. Processed 5 alerts.
```
---
Input format
JSON (`sample_alerts.json` included):
```json
[
  {
    "alert_id":    "ALT-2025-001",
    "title":       "Suspicious Outbound DNS Query to Known C2 Domain",
    "severity":    "high",
    "timestamp":   "2025-04-01T02:14:33Z",
    "source":      "Microsoft Sentinel",
    "description": "Host WKSTN-042 issued DNS queries to fastflux-cdn.ru. External IP 185.220.101.47 ..."
  }
]
```
CSV (export from Sentinel, Splunk, QRadar):
```
alert_id,title,severity,timestamp,source,description
ALT-001,Brute Force Detected,high,2025-04-01T03:00:00Z,Splunk,"220 failed logins from 91.108.4.18..."
```
---
CLI options
```
usage: soc_hub.py [-h] [--no-vt] [--no-abuseipdb] [--report REPORT] [--log LOG] [--analyst ANALYST] input

positional arguments:
  input              Path to alerts JSON or CSV file

optional arguments:
  --no-vt            Skip VirusTotal API calls
  --no-abuseipdb     Skip AbuseIPDB API calls
  --report REPORT    Output HTML report path (default: report.html)
  --log LOG          Output shift log path (default: shift_log.txt)
  --analyst ANALYST  Analyst name for shift log header
```
---
MITRE ATT&CK coverage
Technique	Description	Detection keywords
T1078	Valid Accounts	credential, login, authentication
T1110	Brute Force	brute force, password spray
T1021	Remote Services	lateral movement, SMB, RDP
T1071	C2 Communication	beacon, C2, command and control
T1566	Phishing	phishing, malicious email
T1059	Command Interpreter	PowerShell, cmd, script
T1053	Scheduled Task	persistence, cron
T1486	Data Encrypted for Impact	ransomware, encrypt
---
Free API keys

API	Free tier	Link
VirusTotal	4 requests/min, 500/day	virustotal.com
AbuseIPDB	1,000 checks/day	abuseipdb.com
---
Project structure
```
soc-hub/
├── soc_hub.py          # Main enrichment + report engine
├── sample_alerts.json  # 5 realistic demo alerts
├── requirements.txt
└── README.md
```
---
Author
Vinith Kumaragurubaran — SOC Analyst | CompTIA Security+ | Cisco CCNA  
linkedin.com/in/vinith-kumaragurubaran · github.com/vinith-sec
