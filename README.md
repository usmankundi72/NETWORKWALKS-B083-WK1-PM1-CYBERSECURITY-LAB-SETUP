# NETWORKWALKS-B083-WK1-PM1-CYBERSECURITY-LAB-SETUP
🔐#**Cybersecurity Lab Environment Setup**

Rebuilding my go-to pentesting lab — this time as part of my internship documentation

<p align="center"> <img src="https://img.shields.io/badge/Skill-Cybersecurity-404040?style=flat-square&labelColor=C00000" /> <img src="https://img.shields.io/badge/Ver-Virtualbox%20v7.2-0070C0?style=flat-square&labelColor=000000" /> <img src="https://img.shields.io/badge/Kali%20Linux-v2026.2-E87500?style=flat-square&labelColor=000000&logo=kalilinux&logoColor=white" /> <img src="https://img.shields.io/badge/Skill-Linux-404040?style=flat-square&labelColor=C00000" /> <img src="https://img.shields.io/badge/Network-10.0.0.0%2F24-238F89?style=flat-square&labelColor=000000" /> <img src="https://img.shields.io/badge/Penetration%20Testing-C00000?style=flat-square&labelColor=000000&logo=kalilinux&logoColor=white" /> <img src="https://img.shields.io/badge/Skill-Virtualization-404040?style=flat-square&labelColor=C00000" /> <img src="https://img.shields.io/badge/GitHub-404040?style=flat-square&labelColor=0070C0&logo=github&logoColor=white" /> <img src="https://img.shields.io/badge/NetworkWalks-404040?style=flat-square&labelColor=C00000" /> <img src="https://img.shields.io/badge/Ethical%20Hacking-E87500?style=flat-square&labelColor=000000&logo=kalilinux&logoColor=white" /> <img src="https://img.shields.io/badge/Muhammad%20Usman%20Kundi-Batch%20B083-C00000?style=flat-square" /> </p>

## 📌 **Project Overview**
I've set up this VirtualBox + Kali Linux lab combination more times than I can count at this point, but this build is different: it's the baseline environment I'm using for my internship work, so I wanted the setup documented properly instead of just living in my head.

This isn't a "first time touching a VM" writeup — it's a record of how I configure a clean, isolated lab quickly and reliably, plus the specific choices I made this round (network layout, resource allocation, snapshot strategy) so I — or anyone reviewing my work — can see exactly what's running and why.

The environment sits on its own private network, isolated from anything else on the host, with room to drop in additional target VMs as the internship tasks require.

## 🎯 **What This Setup Covers**
Standing up VirtualBox and getting Kali Linux imported and running

Building a dedicated NAT Network so the lab stays isolated while allowing VMs to communicate with each other

Configuring Kali with a consistent IP address instead of relying on a changing DHCP address

Testing connectivity, DNS resolution, and basic security tools

Creating a clean snapshot so the baseline environment can be restored when needed

## 🛡️ **Why This Lab Exists**
The purpose of this lab is to have an isolated environment for security testing and practice without affecting systems outside the lab.

Typical uses include:

Network reconnaissance and enumeration
Port scanning
Vulnerability assessment
Packet capture and analysis
Web application security testing
Exploitation practice against intentionally vulnerable targets
Testing security tools in a controlled environment

⚠️ Scope note: All testing is limited to systems I own or have explicit authorization to test.


## 🏗️ **Lab Architecture**

<img width="1366" height="768" alt="Screenshot (11)" src="https://github.com/user-attachments/assets/2d52b2cc-353d-4810-a3d0-4fd645f787ca" />


## ⚙️ **Lab Configuration**


| 🧩 **Component**       | ⚙️ **Configuration** |
| ---------------------- | -------------------- |
| 🖥️ **Host OS**         | Windows 10           |
| 🧠 **Host RAM**        | 8 GB                 |
| ⚡ **Processor**       | Intel Core i7        |
| 🧰 **Hypervisor**      | VirtualBox 7.2       |
| 🐉 **Security OS**     | Kali Linux 2026.2    |
| 🧠 **Kali RAM**        | 2048 MB              |
| 🌐 **Virtual Network** | **NAT Network**      |
| 📡 **Network Address** | 10.0.0.0/24          |
| 🐧 **Kali IP Address** | 10.0.0.2/24          |
| 🚪 **Default Gateway** | 10.0.0.1             |
| 🌍 **DNS Server**      | 8.8.8.8              |


# 🪜 **Build Process**
## **Step 1. Install & Extract the Kali Package**
Kali was installed and  was provided as a .7z archive, so I used 7-Zip to extract the package before importing the VM into VirtualBox.

## **Step 2. Install VirtualBox**
Installed VirtualBox and prepared it for the Kali Linux VM.

