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
## 📸 Screenshots

### 1.Splunk-cli-log-sim
![image](https://github.com/user-attachments/assets/db258e74-0332-4c1c-a9f2-6d30b075194a)

### 2.Login on local UI
![Splunk Enterprise Setup & Login via host](screenshots/Splunk%20Enterprise%20Setup%20%26%20Login%20via%20host.png)
![image](https://github.com/user-attachments/assets/a67efd82-456d-49a7-b37c-487ecd2bca81)
![Splunk](screenshots/Splunk%20.jpg)
### Splunk Web Interface – Boot to Login Workflow
Displays the sequence from launching the Splunk Web interface after boot to a successful login.
This confirms:
- Splunk Web UI is accessible from the host browser.
- Port forwarding is working correctly (`127.0.0.1:8000`).
- Admin credentials were successfully used to log in.

### 3.Log Ingestion in Splunk UI
![image](https://github.com/user-attachments/assets/ba3c2ce8-c246-42d7-9511-c1a515ad5dab)
### Splunk Web Interface – Successful Login & Log Ingestion Verified
This confirms:
- Test logs were successfully ingested into Splunk.
- Ingested events are searchable via the Splunk UI.
- Port forwarding is functioning as expected.
- The data pipeline from syslog (/var/log/auth.log) to the Splunk index is operational.

### Host - splunk-server VM
![image](https://github.com/user-attachments/assets/8b2f88b3-05d1-4f19-a59f-cdca0269b93a)
Host: hostname, ip address, or fully qualified domain name of machine from which event originated
![image](https://github.com/user-attachments/assets/ffbd2da3-60b6-480f-859f-5ed565ce933d)
Sources: File/directory path, network path or script where event originate from
![image](https://github.com/user-attachments/assets/5dab44bb-dbe3-429c-bc37-62aa1e86f501)
Sourcetypes: Classification of data



## 🔗 Author

[Hafiz Daud](https://www.linkedin.com/in/muhdhafizdaud/)


