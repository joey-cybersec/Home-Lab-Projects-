# Home-Lab-Projects-
Cyber security exercises 


Lab 1 — Hands-On Internal Network & Firewall Setup 

## Step 1: Download pfSense ISO

**What I did:**  
Downloaded the latest pfSense ISO from the official website.

**Why it matters:**  
pfSense is a real enterprise firewall used by companies. It allows control over traffic, logging, VPNs, and intrusion detection.

**Skills learned:**  
- Recognizing enterprise security tools  
- Understanding the role of firewalls in corporate networks

---

## Step 2: Create pfSense Virtual Machine (VM)

**What I did:**  
Created a new VM in VirtualBox to run pfSense. Assigned sufficient memory and storage for the VM.

**Why it matters:**  
Virtual machines let us safely simulate networks and test configurations without affecting the host system.

**Skills learned:**  
- Virtual machine creation  
- Safe testing environments for cybersecurity  

---

## Step 3: Configure Two Network Adapters

**What I did:**  
- Adapter 1 (WAN) → Connected to Internet  
- Adapter 2 (LAN) → Connected to Internal Network

**Why it matters:**  
pfSense acts as a gatekeeper between untrusted (WAN) and trusted (LAN) networks. Proper interface assignment ensures secure traffic control.

**Skills learned:**  
- Network segmentation  
- Understanding trusted vs. untrusted zones  

---

## Step 4: Install pfSense

**What I did:**  
Installed pfSense on the VM like a normal operating system, setting up disk partitions and initial configurations.

**Why it matters:**  
This teaches how to deploy security appliances and understand OS installation processes for network devices.

**Skills learned:**  
- OS installation  
- Boot process understanding  
- Console configuration  

---

## Step 5: Assign WAN and LAN Interfaces

**What I did:**  
Assigned which interface is WAN and which is LAN during pfSense setup.

**Why it matters:**  
Correct interface assignment ensures traffic flows correctly and internal devices are protected. Misconfiguration could expose devices to the internet.

**Skills learned:**  
- Interface assignment  
- Secure network segmentation  

---

## Step 6: Set LAN IP Address

**What I did:**  
Configured LAN IP to `10.0.0.1` for internal network access.

**Why it matters:**  
This IP acts as the default gateway for internal devices and the management interface for pfSense. Essential for proper routing and network communication.

**Skills learned:**  
- IP addressing  
- Subnetting fundamentals  
- Network gateway setup  

---

## Summary

I have successfully **downloaded pfSense**, **created a VM**, **configured two network adapters**, **installed pfSense**, **assigned WAN/LAN interfaces**, and **set the LAN IP address**.  

These steps establish the **foundation for a secure internal network** that can now be populated with Windows and Linux virtual machines for testing and learning.

Evidence :
<img width="959" height="522" alt="image" src="https://github.com/user-attachments/assets/891389a5-6c81-4d1a-b823-43beecca4a6c" />








## Step 7: Creating Internal Windows VM

**What I did:**  
Installed and configured a Windows 10 virtual machine to act as an internal client behind the pfSense firewall. Set the network adapter to “Internal Network” (LAN) to simulate a protected endpoint within a segmented network.

**Why it matters:**  
This VM represents a typical employee workstation in a corporate environment. It allows for realistic testing of DHCP, firewall rules, and internal traffic flow. Connecting it to pfSense enables controlled access, monitoring, and security enforcement.

**Skills learned:**
- Virtual machine provisioning
- Network adapter configuration in VirtualBox
- DHCP troubleshooting and IP verification (`ipconfig`)
- Real-world segmentation and endpoint simulation


<img width="947" height="533" alt="image" src="https://github.com/user-attachments/assets/61bf8513-d232-4f79-8716-912fa2fbd442" />



#  Lab 2: Windows Logging & Attack Detection (Blue Team)



## Step 1: Set Up the Windows Victim Machine

### What I did:
Created a Windows 10 virtual machine to act as the victim system. This machine represents a typical corporate workstation in an enterprise environment.

