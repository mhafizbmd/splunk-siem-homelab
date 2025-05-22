
# 🔐 SIEM Home Lab – Splunk on Ubuntu (2025)

A hands-on SIEM lab project to simulate and analyze security events using **Splunk Enterprise** on an **Ubuntu Server 24.04 VM**. This lab demonstrates fundamental blue team skills including log ingestion, parsing, search queries (SPL), and simulated alert creation.

---

## 📌 Overview

This home lab was created as part of my 2025 cybersecurity learning roadmap to reinforce blue team concepts and build practical experience in working with SIEM platforms. The lab is built using Splunk Enterprise Free and runs on a local **VirtualBox** VM using **Ubuntu Server**.

---

## 🛠️ Tools & Technologies

- **Splunk Enterprise Free** (Standalone)
- **Ubuntu Server 24.04 LTS**
- **VirtualBox** (NAT + Port Forwarding)
- **UFW** (Firewall)
- **Logger / Simulated SSH logs**
- **SPL (Search Processing Language)**
- **GitHub** for documentation & screenshots

---

## 🎯 Lab Goals

- ✅ Install and configure Splunk on Ubuntu
- ✅ Ingest Linux system logs (e.g., `/var/log/auth.log`)
- ✅ Simulate SSH brute-force attempts using `logger`
- ✅ Create and use SPL queries for log search
- ✅ Practice field extractions (regex)
- ✅ Explore correlation, dashboards, and alerts
- ✅ Document setup and learning process publicly

---

## ⚙️ Lab Setup Summary

- **VM OS:** Ubuntu Server 24.04
- **Hostname:** `splunk-server`
- **Port Forwarding:**
  - SSH: `Host 2222 → Guest 22`
  - Splunk Web: `Host 8000 → Guest 8000`
- **Installed:** OpenSSH, Splunk Enterprise
- **Firewall:** UFW enabled for ports 22 and 8000 only
- **Access:** SSH from host, Splunk Web from browser (`localhost:8000`)

---

## 📅 7-Day Plan (Progress Tracker)

### ✅ Day 1–2: Setup & Initial Log Ingestion
- Installed Ubuntu Server in VirtualBox
- Created user `hafiz`, updated and secured OS
- Installed Splunk Enterprise via CLI
- Enabled Splunk to start at boot
- Created `user-seed.conf` for admin login
- Enabled UFW: allowed SSH and Splunk port
- Configured `inputs.conf` to monitor `/var/log/auth.log`
- Simulated SSH brute force with `logger`
- Verified logs ingested into Splunk Web UI

### ✅ Day 3: SPL Basics & Field Extraction
- Practiced SPL queries: `index=main`, `stats count`, `top`, `timechart`
- Extracted fields using `rex`:
  ```spl
  index=main "Failed password"
  | rex "Failed password for (invalid user )?(?<username>\w+) from (?<src_ip>\d+\.\d+\.\d+\.\d+)"
  | stats count by username, src_ip
  ```

### ⏳ Day 4: Alert Simulation & Dashboards
- Simulate SSH attack variations using `logger`
- Create dashboard panels for failed login trends
- Use time filters to analyze brute force patterns

### ⏳ Day 5: Correlation Rules (Simulated)
- Simulate multiple suspicious activities from various sources
- Experiment with alert thresholds using SPL
- Practice creating correlation rules

### ⏳ Day 6: Linux Forensics + Log Sources
- Ingest additional logs (`/var/log/syslog`, `/var/log/sudo.log`)
- Simulate command execution patterns
- Monitor sudo attempts and privilege escalation indicators

### ⏳ Day 7: Review + GitHub Push
- Finalize documentation and cleanup
- Push logs, configs, screenshots to GitHub
- Write project summary and next steps

---

## 📂 Repo Structure

```
splunk-siem-homelab/
├── README.md
├── /docs
│   └── setup_instructions.md
├── /logs
│   └── sample_auth.log
├── /screenshots
│   └── day1_setup.png
│   └── day3_spl_query.png
├── /configs
│   └── inputs.conf
│   └── user-seed.conf
│   └── ufw_rules.txt
```

---

## 💡 How to Replicate This Lab

1. Install VirtualBox and Ubuntu Server 24.04
2. Configure NAT and port forwarding (2222 for SSH, 8000 for Splunk)
3. Install Splunk:
   ```bash
   sudo dpkg -i splunk_package.deb
   sudo /opt/splunk/bin/splunk enable boot-start
   sudo /opt/splunk/bin/splunk start
   ```
4. Create `inputs.conf` in `/opt/splunk/etc/system/local` to monitor logs
5. Simulate log events:
   ```bash
   logger "Failed password for invalid user admin from 192.168.1.100"
   ```
6. Use SPL to extract fields and build dashboards
7. Document your learning journey

---

## 🚀 Future Enhancements

- Add Sysmon or Windows logs (via forwarding)
- Integrate threat intelligence sources
- Build custom correlation rules
- Deploy Elastic Stack or compare with other SIEMs
- Explore cloud-based Splunk options

---

## 📣 Acknowledgements

This lab is part of my 2025 Cybersecurity Roadmap. Inspired by hands-on learning principles and blue team training via Security Blue Team and community resources.

---
