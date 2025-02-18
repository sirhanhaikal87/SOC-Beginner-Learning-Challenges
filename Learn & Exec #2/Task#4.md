# **Task #4: Detect HTTP and HTTPS Traffic Anomalies**

## **Objective**  
The objective of this task is to help students analyze **HTTP and HTTPS traffic** using **Wireshark** to detect anomalies such as **unusual requests, suspicious URLs, and potential data exfiltration attempts**. By completing this task, students will understand how attackers exploit web traffic for reconnaissance, phishing, and data leaks.

---

## **Lab Setup**  
### **Requirements**  
- **System:** Windows 10/11, Linux (Ubuntu 22.04), or macOS  
- **Tools Required:**  
  - **Wireshark** (Packet capture analysis)  
  - **Web browser (Chrome/Firefox/Edge)**  

---

## **Preparation**  
### **Step 1: Start Wireshark and Capture HTTP/HTTPS Traffic**  
1. Open **Wireshark**.  
2. Select your **active network interface** (Wi-Fi, Ethernet).  
3. Click **Start Capture**.  

---

## **Attack Simulation & Detection**  
We will now **simulate web traffic**, capture it, and analyze potential anomalies.

### **Step 1: Generate HTTP and HTTPS Traffic**  
1. Open a **web browser** and visit a non-secure HTTP site:  
   ```plaintext
   http://example.com
   ```
2. Visit a secure HTTPS site:
```
https://example.com
```
3. Try accessing a suspicious or phishing-like website:
(You can use a URL from VirusTotal's threat feed for realistic testing.)
4. Submit test credentials on an HTTP site to see if they are transmitted in clear text.

### Step 2: Analyze HTTP and HTTPS Traffic in Wireshark
1. Stop the Capture in Wireshark.
2. Use the filter bar to analyze web traffic:
- Filter for HTTP traffic:
```
http
```
- Filter for HTTPS traffic:
```
tls
```
3. Identify:
- Cleartext HTTP login credentials (check "POST" requests in HTTP).
- Unexpected domain requests (potential phishing or malicious C2 domains).
- Unusual HTTPS requests to unknown IPs (possible exfiltration).

### Step 3: Investigate Suspicious Traffic
1. Extract suspicious domains/IPs from the captured traffic.
2. Check them on Threat Intelligence Platforms:
- VirusTotal
- AbuseIPDB
- URLhaus
3. If flagged as malicious, it may indicate:
- Data exfiltration via HTTP POST requests.
- Phishing attempts using lookalike domains.
- Communication with a known malware C2 server.

## Conclusion
✅ Successfully captured and analyzed HTTP and HTTPS traffic using Wireshark.   
✅ Identified cleartext credentials, suspicious domains, and abnormal HTTPS traffic.   
✅ Understood how SOC analysts detect web-based attacks and data exfiltration.   

## Submission
- Share a screenshot of Wireshark showing HTTP or HTTPS anomalies.
- Write a short observation on how web traffic analysis helps detect potential cyber threats.
