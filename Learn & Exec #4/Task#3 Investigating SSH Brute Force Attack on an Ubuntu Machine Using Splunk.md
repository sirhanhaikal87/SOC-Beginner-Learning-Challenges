# **Task #3: Investigating SSH Brute Force Attack on an Ubuntu Machine Using Splunk**

## **Objective**  
The objective of this task is to help students detect **SSH brute-force attacks** on an Ubuntu system by using **Hydra for attack simulation** and analyzing logs in **Splunk**. Students will launch an **SSH brute-force attack**, configure Splunk to collect logs, and create search queries to detect the attack.

---

## **Lab Setup**  
### **Requirements**  
- **Splunk Server:** Installed on an Ubuntu machine (Refer to Task #18 for installation).  
- **Target Machine:** Another Ubuntu system with SSH enabled.  
- **Attacker Machine:** Linux system with Hydra installed.  
- **Tools Required:**  
  - **Splunk Universal Forwarder (on target machine for log collection)**  
  - **Hydra (for brute-force attack simulation)**  
---

## **Preparation**  
- Refer [Task#1](Task%231.md) for setting up Splunk on an Ubuntu Machine
- Refer [Task#2](Task%232.md) for Ingest Syslog of a Remote Ubuntu Machine Using Universal Forwarder

## Attack Simulation & Detection

### Step 1: Simulate an SSH Brute Force Attack Using Hydra
1. From the attacker machine, install Hydra:
```
sudo apt update && sudo apt install hydra -y
```
2. Run the following Hydra command:
```
hydra -l ubuntu -P /usr/share/wordlists/rockyou.txt <target-IP> ssh
```
- Replace `<target-IP>` with the Ubuntu target machine’s IP address.
- `-l ubuntu` specifies the username to attack.
- `-P /usr/share/wordlists/rockyou.txt` uses a password list for brute-forcing SSH.

### Step 2: Detect SSH Brute Force Attempts Using Splunk
1. Open the Splunk Web Interface (http://<splunk-server-ip>:8000).
2. Navigate to Search & Reporting.
3. Run the following search query to detect multiple failed SSH login attempts:

```
index=main sourcetype=ssh_logs "Failed password" | stats count by src_ip, user
```
- This will show failed login attempts per IP and targeted usernames.
4. Run a search query to detect a high number of login attempts from a single IP:

```
index=main sourcetype=ssh_logs "Failed password" | timechart span=1m count by src_ip
```
- If an IP has multiple failed attempts in a short period, it suggests brute-force activity.
5. Run a search query to check for successful logins after multiple failures:

```
index=main sourcetype=ssh_logs ("Failed password" OR "Accepted password") | stats values(message) by src_ip, user
```
- If the same IP had multiple failed attempts followed by a successful login, it may indicate a successful brute-force attack.

6. Run a search query to track login patterns by time:

```
index=main sourcetype=ssh_logs | timechart span=5m count by src_ip
```
- This helps visualize attack timing and frequency.

### Step 3: Extract Indicators of Compromise (IOCs)
1. Extract attacker IP addresses from logs:
```
index=main sourcetype=ssh_logs "Failed password" | table _time src_ip user
```
2. Verify if the attacker’s IP is flagged as malicious using:
- VirusTotal
- AbuseIPDB

## Conclusion
✅ Successfully installed Splunk Universal Forwarder on the target machine.   
✅ Simulated an SSH brute-force attack using Hydra.   
✅ Ingested SSH logs into Splunk for security monitoring.   
✅ Used Splunk search queries to detect brute-force attacks.   
✅ Learned how SOC analysts detect unauthorized login attempts using SIEM tools.   

## Submission
- Share a screenshot of Hydra showing brute-force attack results.
- Share a screenshot of Splunk Web displaying detected SSH brute-force attempts.
- Write a short observation on how SIEM tools like Splunk help in detecting SSH brute-force attacks.