### Why it matters:
Windows endpoints are common targets for attackers. Understanding how Windows records authentication events is critical for effective detection and incident response.

### Skills learned:
- Windows virtual machine setup  
- Enterprise endpoint awareness  
- Blue team fundamentals  

---

## Step 2: Enable Windows Login Auditing

### What I did:
Enabled auditing for successful and failed logon events using **Local Security Policy (secpol.msc)** and applied the policy using `gpupdate /force`.

### Why it matters:
Without logging enabled, attacks cannot be detected. Security logging is the foundation of monitoring, alerting, and forensic investigations in SOC environments.

### Skills learned:
- Windows audit policy configuration  
- Security logging fundamentals  
- Detection preparation  

---

## Step 3: Create a Standard User Account

### What I did:
Created a non-administrator user account to simulate a normal employee account that could be targeted during an attack.

### Why it matters:
Attackers often target standard user accounts. Monitoring failed authentication attempts against these accounts helps detect brute-force and credential-stuffing attacks.

### Skills learned:
- Windows user management  
- Least privilege principles  
- Understanding attacker behavior  

---
## Step 4: Simulate an Attack (Local Failed Logins)

### What I did:
Instead of using a Linux virtual machine, I simulated an attack directly on the Windows system by intentionally entering incorrect passwords on the lock screen. I locked the Windows VM and attempted to log in multiple times using the wrong password for the standard user account. Each failed attempt generated a **4625 Failed Logon** event in the Security log.

### Why it matters:
Even without a second machine, repeatedly entering incorrect credentials still mimics real attacker behavior. Brute‑force attempts, password guessing, and unauthorized access often appear exactly like this in enterprise environments. This method still produces the same Windows log artifacts that SOC analysts investigate when detecting suspicious authentication activity.

### Skills learned:
- Understanding how Windows records failed authentication attempts  
- Recognizing brute‑force patterns without needing external tools  
- Simulating attacker behavior in a simplified, controlled environment  
- Preparing for log analysis and SOC-style investigations  




---

## Step 5: Review Windows Security Logs (Event Viewer)

### What I did:
Opened **Event Viewer** on the Windows virtual machine and navigated to:

I then reviewed the Security log and located multiple **Event ID 4625** entries, which represent failed logon attempts. These events were generated after intentionally entering incorrect passwords on the Windows lock screen. Each 4625 event included details such as the username targeted, the logon type, the timestamp, and the failure reason.

### Why it matters:
Event ID 4625 is one of the most important indicators of suspicious authentication activity.  
SOC analysts rely on these logs to detect brute‑force attempts, password‑guessing attacks, and unauthorized access.  
By reviewing these events directly in Event Viewer, I learned how Windows records failed logins and how to interpret the information attackers leave behind. This step connects the simulated attack to the real evidence stored in system logs.

### Skills learned:
- Navigating Windows Event Viewer  
- Identifying authentication-related Event IDs  
- Understanding how failed logons are recorded  
- Recognizing brute‑force patterns in security logs  
- Building foundational SOC investigation skills  

<img width="784" height="526" alt="image" src="https://github.com/user-attachments/assets/ccd76797-7e9c-40fe-b275-d94415c97bde" />

## Step 6: Analyze the Failed Logon Events

### What I did:
Reviewed the details inside several **Event ID 4625** entries in the Windows Security log. I examined fields such as the targeted username, logon type, failure reason, timestamp, and other metadata. This helped me understand exactly how Windows records failed authentication attempts and what information is available to defenders during an investigation.

### Why it matters:
Simply seeing a 4625 event is not enough — SOC analysts must understand the context behind each field.  
By analyzing these events, I learned how to identify:

- Whether the login was local or remote  
- Which account was targeted  
- How many times the attacker attempted access  
- The reason the login failed (e.g., bad password)  
- Patterns that indicate brute‑force or password‑guessing attacks  

This step builds the analytical skills needed to investigate real incidents and distinguish normal user mistakes from malicious activity.

