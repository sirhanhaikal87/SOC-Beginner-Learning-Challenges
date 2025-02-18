# **Task #2: Analyze Windows Registry Changes After a Malware Infection**

## **Objective**  
The objective of this task is to help students analyze **Windows Registry modifications** that occur during a malware infection. Students will simulate **registry changes**, detect unauthorized modifications, and use **Sysmon logs** to investigate suspicious registry activity.

---

## **Lab Setup**  
### **Requirements**  
- **System:** Windows 10/11 or Windows Server 2019/2022  
- **Tools Required:**  
  - **Registry Editor (Pre-installed on Windows)**  
  - **Sysmon (System Monitor) from Sysinternals**  
  - **PowerShell**  

---

## **Preparation**  
### **Step 1: Install Sysmon (If Not Installed)**  
1. Download **Sysmon** from Microsoft Sysinternals:  
   [https://docs.microsoft.com/en-us/sysinternals/downloads/sysmon](https://docs.microsoft.com/en-us/sysinternals/downloads/sysmon)  
2. Extract the Sysmon ZIP file.  
3. Open **Command Prompt (Run as Administrator)** and install Sysmon with registry monitoring enabled:  
   ```powershell
   sysmon -accepteula -i
   ```
4. Verify installation by checking for Sysmon logs in Event Viewer:
```
Applications and Services Logs → Microsoft → Windows → Sysmon → Operational
```

## Attack Simulation & Detection
We will now simulate unauthorized registry modifications, detect them using Event Viewer, and analyze logs with Sysmon.

## Step 1: Simulate a Suspicious Registry Modification
1. Open PowerShell (Run as Administrator).
2. Run the following command to modify the Windows Run Key (often used for persistence by malware):
```
New-ItemProperty -Path "HKCU:\Software\Microsoft\Windows\CurrentVersion\Run" -Name "MaliciousEntry" -Value "C:\Windows\System32\cmd.exe" -PropertyType String
```
- This command adds a registry entry that launches `cmd.exe` on system startup.
- Attackers often use this technique to maintain persistence on compromised machines.
Step 2: Detect the Registry Modification Using Registry Editor
1. Open Registry Editor (`Win + R`, type `regedit`, press Enter).
2. Navigate to:
```
HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Run
```
3. Look for the "MaliciousEntry" key and verify that its value is set to cmd.exe.

### Step 3: Detect Registry Changes Using Sysmon Logs
1. Open Event Viewer (Win + R, type eventvwr.msc, press Enter).
2. Navigate to:
```
Applications and Services Logs → Microsoft → Windows → Sysmon → Operational
```
3. Click Filter Current Log and enter Event ID 13 (Registry Value Set).
4. Look for:
- Registry Key Path: `HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Run`
- Value Name: `MaliciousEntry`
- Value Data: `C:\Windows\System32\cmd.exe`
- Process Responsible: `powershell.exe`

### Step 4: Retrieve Registry Modification Logs Using PowerShell
Instead of using Event Viewer, use PowerShell to extract Sysmon registry modification logs:

```
Get-WinEvent -LogName "Microsoft-Windows-Sysmon/Operational" | Where-Object {$_.Id -eq 13} | Select-Object TimeCreated, Message | Format-Table -AutoSize
```
- This command retrieves all registry modification events.
- Look for suspicious registry modifications made by PowerShell.

### Step 5: Remove the Suspicious Registry Entry
To clean up the simulated registry modification, run:

```
Remove-ItemProperty -Path "HKCU:\Software\Microsoft\Windows\CurrentVersion\Run" -Name "MaliciousEntry"
```
- This removes the persistence entry from the registry.


## Conclusion
✅ Successfully simulated a suspicious registry modification.   
✅ Detected registry changes using Registry Editor and Sysmon logs.   
✅ Identified persistence mechanisms attackers use to maintain access.   
✅ Learned how SOC analysts detect registry-based persistence threats.   

## Submission
- Share a screenshot of Sysmon logs showing the registry modification (Event ID 13).
- Write a short observation on how registry analysis helps detect persistence mechanisms used by malware.
