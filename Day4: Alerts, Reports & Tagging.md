# **Day 4: Alerts, Reports and Tagging**

On Day 4, we simulated SSH failures, created a scheduled report, built a real-time alert, and tested it. Below are the detailed steps for Parts 1–5.
1. Verify Data Ingestion

    SSH into the Ubuntu VM (if not already):

ssh -p 2222 hafiz@localhost

Check Splunk status:

sudo /opt/splunk/bin/splunk status

    If Splunk is not running, start it:

    sudo /opt/splunk/bin/splunk start --accept-license

Confirm /var/log/auth.log events are indexed.
In Splunk Web (http://localhost:8000), go to Search & Reporting and run:

    index=main sourcetype=linux_secure | head 10

        If you see recent SSH/login entries, ingestion is working.

        Screenshot: day4/screenshots/auth_indexing.png

2. Simulate Additional SSH Events

Generate enough “Failed password” entries to test downstream alerts and reports.

    On the Ubuntu VM, run:

for i in {1..10}; do
  logger -p authpriv.warning \
    "sshd[9999]: Failed password for invalid user attacker from 192.168.56.$i port $((2000 + i)) ssh2"
  sleep 1
done

    This produces 10 “Failed password” lines, each from a different fake IP.

(Optional) Log a successful SSH attempt for control:

logger -p authpriv.info \
  "sshd[9999]: Accepted password for hafiz from 192.168.56.50 port 2222 ssh2"

In Splunk Web → Search & Reporting, verify these new events:

    index=main sourcetype=linux_secure "Failed password"

        Screenshot: day4/screenshots/auth_search.png

3. Create a Scheduled Report (“Failed SSH Attempts – Daily”)

Count failed SSH attempts by source IP over the last 24 hours, then schedule it to run daily.

    In Splunk Web → Search & Reporting → New Search, enter:

index=main sourcetype=linux_secure "Failed password"
| rex field=_raw "from\s+(?<src_ip>\d+\.\d+\.\d+\.\d+)"
| stats count AS failures BY src_ip
| table src_ip failures

    The rex extracts src_ip from each event; without it, stats BY src_ip returns no rows.

Click Save As → Report:

    Report Name: Failed SSH Attempts – Daily

    Description: “Counts of failed SSH logins by IP, once per day.”

    Permissions: “Everyone” (or “App (search)”).

Under Schedule, select Run Once Daily and set time to 23:59.

    Click Schedule → confirm the cron expression:

        23 59 * * *

    Click Save.

    Go to Settings → Searches, Reports, and Alerts, find Failed SSH Attempts – Daily, and confirm “Next Scheduled” shows 23:59.

        Screenshot: day4/screenshots/report_list.png

4. Build a Real-Time Alert (“SSH Failure Spike”)

Trigger an alert when any single IP logs more than 5 “Failed password” events in a 5-minute window.

    In Search & Reporting → New Search, enter:

    index=main sourcetype=linux_secure "Failed password"
    | rex field=_raw "from\s+(?<src_ip>\d+\.\d+\.\d+\.\d+)"
    | bucket _time span=5m
    | stats count AS failures BY src_ip, _time
    | where failures > 5
    | table _time src_ip failures

        Explanation:

            rex extracts src_ip.

            bucket _time span=5m groups events into 5-minute buckets.

            stats count AS failures BY src_ip, _time counts failures per IP per bucket.

            where failures > 5 filters buckets where count > 5.

            table _time src_ip failures ensures those fields appear in Statistics.

    Click Save As → Alert:

        Alert Name: SSH Failure Spike

        Description: “Trigger when any IP logs > 5 failed SSHs in 5 minutes.”

        Alert Type: Real-time (streaming).

        Trigger Condition: Per Result.

        Throttle: (Optional) Throttle on src_ip for 30 minutes to reduce duplicates.

    Under Alert Actions, enable:

        Send Email (if configured).

        Add to Triggered Alerts (default).

        Log Event: Check “Write to index” and choose main.

    Click Save.

    Go to Settings → Searches, Reports, and Alerts, verify SSH Failure Spike appears with Type = Real-time.

        Screenshot: day4/screenshots/alert_list.png

5. Test the Real-Time Alert

Generate 6 “Failed password” events from the same IP to trigger the alert.

    On the Ubuntu VM, run:

    for i in {1..6}; do
      logger -p authpriv.warning \
        "sshd[9999]: Failed password for invalid user hacker from 192.168.56.99 port $((3000 + i)) ssh2"
      sleep 1
    done

    Within one minute, in Splunk Web → Activity → Triggered Alerts, verify:

        There is an entry for SSH Failure Spike with src_ip=192.168.56.99.

        Screenshot: day4/screenshots/alert_fired.png

Directory Structure for Day 4

day4/
├── README.md
├── configs/
│   ├── tags.conf
│   ├── eventtypes.conf
│   ├── failed_ssh_daily_report.savedsearch
│   └── ssh_failure_spike_alert.savedsearch
└── screenshots/
    ├── auth_indexing.png
    ├── auth_search.png
    ├── report_list.png
    ├── alert_list.png
    └── alert_fired.png

    tags.conf and eventtypes.conf will be added in Part 6.

    Screenshots from Parts 1–5 should be saved under day4/screenshots/ with the indicated filenames.

Next Steps

Proceed to Part 6: Tag Events & Define Event Types, then Part 7: (Optional) Transaction-Based Correlation.




















### **[Screenshots]**

**Simulation of SSH events**
![image](https://github.com/user-attachments/assets/c0bce5b6-75a5-4fab-8d1c-e1e2c1ae2b1c)
**Verfication of events via Search - index=main sourcetype=linux_secure "Failed password"**
![image](https://github.com/user-attachments/assets/9ccb65f4-0cc3-49dc-a5f4-9cfdb3cd14cd)
#### **Failed Password aggregated by different IP**
![image](https://github.com/user-attachments/assets/2501f412-b16a-4fa1-805b-82efd609d7dc)
#### **Sorted by Statistics**
![image](https://github.com/user-attachments/assets/6b272e1b-010f-4e0a-af21-0197704984bc)
**Creating Report**
![image](https://github.com/user-attachments/assets/cdfe2875-b8f5-4367-bf9c-e2872335c226)
![image](https://github.com/user-attachments/assets/ac2e080e-13b6-48cd-b71e-2083c6aab400)
![image](https://github.com/user-attachments/assets/e50065a3-7776-489b-993d-ee91a6c7104f)
![image](https://github.com/user-attachments/assets/d2c9656f-0a4d-411f-affc-f3c8fd7778c0)
![image](https://github.com/user-attachments/assets/9642bbf0-6236-4fb3-a159-08848bb06aee)
**Simulation of more Failed Password**
![image](https://github.com/user-attachments/assets/c94d47b4-7c5d-48d5-8a94-bc1ddeaad62e)
**And more loggers**
![image](https://github.com/user-attachments/assets/a831e995-31b1-4a38-917f-c84af4d43136)
**[Part 4: Building Time Alert]**
#### **To pull the src_ip out**
![image](https://github.com/user-attachments/assets/010a94e7-f1f1-45f3-8cf5-77280d582fd1)
![image](https://github.com/user-attachments/assets/5d6894bb-9d88-4b99-a7ea-7be297e2f29d)

![image](https://github.com/user-attachments/assets/bb19278a-f6d1-45b4-baa5-51409dab5c1e)
**Created Alert**
![image](https://github.com/user-attachments/assets/2607ac79-a481-4a7a-a4a6-9d82e362fc79)

![image](https://github.com/user-attachments/assets/c9a0acc2-c1e3-48a0-bc5d-4e61cbc79cdc)
![image](https://github.com/user-attachments/assets/4ff636ea-57c6-40fd-a4db-f3fd5696b0a2)
![image](https://github.com/user-attachments/assets/4138e46c-6fec-4f83-8420-5737c08876fd)
