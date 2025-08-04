

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



