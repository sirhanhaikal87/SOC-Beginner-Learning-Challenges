# **Task #2: Identify Malicious IPs from a PCAP File**

## **Objective**  
The objective of this task is to introduce students to **analyzing packet capture (PCAP) files** in Wireshark. Students will learn how to **identify suspicious IP addresses**, extract network metadata, and correlate findings with threat intelligence sources.

---

## **Lab Setup**  
### **Requirements**  
- **System:** Windows 10/11, Linux (Ubuntu 22.04), or macOS  
- **Tools Required:**  
  - **Wireshark** (Packet capture analysis)  
  - **Threat Intelligence Platforms** (e.g., VirusTotal, AbuseIPDB)  
  - **Pre-downloaded PCAP file**  

---

## **Preparation**  
### **Step 1: Download a Sample PCAP File**  
1. Download a sample PCAP file from:  
   - [https://www.malware-traffic-analysis.net/](https://www.malware-traffic-analysis.net/)  
   - [https://www.netresec.com/?page=PCAP](https://www.netresec.com/?page=PCAP)  
2. Save the **PCAP file** on your local machine.  
3. Open **Wireshark** and load the PCAP file:  
   - Click **File → Open** → Select the downloaded **PCAP file**.

---

## **Attack Simulation & Detection**  
We will now analyze the PCAP file, extract **malicious IPs**, and verify them using threat intelligence sources.

### **Step 1: Identify External IPs in the PCAP File**  
1. In Wireshark, apply the following filter to display all IP communications:  
   ```plaintext
   ip.src || ip.dst
   ```
2. Identify external IPs communicating with the host system.
3. Look for suspicious traffic patterns, such as:
- Multiple failed connection attempts
- Unusual geographic locations
- Traffic on non-standard ports

### Step 2: Extract IP Addresses for Investigation
1. Export all captured IP addresses:
- Click Statistics → Conversations → Select IPv4 tab.
- Export the list of source and destination IPs.
2. Alternatively, use this Wireshark filter to list unique IPs:
```
ip.addr
```
3. Note down any suspicious IPs.

### Step 3: Investigate Malicious IPs
1. Copy the identified IPs and check them against Threat Intelligence Databases:
- VirusTotal
- AbuseIPDB
- AlienVault OTX

2. If an IP appears in malware blacklists, it could indicate:
- C2 (Command and Control) communication
- Brute-force attacks
- Data exfiltration attempts

## Conclusion
✅ Successfully loaded and analyzed a PCAP file in Wireshark.
✅ Identified suspicious IP addresses and verified them using threat intelligence.
✅ Learned how SOC analysts investigate network threats and correlate findings.

## Submission
- Share a screenshot of Wireshark with a highlighted suspicious IP.
- Write a short observation on how threat intelligence helps in detecting malicious activity.