### Skills learned:
- Reading and interpreting Windows Security event fields  
- Understanding logon types and failure reasons  
- Identifying suspicious authentication patterns  
- Building investigative thinking used in SOC environments  

<img width="198" height="14" alt="image" src="https://github.com/user-attachments/assets/8d3b5f56-97a6-429f-adaa-12093e75189e" />

Absolutely Joey — here is a **clean, GitHub‑ready rewrite** of your Lab 3 section that clearly explains:

- What Lab 3 is actually about  
- That **before starting Lab 3**, you had to install and configure all required machines (Windows client, pfSense, Ubuntu Wazuh Server)  
- That you encountered system‑level issues on the Ubuntu server  
- And that you fixed them before beginning the actual lab tasks  

This version fits perfectly into your README and matches the real structure of your course.

---

# ##  Lab 3 — Preparing the Wazuh SIEM Server  
### **Fixing Sudo, Root Access & System Recovery Issues Before Starting the Lab**

##  **Overview**
Before beginning the official tasks in **Lab 3**, I first needed to install and configure all required virtual machines for the SIEM environment:

- **pfSense Firewall**  
- **Windows Client Machine**  
- **Ubuntu Wazuh Server (SIEM Server)**  

These machines form the core of the lab environment.  
However, during the setup of the **Ubuntu Wazuh Server**, I encountered several system‑level issues that prevented me from starting Lab 3 properly.

This section documents the troubleshooting and repair process I completed **before** beginning the actual lab instructions.

---

##  **Problem Summary**
When preparing the Ubuntu server for Wazuh installation, the following issues were discovered:

- `sudo` was not installed  
- `su -` failed with **Authentication failure**  
- The root account had **no password**  
- The user account (`joey`) was **not in the sudo group**  
- The system could not install or modify packages  
- The Wazuh installer could not run  

These issues made it impossible to begin Lab 3’s SIEM configuration steps.

---

##  **Step 1 — Boot Into Recovery Mode**
To regain administrative access, the VM was rebooted into Ubuntu’s Recovery Mode.

1. Restart the VM  
2. Open the GRUB menu (Shift/Esc during boot)  
3. Select:

   ```
   Advanced options for Ubuntu
   ```

4. Choose the entry ending with:

   ```
   (recovery mode)
   ```

5. From the Recovery Menu, select:

   ```
   root – Drop to root shell prompt
   ```

This provides root access without requiring a password.

---

##  **Step 2 — Remount Filesystem as Read/Write**
Recovery mode mounts the system read‑only. To make changes:

```bash
mount -o remount,rw /
```

---

##  **Step 3 — Set a Root Password**
The root account had no password, which is why `su -` failed earlier.

```bash
passwd
```

A new root password was created successfully.

---

##  **Step 4 — Install sudo**
The system was missing the `sudo` package entirely. It was installed using the root shell:

```bash
apt update
apt install sudo -y
```

---

##  **Step 5 — Add User “joey” to the sudo Group**
To allow the normal user to run administrative commands:

```bash
usermod -aG sudo joey
```

---

##  **Step 6 — Reset Joey’s User Password**
After rebooting, `sudo` still failed because the user password was incorrect.  
The password was reset from the root shell:

```bash
su -
passwd joey
```

Password updated successfully.

---

##  **Step 7 — Verify sudo Access**
Logged back in as **joey** and tested:

```bash
sudo ls
```

The password prompt worked, confirming:

- sudo is installed  
- user is in the sudo group  
- authentication works correctly  

The system is now fully repaired and ready for the **actual Lab 3 tasks**.

---


---

##  **Skills Learned**
- Using Ubuntu Recovery Mode for system repair  
- Resetting root and user passwords  
- Installing missing system packages  
- Managing sudo permissions  
- Understanding Linux boot and recovery workflows  
- Preparing a server for SIEM deployment  

---

---
Absolutely, Joey — I can take what you already have and elevate it into a **clean, professional, deeply explained GitHub Part 1** that shows:

- what you did  
- the exact commands you wrote  
- what went wrong  
- why it went wrong  
- how you fixed it  
- what you learned from each issue  

