# 7-Day SIEM (Splunk Enterprise) Home Lab Plan  
**Curated with the help of chatGPT to summarise**

[![Splunk](https://img.shields.io/badge/Splunk-Enterprise-green.svg)](https://www.splunk.com/)  
[![Ubuntu](https://img.shields.io/badge/Ubuntu-24.04-orange.svg)](https://ubuntu.com/)  
[![VirtualBox](https://img.shields.io/badge/VirtualBox-6.1-blue.svg)](https://www.virtualbox.org/)  
[![UFW](https://img.shields.io/badge/UFW-Firewall-blue.svg)](https://help.ubuntu.com/community/UFW)  
[![License](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)

A hands-on 7-day SIEM home lab using **Splunk Enterprise** on an **Ubuntu Server 24.04 VM**. This high-level overview sits at the repository root; each “Day X” has its own subdirectory with detailed steps, configurations, and screenshots.

---

## 📋 Table of Contents

1. [Overview](#overview)  
2. [Tools & Technologies](#tools--technologies)  
3. [Lab Goals](#lab-goals)  
4. [Lab Setup Summary](#lab-setup-summary)  
5. [7-Day Plan (High-Level)](#7-day-plan-high-level)  
6. [Repository Structure](#repository-structure)  
7. [How to Replicate](#how-to-replicate)  
8. [Future Enhancements](#future-enhancements)  
9. [Author](#author)

---

## **Overview**

This lab is part of a 2025 cybersecurity learning roadmap. Over seven days, you will:

- Deploy Splunk Enterprise Free on Ubuntu 24.04 in VirtualBox  
- Ingest Linux logs (e.g., `/var/log/auth.log`, `/var/log/sudo.log`, `/var/log/syslog`)  
- Simulate SSH brute-force and other threat behaviors via `logger`  
- Build searches, alerts, dashboards, and correlation rules in Splunk  
- Document every step publicly (GitHub) to reinforce blue team skills and SOC workflows  

---

## **Tools & Technologies**

Splunk Enterprise Free (Standalone)
Ubuntu Server 24.04 LTS
VirtualBox (NAT + Port Forwarding)
UFW (Firewall)
Logger (for simulated logs)
SPL (Splunk’s Search Processing Language)
GitHub (for documentation & screenshots)


---

## **Lab Goals**

- Deploy and configure Splunk on Ubuntu  
- Ingest and visualize Linux system logs  
- Simulate adversary activity (e.g., SSH brute force)  
- Create and schedule SPL searches, alerts, and reports  
- Design dashboards to monitor threat indicators  
- Implement basic correlation rules for multi-vector detection  
- Perform simple Linux forensics searches (e.g., `/etc/passwd` changes, sudo abuse)  
- Publish all configurations, scripts, and findings to GitHub  

---

## **Lab Setup Summary**



---

## **Lab Goals**

- Deploy and configure Splunk on Ubuntu  
- Ingest and visualize Linux system logs  
- Simulate adversary activity (e.g., SSH brute force)  
- Create and schedule SPL searches, alerts, and reports  
- Design dashboards to monitor threat indicators  
- Implement basic correlation rules for multi-vector detection  
- Perform simple Linux forensics searches (e.g., `/etc/passwd` changes, sudo abuse)  
- Publish all configurations, scripts, and findings to GitHub  

---

## **Lab Setup Summary**
VM OS: Ubuntu Server 24.04
Hostname: splunk-server

Network: VirtualBox NAT with Port Forwarding
• Host 2222 → Guest 22 (SSH)
• Host 8000 → Guest 8000 (Splunk Web)

Installed:
• OpenSSH
• Splunk Enterprise Free

Firewall (UFW):
• Allow 22/tcp (SSH)
• Allow 8000/tcp (Splunk Web)

Access:
• SSH from host: ssh -p 2222 hafiz@localhost
• Splunk Web from host: http://localhost:8000



---

## **7-Day Plan (High-Level)**

- **Day 1–2: Environment Setup & Log Ingestion**  
  Spin up Ubuntu 24.04 VM, install Splunk, configure UFW, set up `inputs.conf` for `/var/log/auth.log`, and verify ingestion.

- **Day 3: SPL Fundamentals & Field Extraction**  
  Learn basic SPL searches (`stats`, `top`, `timechart`), apply `rex` to extract fields from `/var/log/auth.log`, and save sample searches.

- **Day 4: Alerts, Reports & Tagging**  
  Define scheduled searches and real-time alerts (e.g., failed SSH threshold), tag event types (`ssh_failed_login`), and document in GitHub.

- **Day 5: Dashboard Design & Visualization**  
  Create dashboards (trendlines, tables, single-value indicators) linked to saved searches (“SSH Attack Monitor”), and capture screenshots.

- **Day 6: Simulated Threats & Detection Logic**  
  Ingest `/var/log/sudo.log` and `/var/log/syslog`, simulate sudo abuse and service restarts, build correlation searches (e.g., SSH failures + sudo events), and tune with `tstats`.

- **Day 7: Wrap-Up, Final Report & Sharing**  
  Complete any remaining Splunk eLearning/BTL1 modules, write a final summary, organize GitHub subdirectories (`day1/` … `day7/`), and share results.


## Notes:

- Lab conducted alongside:
  - Splunk Learning Course: https://education.splunk.com/
  - Security Blue Team BTL1 Certification

- VM and host communicate via VirtualBox NAT:
  - Port 2222 → 22 for SSH
  - Port 8000 → 8000 for Splunk Web access

- Simulated data is generated via `logger` in Ubuntu
- All key logs ingested from `/var/log/auth.log`
- Visual documentation placed under `/screenshots` in the GitHub repo

## 🔗 Author

[Hafiz Daud](https://www.linkedin.com/in/muhdhafizdaud/)
