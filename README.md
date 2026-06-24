# All My Home-Lab-Projects EVIDENCE-
Cyber security exercises 


#  Lab 1


Evidence :
<img width="959" height="522" alt="image" src="https://github.com/user-attachments/assets/891389a5-6c81-4d1a-b823-43beecca4a6c" /> 


<img width="947" height="533" alt="image" src="https://github.com/user-attachments/assets/61bf8513-d232-4f79-8716-912fa2fbd442" />



#  Lab 2: Windows Logging & Attack Detection (Blue Team)


<img width="784" height="526" alt="image" src="https://github.com/user-attachments/assets/ccd76797-7e9c-40fe-b275-d94415c97bde" />



<img width="198" height="14" alt="image" src="https://github.com/user-attachments/assets/8d3b5f56-97a6-429f-adaa-12093e75189e" />



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

- what you did  
- the exact commands you wrote  
- what went wrong  
- why it went wrong  
- how you fixed it  
- what you learned from each issue  



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

---

# **Lab 4 — Message Confidentiality & Authentication Using GPG**

## **Step 1: Generate GPG Key Pairs (Alice & Bob)**

### **What I did:**  
I created two separate asymmetric key pairs using the `gpg --gen-key` command:

- **Alice** → `alice@example.com`  
- **Bob** → `bob@example.com`  

Each key pair was created with a unique passphrase to allow encryption, decryption, and signing.

### **Why it matters:**  
Public‑key cryptography is the foundation of secure communication.  
It ensures:

- Only the intended recipient can read encrypted data  
- Only the legitimate sender can sign/authenticate messages  
- Messages cannot be modified without detection  

This mirrors how real‑world secure email, SSH, and VPN authentication work.

### **Skills learned:**  
- Generating asymmetric key pairs  
- Understanding public vs private key roles  
- Managing cryptographic identities in Linux  

---

## **Step 2: Export Public & Private Keys**

### **What I did:**  
I exported both public and private keys for Alice and Bob into ASCII‑armored `.asc` files using:

```bash
gpg --armor --export alice@example.com > alice_pubkey.asc
gpg --armor --export-secret-keys alice@example.com > alice_privatekey.asc

gpg --armor --export bob@example.com > bob_pubkey.asc
gpg --armor --export-secret-keys bob@example.com > bob_privatekey.asc
```

This produced four key files:

- `alice_pubkey.asc`  
- `alice_privatekey.asc`  
- `bob_pubkey.asc`  
- `bob_privatekey.asc`


### **Why it matters:**  
Exported keys allow secure sharing of public keys and safe backup of private keys.  
This is essential for:

- Secure messaging  
- Identity verification  
- Key distribution in real networks  

### **Skills learned:**  
- Exporting keys in ASCII‑armored format  
- Managing key files in Linux  
- Understanding how keys are stored and shared  

Evidence:
<img width="229" height="110" alt="image" src="https://github.com/user-attachments/assets/89619c1e-c166-4bf0-9b13-6708c26f7fc7" />

---

## **Step 3: Message Confidentiality (Encrypt → Decrypt)**

### **What I did:**  
I created a plaintext file:

```bash
nano mytext
```

Then encrypted it using **Alice’s public key**:

```bash
gpg --output mytext.gpg --encrypt --recipient alice@example.com mytext
```

Alice decrypted it using her **private key**:

```bash
gpg --output decrypted_mytext --decrypt mytext.gpg
```

### **Why it matters:**  
This demonstrates **confidentiality** — only the intended recipient (Alice) can decrypt the message because only she has the matching private key.

Even if the encrypted file is intercepted, it cannot be read.

### **Skills learned:**  
- Encrypting files with a recipient’s public key  
- Decrypting files using a private key  
- Understanding asymmetric confidentiality  

Eivdence: <img width="311" height="145" alt="image" src="https://github.com/user-attachments/assets/d8dac33d-0dc0-4e41-a614-d44a2d9a156e" />

---

## **Step 4: Message Authentication (Sign → Verify)**

### **What I did:**  
Bob signed the message using his **private key**:

```bash
gpg --local-user bob@example.com --sign mytext
```

Alice verified the signature using Bob’s **public key**:

```bash
gpg --verify mytext.gpg
```

Alice also decrypted the signed message:

```bash
gpg --output decrypted_signed_text --decrypt mytext.gpg
```

### **Why it matters:**  
This demonstrates **authentication and integrity**:

- Only Bob can create a valid signature (private key)  
- Anyone with Bob’s public key can verify it  
- If the message is modified, verification fails  

