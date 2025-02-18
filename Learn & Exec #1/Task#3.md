# **Task #3: Extract and Analyze Sysmon Logs for Process Creation**

## **Objective**
The goal of this task is to introduce students to **Sysmon (System Monitor)** for process tracking and security monitoring. By completing this task, students will learn how to **detect malicious process execution**, investigate suspicious activity, and analyze **Sysmon logs** using **Event Viewer** and **PowerShell**.

---

## **Lab Setup**
### **Requirements**
- **System:** Windows 10/11 or Windows Server 2019/2022  
- **Tools:**  
  - **Sysmon (System Monitor)**
  - **PowerShell**
  - **Windows Event Viewer**
  - **Notepad or Excel (for documentation)**  

### **Step 1: Install Sysmon**
If Sysmon is not installed, download and install it using the following steps:
1. Download **Sysmon** from Microsoft Sysinternals:  
   [https://docs.microsoft.com/en-us/sysinternals/downloads/sysmon](https://docs.microsoft.com/en-us/sysinternals/downloads/sysmon)
2. Extract the Sysmon ZIP file.  
3. Open a **Command Prompt (Run as Administrator)** and install Sysmon with basic logging:  
   ```powershell
   sysmon -accepteula -i
4. Verify installation by checking for Sysmon logs in Event Viewer:
   ```
   Applications and Services Logs → Microsoft → Windows → Sysmon → Operational
    ```

## Attack Simulation & Detection Using Sysmon
We will now simulate a suspicious process execution and detect it using Sysmon logs.

### Step 2: Simulate Suspicious Process Execution
1. Open PowerShell (Run as Administrator).
2. Execute a command that mimics an attacker running a new process:
```
Start-Process notepad.exe
```
- This command spawns a new Notepad process, a technique attackers often use to execute malware.
- Sysmon will log this under Event ID 1 (Process Creation).

### Step 3: Detect Process Execution Using Windows Event Viewer
1. Open Event Viewer (Win + R, type eventvwr.msc, press Enter).
2. Navigate to:
```
Applications and Services Logs → Microsoft → Windows → Sysmon → Operational
```
3. Click Filter Current Log and enter Event ID 1 (Process Creation).
4. Locate the Notepad.exe execution event and take a screenshot.

### Step 4: Retrieve Sysmon Process Logs Using PowerShell
Instead of using Event Viewer, use PowerShell to extract process execution logs:

**List All Processes Logged by Sysmon**
```
Get-WinEvent -LogName "Microsoft-Windows-Sysmon/Operational" | Where-Object {$_.Id -eq 1} | Select-Object TimeCreated, Message | Format-Table -AutoSize
```
- This command retrieves all process execution logs recorded by Sysmon.
**Filter for Notepad Execution**
```
Get-WinEvent -LogName "Microsoft-Windows-Sysmon/Operational" | Where-Object {$_.Message -like "*notepad.exe*"} | Select-Object TimeCreated, Message | Format-Table -AutoSize
```
- This command extracts only Notepad execution events, mimicking how SOC analysts filter logs for specific process execution.

**Identify Suspicious Parent-Child Processes**
```
Get-WinEvent -LogName "Microsoft-Windows-Sysmon/Operational" | Where-Object {$_.Message -like "*powershell.exe*"} | Select-Object TimeCreated, Message | Format-Table -AutoSize
```
- If a suspicious PowerShell script spawns a child process, it might indicate malware execution or persistence mechanisms.

## Conclusion
✅ Successfully installed Sysmon and enabled process tracking.
✅ Simulated a suspicious process execution using PowerShell.
✅ Detected and retrieved logs using Windows Event Viewer and PowerShell.
✅ Understood how SOC analysts can track process execution and identify malicious activity.

## Submission
- Share a screenshot of the Sysmon process creation log (Event ID 1 for Notepad.exe).
- Write a short observation on why process tracking is important in detecting malware activity.
