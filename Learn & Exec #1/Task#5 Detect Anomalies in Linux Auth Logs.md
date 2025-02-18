# **Task #5: Detect Anomalies in Linux Auth Logs**

## **Objective**
The objective of this task is to help students analyze **Linux authentication logs** to detect **failed login attempts, privilege escalations, and unauthorized access**. Students will simulate login attempts, extract logs using **Linux commands**, and identify anomalies that indicate potential security threats.

---

## **Lab Setup**
### **Requirements**
- **System:** Linux (Ubuntu 22.04 / Debian / CentOS)  
- **Tools Required:**  
  - **Terminal**
  - **Syslog (Pre-installed)**
  - **grep, awk, cut** (Pre-installed)
  - **Notepad or Excel (for documentation)**  

---

## **Preparation**
### **Step 1: Verify Authentication Logging is Enabled**
Authentication logs are usually stored in `/var/log/auth.log` (Ubuntu/Debian) or `/var/log/secure` (CentOS/RHEL).  
To check if logs are available, run:  
```bash
ls -lh /var/log/auth.log
```

or

```
ls -lh /var/log/secure
```
If logs exist, proceed to the next step.

## Attack Simulation & Detection
We will now simulate failed login attempts and privilege escalation, then detect anomalies.

### Step 1: Simulate Failed SSH Logins
1. Open Terminal.
2. Attempt to SSH into the system with incorrect credentials:
```
ssh invalid_user@localhost
```
3. Try multiple incorrect passwords:
```
ssh youruser@localhost
```
- Enter incorrect passwords at least three times.

### Step 2: Detect Failed Login Attempts
1. Open the authentication log file:

```
cat /var/log/auth.log | grep "Failed password"
```
or for CentOS/RHEL:
```
cat /var/log/secure | grep "Failed password"
```
- This will display all failed SSH login attempts.

2. To extract failed login attempts along with IP addresses:

```
grep "Failed password" /var/log/auth.log | awk '{print $1, $2, $3, $9, $11}'
```
- This command displays timestamps, usernames, and IPs.

### Step 3: Detect Privilege Escalation Attempts
1. Check if anyone tried to escalate privileges using sudo:

```
cat /var/log/auth.log | grep "sudo"
```
- This logs all commands executed with sudo.

2. To filter failed sudo attempts, use:

```
grep "authentication failure" /var/log/auth.log
```
### Step 4: Detect Multiple Failed Login Attempts (Brute-force Indicator)
To find users with multiple failed login attempts:

```
grep "Failed password" /var/log/auth.log | awk '{print $9}' | sort | uniq -c | sort -nr
```
- If a single user or IP has more than 5 failed attempts, it might indicate a brute-force attack.

## Conclusion
✅ Successfully analyzed Linux authentication logs.
✅ Detected failed SSH login attempts and privilege escalation attempts.
✅ Used Linux commands to extract and analyze potential security threats.
✅ Understood how SOC analysts detect brute-force attacks and unauthorized access.

## Submission
- Share a screenshot of the terminal output showing failed login attempts.
- Write a short observation on how authentication logs help in identifying security threats.

