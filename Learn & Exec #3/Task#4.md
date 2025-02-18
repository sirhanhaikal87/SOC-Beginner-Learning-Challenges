# **Task #4: Detect Lateral Movement Using Windows Event Logs**

## **Objective**  
The objective of this task is to help students detect **lateral movement** in a Windows environment by analyzing **Windows Event Logs**. Students will simulate **remote command execution**, capture relevant logs, and analyze activity to identify potential unauthorized access.

---

## **Lab Setup**  
### **Requirements**  
- **System:** Two Windows machines (Attacker & Victim) in the same network (or a single system with PowerShell Remoting enabled).  
- **Tools Required:**  
  - **PowerShell** (For attack simulation)  
  - **Windows Event Viewer**  
  - **Sysmon (System Monitor) from Sysinternals**  

---

## **Preparation**  
### **Step 1: Enable PowerShell Remoting (On the Victim Machine)**  
1. Open **PowerShell (Run as Administrator)** and run:  
   ```powershell
   Enable-PSRemoting -Force
   ```
2. Verify that remoting is enabled:
```
Test-WsMan <Victim_IP>
```
3. Ensure Windows Firewall allows remote PowerShell connections:
```
netsh advfirewall firewall set rule group="Windows Remote Management" new enable=yes
```

## Attack Simulation & Detection
We will now simulate a lateral movement attack using PowerShell remoting, detect it using Windows Event Logs, and analyze it with Sysmon.

## Step 1: Simulate Lateral Movement via PowerShell Remoting
1. On the Attacker Machine, open PowerShell.
2. Execute the following command to connect to the Victim Machine:
```
Enter-PSSession -ComputerName <Victim_IP> -Credential <Username>
```
- Replace <Victim_IP> with the target machine's IP address.
- Enter valid administrator credentials when prompted.
3. Once inside the victim system, execute a remote command:
```
whoami
```
- This verifies that the attack was successful.

### Step 2: Detect Remote Execution Using Windows Event Logs
1. Open Event Viewer (Win + R, type eventvwr.msc, press Enter).
2. Navigate to:
```
Windows Logs → Security
```
3. Click Filter Current Log and enter:
- Event ID 4624 (Successful Login) → Look for Logon Type 3 or Logon Type 10 (indicating remote access).
- Event ID 4672 (Special Privileges Assigned) → Indicates admin privileges were granted.
- Event ID 5145 (Network Share Access) → Shows remote access to files.

### Step 3: Detect Remote Execution Using Sysmon Logs
1. Open Event Viewer and navigate to:
```
Applications and Services Logs → Microsoft → Windows → Sysmon → Operational
```
2. Click Filter Current Log and enter Event ID 1 (Process Creation).

3. Look for:

- Process Name: powershell.exe
- Parent Process: svchost.exe or winlogon.exe (indicating remote execution)
- Command Line Arguments: Enter-PSSession -ComputerName

4. Check for Event ID 3 (Network Connection Initiated) to see if PowerShell made a connection to a remote host.

### Step 4: Retrieve Lateral Movement Logs Using PowerShell
Instead of using Event Viewer, use PowerShell to extract logs:

**Check for Remote Logins (Event ID 4624)**
```
Get-WinEvent -FilterHashtable @{LogName='Security'; ID=4624} | Where-Object {$_.Message -like "*Logon Type: 3*" -or $_.Message -like "*Logon Type: 10*"} | Select-Object TimeCreated, Message
```
- This retrieves all successful remote login attempts.

**Check for Remote PowerShell Execution (Sysmon Event ID 1)**
```
Get-WinEvent -LogName "Microsoft-Windows-Sysmon/Operational" | Where-Object {$_.Id -eq 1 -and $_.Message -like "*powershell.exe*"} | Select-Object TimeCreated, Message | Format-Table -AutoSize
```
- This extracts all PowerShell-based process execution logs.

**Check for Network Connections Initiated (Sysmon Event ID 3)**
```
Get-WinEvent -LogName "Microsoft-Windows-Sysmon/Operational" | Where-Object {$_.Id -eq 3} | Select-Object TimeCreated, Message | Format-Table -AutoSize
```
- This identifies network connections made during lateral movement.

## Conclusion
✅ Successfully simulated lateral movement via PowerShell remoting.   
✅ Detected remote logins and privilege escalations using Windows Event Logs.    
✅ Identified process execution patterns associated with remote attacks using Sysmon.   
✅ Learned how SOC analysts detect unauthorized remote access attempts in enterprise networks.   

## Submission
- Share a screenshot of Sysmon logs showing remote PowerShell execution (Event ID 1 and Event ID 3).
- Write a short observation on how detecting lateral movement helps prevent internal network compromises.

