# **Day 5: Dashboards, Visualization & Tagging**

On Day 4, we will be simulating SSH failures, created a scheduled report, built a real-time alert, and tested it.

### Actions
- **Event type**: defined `ssh_failed_login` to capture all “Failed password” SSH events  
  _(search: `index=main sourcetype="linux_secure" "Failed password"`)_
- **Scheduled report**: saved “Daily SSH Failures” to run at 00:00 daily, showing the count of failed logins over the past 24 hours  
- **Real-time alert**: created “SSH Failure Threshold” (trigger when > 5 failures in 5 min) with email action  
- **Test run**: simulated 10 failed SSH attempts via `logger` in the VM to validate alert firing  
- **Verification**: confirmed the alert under **Activity → Triggered Alerts** in Splunk Web  
- **Documentation**: updated `Day4: Alerts, Reports & Tagging.md` on GitHub with these configs and outcomes  

### Lessons I learnt for today
- Automated reports give a quick daily snapshot—no manual searches needed  
- Threshold tuning is crucial: too low → noise; too high → missed spikes  
- Real-time alerts work great, but always test with simulated data first  
- Clear naming (event types & alerts) pays off when scaling to dashboards later  




### Comprehensive Steps
Prerequisites
• Splunk Enterprise running at http://localhost:8000
• SSH logs (/var/log/auth.log, sourcetype linux_secure) ingested and searchable

Plan Your Dashboard
• Name: SSH Login Insights (visualize overall SSH volume & top sources)

Create a New Dashboard
• In Splunk Web → Dashboards → Create New Dashboard
• Title: SSH Login Insights
• ID: ssh_login_insights
• App Context: search
• Permissions: Shared in App
• Click Create

Add Panel 1: Login Volume Over Time
• Search:
index=main sourcetype="linux_secure"
| timechart span=1h count AS “Total SSH Events”
• Visualization: Line Chart
• Panel Title: SSH Events per Hour
• Save

Add Panel 2: Top SSH Failure Sources
• Search:
index=main sourcetype="linux_secure" "Failed password"
| top limit=10 src_ip AS “Source IP”
• Visualization: Bar Chart
• Panel Title: Top 10 Failed Login IPs
• Save

Configure Auto-Refresh
• In dashboard, click Edit → Dashboard Settings
• Auto-Refresh: 1m
• Save

Define & Tag a New Event Type
• In Splunk Web → Settings → Event types → New Event Type
– Name: ssh_success_login
– Search: index=main sourcetype="linux_secure" "Accepted password"
– App: search
– Save
• In Splunk Web → Settings → Tags → New Tag
– Tag: ssh_success_login
– Type: Event type
– Value: ssh_success_login
– Save

Test Your Tagging
• Run: index=main sourcetype="linux_secure" tag::ssh_success_login=*
• Verify events with “Accepted password” are tagged

Export Dashboard Definition
• Dashboards → ⋮ on SSH Login Insights → Export to JSON
• Save as ssh_login_insights.json
