# **Task #1: Acquiring Linux Memory Dump Using KVML**

## **Objective**  
The objective of this task is to help students **acquire a Linux memory dump using KVML** for forensic analysis. By completing this task, students will learn how to **capture live memory from an Ubuntu machine and transfer it to another system for further forensic investigation**.

---

## **Preparation: Lab Setup**  
### **Requirements**  
- **System 1 (Target Machine):** Ubuntu 22.04/20.04 (Machine from which memory will be acquired).  
- **System 2 (Analysis Machine):** Ubuntu 22.04/20.04 (Machine where the memory dump will be transferred and analyzed).  
- **User Permissions:** Root or sudo privileges on both machines.  
- **Tools Required:**  
  - **AVML ( by Microsoft) for memory acquisition**  
  - **strings & grep** for quick validation  
  - **scp or rsync** for transferring the memory dump  

---

## **Forensic Acquisition Steps**  

### **Step 1: Download AVML on Target Machine**
1. Open a terminal on the **Target Machine (System 1)**.
2. Download KVML from the official GitHub releases:  
   ```bash
   wget https://github.com/microsoft/avml/releases/latest/download/avml
   ```

### Step 2: Make AVML Executable
1. Grant execute permissions:
```
chmod +x ./avml
```
2. Verify execution by running:
```
./avml --help
```
Step 3: Create a Memory Dump
1. Run the following command to acquire a memory dump and save it as linux_memory.mem:
```
sudo ./avml linux_memory.mem
```
2. Confirm the memory dump file has been created:
```
ls -lh linux_memory.mem
```

### Step 4: Validate the Memory Dump
1. Use `strings` and `grep` to check if known data appears in the dump:
```
strings linux_memory.mem | grep "mozilla"
```
- If strings related to applications, files, or processes appear, the dump is valid.

### Step 5: Transfer the Memory Dump to the Analysis Machine
1. On the Analysis Machine (System 2), create a directory to store the memory dump:

```
mkdir ~/forensics/memory_dumps
```
2. Transfer the memory dump from the Target Machine to the Analysis Machine using scp:

```
scp user@<Target-Machine-IP>:/home/user/linux_memory.mem ~/forensics/memory_dumps/
```
- Replace <Target-Machine-IP> with the actual IP of the Target Machine.
- Enter the password when prompted.

3. Verify the file is successfully transferred:

```
ls -lh ~/forensics/memory_dumps/
```

## Conclusion
✅ Successfully acquired a Linux memory dump using AVML.   
✅ Performed basic validation to confirm the memory dump’s integrity.   
✅ Transferred the memory dump to a forensic workstation for further analysis.   
✅ Learned how SOC analysts and forensic investigators collect volatile memory for incident response.   

## Submission
- Share a screenshot showing the successful memory dump creation.
- Share a screenshot of the memory dump transfer to the forensic machine.
- Write a short observation on how memory acquisition plays a crucial role in digital forensics investigations.
