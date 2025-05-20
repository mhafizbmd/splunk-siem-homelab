## Day 3 – SPL Fundamentals & Field Extraction

Today focused on mastering core SPL searches, filtering log events, and performing basic field extraction.

Actions:
- Explored logs in `/var/log/auth.log`
- Filtered login failure attempts
- Used `table`, `stats`, `top` to summarize key fields
- Extracted attacker IPs and usernames using `rex`
- Documented results with screenshots

Goal:
- Understand and use core SPL commands to analyze logs.
- Extract meaningful fields from raw log data.
- Build a foundation for threat detection and correlation.

Tools Used:
- Splunk Web UI
- SPL (Search Processing Language)

Training Reference:
- Splunk Search Fundamentals I (via Splunk Education)

## Comprehensive Steps

### ✅ Part 1: Explore Ingested Logs in Splunk
- Log into Splunk Web: Access via: http://127.0.0.1:8000
- Use your admin credentials set during user-seed.conf setup.
- Navigate to Search & Reporting
- App > Search & Reporting > New Search
- Run a Basic Search:
spl
Copy
Edit
index=* OR index=main
- This shows all events — a good place to confirm ingestion.

### ✅ Part 2: Learn SPL Basics
- Search by Source:
spl
Copy
Edit
source="/var/log/auth.log"
- Filter Specific Keywords:
spl
Copy
Edit
source="/var/log/auth.log" "Failed password"
- Add Time Filters
Use the time range selector in the top right (e.g., Last 60 minutes).

### ✅ Part 3: Understand Fields: host, source, sourcetype
These are Splunk’s default fields:

Field	What it Represents
host	Name/IP of the system sending logs
source	The file/device logs are ingested from
sourcetype	Format/type of data (e.g., syslog, csv, etc.)

Run this:
spl
Copy
Edit
index=* | stats count by host, source, sourcetype
This shows which logs are coming from where.

### ✅ Part 4: Field Extraction and Filtering
7. Explore Automatic Fields
On the left pane, Splunk lists fields it auto-detects.

Click any field (e.g., user, pid, message) to filter logs.

8. Use fields Command
spl
Copy
Edit
index=* source="/var/log/auth.log" | fields _time, host, message
9. Table View:
spl
Copy
Edit
index=* source="/var/log/auth.log" | table _time, user, message
Use this to structure logs into a readable format.

### ✅ Part 5: Start Using SPL Filters
10. Search for SSH login failures:
spl
Copy
Edit
source="/var/log/auth.log" "Failed password"
11. Show top usernames that failed login:
spl
Copy
Edit
source="/var/log/auth.log" "Failed password" | top user
12. Count failed attempts by IP:
spl
Copy
Edit
source="/var/log/auth.log" "Failed password" | rex "from (?<ip>\d{1,3}(\.\d{1,3}){3})" | stats count by ip
This uses rex to extract IP addresses from the raw message.

### ✅ Part 6: Field Extraction with rex & regex
13. Use rex for custom field extraction
spl
Copy
Edit
index=* source="/var/log/auth.log" | rex "from (?<attacker_ip>\d{1,3}(\.\d{1,3}){3})"
Now you can use attacker_ip in further analysis.

14. Extract username from logs:
spl
Copy
Edit
index=* source="/var/log/auth.log" | rex "user (?<username>\w+)" | table _time, username, message
### ✅ Part 7: Document Your Work
Take screenshots of:
- Raw log search
- Table view with filtered fields
- Successful field extraction using rex

### Simulated some "SSH brute force", "SSH Login Success" and "Reconnaisance-like Attempts" logs ###
![image](https://github.com/user-attachments/assets/6e8e9c54-1331-4960-aabe-380c4a9c939e)
### Splunk Search for simulated logs
![image](https://github.com/user-attachments/assets/044ecee6-7bcf-4e90-9af5-41b205a7d5eb)
### Basic Search for "Failed password"
![image](https://github.com/user-attachments/assets/92053250-7353-40a3-997e-4db32dd9d4e8)
### Advanced SPL Search (Aggregation) 
1) To count login failures per IP/user. Helps detect brute force sources
index=main "Failed password" 
| rex "from (?<src_ip>\d+\.\d+\.\d+\.\d+)" 
| stats count by src_ip
2) 

![image](https://github.com/user-attachments/assets/66d75fd7-df89-4ba0-b0e7-7b53c8787a95)

![image](https://github.com/user-attachments/assets/a38a156a-206a-44d5-b04a-e8ffdd763c2a)


### Count by IP Address
To extract the IP and count how many failed attempts came from each:
1) rex: extracts the IP using regex into a field called src_ip
2) stats count by src_ip: gives you a count of events per IP.
3) To note: For (?<src_ip>\d+\.\d+\.\d+\.\d+)  /d = matched any digit (0-9) | + = Matches 1 or more of the preceding character (eg; 192.168.1.1)
![image](https://github.com/user-attachments/assets/92771e2a-b6ad-4e4b-880e-8b272bd0973b)






