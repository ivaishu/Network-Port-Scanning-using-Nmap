

# 🔍 Task 1: Local Network Port Scan using Nmap on Kali Linux

## 🎯 Objective
To scan the local network for active devices and their open ports using Nmap. This task helps understand the basics of network reconnaissance and service exposure.

---

## 🛠 Environment

| Tool | Description |
|------|-------------|
| **Kali Linux** | Used for performing the scan (running as a virtual machine) |
| **Oracle VirtualBox** | Virtualization platform to run Kali Linux |
| **Nmap** | Network scanner used to identify active hosts and open ports |
| **Wireshark** | Packet analyzer to observe scan traffic in real-time |

---
## 🧩 Optional: Install Tools (if not already installed)

### 📦 Install Nmap
```bash
sudo apt update
sudo apt install nmap
```

### 📦 Install Wireshark
```bash
sudo apt install wireshark
```
---
## 📡 Network Setup

- **Host Machine:** Windows 11
- **Virtual Machine:** Kali Linux running in VirtualBox
- **Network Adapter Mode:** Nat Network
- **Local IP Range Used:** `10.0.2.0/24`

   ### 🖥️ Virtual Machines Used

| VM Name       | Role         | OS            |
|---------------|--------------|----------------|
| **Kali Linux** | Scanner      | Kali (latest)  |
| **Debian**     | Target VM    | Debian Linux   |
| **Windows 7**  | Target VM    | Windows 7 SP1  |

### 🌐 NAT Network Details

- All VMs are connected to the same **VirtualBox NAT Network**, enabling:
  - Internet access from each VM
  - Full network communication between VMs (internal subnet)
- Example Subnet: `10.0.2.0/24` *(your range may differ)*
- Each VM gets a virtual IP like `10.0.2.15`, `10.0.2.16`, etc.

### 🔒 Why NAT Network is Safer for Public Sharing

Using a NAT network ensures the following privacy and security benefits when uploading your work online (e.g., to GitHub):

- 🛡 **Isolation from your real network**  
  - No scanning or exposure of real devices (e.g., your router, smart TV, printer)
- 📤 **Safe to publish**  
  - NAT-assigned IPs (e.g., `10.0.2.X`) are virtual and non-identifiable
- 🧪 **Perfect lab simulation**  
  - Ideal for practicing scanning in a controlled environment
- 🔐 **Limits exposure**  
  - Even if you accidentally run aggressive scans or exploits, your real system is unaffected

---

## 🧪 Commands Used

### 🔎 Step 1: Find Local IP Range
```bash
ifconfig
```
Used ifconfig to identify the local IP address. Example output: inet 10.0.2.5 → local subnet: 10.0.2.0/24

### 🔍 Step 2: Perform TCP SYN Scan
```bash

nmap -sS 10.0.2.0/24 -oN scan-results.txt
```
#### 🔍 Explanation of Options Used:
|Option |	Description |
|---------------|--------------|
| **-sS** |	TCP SYN Scan – Performs a "half-open" scan by sending only the SYN packet. If a SYN-ACK is received, the port is open. It doesn’t complete the full TCP handshake, making it faster and less detectable. |
| **10.0.2.0/24** |	Target IP Range – Scans all IPs from 10.0.2.1 to 10.0.2.254, which is the typical subnet used in a VirtualBox NAT Network. |
| **-oN scan-results.txt** |	Normal Output Format – Saves the scan results in a readable .txt file. Ideal for reviewing or including in reports and GitHub repositories. |

### 📶 Optional: Setup to Capture Traffic Using Wireshark

Wireshark helps visualize packets sent during the Nmap scan.

#### 🛠 Quick Steps:

- **Launch Wireshark with root**:
    ```bash
   sudo wireshark
   ```
- **Select the active interface (e.g., eth0, ens33). Check with**:

- **(Optional) Set a capture filter:**
     -TCP only: tcp or SYN packets: tcp[tcpflags] & tcp-syn != 0
  
- **Start capture (blue shark fin icon).**

- **In another terminal, run:**
```bash
nmap -sS 10.0.2.0/24
```
- **Stop capture (red square) and save as:**
   -wireshark-capture.pcapng

---
## 📄 Output Summary

The TCP SYN scan (`nmap -sS 10.0.2.0/24`) was executed across the NAT network in Oracle VirtualBox, targeting two virtual machines:

---

### 🖥️ Debian VM – Detected Open Ports and Services:

| Port | Service         | Description & Impact |
|------|------------------|-----------------------|
| `21/tcp` | FTP              | File Transfer Protocol — commonly used for file uploads. If anonymous access or weak credentials are enabled, it could lead to unauthorized data access or server compromise. |
| `80/tcp` | HTTP             | Web server — may expose web applications or configuration pages. Outdated or unpatched web apps on this port can be vulnerable to exploits like XSS or RCE. |
| `2222/tcp` | EtherNet/IP-1    | Industrial protocol (often used in SCADA/ICS). If unintentionally exposed, it may allow interaction with automation hardware or simulation services. |

> 🔐 **Security Note:** These services should be monitored, patched, and access-controlled — especially if exposed beyond the virtual lab. FTP in particular is insecure by default as it transmits data (including credentials) in plain text.

---

### 🖥️ Windows 7 VM – Scan Result:

- **All 1000 default TCP ports returned:** `closed`
- Indicates no services were listening or reachable on the common ports scanned.
- Could be due to firewall settings or minimal service configuration.
---
## ✅ Conclusion 

This task demonstrated fundamental network reconnaissance skills using Nmap and Wireshark in a controlled virtual lab setup. The TCP SYN scan successfully identified active hosts and services running within the VirtualBox NAT network.

### 🔍 Key Findings:

- The **Debian VM** was hosting multiple services (`FTP`, `HTTP`, and `EtherNet/IP`) — highlighting typical attack surfaces in networked systems.
- The **Windows 7 VM** returned all ports as closed, likely due to strict firewall rules or limited active services.
- **Wireshark** validated the behavior of a SYN scan by capturing SYN, SYN-ACK, and RST packets — showcasing the low-noise nature of half-open scanning.
- Using a **NAT Network** ensured isolation from the real LAN, making it safe to analyze and share scan results publicly (e.g., on GitHub).


