🗓️ 7-Day SIEM (Splunk Enterprise) Home Lab Plan
✅ Day 1 – Environment & Splunk Setup
Created a VirtualBox VM using Ubuntu Server 24.04 ISO.

Installed Splunk Enterprise on the VM.

Configured Splunk admin credentials via user-seed.conf.

Enabled port forwarding:

2222 → 22 for SSH

8000 → 8000 for Splunk Web

Verified Splunk services using CLI:

splunk status, netstat, ufw status, etc.

Enabled UFW (firewall) and allowed only necessary ports (22, 8000).

Set Splunk to auto-start at boot.

SSH access from host using PowerShell.

Created and applied inputs.conf to monitor /var/log/auth.log.

Simulated log events with logger.

Verified log ingestion via Splunk Web UI (index=* source="/var/log/auth.log").

Documented and committed results to GitHub.

📌 Day 2 – Ingest More Log Sources
Add /var/log/syslog and /var/log/kern.log to inputs.conf.

Create separate indexes for different log types.

Visualize event types in Splunk Web (Search > Stats > Timechart).

Document setup and ingestion screenshots.

📌 Day 3 – User & Permission Management
Explore user roles in Splunk.

Create a limited analyst user with search role.

Demonstrate RBAC (Role-Based Access Control) via dashboards/searches.

Add screenshots of role config.

📌 Day 4 – SPL Practice & Threat Use Cases
Learn core SPL (stats, table, where, eval).

Use queries to:

Detect brute force login

Show login failures by IP

Save searches and create simple dashboards.

📌 Day 5 – Alerts & Dashboards
Convert searches into scheduled alerts.

Set up trigger actions (email/log/etc).

Build a basic SOC dashboard (failed logins, top source IPs).

Export and commit dashboard JSON to GitHub.

📌 Day 6 – Simulate More Attack Scenarios
Simulate:

SSH brute force

Unauthorized file access

Port scans

Tag events with custom sourcetypes.

Tune detection rules in Splunk.

📌 Day 7 – Documentation & Cleanup
Write a final reflection in README.md:

What was learned

What can be improved

Organize repo folders:

/screenshots

/configs

/docs

Create a PDF or printable summary.

