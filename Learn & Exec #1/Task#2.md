# **Task #2: Identify Failed and Successful Logins from Windows Logs**

## **Objective**
The goal of this task is to help students analyze **Windows login events** and understand how to differentiate between **successful and failed login attempts** using **Windows Event Viewer** and **PowerShell**. By the end of this task, students will be able to detect suspicious login activities and identify possible brute-force attempts.

---

## **Lab Setup**
### **Requirements**
- **System:** Windows 10/11 or Windows Server 2019/2022  
- **Tools:**  
  - **PowerShell (Pre-installed)**
  - **Windows Event Viewer**
  - **Notepad or Excel (for documentation)**  

### **Enable Security Auditing for Logins**
To ensure login events are being recorded, verify that auditing is enabled by running the following command in **an elevated (Admin) PowerShell session**:  
```powershell
auditpol /set /category:"Logon/Logoff" /success:enable /failure:enable
```
This ensures that both successful and failed login attempts are logged.

## Attack Simulation & Detection Using PowerShell
We will now simulate both successful and failed login attempts and analyze them in Event Viewer.

### Step 1: Simulate Login Events
Successful Login:

Lock your machine `(Win + L)` and log in with the correct password.
This will generate a successful login event (Event ID 4624).
Failed Login Attempt:

Lock your machine again and try logging in with the wrong password at least three times.
This will generate failed login events (Event ID 4625).
### Step 2: Detect Login Events using Windows Event Viewer
1. Open Event Viewer (Win + R, type eventvwr.msc, press Enter).
2. Navigate to:
```
Windows Logs → Security
```
3. Click Filter Current Log and enter the following Event IDs:
- 4624 → Successful login
- 4625 → Failed login
4. Locate the logs corresponding to the login attempts and take a screenshot.
### Step 3: Retrieve Login Events using PowerShell
Instead of using Event Viewer, use PowerShell to extract and analyze login attempts:

**Check for Successful Logins**
```
Get-WinEvent -FilterHashtable @{LogName='Security'; ID=4624} | Select-Object TimeCreated, Message | Format-Table -AutoSize
```
- This command retrieves all successful login events from the Security log.

**Check for Failed Logins**
```
Get-WinEvent -FilterHashtable @{LogName='Security'; ID=4625} | Select-Object TimeCreated, Message | Format-Table -AutoSize
```
- This command retrieves all failed login attempts from the Security log.

**Analyze Login Attempts for Possible Brute-Force Activity**
To check for multiple failed login attempts from the same user or IP:

```
Get-WinEvent -FilterHashtable @{LogName='Security'; ID=4625} | Group-Object -Property Message | Sort-Object Count -Descending | Format-Table -AutoSize
```
- If you see multiple failed attempts for the same account within a short time, it might indicate brute-force activity.

## Conclusion
✅ Successfully simulated both successful and failed login attempts.   
✅ Detected the login attempts using Windows Event Viewer and PowerShell log extraction.    
✅ Understood how SOC analysts can track brute-force attacks and suspicious login patterns.    

## Submission
- Share a screenshot of the login event logs (Event ID 4624 & 4625).
- Write a short observation on how login attempts can be analyzed to detect suspicious activity.