This mirrors real‑world digital signatures used in software distribution, secure email, and certificates.

### **Skills learned:**  
- Signing files with a private key  
- Verifying signatures with a public key  
- Understanding message integrity and sender authenticity  

Evidence : <img width="310" height="92" alt="image" src="https://github.com/user-attachments/assets/fe4158e5-4349-4e18-a684-c15603f0e699" />

---

## **Overall Skills Gained**

- Public‑key cryptography fundamentals  
- Key generation, export, and management  
- File encryption and decryption  
- Digital signatures and verification  
- Secure communication workflow  
- Linux command‑line cryptography  

---

## **iptables Firewall Configuration (Parts 1.1.1 – 1.1.3)**

---

##  Overview

This lab demonstrates how to configure a Linux firewall using **iptables** to:

* Enforce strict default-deny policies
* Allow only required traffic
* Block unauthorized communication
* Enable essential services (DNS, HTTP)

---

##  Part 1 — Default Policy Configuration

### Step 1 — Reset Firewall Rules

```bash
sudo iptables -F     # Flush rules
sudo iptables -X     # Delete custom chains
sudo iptables -Z     # Reset counters
```

### Step 2 — Apply Default DROP Policies

```bash
sudo iptables -P INPUT DROP
sudo iptables -P OUTPUT DROP
sudo iptables -P FORWARD DROP
```

### Step 3 — Verify Blocking

```bash
ping google.com
```

Expected result:  Fail (all traffic blocked)

### Step 4 — Allow Outbound Traffic

```bash
sudo iptables -A OUTPUT -j ACCEPT
```

### Step 5 — Allow Established Connections

```bash
sudo iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT
```

### Step 6 — Re-test Connectivity

```bash
ping google.com
```

Expected result: ✅ Success

Part 1 Evidence:
<img width="367" height="266" alt="image" src="https://github.com/user-attachments/assets/21814586-fa9b-495b-b323-cdfdfed56ff3" />

<img width="325" height="205" alt="image" src="https://github.com/user-attachments/assets/0b98da3e-c6fb-42bf-85c8-ad203ec98419" />

---

##  Part 2 — Block Outbound Traffic

### Step 1 — Remove OUTPUT Rules

```bash
sudo iptables -F OUTPUT
```

### Step 2 — Enforce OUTPUT DROP

```bash
sudo iptables -P OUTPUT DROP
```

### Step 3 — Test Restrictions

Ping local host:

```bash
ping 192.168.51.6
```

Test web access:

```bash
curl http://example.com
```

Expected result: ❌ Both fail

---
Part 2 Evidence:
<img width="321" height="62" alt="image" src="https://github.com/user-attachments/assets/2b1af2a2-d9fe-44bd-b05f-07371c566828" />
<img width="234" height="42" alt="image" src="https://github.com/user-attachments/assets/d8219a2f-5aa0-4eed-92f8-8b65f344549d" />



##  Part 3 — Allow Required Services

### Step 1 — Allow Specific Host Access

```bash
sudo iptables -A OUTPUT -d 192.168.51.6 -j ACCEPT
```

Test:

```bash
ping 192.168.51.6
```

Expected result:  Success

---

### Step 2 — Enable DNS (Port 53 UDP)

```bash
sudo iptables -A OUTPUT -p udp --dport 53 -j ACCEPT
```

---

### Step 3 — Enable HTTP (Port 80 TCP)

```bash
sudo iptables -A OUTPUT -p tcp --dport 80 -j ACCEPT
```

---

### Step 4 — Test Web Access

```bash
curl http://example.com
```

Expected result:  Success

---

##  Key Concept — Why DNS is Required

DNS (Domain Name System) translates human-readable domain names into IP addresses.

Example:

```
example.com → 93.184.216.34
```

Without DNS:

* The system cannot resolve domain names
* HTTP/HTTPS connections fail before starting

---
Part 3 Evidence:
<img width="260" height="79" alt="image" src="https://github.com/user-attachments/assets/ff099e77-2421-4c1c-ac9a-e84dced18869" />
<img width="321" height="215" alt="image" src="https://github.com/user-attachments/assets/5b95cc9c-d9c2-4599-accf-ea6f44d5c7e9" />

---

---

## Conclusion

The firewall was successfully configured using a **default-deny approach**, ensuring:

* Only trusted traffic is permitted
* Attack surface is minimized
* Network behaviour is controlled and predictable

---


---


Incident Response Simulation Lab
