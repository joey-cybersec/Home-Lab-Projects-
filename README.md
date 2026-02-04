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



# 🧪 Lab 2: Windows Logging & Attack Detection (Blue Team)

## 🎯 Goal
Learn how attacks appear in Windows security logs and how SOC analysts detect suspicious authentication activity.

---

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

## Step 4: Simulate an Attack from Linux

### What I did:
Used a Linux virtual machine to simulate multiple failed login attempts against the Windows system using incorrect credentials over network-based authentication methods such as SMB or RDP.

### Why it matters:
Simulating attacks provides hands-on experience with how malicious activity appears in logs. This mirrors how SOC analysts validate detections and investigate incidents.

### Skills learned:
- Attack simulation techniques  
- Brute-force authentication behavior  
- Blue team vs attacker perspective  

---

