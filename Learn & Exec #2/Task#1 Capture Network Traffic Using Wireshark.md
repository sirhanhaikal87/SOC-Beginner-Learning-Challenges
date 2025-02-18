# **Task #1: Capture Network Traffic Using Wireshark**

## **Objective**  
The objective of this task is to introduce students to **network traffic analysis** using **Wireshark**. Students will learn how to **capture, filter, and analyze network packets**, identify key protocols, and detect potential security threats.

---

## **Lab Setup**  
### **Requirements**  
- **System:** Windows 10/11, Linux (Ubuntu 22.04), or macOS  
- **Tools Required:**  
  - **Wireshark** (Network packet analyzer)  
  - **TCPDump** (Optional for Linux users)  
  - **Active Internet connection**  

---

## **Preparation**  
### **Step 1: Install Wireshark (If Not Installed)**  
#### **For Windows:**  
1. Download **Wireshark** from:  
   [https://www.wireshark.org/download.html](https://www.wireshark.org/download.html)  
2. Install Wireshark with **WinPcap/Npcap** (for network packet capture).  
3. Restart your system after installation.  

#### **For Linux:**  
Run the following command to install Wireshark:  
```bash
sudo apt update && sudo apt install wireshark -y
```
Allow non-root users to capture packets:

```
sudo usermod -aG wireshark $USER
```
Restart your system to apply changes.

For macOS:
Install Wireshark using Homebrew:

```
brew install --cask wireshark
```

## Attack Simulation & Detection
We will now capture live network traffic, filter HTTP packets, and analyze key information.

### Step 1: Start Wireshark and Capture Traffic
1. Open Wireshark.
2. Select your active network interface (e.g., Wi-Fi, Ethernet).
3. Click Start Capture.

### Step 2: Simulate Network Traffic
1. Open a web browser and visit:
```
http://example.com
```
- This generates HTTP packets that can be analyzed in Wireshark.

2. Run a simple ping command to generate ICMP traffic:

```
ping -c 4 google.com
```

### Step 3: Analyze Captured Traffic in Wireshark
1. Stop the Capture in Wireshark after a few minutes.
2. Use the Filter Bar to filter different types of network traffic:
- Filter for HTTP traffic:
```
http
```
- Filter for ICMP traffic (Ping requests):
```
icmp
```

- Filter for DNS queries:
```
dns
```

3. Click on a packet and analyze:
- Source and Destination IP
- Protocol used (TCP, UDP, ICMP)
- Payload data (if available)

## Conclusion
✅ Successfully installed and configured Wireshark.
✅ Captured live network traffic and analyzed HTTP, ICMP, and DNS packets.
✅ Learned how SOC analysts investigate network traffic for anomalies.

## Submission
- Share a screenshot of Wireshark with filtered network traffic.
- Write a short observation on how network traffic analysis can help detect potential threats.

