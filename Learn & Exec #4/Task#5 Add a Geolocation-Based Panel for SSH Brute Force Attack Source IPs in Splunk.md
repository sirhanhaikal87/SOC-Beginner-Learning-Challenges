# **Task #5: Add a Geolocation-Based Panel for SSH Brute Force Attack Source IPs in Splunk**

## **Objective**  
The objective of this task is to help students **add a geolocation-based panel** in Splunk to track the **geographic location of IP addresses involved in SSH brute-force attacks**. This panel will provide insights into **where attacks originate from**, helping security analysts **visualize attack sources** on a map.

---

## **Lab Setup**  
### **Requirements**  
- **Splunk Server:** Installed and configured (Refer to Task #18).  
- **Splunk Universal Forwarder:** Installed on the target Ubuntu machine (Refer to Task #2).  
- **SSH Logs Ingested:** Ensure SSH logs (`/var/log/auth.log`) are being forwarded to Splunk (Refer to Task #20).  

---

## **Steps to Add a Geolocation Panel in Splunk Dashboard**  

### **Step 1: Add Geolocation Data to SSH Logs
1. Open Splunk Web Interface → Navigate to Search & Reporting.
2. Run the following query to enrich SSH brute-force attack logs with geolocation data:
```
index=main sourcetype=ssh_logs "Failed password" 
| iplocation src_ip 
| table _time src_ip Country Region City
```
3. If Splunk correctly resolves IP addresses to locations, move to the next step.

### Step 2: Create a Geolocation Panel in the Dashboard
1. Navigate to Dashboards → SSH Login Monitoring.
2. Click Edit → Add Panel → Select New Search.
3. Enter the following query to generate a map visualization of SSH brute-force attack sources:
```
index=main sourcetype=ssh_logs "Failed password" 
| iplocation src_ip 
| geostats count by Country
```
4. Click Apply and name the panel Attack Sources (Geolocation).
5. Choose Choropleth Map or Clustered Map Visualization.
6. Click Save.

Step 3: Verify the Geolocation Panel
1. Open the SSH Login Monitoring Dashboard.
2. Confirm that the map is correctly displaying attack sources based on geolocation.

## Conclusion
✅ Successfully added a geolocation-based visualization for SSH brute-force attacks.
✅ Created a map-based panel to visualize attack sources.
✅ Learned how SOC analysts track cyber attack origins using SIEM tools.

## Submission
- Share a screenshot of the Splunk dashboard with the geolocation-based map.
- Write a short observation on how geographic attack tracking helps in threat intelligence and security operations.
