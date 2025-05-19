# Day 1–2 – SIEM (Splunk Enterprise) Home Lab Setup

This Day 1–2 lab sets up a working Splunk Enterprise environment inside a VirtualBox-hosted Ubuntu Server. It simulates log ingestion and prepares a foundation for hands-on SOC analyst tasks.

---

## ✅ Environment Setup

- Created a virtual machine using **Ubuntu Server 24.04 ISO** in VirtualBox.
- Installed **Splunk Enterprise** on the VM.
- Configured admin credentials using `user-seed.conf`.
- Logged into the **Splunk Web UI** from the host machine.

---

## 🔁 Port Forwarding Configuration

To enable smooth workflow and local web access:

- Forwarded host port `8000` to VM port `8000` (Splunk Web).
- Forwarded host port `2222` to VM port `22` (SSH).

**Allowed:**
- SSH access via PowerShell (for easier command input).
- Browser access to `http://127.0.0.1:8000` to reach the Splunk UI.

---

## 🔐 Security Configuration

- Enabled **UFW (Uncomplicated Firewall)** on Ubuntu VM.
- Allowed only the necessary ports:
  - `22/tcp` – SSH  
  - `8000/tcp` – Splunk Web UI  
- Verified firewall status and rule effectiveness.

---

## 📦 Log Forwarding & Ingestion

- Created `inputs.conf` to monitor `/var/log/auth.log`.
- Restarted Splunk to apply the configuration.
- Simulated logs using the `logger` command.
  - Example: `Suspicious login attempt from 192.168.x.x`
- Verified ingestion inside **Splunk Web Search** using:
  ```spl
  index=* source="/var/log/auth.log"



```
## Port Forwarding Setup
+------------------------+      +------------------------------+
| Host (Windows)         |      | Ubuntu Server VM (Splunk)   |
|                        |      |                              |
| PowerShell + Browser   | <--> | NAT Interface                |
|                        |      | 2222→22  | 8000→8000         |
| 127.0.0.1:8000         | <--> | Splunk Web UI (8000)         |
+------------------------+      +------------------------------+
```
Screenshots
1. Splunk Enterprise Setup & Login via Host Browser
Displays the sequence of launching the web interface after boot and login.

Splunk Web UI is accessible from the host browser.

Port forwarding is working (127.0.0.1:8000).

Admin credentials were successfully used.


2. Splunk Web Interface – Successful Login & Log Ingestion Verified
This confirms:

Test logs were successfully ingested into Splunk.

Ingested events are searchable via the Splunk UI.

Port forwarding is functioning as expected.

The data pipeline from syslog (/var/log/auth.log) to the Splunk index is operational.

![Log Ingestion](screenshots/Log Ingestion in Splunk UI Successful.jpg)

3. CLI – Log & Port Monitoring
Verified port 8000 is open and Splunk is listening.

Confirmed logs were ingested using tail and logger.


4. Host – splunk-server VM
Shows that Splunk recognizes the VM as a data source.

![Host – splunk-server](screenshots/Host - splunk-server VM.jpg)

5. Source – /var/log/auth.log
Validated the log file being ingested.

![Source](screenshots/Host - var log auth.jpg)

6. Sourcetype – syslog
Classified log type in Splunk UI.

![Sourcetype](screenshots/Source - syslog.jpg)



