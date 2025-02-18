# **Task #3: Analyze DNS Queries for Suspicious Activity**

## **Objective**  
The objective of this task is to help students analyze **DNS traffic** to detect suspicious or malicious domain resolution attempts. By completing this task, students will learn how to capture **DNS queries**, identify abnormal domain lookups, and correlate them with threat intelligence.

---

## **Lab Setup**  
### **Requirements**  
- **System:** Windows 10/11, Linux (Ubuntu 22.04), or macOS  
- **Tools Required:**  
  - **Wireshark** (Packet capture analysis)  
  - **nslookup** or **dig** (For DNS query simulation)  
  - **VirusTotal, AbuseIPDB** (Threat Intelligence Lookup)  

---

## **Preparation**  
### **Step 1: Install Dig (If Not Installed)**  
For Linux/macOS users, install `dig` for DNS queries:  
```bash
sudo apt install dnsutils -y   # Ubuntu/Debian  
sudo yum install bind-utils -y  # CentOS/RHEL
```
Windows users can use nslookup, which is pre-installed.

## Attack Simulation & Detection
We will now simulate DNS lookups, capture traffic in Wireshark, and analyze suspicious domains.

### Step 1: Start Wireshark and Capture DNS Traffic
1. Open Wireshark.
2. Select your active network interface (Wi-Fi, Ethernet).
3. Click Start Capture.
4. In the Wireshark filter bar, enter:
```
dns
```
- This will display only DNS traffic.

### Step 2: Generate DNS Queries for Testing
1. Open a command prompt (Windows) or terminal (Linux/macOS).
2. Run the following DNS lookup commands:
- Using nslookup (Windows/Linux/macOS):
```
nslookup example.com
```
- Using dig (Linux/macOS):
```
dig example.com
```
- Simulate a query for a known malicious domain:
```
nslookup malware-test.com
dig malware-test.com
```
(Use a domain from VirusTotal’s threat feed for realistic testing.)

### Step 3: Analyze Captured DNS Queries in Wireshark
1. Stop the Capture after executing the DNS queries.
2. In Wireshark, filter only DNS requests:
```
dns.qry.name
```
3. Look for:
- Unusual domains (randomized subdomains, long alphanumeric names).
- Frequent failed resolution attempts.
- Unusual destinations (queries to non-standard DNS servers).

### Step 4: Investigate Malicious Domains
1. Copy any suspicious domains found in Wireshark.
2. Check them on Threat Intelligence Platforms:
- VirusTotal
- AbuseIPDB
- URLhaus
3. If flagged as malicious, it may indicate:
- Phishing attack (malicious domains mimicking legitimate sites).
- Command & Control (C2) communication (used by malware).
- Domain Generation Algorithm (DGA) activity (random domains created by malware).

## Conclusion
✅ Successfully captured and analyzed DNS traffic using Wireshark.
✅ Identified suspicious domain queries and correlated findings with threat intelligence.
✅ Understood how SOC analysts detect malicious DNS activity and prevent attacks.

## Submission
- Share a screenshot of Wireshark showing a suspicious DNS query.
- Write a short observation on how DNS analysis helps in detecting security threats.
