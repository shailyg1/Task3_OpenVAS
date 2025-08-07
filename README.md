#  Task 3 - Vulnerability Assessment Report

This report documents the results of a basic vulnerability assessment performed on a Kali Linux machine as part of a cybersecurity internship task. Multiple industry-recognized tools were used to detect system misconfigurations, malware, rootkits, intrusion attempts, and web vulnerabilities.

---

##  Tools Used and Purpose

| Tool        | Purpose                                                                 |
|-------------|-------------------------------------------------------------------------|
| **Lynis**      | System auditing and hardening recommendations                         |
| **Nmap**       | Port scanning and vulnerability detection on localhost                |
| **Nikto**      | Web server vulnerability scanner (127.0.0.1)                          |
| **ClamAV**     | Malware, trojans, and virus detection                                 |
| **Rkhunter**   | Rootkit and hidden exploit detection                                  |
| **CrowdSec**   | Real-time behavior-based intrusion detection using logs               |

---

##  Results Summary

###  Lynis Report (`lynis_report.txt`)
- Detected 6 warnings including:
  - Weak permissions on `/etc/sudoers.d`
  - Improper DNS redundancy
  - Empty firewall ruleset
  - Long crypto test execution time
  - Weak home directory permissions

Command used:
```bash
sudo lynis audit system > lynis_report.txt
grep -i warning lynis_report.txt
```

---

###  Nmap Report (`nmap_report.txt`)
Performed a vulnerability scan against localhost (`127.0.0.1`):

- Open port: **5432/tcp** (PostgreSQL)
- No known vulnerabilities from `--script vuln`

Command used:
```bash
sudo nmap -sV --script vuln 127.0.0.1 > nmap_report.txt
```

---

###  Nikto Report (`nikto_report.txt`)
- Web server vulnerability scan using `Nikto`
- If Apache wasn't active, result might be empty (0 hosts tested)
- Optionally started Apache2 for accurate testing

Command used:
```bash
sudo systemctl start apache2
nikto -h http://127.0.0.1 > nikto_report.txt
```

---

###  ClamAV Report (`clamav_report.txt`)
- Full system malware scan of `/home` directory
- Flags suspicious/malicious files like trojans, backdoors, etc.

Command used:
```bash
sudo freshclam
sudo clamscan -r /home > clamav_report.txt
```

---

###  Rkhunter Report (`rkhunter_report.txt`)
- Detected hidden files, suspicious binaries, and rootkit-like behavior

Command used:
```bash
sudo rkhunter --update
sudo rkhunter --check --report-warnings-only > rkhunter_report.txt
```

---

###  CrowdSec Report (`crowdsec_alerts.txt`)
- Real-time detection of brute-force, SSH, and suspicious log-based attacks
- Alert and decision listing

Command used:
```bash
sudo systemctl start crowdsec
sudo cscli alerts list > crowdsec_alerts.txt
sudo cscli decisions list >> crowdsec_alerts.txt
```

---

##  Files Attached

```
task-3/
├── clamav_report.txt
├── crowdsec_alerts.txt
├── lynis_report.txt
├── nikto_report.txt
├── nmap_report.txt
├── rkhunter_report.txt
└── README.md
```

---

##  Conclusion

This assessment provides a basic yet robust overview of the system's security posture. These tools help identify potential vulnerabilities and misconfigurations to improve the system's hardening and response posture.

>  Always run security scans regularly and keep your tools updated.