This version reads like something a real junior SOC analyst or cybersecurity student would proudly put on GitHub.  
Here it is — polished, structured, and detailed.

---

# **Lab 3 – Part 1: Wazuh All‑In‑One Installation**  
## **Overview**
Part 1 of Lab 3 involved installing the Wazuh All‑In‑One SIEM stack on my Ubuntu Server VM.  
This required downloading the installer, resolving multiple command‑execution issues, and running the installation with the correct flags.

This documentation explains **exactly what I did**, the **problems I encountered**, the **commands I used**, and **what I learned** from each step.

---

# **Step‑by‑Step Process, Issues, Fixes & Lessons Learned**

---

## **1. Attempting to Download the Wazuh Installer**
### **What I Tried**
I attempted to download the Wazuh installer using curl:

```bash
curl -v -0 https://packages.wazuh.com/4.7/wazuh-install.sh
```

### **Issue**
The command didn’t work because I accidentally used:

- `-0` (zero) ❌  
instead of  
- `-O` (capital O) ✔  

Curl treated `-0` as an invalid flag, so the file never downloaded.

### **Fix**
I corrected the command:

```bash
curl -v -O https://packages.wazuh.com/4.7/wazuh-install.sh
```

This successfully downloaded the script.

### **What I Learned**
- Curl flags are case‑sensitive.  
- `-O` saves the file with its original name.  
- A single character typo can break an entire workflow.  
- Always re‑read your command before assuming the tool is broken.

---

## **2. Shell Was Not Executing Commands Properly**
### **What Happened**
Before fixing the curl typo, my terminal kept re-running a script every time I pressed Enter.  
Commands like `curl`, `ls`, and even simple `echo` weren’t executing.

### **Diagnosis**
I checked running processes:

```bash
ps -f
```

This confirmed no script was running — meaning the shell had finally returned to normal.

### **Fix**
Once the shell was behaving normally, commands executed correctly again.

### **What I Learned**
- When the terminal behaves strangely, check the running processes.  
- A stuck shell can mimic “broken commands.”  
- `ps -f` is a quick way to confirm whether a script is hijacking your session.

---

## **3. Running the Installer Without Flags**
### **What I Tried**
I ran the installer normally:

```bash
sudo bash wazuh-install.sh
```

### **Issue**
The script only displayed the help menu.  
Newer Wazuh versions require you to explicitly choose an installation mode.

### **Fix**
I selected the All‑In‑One mode:

```bash
sudo bash wazuh-install.sh -a
```

### **What I Learned**
- Wazuh no longer defaults to AIO mode.  
- The help menu isn’t an error — it’s telling you what it needs.  
- Always read installer output carefully before assuming something is wrong.

---

## **4. Ubuntu Version Not in Recommended List**
### **What Happened**
The installer stopped with:

```
ERROR: The current system does not match the recommended list.
Use -i|--ignore-check to skip this check.
```

My VM was running a newer Ubuntu version than Wazuh officially supports.

### **Fix**
I combined both required flags:

```bash
sudo bash wazuh-install.sh -a -i
```

This bypassed the OS check and allowed the installation to continue.

### **What I Learned**
- Compatibility warnings aren’t always blockers — especially in lab environments.  
- The installer tells you exactly how to bypass checks safely.  
- Combining flags is often necessary for advanced installers.

---

# **Final Working Command**

```bash
sudo bash wazuh-install.sh -a -i
```

This triggered the full installation of:

- Wazuh Indexer  
- Wazuh Manager  
- Filebeat  
- Wazuh Dashboard  

---


# **Skills Learned in Part 1**
- Troubleshooting shell execution issues  
- Understanding curl flags and download behaviour  
- Reading installer help menus and interpreting errors  
- Using flags to control installer behaviour  
- Bypassing compatibility checks safely  
- Understanding the Wazuh installation workflow  

---

Evidence  of part one :

<img width="637" height="431" alt="image" src="https://github.com/user-attachments/assets/53ffbcc0-3389-4d40-84a2-e506820a992e" />
