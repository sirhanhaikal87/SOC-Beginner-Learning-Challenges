# **Task #2: Phishing Investigation Using Splunk**  

## **Objective**  
The objective of this task is to help students analyze **Microsoft Defender for Office 365 email logs** in Splunk to investigate **phishing emails**. By completing this task, students will learn how to:  
- **Ingest and analyze email security logs**  
- **Identify patterns of phishing emails**  
- **Detect common malicious indicators such as sender domains, IPs, and attachment types**  
- **Investigate phishing campaigns targeting an organization**  

---

## **Preparation: Ingest Email Logs into Splunk**  
1. **Ensure Splunk is installed and running** (Refer to Task #18 if needed).  
2. **Upload the Microsoft Defender for Office 365 email logs** into Splunk:  
   - If logs are in CSV format, upload using:  
     ```splunk
     | inputlookup phishing_email_logs.csv
     ```
   - If logs are forwarded via **Microsoft Graph API or Security & Compliance Logs**, ensure the logs are indexed properly.  
3. Verify that logs contain relevant fields such as:  
   - `sender_email` → Email sender  
   - `recipient_email` → Email recipient  
   - `subject` → Email subject  
   - `attachment_name` → File name attached to the email  
   - `ip_address` → Sending IP  
   - `email_status` → Delivered, Quarantined, Rejected  
   - `timestamp` → Time of email event  

---

## **Investigation Steps: Detecting Phishing Indicators**  

### **Step 1: Identify Emails Marked as Potential Phishing**  
Run the following Splunk query to filter suspicious emails:  
```splunk
index=email_logs sourcetype=ms_o365_email email_status="Quarantined" OR email_status="Rejected"
| table timestamp, sender_email, recipient_email, subject, ip_address, attachment_name, email_status
```
- This provides a list of quarantined and rejected emails, which may indicate phishing attempts.

### Step 2: Identify Common Sender Domains in Phishing Emails
```
index=email_logs sourcetype=ms_o365_email email_status="Quarantined"
| rex field=sender_email "(?<=@)([^>]+)"  
| stats count by sender_email  
| sort -count
```
- This extracts and groups phishing attempts by sender domains to identify common attack sources.

### Step 3: Identify Repeated Malicious IP Addresses
```
index=email_logs sourcetype=ms_o365_email email_status="Quarantined"
| stats count by ip_address  
| sort -count
```
- This detects IPs sending multiple phishing emails, indicating a coordinated attack.
- These IPs can be checked using AbuseIPDB: https://www.abuseipdb.com/

### Step 4: Identify Emails with Malicious Attachments
```
index=email_logs sourcetype=ms_o365_email email_status="Delivered" attachment_name!=""
| stats count by attachment_name  
| sort -count
```
- This finds commonly attached files used in phishing attempts (e.g., `.docm`, `.exe`, `.html`).
- Suspicious attachments should be uploaded to VirusTotal for analysis: https://www.virustotal.com/gui/home/upload

### Step 5: Detect Phishing Campaigns Targeting Multiple Users
```
index=email_logs sourcetype=ms_o365_email email_status="Delivered"
| stats count by sender_email, subject
| sort -count
```
- If the same sender has sent similar phishing emails to multiple recipients, it suggests a targeted phishing campaign.

### Step 6: Check If Employees Clicked on Phishing Links (Optional)
If proxy logs are available, run:

```
index=proxy_logs url="*malicious-domain.com*"
| stats count by src_ip, user
```
- This identifies users who clicked phishing links, which may require incident response actions.

## Conclusion
✅ Successfully ingested and analyzed Microsoft Defender for Office 365 logs in Splunk.
✅ Identified suspicious emails, sender domains, and repeated malicious IPs.
✅ Detected phishing emails containing attachments and fake links.
✅ Learned how SOC analysts investigate phishing attacks in enterprise environments.

## Submission
- Share a screenshot of a Splunk query result showing quarantined or rejected phishing emails.
- Share a screenshot of sender domains and IP addresses linked to phishing attempts.
- Write a short observation on how phishing email investigation helps in identifying and mitigating email-based threats.
