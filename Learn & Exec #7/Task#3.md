# **Task #3: Linux Memory Forensics Investigation Using Volatility 3 on REMnux**  

## **Objective**  
The objective of this task is to help students **analyze a Linux memory dump using Volatility 3** on **REMnux**. By completing this task, students will learn how to **extract processes, network connections, files, and other artifacts from memory for forensic investigation**.  

---

## **Preparation: Lab Setup**  

### **Requirements**  
- **REMnux** (Pre-installed or running as a VM).  
- **Linux memory dump file** (Sample file will be provided).  
- **Volatility 3 installed on REMnux**.  

### **Step 1: Install Volatility 3 on REMnux (If Not Installed)**  
```bash
cd /opt
sudo git clone https://github.com/volatilityfoundation/volatility3.git
cd volatility3
sudo python3 -m pip install -r requirements.txt
```
- Verify installation:
```
python3 vol.py -h
```

## Forensic Investigation Steps Using Volatility 3

### Step 1: Identify Memory Profile
Run the following command to detect the Linux kernel version from the memory dump:

```
python3 vol.py -f <memory-dump> windows.info.Info
```
- The kernel version helps in selecting the correct profile for analysis.

### Step 2: List Running Processes
Retrieve active processes from the memory dump:

```
python3 vol.py -f <memory-dump> linux.pslist.PsList
```
- Look for suspicious processes, especially those running as root or unusual names.

### Step 3: List Open Network Connections
Identify active network connections during the memory capture:

```
python3 vol.py -f <memory-dump> linux.netstat.NetStat
```
- Look for unusual network activity, such as connections to unknown IPs.

### Step 4: List Loaded Kernel Modules
Check for loaded malicious kernel modules (rootkits):

```
python3 vol.py -f <memory-dump> linux.lsmod.Lsmod
```
- If any suspicious module is loaded, investigate further.

### Step 5: Extract File Handles Opened by Processes
Identify files that were accessed or in use by processes:

```
python3 vol.py -f <memory-dump> linux.lsof.Lsof
```
- Look for files accessed by suspicious processes.

### Step 6: Extract Suspicious Process Memory Dump
Dump the memory of a suspicious process (replace PID accordingly):

```
python3 vol.py -f <memory-dump> -o ./process_dumps linux.proc.Maps --pid <PID>
```
- Analyze extracted dumps with strings or malware analysis tools.

### Step 7: Extract Bash Command History
Retrieve shell commands executed before the memory capture:

```
python3 vol.py -f <memory-dump> linux.bash.Bash
```
- This may reveal malicious command execution.

### Step 8: Check Open TCP and UDP Sockets
Find open network sockets:

```
python3 vol.py -f <memory-dump> linux.netstat.NetStat
```
- Look for unexpected listening ports.

### Step 9: Extract Files from Memory
Recover in-memory files, such as scripts or binaries:

```
python3 vol.py -f <memory-dump> linux.find_file.FindFile --path /tmp/malware.sh
```
- Extract the file for further analysis.

## Conclusion
✅ Successfully analyzed a Linux memory dump using Volatility 3 on REMnux.    
✅ Identified active processes, suspicious network connections, and executed commands.    
✅ Extracted important forensic artifacts for deeper investigation.    
✅ Learned how SOC analysts and forensic investigators conduct Linux memory analysis.    

## Submission
- Share screenshots of Volatility 3 output for active processes and network connections.
- Share a summary of any suspicious findings from the memory dump.
- Write a short observation on how memory forensics helps detect live threats in compromised systems.
