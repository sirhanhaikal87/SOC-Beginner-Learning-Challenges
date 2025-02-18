# **Task #1: Analyze Apache Logs for Suspicious Access Attempts Using Splunk**  

## **Objective**  
The objective of this task is to help students **analyze Apache access logs in Splunk to detect suspicious access attempts**. By completing this task, students will learn how to identify **brute-force attempts, directory traversal attacks, SQL injection attempts, and unusual user agents** from web server logs.

---

## **Preparation: Ingest Apache Logs into Splunk**  

### **Requirements**  
- **Splunk Server:** Installed and running.  
- **Apache Web Server Logs:** (Access log file will be provided).  
- **Splunk Forwarder (Optional):** If ingesting logs from a remote web server.  

### **Step 1: Upload Apache Logs to Splunk**  
1. If logs are in a file format (e.g., `access.log`), upload them manually:  
   ```splunk
   | inputlookup apache_logs.csv
   ```
If logs are forwarded via Universal Forwarder, ensure they are indexed correctly by running:
splunk
Copy
Edit
index=web sourcetype=apache_access
Verify that logs contain key fields such as:
client_ip → Source IP address
request → HTTP request path
status → HTTP response code
user_agent → Browser or bot details
bytes_sent → Data sent in response
Investigation Steps: Detecting Suspicious Access Attempts
Step 1: Detect High Number of 404 Errors (Reconnaissance Attempts)
Run the following query to identify IPs repeatedly requesting non-existent pages:

splunk
Copy
Edit
index=web sourcetype=apache_access status=404
| stats count by client_ip
| sort -count
Attackers often probe for admin panels or sensitive files.
Step 2: Detect Brute-Force Login Attempts
Run this query to identify repeated login attempts:

splunk
Copy
Edit
index=web sourcetype=apache_access request="/login"
| stats count by client_ip
| where count > 10
If an IP has too many login attempts, it might be a brute-force attack.
Step 3: Detect Directory Traversal Attacks
Run this query to find requests attempting to access system files:

splunk
Copy
Edit
index=web sourcetype=apache_access request="*../*" OR request="*../../*"
If found, it indicates an attempt to escape the web root directory.
Step 4: Detect SQL Injection Attempts
Run this query to find common SQL injection patterns in URLs:

splunk
Copy
Edit
index=web sourcetype=apache_access request="*UNION*" OR request="*SELECT*" OR request="*DROP*" OR request="*INSERT*"
Attackers may be trying to execute unauthorized SQL queries.
Step 5: Detect Suspicious User Agents
Run this query to identify unusual user agents (bots or scrapers):

splunk
Copy
Edit
index=web sourcetype=apache_access
| stats count by user_agent
| sort -count
Look for rare or missing user agents (e.g., curl, wget, or empty fields).
Step 6: Extract Top Attacking IPs
Run this query to list top offending IP addresses:

splunk
Copy
Edit
index=web sourcetype=apache_access status=403 OR status=401 OR status=404
| stats count by client_ip
| sort -count
These IPs can be checked against threat intelligence platforms like AbuseIPDB.
Conclusion
✅ Successfully ingested and analyzed Apache logs in Splunk.
✅ Detected brute-force attempts, directory traversal, SQL injection, and reconnaissance activity.
✅ Extracted top suspicious IPs and uncommon user agents for further investigation.
✅ Learned how SOC analysts use Splunk to monitor web server activity and detect attacks.

Submission
Share a screenshot of a Splunk query result showing detected suspicious activities.
Share a screenshot of logs confirming multiple failed access attempts.
Write a short observation on how web log analysis helps prevent security breaches.
