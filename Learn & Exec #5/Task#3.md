# **Task #3: Phishing Campaign Investigation Using Splunk**

## **Objective**  
The objective of this task is to help students detect and analyze **phishing campaigns** targeting an organization using **Microsoft Defender for Office 365 email logs** in Splunk. Students will identify **common sender patterns, repeated attack subjects, and campaign indicators** to track large-scale phishing attempts.

---

## **Preparation: Ingest Email Logs into Splunk**  
1. Ensure **Splunk is installed and running** (Refer to Task #18 if needed).  
2. Upload the **Microsoft Defender for Office 365 email logs** into Splunk:  
   - If logs are in CSV format, upload using:  
     ```splunk
     | inputlookup phishing_email_logs.csv
     ```
   - If logs are forwarded via **Microsoft Graph API or Security & Compliance Logs**, ensure they are indexed properly.  
3. Verify that logs contain key fields such as:  
   - `sender_email` → Email sender  
   - `recipient_email` → Email recipient  
   - `subject` → Email subject  
   - `attachment_name` → File name attached to the email  
   - `ip_address` → Sending IP  
   - `email_status` → Delivered, Quarantined, Rejected  
   - `timestamp` → Time of email event  

---

## **Hunting Steps: Investigating Phishing Campaigns**  

### **Step 1: Detect Large-Scale Phishing Emails Sent from the Same Sender**  
Run the following query to identify **senders that sent multiple phishing emails**:  
```splunk
index=email_logs sourcetype=ms_o365_email email_status="Delivered" OR email_status="Quarantined"
| stats count by sender_email
| sort -count
```
- If a single sender email has sent multiple phishing emails, it could indicate a phishing campaign.

### Step 2: Identify Common Phishing Subjects Used in Emails
Run this query to find recurring email subjects used in phishing attempts:

```
index=email_logs sourcetype=ms_o365_email email_status="Delivered" OR email_status="Quarantined"
| stats count by subject
| sort -count
```
- If the same email subject appears across multiple emails, the attacker may be using a social engineering tactic to lure multiple victims.

### Step 3: Detect Phishing Campaigns Targeting Multiple Users
Run the following query to identify phishing campaigns that use the same sender and subject to target multiple recipients:

```
index=email_logs sourcetype=ms_o365_email email_status="Delivered"
| stats count by sender_email, subject
| sort -count
```
- If the same sender has sent similar phishing emails to multiple recipients, it suggests a targeted phishing campaign.

### Step 4: Extract Common Malicious Indicators (IOCs)
To find repeated malicious IPs used in phishing campaigns:

```
index=email_logs sourcetype=ms_o365_email email_status="Quarantined"
| stats count by ip_address
| sort -count
```
- hese IPs can be checked on AbuseIPDB: https://www.abuseipdb.com/.
To detect emails with suspicious attachments:

```
index=email_logs sourcetype=ms_o365_email email_status="Delivered" attachment_name!=""
| stats count by attachment_name
| sort -count
```
- Attachments such as .html, .docm, .exe, or .zip could indicate malware-laced phishing emails.

## Conclusion
✅ Successfully analyzed Microsoft Defender for Office 365 email logs in Splunk.
✅ Identified large-scale phishing campaigns by tracking sender email and subjects.
✅ Extracted malicious IPs and suspicious attachments linked to phishing emails.
✅ Learned how SOC analysts investigate email-based phishing campaigns.

## Submission
- Share a screenshot of a Splunk query result showing sender patterns or phishing campaign indicators.
- Share a screenshot of sender domains, email subjects, or IP addresses linked to phishing campaigns.
- Write a short observation on how detecting phishing campaigns helps prevent security breaches.
