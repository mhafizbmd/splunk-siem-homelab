# **Day 5: Dashboards, Visualization & Tagging**
### Actions
- Spun up a new dashboard **SSH Login Insights** with two panels:
  - **SSH Events per Hour** (hourly count of all SSH activity)  
  - **Top 10 Failed Login IPs** (bar chart of most frequent failure sources)  
- Enabled **auto-refresh** (1 min) so data stays live without manual reload  
- Defined a new event type **ssh_success_login** to group “Accepted password” events  
- Tagged `eventtype=ssh_success_login` for easy lookup in future searches  

### Lessons learned
- Dashboards turn raw searches into an easy at-a-glance monitoring tool, use the appropriate panel type 
- Unable to use Search query below as there were no IP field
  - index=main sourcetype="linux_secure" "Failed password" | top limit=10 src_ip AS “Source IP”
- Refined Search query to extract IP
  - index=main sourcetype="linux_secure" "Failed password" | rex field=_raw "from (?<src_ip>\d{1,3}(?:\.\d{1,3}){3})" | stats count AS Failures by src_ip | sort -Failures | head 10 | rename src_ip AS "Source IP"
- Tagging both failures and successes in dashboard makes comparisons and ratios easier   
- Starting with simple visuals paves the way for more advanced panels later



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

### Screenshots
- #### Dashboards, Visualization
![image](https://github.com/user-attachments/assets/e982d22f-6412-4e9b-9479-55bed04190af)
![image](https://github.com/user-attachments/assets/516be35b-18c0-43b7-a3bc-a454d34b8f5d)

index=main sourcetype="linux_secure" "Failed password" | top limit=10 src_ip AS “Source IP”  No Result![image](https://github.com/user-attachments/assets/1f562155-402f-4282-916e-0e482ea5a51c) 

Checked the events, it was working. Issue likely due to there being no IP in the field on the left ![image](https://github.com/user-attachments/assets/2f87c69d-681f-4a64-9634-cbcbe3881263)
Use a regular expression to pull the IP out of _raw each time its run. rex grabs whatever looks like 1.2.3.4 after the word “from” and saves it as src_ip while stats count by src_ip gives the top failure sources![image](https://github.com/user-attachments/assets/dd77efe1-3604-44c2-8711-9ae266731158)

Edited the Search query for "Top 10 Failed Login IPs" ![image](https://github.com/user-attachments/assets/82820ff6-5936-4674-ace2-251165c9684c)
Both Panel![image](https://github.com/user-attachments/assets/4253a026-f9a9-475d-9b3b-5fd9d91517e0)
- #### Tagging
Create new event type![image](https://github.com/user-attachments/assets/4996c7a8-2f68-4133-bd18-0d16a7df0d94)
Create tag and tagging it to the earlier event![image](https://github.com/user-attachments/assets/ca28708f-4ccb-4561-887c-12bdeadbce33)
ssh_success_login successfully returned password accepted![image](https://github.com/user-attachments/assets/3f495ad0-367d-42f9-8c75-7fd6149edc0a)








