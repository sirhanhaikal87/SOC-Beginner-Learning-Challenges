# **Task #4: Create a Splunk Dashboard to Monitor SSH Login Activity**

## **Objective**  
The objective of this task is to help students **create a Splunk dashboard** to monitor **SSH login attempts, failed logins, top users, and top brute-force attempts**. This dashboard will provide real-time visibility into **user authentication trends and potential brute-force attacks**.

---

## **Lab Setup**  
### **Requirements**  
- **Splunk Server:** Installed and configured (Refer to Task #18).  
- **Splunk Universal Forwarder:** Installed on the target Ubuntu machine to collect logs (Refer to Task #19).  
- **SSH Logs Ingested:** Ensure SSH logs (`/var/log/auth.log`) are being forwarded to Splunk (Refer to Task #20).  

---

## **Steps to Create an SSH Login Monitoring Dashboard in Splunk**  

### **Step 1: Access the Splunk Web Interface**
1. Open a browser and go to:  
http://<splunk-server-ip>:8000

yaml
Copy
Edit
2. Log in with **admin credentials**.

---

### **Step 2: Create a New Dashboard**
1. Navigate to **Dashboards** → Click **Create New Dashboard**.  
2. Name the dashboard:  
SSH Login Monitoring

yaml
Copy
Edit
3. Select **Private** or **Shared** (if needed).  
4. Click **Create Dashboard**.

---

### **Step 3: Panel: Total SSH Login Attempts (Success & Failed)**
1. Click **Add Panel** → **New Search**.
2. Enter the following search query to count login attempts:  
```splunk
index=main sourcetype=ssh_logs ("Failed password" OR "Accepted password") 
| stats count by message
```
3. Click Apply and name the panel Total SSH Login Attempts.
Panel: SSH Successful vs. Failed Logins (Timechart)
1. Click Add Panel → New Search.
2. Enter the following query to track login trends over time:
```
index=main sourcetype=ssh_logs ("Failed password" OR "Accepted password") 
| timechart span=5m count by message
```
3. Click Apply and name the panel SSH Login Trends.
### **Step 4: Panel-Top SSH Users by Login Attempts
1. Click Add Panel → New Search.
2. Enter the following search query to list top users by login attempts:
```
index=main sourcetype=ssh_logs ("Failed password" OR "Accepted password") 
| stats count by user 
| sort -count
```
3. Click Apply and name the panel Top SSH Users by Login Attempts.
### **Step 5: Panel-Top Brute Force Attackers (IPs with Most Failed Logins)
Click Add Panel → New Search.
Enter the following query to find the top brute-force IPs:
```
index=main sourcetype=ssh_logs "Failed password" 
| stats count by src_ip 
| sort -count
```
3. Click Apply and name the panel Top Brute Force Attackers.
### **Step 6: Panel- Users with Most Failed Logins
1. Click Add Panel → New Search.
2. Enter the following query to find users with repeated failed login attempts:
```
index=main sourcetype=ssh_logs "Failed password" 
| stats count by user 
| sort -count
```
3. Click Apply and name the panel Top Users with Failed Logins.
### **Step 7: Save and View the Dashboard
- Click Save on the dashboard.
- Open the dashboard to view real-time SSH login monitoring.

## Conclusion
✅ Successfully created a Splunk dashboard to monitor SSH authentication logs.
✅ Configured key panels for login trends, top users, and brute-force detection.
✅ Learned how SOC analysts use dashboards for real-time SSH security monitoring.

## Submission
- Share a screenshot of the completed Splunk dashboard.
- Write a short observation on how real-time monitoring helps detect SSH brute-force attacks.
