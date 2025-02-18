# **Task #4: Use Splunk to Search for Login Attempts**

## **Objective**
The objective of this task is to help students learn how to **search and analyze login attempts** using **Splunk**. Students will ingest Windows Security logs into Splunk, identify **successful and failed login events**, and create a **basic search query** to extract relevant security insights.

---

## **Lab Setup**
### **Requirements**
- **System:** Windows 10/11 or Windows Server 2019/2022  
- **Tools Required:**  
  - **Splunk Enterprise (Free version)**
  - **PowerShell**
  - **Windows Event Viewer**
  - **Notepad or Excel (for documentation)**  

---

## **Preparation**
### **Step 1: Install Splunk (If Not Already Installed)**
1. Download **Splunk Enterprise** from the official website:  
   [https://www.splunk.com/en_us/download.html](https://www.splunk.com/en_us/download.html)
2. Install Splunk with the default settings.
3. Open **Splunk Web Interface** (`http://localhost:8000`) and log in.
4. Navigate to **Settings → Add Data → Monitor Windows Logs**.
5. Select **Security Event Logs** to ingest logs into Splunk.
6. Click **Save & Index the data**.

### Step 2: Search for Login Events in Splunk
1. Open Splunk Web Interface.
2. Run the following search query to find all successful login attempts:
```
index=windows_logs EventCode=4624 | table _time, User, Source_Network_Address, Logon_Type
```
- This filters and displays login attempts with timestamps, usernames, and source IPs.
3. Run the following query to find all failed login attempts:

```
index=windows_logs EventCode=4625 | table _time, User, Source_Network_Address, Failure_Reason
```
- This displays failed login attempts, including the reason for failure.

4. To detect multiple failed login attempts from a single source (potential brute-force attack), use:

```
index=windows_logs EventCode=4625 | stats count by User, Source_Network_Address | where count > 5
```
- This highlights accounts with five or more failed attempts, a possible brute-force attack indicator.
### Step 3: Take Screenshots of Query Results
- Capture screenshots of:
- Successful login event query results (Event ID 4624).
- Failed login event query results (Event ID 4625).
- Brute-force detection query results (if applicable).

## Conclusion
✅ Successfully ingested Windows Security logs into Splunk.
✅ Simulated successful and failed login attempts.
✅ Used Splunk queries to extract and analyze login activities.
✅ Learned how SOC analysts detect brute-force attacks using Splunk.

## Submission
- Share screenshots of Splunk query results for successful and failed login attempts.
- Write a short observation on how Splunk helps in identifying authentication-based security threats.

