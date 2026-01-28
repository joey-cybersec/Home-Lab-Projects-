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

