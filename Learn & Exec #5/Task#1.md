# **Task #1: Phishing Email Analysis**

## **Objective**  
The objective of this task is to help students **analyze a phishing email**, extract **Indicators of Compromise (IOCs)**, and investigate the email’s authenticity. Students will learn how to **identify phishing attempts using email headers, links, and attachments** without relying on Splunk.

---

## **Preparation**  
### **Requirements**  
- **System:** Windows/Linux/macOS  
- **Tools Required:**  
  - **Email Header Analyzer** (e.g., MXToolBox, Google Admin Toolbox, or manual analysis)  
  - **VirusTotal** (For scanning URLs and attachments)  
  - **WHOIS Lookup** (For domain investigation)  
  - **URL Decoder** (For checking obfuscated URLs)  
  - **Python (optional for decoding obfuscated phishing links)**  

---

## **Investigation Steps**  

### **Step 1: Review Email Headers**  
1. Open the **email headers** from the sample email.
2. Extract key fields such as:  
   - **From:** Sender’s email address.  
   - **Reply-To:** If different from the sender, it may be spoofed.  
   - **Return-Path:** The actual email return address.  
   - **Received:** Check if the email originated from a suspicious server.  
   - **SPF/DKIM/DMARC Results:** Look for authentication failures.  

3. Use an **online header analysis tool** like:  
   - [MXToolBox Email Header Analyzer](https://mxtoolbox.com/EmailHeaders.aspx)  
   - [Google Admin Toolbox Message Header](https://toolbox.googleapps.com/apps/messageheader/)  

---

### **Step 2: Extract and Analyze URLs in the Email**  
1. Identify **hyperlinks** in the email body.  
2. Decode **shortened or obfuscated links**:  
   ```bash
   curl -s -L "hxxp://bit[.]ly/phishlink"
   ```
3. Submit the URLs to VirusTotal:

- https://www.virustotal.com/gui/home/url
4. Perform a WHOIS lookup on the domain:

```
whois <domain-name>
```
- If the domain is newly registered, it may be suspicious.

### Step 3: Inspect Attachments
1. If the email contains an attachment, DO NOT OPEN IT directly.
2. Upload it to VirusTotal for scanning:
- https://www.virustotal.com/gui/home/upload
3. If the file is a Word document or PDF, extract and inspect macros:
```
oletools olevba <file-name>
```
- Look for suspicious VBA macros that may execute malware.

### Step 4: Investigate Domain Reputation
1. Use AbuseIPDB to check if the domain or IP has been flagged:
- https://www.abuseipdb.com/
2. Use Google Safe Browsing to check if a URL is marked as unsafe:
- https://transparencyreport.google.com/safe-browsing/search


## Conclusion
✅ Successfully analyzed a phishing email for IOCs.    
✅ Investigated email headers, URLs, and attachments.    
✅ Used threat intelligence tools to verify email authenticity.    
✅ Learned how SOC analysts detect phishing threats and protect users.    

## Submission
- Share a screenshot of your email header analysis tool results.
- Share a screenshot of VirusTotal results for a URL or attachment.
- Write a short observation on how phishing analysis helps detect cyber threats.