### **Step 3. Build the NAT Network**
I created a dedicated NAT Network in VirtualBox instead of using the default NAT configuration. This allows multiple VMs on the same virtual network to communicate with each other while still providing internet access.
Configuration:
Network Name: NatNetwork
IPv4 Prefix:  10.0.0.0/24
DHCP:         Enabled
IPv6:         Disabled


<img width="1364" height="768" alt="{AE039402-98E4-4869-B2D7-6124D395B77D}" src="https://github.com/user-attachments/assets/92298e0b-800c-4461-a2df-61284a5dd50c" />

Using a NAT Network is useful for this lab because additional attacker and target VMs can communicate with each other while remaining separated from the host's main network.

### **Step 4. Import and Configure Kali**
I imported the Kali Linux VM into VirtualBox and configured its network adapter to use the NAT Network.

Adapter 1
Attached to:  NAT Network
Network:      NatNetwork
Adapter Type: Intel PRO/1000 MT Desktop

The VM was allocated:

RAM: 5153 MB

<img width="1366" height="768" alt="Screenshot (11)" src="https://github.com/user-attachments/assets/02933b87-69bc-41da-9317-de6b082f2cdc" />


A shared folder was also configured between the host and guest to make it easier to transfer files such as scripts, wordlists, and scan results.

### **Step 5. Configure the Kali IP Address**

After confirming that Kali received an IP address through DHCP, I configured a fixed IP address for the lab. Since the NAT Network uses the 10.0.0.0/24 network, the Kali VM was assigned an address within the same network.

This makes the environment easier to document and ensures the Kali VM can be reached at the same address after restarting.

Network:     10.0.0.0/24
IP Address:  10.0.0.2
Subnet Mask: 255.255.255.0
Gateway:     10.0.0.1
DNS:         8.8.8.8


<img width="1280" height="703" alt="Screenshot_2026-09-18_13_02_05" src="https://github.com/user-attachments/assets/69729b58-0d6e-48b6-8e0c-be0be399f909" />


After applying the configuration, I checked the network interface and tested connectivity to confirm that the settings were working correctly.

### **Step 6. Clean Snapshot**

After completing the basic configuration and connectivity checks, I created a snapshot of the clean Kali environment.

The snapshot serves as a restore point before installing additional tools or making further configuration changes. If something breaks during a security exercise, the VM can be restored to this clean state instead of rebuilding the environment from scratch.

Clean Kali - Network Setup

<img width="1280" height="703" alt="Screenshot_2026-09-18_14_49_50" src="https://github.com/user-attachments/assets/92c43b02-5a32-42b9-9a6f-15327b0781a3" />

The snapshot was taken after confirming that the network configuration, IP address, gateway, DNS, and basic tools were working correctly.



# 🔎 **Verifying the Build**


| ✅ **Test**                       | 🧾 **Command**                | 🎯 **Expected Result**        |
| --------------------------------- | ------------------------------- | ------------------------------- |
| 🌐 **Check IP address**           | `ip a`                          | Correct Kali IP displayed       |
| 📡 **Test gateway**               | `ping <gateway IP>`             | Successful replies              |
| 🌍 **Test internet connectivity** | `ping 8.8.8.8`                  | Successful replies              |
| 🔎 **Test DNS resolution**        | `nslookup <domain>`             | Domain resolves                 |
| 🧰 **Verify Nmap**                | `nmap --version`                | Nmap version displayed          |
| 🔄 **Verify snapshot**            | Restore snapshot and run `ip a` | Baseline configuration restored |



# 💡 **Notes From This Build**

Nothing here was new to me technically, but a few things stood out as worth doing differently — or doing more deliberately — now that this lab supports internship work instead of just practice.

### **On NAT vs NAT Network**

A standard NAT adapter is useful when a VM mainly needs internet access. A NAT Network is more suitable for this lab because multiple VMs can communicate with each other while still using NAT for external connectivity.

### **On treating the lab like infrastructure, not a one-off VM**

Using a consistent network configuration, keeping a known IP address, taking snapshots, and documenting the setup makes the lab easier to maintain and rebuild.

### **On what I'd change next time**

For the next rebuild, I would look for ways to make the setup faster and more consistent, such as keeping the configuration steps organized and using the same network and snapshot structure from the start.

---

# 🔐 **Security & Ethical Use**

Everything in this lab stays within systems I own or have explicit authorization to test — internship work included.

---

# 🔗 **Tools & Resources**

* **7-Zip:** https://7-zip.org/download.html
* **VirtualBox:** https://virtualbox.org/wiki/Downloads
* **Kali Linux:** https://kali.org/get-kali

---

# 👤 **Author**

**Muhammad Usman Kundi**
Cybersecurity Intern — Batch B083

LinkedIn: `<www.linkedin.com/in/usmankundi-cybersecurity>`

---

## 📌 **Project Information**
Program Name: Cybersecurity at Networkwalks | Week: 01 | Project: Cybersecurity & Pentesting Lab Setup | Repository: GitHub






